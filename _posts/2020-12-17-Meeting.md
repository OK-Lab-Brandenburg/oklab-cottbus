---
layout: post
title: Meeting 17.12.2020
excerpt: Wir haben das Logo in die Weibseite eingebunden und Sebastian hat seinen EU-Kalender-Scraper wieder aufgesetzt
category: Meetings
---

# Webseite

Wir haben das [SVG-Logo](https://github.com/oklab-cottbus/Logo_Design) nun in die Webseite eingebaut und darauf geachtet, dass es runterskaliert, wenn man die Seite auf dem Tablet oder Handy anschaut (hier der [github commit](https://github.com/OK-Lab-Brandenburg/oklab-cottbus/commit/d20c6e8eca0a222869efc876ca8cc39e63b90b7b) zu den Änderungen). Die Farben müssten eventuell noch ein wenig angepasst werden. 

Nanu hat die Funktion von [Jekyll](https://jekyllrb.com/) und [scss](https://sass-lang.com/) erläutert und demonstriert.

# OpenEuCalendar:
Ein altes Script von Sebastian wurde wiederbelebt. Es sammelt alle Termine der EUKommissionsmitglieder über deren [offenen Onlinekalender](https://ec.europa.eu/commission/commissioners/calendar/commission/commissioners/2019-2024%3Fpage%3D1_en).
Das Script läuft nun in einem Dockercontainer auf unserem Experimentalserver und wird jede Stunde ausgeführt. Die gesammelten Daten werden vorerst in einer csv Datei gespeichert aus der im späteren Verlauf Visualisierungen und Auswertungen erstellt werden können. Das Script und die Dokumentation wird noch https://github.com/oklab-cottbus/OpenEUCalendar hochgeladen.
Beim nächsten Treffen können wir dann mal schauen was da für neue Daten hereinplätschern...

Für die Webseite sollten wir dann für die einzelnen Projekte auch Projektseiten anlegen mit Beschreibungen zum Projekt, Ansprechpartner und "wie kann ich mitmachen" und wenn bei dem Projekt Visualisierungen generiert werden könnten die dort auch eingestellt werden. Hier wurde ein [Issue](https://github.com/OK-Lab-Brandenburg/oklab-cottbus/issues/1) angelegt. Da können wir dann demnächst dran arbeiten.
