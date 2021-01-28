---
layout: post
title: "Meeting 14.01.2021"
author: "OKlab"
excerpt: "Themen: CCC-Vortrag: Parkhäuser in Magdeburg, Projektidee: Straßennamen m/w/d"
category: Meetings
---


# Rückblick CCC

Interessanter Vortrag vom CCC 2020: [Parkhäuser in Marburg](https://media.ccc.de/v/rc3-663787-a_few_quantitative_thoughts_on_parking_in_marburg) -> wann sind welche Parkhäuser wie stark besetzt?



# Projektidee: Straßennamen m/w/d

Welche Stadt hat die meisten nach Frauen benannten Straßen im Vergleich zu den nach Männern benannten Straßen? Inspiriert von [diesem Projekt](https://code-for-magdeburg.github.io/streetnames-md/) des OKlab Magdeburg ([Sourcecode](https://github.com/code-for-magdeburg/streetnames-md)).

Koordinaten, die die Stadt Magdeburg in OpenStreetMap beschreiben:

https://github.com/code-for-magdeburg/streetnames-md/blob/master/magdeburg.poly

Scheinbar werden mittels Shell-Skripten den Namen Geschlechter zugewiesen.
Diese erstellen letztlich die *gender.json* Datei, die dann mittels Leaflet dargestellt wird:

https://github.com/code-for-magdeburg/streetnames-md/tree/master/docs

Dafür benötigt es eine CSV Tabelle mit Straßennamen und Geschlecht Spalten (also scheinbar nicht automatisiert :cry:):

https://github.com/code-for-magdeburg/streetnames-md/blob/master/names-magdeburg.csv

## Overpass API

Mit der [Overpass API](https://overpass-turbo.eu/) kann man schnell und einfach bestimmte Merkmale von mit OpenStreetMap abfragen und darstellen:



Z.B. Abfrage der Stadtgrenze von Cottbus:
    
```
[out:json][timeout:25];
(
  node["place"="city"]({{bbox}}); 
  way["place"="city"]({{bbox}});
  relation["place"="city"]({{bbox}});
);
out body;
>;
out skel qt;
```

Wenn man es als GeoJSON exportiert kann man die Koordinaten extrahieren.


Die Overpass API kann auch mit einem R-Paket abgefragt werden. Hier ein Blogpost dazu: [Accessing OpenStreetMap data with R | Dominic Royé](https://dominicroye.github.io/en/2018/accessing-openstreetmap-data-with-r/)

## Wikidata

Um evtl. automatisiert Straßennamen und Geschlechter abzufragen könnte man [WikiData](https://www.wikidata.org) verwenden: 


* https://www.wikidata.org/wiki/Q100480333

* https://query.wikidata.org/

* Es gibt ein [R-Paket zum herunterladen von Wikidata](https://cran.r-project.org/web/packages/WikidataR/vignettes/Introduction.html)

Hier Podcasts und Talks mit Hintergrundinformationen zu Wikidata:

* [Chaosradio Express 205 "Wikidata"](https://cre.fm/cre205-wikidata)

* [# rC3 - Introduction to Wikidata - YouTube](https://www.youtube.com/watch?v=AcY08gIweDQ)
* [# rC3 - Wikidata for (Data) Journalists - YouTube](https://www.youtube.com/watch?v=oiVOG3FuUFQ)



