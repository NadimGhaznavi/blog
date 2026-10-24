---
layout: posts
author_profile: true
title: "Ein Dienst für dynamisches DNS"
date: 2026-10-07
lang: de
locale: de
language_name: Deutsch
translation_key: bmdynip-project
permalink: /de/journal/bmdynip-project/
category: journal
tags:
  - dynamic-dns
  - dns
  - godaddy
  - self-hosted
  - homelab
---

Dieses Projekt begann als Cronjob mit einer einfachen Textdatei. Ehrlich gesagt würde das für ein kleines Heimnetz mit zwei oder drei Rechnern völlig ausreichen. Bei mir laufen inzwischen acht Rechner mit unterschiedlichen Versionen von Fremdsoftware und eigenen Anwendungen. Den DNS-Eintrag meiner öffentlichen IP-Adresse aktuell zu halten, ist nur ein weiteres Teil meiner Heimlabor-Infrastruktur. Falls ihr euch fragt, warum ich eine Lösung auf *Unternehmensniveau* gebaut habe: *Weil ich nun einmal so ticke!* :D

## Überblick über BMDynIP

![BMDynIP-Architektur](/images/2026-10-07-bmdynip-architecture.png)

**BMDynIP** ist ein dynamischer DNS-Dienst für Menschen, die:

- Eine dynamische öffentliche IP-Adresse haben.
- Eine Domain bei GoDaddy registriert haben, etwa **osoyalce.com**.
- Einen oder mehrere Dienste selbst im Internet anbieten möchten.

BMDynIP verwendet das [GoDaddy-CLI-Werkzeug](https://github.com/godaddy/cli), um DNS-A-Einträge für einen oder mehrere Hostnamen wie `www.osoyalce.com` zu verwalten. So zeigen diese Einträge stets auf die aktuelle öffentliche IP-Adresse.

Der Dienst:

- Läuft als Linux-Dienst.
- Bietet eine Weboberfläche für die Konfiguration und Verwaltung von Hostnamen.
- Erkennt die öffentliche IP-Adresse und überprüft sie regelmäßig.
- Aktualisiert bei Änderungen automatisch die DNS-Einträge bei GoDaddy.
- Speichert Status und Verlauf in MariaDB.

## Die Weboberfläche von BMDynIP

![Weboberfläche von BMDynIP](/images/2026-10-07-bmdynip-web-interface.png)

Hier ist ein Screenshot der Weboberfläche.

## Benutzerhandbücher für BMDynIP

- [Architektur](https://bmdynip.osoyalce.com/pages/architecture/)
- [Installation](https://bmdynip.osoyalce.com/pages/installation/)
  - [GoDaddy-Authentifizierung für root](http://bmdynip.osoyalce.com/pages/root-authentication/)
- [Konfiguration](https://bmdynip.osoyalce.com/pages/configuration/)
- [Deinstallation](https://bmdynip.osoyalce.com/pages/uninstall/)
- [Weboberfläche](https://bmdynip.osoyalce.com/pages/web-interface/)
- [Betrieb und Überwachung](https://bmdynip.osoyalce.com/pages/running/)
