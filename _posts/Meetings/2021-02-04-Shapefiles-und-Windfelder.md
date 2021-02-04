---
layout: post
title: "Meeting 04.02.2021"
author: "OKlab"
excerpt: "Themen: Umsetzung Straßennamenprojekt m/w/d"
category: Meetings
---

# Protokoll 04.02.2021


# [Straßennamen](https://github.com/oklab-cottbus/streetnames-cb)

Tobias hat es geschafft ein Shapefile mit den Cottbuser Bezirken zu erstellen

Testcode dazu in [ISSUE #1](https://github.com/oklab-cottbus/streetnames-cb/issues/1)

link zum wfs cottbus:

https://cardo.cottbus.de/net3/public/IkxCms/Default.aspx?pgid=75

https://www.openstreetmap.org/api/0.6/relation/62430


Sebastian hat weiter an SPARQL gearbeitet [ISSUE #3](https://github.com/oklab-cottbus/streetnames-cb/issues/3)

# Sonstiges

## Influxdb

Wie man die RAM-usage der influxdb verringern kann

https://www.influxdata.com/blog/how-to-overcome-memory-usage-challenges-with-the-time-series-index/


## Windfinder und Winddaten


Luftdaten.info Windlayer analysiert

Windlayer besteht aus einem [json](https://maps.sensor.community/data/v1/wind.json)

Wie dieses Json generiert wird, wissen wir nicht.

Im [Websitesource](https://github.com/opendata-stuttgart/feinstaub-map-v2/blob/master/src/index.html
) ist nicht ersichtlich wie diese generiert wird:

Nur das in [commit 0bede...](https://github.com/opendata-stuttgart/feinstaub-map-v2/commit/0bede12e9c57dc1a5bf821b04a088a67376221ac) der Windlayer hinzugefügt wird.

Leaflet package für die velocity-Darstellung: https://github.com/danwild/leaflet-velocity



### weitere Windspielereien

https://de.windfinder.com/historical-weather-data/

[leaflet velocity R-Paket](https://trafficonese.github.io/leaflet.extras2/)

http://aparshin.github.io/leaflet-GIBS/examples/


## Sourcode Größen im Vergleich

Zum Schluss kam noch die Frage auf, wie wohl die Größen verschiedener Sourcecodes im Vergleich sind. Dazu haben wir [diese schöne Darstellung](https://infobeautiful3.s3.amazonaws.com/2013/10/1276_lines_of_code5.png) gefunden. Der Sourcecode von Firefox ist z.B. größer als der Code, der eine Boing 787 zum fliegen bringt.
