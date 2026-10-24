---
layout: posts
author_profile: true
title: "Lokale GeoIP-Datenbank"
date: 2026-10-05
lang: de
locale: de
language_name: Deutsch
translation_key: bmgeoip-project
permalink: /de/journal/bmgeoip-project/
category: journal
tags:
  - geoip
  - ip-geolocation
  - mariadb
  - self-hosted
  - homelab
---

Das [Bear & Moose GeoIP-Projekt](https://bmgeoip.osoyalce.com) stellt einen einfach zu nutzenden GeoIP-Dienst für Anwendungen und interaktive Abfragen bereit.

![Weboberfläche](/images/2026-10-07-bmdynip-web-interface.png)

- BmGeoIP bringt einfache Skripte für Installation, Aktualisierung und Deinstallation mit: `install.sh`, `upgrade.sh` und `uninstall.sh`.
- Öffentliche Geolokalisierungsdaten werden nach einem konfigurierbaren Zeitplan von ipapi.is heruntergeladen.
- Die Datensätze werden in einer optimierten MariaDB-Datenbank gespeichert.
- Eine Weboberfläche unter https://localhost:54300 erlaubt interaktive IP-Abfragen und die Konfiguration des Download-Zeitplans.
- Anwendungen können Abfragen über ZeroMQ oder HTTP stellen.
