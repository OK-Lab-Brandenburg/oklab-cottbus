---
layout: post
title: "Meeting 25.02.2021"
author: "OKlab"
excerpt: "Themen: MediaWiki und SPARQL Querys m/w/d"
category: Meetings
---

# [Straßennamen](https://github.com/oklab-cottbus/streetnames-cb)

![](https://raw.githubusercontent.com/OK-Lab-Brandenburg/oklab-cottbus/main/_posts/Meetings/get_gender.gif)

An der Namensabfrage gearbeitet weitere infos in [ISSUE#3](https://github.com/oklab-cottbus/streetnames-cb/issues/3)

  Es lassen sich nun Geschlechter von Personen abfragen.
  Die implementierung ist sehr langsam, da 3-53 http-querys gemacht werden müssen.
  Das Problem liegt darin, dass nicht bestimmt werden kann, dass nur Personen zurückgegeben werden.
  Die Abfrage "Douglas" gibt als erstes Element in der Liste ein "Namen" zurück. Dieser hat in WikiData aber kein Geschlecht.
  Wir suchen solange durch die Liste, bis ein Element gefunden wurde welches das Attribut "Mensch" hat.
  Von diesem Element wird dann das Geschlecht abgefragt.
  
  
Mit SPARQL querys gespielt:
  Wir haben einen Query geschrieben, der uns alle in Wikidata verzeichneten Straßen eines Ortes ausgibt die nach "etwas" benannt sind:
  Q79007 steht für Innerortsstraße
  Q64 steht für Berlin
  P31 bedeutet "ist ein/Instanz von"
  P131 bedeutet "der Verwaltungszone untergeordnet"
  P138 bedeutet "benannt nach"

```
  SELECT ?Stra_e ?Stra_eLabel WHERE {
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],de". }
  ?Stra_e wdt:P31 wd:Q79007.
  ?Stra_e wdt:P131 wd:Q64.
  ?Stra_e wdt:P138 ?person.
  }
  LIMIT 100
```    

Diese Abfrage kann [hier](https://query.wikidata.org/) ausprobiert werden.
  
