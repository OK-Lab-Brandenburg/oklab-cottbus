---
layout: post
title: Meeting 15.04.2021
excerpt: Daten an CouchDB auf cloudant.com übertragen
category: Meetings
---

Um die Daten der [CO2-Ampel](https://civilize.it/t/co2-ampel-verstehbahnhof/342) ins Web zu übertragen haben wir mit CouchDB herumexperimentiert.


## `jq` - Command-line JSON processor

* `man jq` is your friend
* [jq cookbook](https://github.com/stedolan/jq/wiki/Cookbook)

### get an cloudant.com access token

```sh
#!/bin/bash
# source this file once to get CLOUDANT_TOKEN environment variable for cloudant.sh

RESP=$(curl -s \
         --header "Content-Type: application/x-www-form-urlencoded" \
         --header "Accept: application/json" \
         --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
         --data-urlencode "apikey=_H7VZ92r1LrHpq4XJLblY11-S17J05RcD69XUWzY82pw" \
         "https://iam.cloud.ibm.com/identity/token")

if [ $# -eq 0 ]
then
    export CLOUDANT_TOKEN=$(echo $RESP|jq -r .access_token )
else
    echo $RESP
fi

```

### do couchdb stuff with token

```sh
#!/bin/bash -xe

# http method GET and POST get inferred
#METHOD="GET"

HOST="0f752390-1de3-4f38-8e20-d74fbc4c4199-bluemix.cloudant.com"
DB="test"

# query view index by_created_at
#QUERY="_design/app/_view/by_created_at?limit=5&include_docs=true&descending=true"

# get continuous updates
SINCE="now"
# to not wait upto 12s for a new doc (or no new are created), enter a 'seq' here
#SINCE="1186-g1AAAAdreJy11D9OwzAUBvAIkBjpDWCCKcV2bNeRkOiAYGCgbGzg9-yoqvoHoXaGG8AN4AZwA7gB3ABuADcosR3RZmoYvDhSZH0_vXzKGyZJ0uqvm2TXAE5ubNdApw2jFDG1s9TYlPI2Diczo8fT9thOh-X9NZ3A1nw-H_TX9cmofLEpGSgg0DilDoqVILTKE7Yr89ibKjccJWscVDfZanPHmfuVeerNAhVVBW8c5My9v6u0AUocelj7uJmATBaqedJ_64SuQ88q9MijQhK0MovXaM-Zl5XZ86Y2hEtU8Rq9cuakNiczCBRUxEavHXpbGxRMRpCJeI2ON8ozuSsfpXvv4HMPIzKFeSdaq8F9CO7jYmAJlHKjozUb3KfgPi-5hKocIV67AX4J8OtiUYCU1pAidsNvAX5fLAuTSSE4i9zwR3A_nXvgXa45kdRGbvgruN_OvfAutVpojPj_BvgnwEuLg9uOtTlvnjb4Bf-dUUU"
QUERY="_changes?feed=continuous&include_docs=true&since=$SINCE"

#JQ='.seq'
JQ='{error: .error, id: .doc._id, created_at: .doc.created_at, warmed_up: .doc.warmed_up, co2: .doc.co2, temp: .doc.temp}'

# useful curl options:
# -v ... verbose (shows HTTP Headers)
# -s ... silent
# -S ... shows error (like token expired)
# -N ... unbuffered output (doesn't wait for request to finish, useful for continuous feeds)

#useful jq options:
# -c ... compact output (less whitespace)
# --unbuffered ... unbuffered output (doesn't wait for end of input)
# -r ... raw output of strings ('lol' instead of '"lol"')

curl -SsN \
  -H "Authorization: Bearer $CLOUDANT_TOKEN" \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  "https://$HOST/$DB/$QUERY" |
    jq -c --unbuffered "$JQ"

```

### advanced use

1) create a file full of tokens

```sh
while(true); do ./cloudant_token.sh -c >> tokens; utimer -qc 10s; done
```

2) wait 3480 seconds
3) get a token with expiration only 2min in future instead of 3600s

```sh
#!/bin/bash -xe

SEC=$(($(date +%s) + 120))

OBJ=$(jq -cr "select(.expiration >= $SEC)" <tokens |head -n 1)
TOK=$(echo $OBJ | jq ".access_token")
EXP=$(echo $OBJ | jq ".expiration")
printf "token = $TOK;\nexpiration = $EXP;\n" | xclip -i
```

## playing with a token

there are 3 parts, seperated by a dot. the first 2 are base64 encoded json, the third is probably an RSA-signature (using the specifiey key id).
see: [JSON Web Token](https://en.wikipedia.org/wiki/JSON_Web_Token).

```sh
TOK1='eyJraWQiOiIyMDIxMDMyMTE4MzUiLCJhbGciOiJSUzI1NiJ9'
echo $TOK1 | base64 -d | jq

>>> {"kid":"202103211835","alg":"RS256"}

TOK2='eyJpYW1faWQiOiJpYW0tU2VydmljZUlkLTFiZWUzNWU3LTQxZmUtNDU1MC1hM2RhLTg2MTBhMGM4YmFjNCIsImlkIjoiaWFtLVNlcnZpY2VJZC0xYmVlMzVlNy00MWZlLTQ1NTAtYTNkYS04NjEwYTBjOGJhYzQiLCJyZWFsbWlkIjoiaWFtIiwianRpIjoiYzEzOWVjNmEtNzIzNy00OGRiLWI2MjYtOGQzMjc2OTJlYWMyIiwiaWRlbnRpZmllciI6IlNlcnZpY2VJZC0xYmVlMzVlNy00MWZlLTQ1NTAtYTNkYS04NjEwYTBjOGJhYzQiLCJuYW1lIjoid3JpdGFibGUiLCJzdWIiOiJTZXJ2aWNlSWQtMWJlZTM1ZTctNDFmZS00NTUwLWEzZGEtODYxMGEwYzhiYWM0Iiwic3ViX3R5cGUiOiJTZXJ2aWNlSWQiLCJhdXRobiI6eyJzdWIiOiJpYW0tU2VydmljZUlkLTFiZWUzNWU3LTQxZmUtNDU1MC1hM2RhLTg2MTBhMGM4YmFjNCIsImlhbV9pZCI6ImlhbS1pYW0tU2VydmljZUlkLTFiZWUzNWU3LTQxZmUtNDU1MC1hM2RhLTg2MTBhMGM4YmFjNCIsInN1Yl90eXBlIjoiMSIsIm5hbWUiOiJ3cml0YWJsZSJ9LCJhY2NvdW50Ijp7InZhbGlkIjp0cnVlLCJic3MiOiIyZjIyNzBhYmU1NTM0YzZjOWVmOTc2NjdiNDQ3YjM0MCIsImZyb3plbiI6dHJ1ZX0sImlhdCI6MTYxODQ4MjQzNCwiZXhwIjoxNjE4NDg2MDM0LCJpc3MiOiJodHRwczovL2lhbS5jbG91ZC5pYm0uY29tL2lkZW50aXR5IiwiZ3JhbnRfdHlwZSI6InVybjppYm06cGFyYW1zOm9hdXRoOmdyYW50LXR5cGU6YXBpa2V5Iiwic2NvcGUiOiJpYm0gb3BlbmlkIiwiY2xpZW50X2lkIjoiZGVmYXVsdCIsImFjciI6MSwiYW1yIjpbInB3ZCJdfQ'
echo $TOK2 | base64 -d

>> {"iam_id":"iam-ServiceId-1bee35e7-41fe-4550-a3da-8610a0c8bac4","id":"iam-ServiceId-1bee35e7-41fe-4550-a3da-8610a0c8bac4","realmid":"iam","jti":"c139ec6a-7237-48db-b626-8d327692eac2","identifier":"ServiceId-1bee35e7-41fe-4550-a3da-8610a0c8bac4","name":"writable","sub":"ServiceId-1bee35e7-41fe-4550-a3da-8610a0c8bac4","sub_type":"ServiceId","authn":{"sub":"iam-ServiceId-1bee35e7-41fe-4550-a3da-8610a0c8bac4","iam_id":"iam-iam-ServiceId-1bee35e7-41fe-4550-a3da-8610a0c8bac4","sub_type":"1","name":"writable"},"account":{"valid":true,"bss":"2f2270abe5534c6c9ef97667b447b340","frozen":true},"iat":1618482434,"exp":1618486034,"iss":"https://iam.cloud.ibm.com/identity","grant_type":"urn:ibm:params:oauth:grant-type:apikey","scope":"ibm openid","client_id":"default","acr":1,"amr":["pwd"]}
```


Die Signatur wird dadurch erzeugt, dass der Header und der Payload im Base64 kodierten und durch einen Punkt getrennten Format mit der spezifizierten Hashmethode gehashed wird: 

```js
var encodedString = base64UrlEncode(header) + "." + base64UrlEncode(payload);
var hash = HMACSHA256(encodedString, secret);
```

Aber das Secret ist nur dem Server bekannt. [source](https://de.wikipedia.org/wiki/JSON_Web_Token#Signatur)

## CouchDB and Cloudant.com resources

### getting started

https://cloud.ibm.com/docs/Cloudant?topic=Cloudant-getting-started-with-cloudant

### authentification

https://cloud.ibm.com/docs/Cloudant?topic=Cloudant-connecting
https://cloud.ibm.com/docs/account?topic=account-iamapikeysforservices#token_auth

### couchdb doku

https://docs.couchdb.org/en/2.3.1/index.html (cloudant uses couchdb 2.1.1)
