---
layout: post
title: Meeting 20.05.2021
author: "Sebastian, Martin, Tobias"
excerpt: Curl mpirun und Zebras
category: Meetings
---

# Protokoll 20.05.2021

# `curl` impfterminservice.de

Die Website www.impfterminservice.de bietet die Möglichkeit verfügbare  Impftermine deutschlandweit abzufragen und zu reservieren. Da häufig keine Termine zur Verfügung stehen wollten wir eine automatische Abfrage mit `curl` der Website ermöglichen.

Die eigentliche Terminabfrage geschieht in diesem Query:


https://097-iz.impfterminservice.de/rest/suche/termincheck?plz=03042&leistungsmerkmale=L920,L921,L922,L923

Als Ergebnis erhält man eine json response:

`{"termineVorhanden":false,"vorhandeneLeistungsmerkmale":[]}`

Hier eine Liste der möglichen Leistungsmerkmale über einen weiteren Query:

https://www.impfterminservice.de/assets/static/its/vaccination-list.json
```
[
  {
    "qualification": "L920",
    "name": "Comirnaty (BioNTech)",
    "short": "BioNTech",
    "tssname": "BioNTech",
    "interval": 40,
    "age": "16+",
    "tssage": "16-17"
  },
	{
    "qualification": "L921",
    "name": "mRNA-1273 (Moderna)",
    "short": "Moderna",
    "tssname": "Moderna, BioNTech",
	  "interval": 40,
    "age": "18+",
    "tssage": "18-59"
  },
	{
    "qualification": "L922",
    "name": "COVID-1912 (AstraZeneca)",
    "short": "AstraZeneca",
    "tssname": "Moderna, BioNTech, AstraZeneca",
    "interval": 40,
    "age": "60+",
    "tssage": "60+"
  },
  {
    "qualification": "L923",
    "name": "COVID-19 Vaccine Janssen (Johnson & Johnson)",
    "short": "Johnson&Johnson",
    "tssname": "Johnson&Johnson",
    "age": "18+"
  }
]
```

### Cookie benötigt

Wenn man die Abfrage einfach ausführt bekommt man eine leere json Antwort.

Wenn man aber in einer Session schon einmal auf www.impfterminservice.de war funktioniert die Abfrage wieder. Das deutet darauf hin, dass man auf dieser Seite Cookies erhält, die man für den Query benötigt. Somit muss man sich erst diese Cookies abholen und sie dann im Query verwenden.

Über den Netzwerkmonitor in Firefox kann man sich mit Rechtsclick auf den entsprechenden Request, dann`Copy`, dann  `as cURL` die komplette Abfrage inklusive Cookies ausgeben lassen.

Beispiel:

```
curl 'https://097-iz.impfterminservice.de/rest/suche/termincheck?plz=03042&leistungsmerkmale=L920,L921,L922,L923' -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:87.0) Gecko/20100101 Firefox/87.0' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'DNT: 1' -H 'Connection: keep-alive' -H 'Cookie: ak_bmsc=577E6B98479A226511695F877D18F8DDB819EF0EF019000096C8A660179D427A~plbABxVaDflfL1PLhA1L+D1blnHPdA/eaWl5OTkYfi0jO99yISfZ6/OMpY0eXmockMPPumBsQ8CVmfUfDSVK6mP6DqU+Aq8CociH8zjidZ3RhFCqV+Rk1HyewU1q56PHR6WTfHhXKPIHAAXe+dbP0WX5/M+mmlpjI/mWwfWZ5t0/sFv9CLciSjUUEDVbZoeSi6GQS8yIektzVEnAxYiEbiXtw/1uIwNUYaEHJU7xU0qGhMFKzVf8E9hlbmhQ4+ja5v; bm_sz=92DA23438961F1BF8884BCADD9526F9F~YAAQDu8ZuC2KGVd5AQAA54x/iwsVtv1HqFBKsqKmKEyU3glpluyCoT0DT7In7IOTOibrXbHWD2JK7RZHhFeh9wCuZ45Kw5IEJDXb7BuOV0cpiXN8cEvFv/v1r0xE2LiWwPZFv34UaWzYJHIQIvDGRIUA3qCvaGg/Ni8nhybvjihNIjzVw61JuW10bUb98qi5QpH4PcsV8hsvhA==; _abck=2F871389141352F7B06E109CB093111C~0~YAAQD+8ZuM5HNzV5AQAAFnmNiwUN3YxYXTerC9JUq67m7rIO9gGXNs4fo3n4ZSosAGjxd30QCALNRdB72S7wersOLxluYos4+czKB/Wz0RfIz95sX/ULE9cifiOf+k/k39n//+naTCVwKDGasA1Dar6pE4/H82kvc5N5lAdO6rDAKQmUQ5GwNB2u6eBPiK7XpxMDBj2fd1c7bM6tvBDQw/D1gfaQRzuawLY1fJYxBOx72QOHeBQxLv9nuebBXNSuM4uG9lGsyo4Am6g8obVhD6TPCSyeZRb7ptlJFtjv8eUvWWUwmv0xzQ4+Ck6PD8tAYqgGaf+GMkZCVTfeEQK6+sORQmTmTKgTz10BMHE0NswJPnYsHVA6CLAwJsji8h6bOwKYxH+heCi51Z7Ex9dFUQlSlEvN3F7swn961YP2Ih5puA==~-1~-1~-1; bm_sv=C8152A244FF803FDF12108EB06F7963D~+UuR2IPZogxeWIo6ppFLIzGAv+korf+/Shf1bdw6t6uxNY1A2wqARw+mWAn3nDweroaZJSFoqwchi942cNCiYi8MjCDYeWeI/KxBKyjEaxpqaduwKSw+FTYCSMVmre53mw9WxEEHvmEclvni3yC1CJevsJvIan1HxCo99Mgl50k=; akavpau_User_allowed=1621544575~id=4ea8067d444ab2260cbcc2a41d1cd0e2' -H 'Upgrade-Insecure-Requests: 1'
```

Die Cookies heißen:

- ak\_bmsc
- bm\_sz
- \_abck
- bm\_sv

Diese 4 Cookies werden also benötigt.

Schaut man sich in Firefox den ersten Aufruf von www.impfterminservice.de an, sieht man welche Cookies man von der Website bekommt. Hier sind auch genau die oben genannten 4 Cookies enthalten.

### Umsetzung mit Curl

Im ersten Schritt muss man die Website davon überzeugen, dass man eigentlich als Browser dort eine Anfrage stellt. Dafür muss man einen sogenannten `UserAgent` definieren der einem gängigen Browser entspricht. Für den hier verwendetet Firefox ist das: 

`Mozilla/5.0 (X11; Linux x86_64; rv:87.0) Gecko/20100101 Firefox/87.0`

Die `-A` Option in curl erlaubt es diesen UserAgent der Anfrage mit zu geben. Nun werden noch die Cookies beötigt.

Die Option `--cookie-jar <Filename>` erlaubt es Cookies, die von einer angefragten Website geliefert werden, in einer Datei abzuspeichern um sie dann mit `--cookie <Filename>` in der nächsten Anfrage mitzusenden.

Die Option `-i` gibt die gesendeten und erhaltenen HTTP Header mit aus (ähnlich `-v` aber ohne übermäßige TLS Ausgaben).

### Idee

Um die Cookies zu bekommen:

`curl --cookie-jar mycookie -iA "Mozilla/5.0 (X11; Linux x86_64; rv:87.0) Gecko/20100101 Firefox/87.0" https://www.impfterminservice.de/impftermine`

Termine abfragen und Cookies verwenden:

`url -i --cookie mycookie 'https://097-iz.impfterminservice.de/rest/suche/termincheck?plz=03042&leistungsmerkmale=L920,L921,L922,L923' -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:87.0) Gecko/20100101 Firefox/87.0' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,/;q=0.8' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'DNT: 1' -H 'Connection: keep-alive' -H 'Upgrade-Insecure-Requests: 1'`

(Zusätzliche Argumente wurden aus dem von Firefox generierten `curl` übernommen)

### Problem

Die erste Abfrage liefert nicht alle Cookies die benötigt werden. Es kommen nur drei Cookies an.

Header der ersten Abfrage gefiltert mit `grep "Set-Cookie"`:

```
Set-Cookie: akaalb_www=~op=wwwLoadBalancer:353origin|~rv=55~m=353origin:0|~os=4bf749f7d0b035df6e0aa0f4dd7a648b~id=a6e4eaa818aca40eeb0eba1a11987c04; path=/; Secure; SameSite=None
Set-Cookie: bm_sz=649C930255F88F7FA253CF66D25F85CB~YAAQD+8ZuIjcNjV5AQAAhBF5iwtDcMeYRxBENiBmB1BP4ttK5NRrgTcSM227PswF/QAg51ds4w/W+o7SygXhhzW5r5nWtdT4p9mHEUeIq6Q6Ljtg5Xk3C4AUloxeasZXlj+7aVMbTuuXPQrIUGnJggv9PTfCyuMq2FXCjufPbbBLjCit97JIudHFkCts7Gw32A0Unco05aIBbg==; Domain=.impfterminservice.de; Path=/; Expires=Fri, 21 May 2021 00:30:37 GMT; Max-Age=14400; HttpOnly
Set-Cookie: _abck=412BC725ABCDB9F30F802F6A335B32A3~-1~YAAQD+8ZuIncNjV5AQAAhBF5iwUgbKhqdHYnXw2YOiW/1Ya6+G10JQFgeLFc2xRNHeNNQ5ixXrfubvRAKXOQmHsz0cHDwhkPLhumDpG/DMZpEW2HY/3MfTb8MSs7qu40VQqjrxpYGc6eddjNwPIUTP86UhZ/R8/jEWJ9w32YGso2hKqiYexcTMpAcaiiYamEs4mUPGOcrL1dHRQI/bl3QU190euthOuBToMiqdjLqYhSYQ800DwqphHRCElAX6QjfYzFvfxWxd9MXPUJ+SfG+HAHVZI2gy7dBvkeWDl0aaZMAn4NDP/4p+ode7jjkwgY44zHXvX4k5K4aGMOrIuT3BFRPkFHWc81rcY57jaBWJ/7RhSm3ycJvLBmz78G1uUmRvbnEoIr~-1~-1~-1; Domain=.impfterminservice.de; Path=/; Expires=Fri, 20 May 2022 20:30:37 GMT; Max-Age=31536000; Secure
```

Es fehlen:
- bm\_sv
- \_abck

Führt man die Abfrage in Firefox aus sieht man aber, dass eigentlich alle Cookies in den Request-Cookies stehen, also der Browser diese bekommen haben muss.
![grafik](https://user-images.githubusercontent.com/64002240/119052186-802b9300-b9c4-11eb-9f2e-46ec62667426.png)



Fortsetzung folgt....


## `mpirun` von `PALM` benutzt nicht alle CPU Cores auf einer lokalen Maschine

>PALM is an advanced and modern meteorological model system for atmospheric and oceanic boundary-layer flows. 
> ([PALM Website](https://palm.muk.uni-hannover.de/trac))

`mpirun` Fehlermeldung:
>\--------------------------------------------------------------------------
>There are not enough slots available in the system to satisfy the 4 slots
>that were requested by the application:
>  ./xyz
>
>Either request fewer slots for your application, or make more slots available
>for use.
>\--------------------------------------------------------------------------

[StackOverflow](https://stackoverflow.com/a/48836143) sagt:

```
mpirun --use-hwthread-cpus ...
```

Diese Option muss dann auch in der `palm.config`, bei `%execute_command` hinzugefügt werden.

> I had to add the --use-hwthread-cpus option to
>
>%execute_command mpirun -n {{mpi_tasks}} ./palm
>
>in the .palm.config.default
[see ticket](https://palm.muk.uni-hannover.de/trac/ticket/1319#comment:13)

## Werkzeug des Tages

Der StackOverflow Faden oben empfielt auch das Tol `lstopo` aus dem `hwloc` Paket, das schön übersichtlich anzeigt wieviele CPU Cores es gibt, wieviele (Hyper)Threads die haben und wieviel Speicher in welchen Cache Layern und RAM zur Verfügung steht.

Weniger Information aber viel übersichtlicher als z.B. `cat /proc/cpuinfo`.

## Literaturempfehlungen des Tages

### [`How to invent everything` von Paul Ryan](https://www.howtoinventeverything.com/): 


> The survival guide for the stranded time traveller that teaches YOU how to invent, well, EVERYTHING
> 
> (zB Schrift, Sprache, Medizin, Ackerbau, Wie kann erkannt werden, Wo und Wann mensch gestrandet ist)

### [`The Innovators` von Walter Isaacson](https://www.penguinrandomhouse.de/Buch/The-Innovators/Walter-Isaacson/C-Bertelsmann/e474598.rhd)

> Sind sie jetzt Nerds, Weltverbesserer oder Spieler – diejenigen, die alles für möglich halten und nur durch die Frontscheibe schauen? Der Steve-Jobs-Biograf Walter Isaacson gibt diesen Vordenkern des digitalen Zeitalters ein Gesicht. Er blickt auf Erfinder und abenteuerlustige Unternehmer, die keine Grenzen akzeptieren, die unerbittlich und lustvoll Zukunft machen wollen. Die großen Namen wie Jobs und Gates stehen dabei immer für die Vielen, die in einem Zeitalter, das keine Alleinherrscher über Informationen duldet, permanent Ideen produzieren und Entwicklungen vorantreiben. Die Reise geht von Ada Lovelace über Alan Turing, John von Neumann, Konrad Zuse und Grace Hopper bis zu den genialen Kindern des Silicon Valley.


### [`Americapox: The Missing Plague` von CGPGrey](https://www.youtube.com/watch?v=JEYh5WACqEk)

> Why didn't the Europeans get sick when they made contact with the American Indians?

[Part2](https://www.youtube.com/watch?v=wOmjnioNulo): 
> Why didn't Africans on Zebra conquer the world?  Why don't we have war bears?


## Sonstiges

Geschichten von Interaktionen mit dem Rechenzentrum der BTU Cottbus, InfiniBand und PBS und mpirun auf HPC Clustern,...
