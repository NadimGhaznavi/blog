---
layout: posts
author_profile: true
title: "DiskHA: Datenträgerreplikation, Zustandsprüfung und E-Mail-Warnungen"
date: 2026-10-16
lang: de
locale: de
language_name: Deutsch
translation_key: diskha-project
permalink: /de/journal/diskha-project/
category: journal
tags:
  - DiskHA
  - cron
  - rsync
  - data replication
  - SMART monitoring
---

Ich betreibe einen NFS-Server in meinem Netzwerk. Zwei große Festplatten und ein Cronjob mit rsync bieten **grundlegenden** Datenschutz. Gegen Überspannung, Feuer, Diebstahl oder versehentliches Löschen hilft das nicht. Aber es funktioniert und ist genau richtig für meine Heiminstallation. Wenn eine Festplatte ausfällt, was irgendwann unvermeidlich ist:

- Fahre ich den Server herunter.
- Trenne beide Laufwerke.
- Bestelle Ersatz.
- Baue das neue Laufwerk ein und schließe das verbliebene funktionierende wieder an.
- Synchronisiere die Daten von der guten Platte auf die neue.

Das ist in Ordnung. Bis es nicht mehr in Ordnung ist.

Zusätzlich braucht man einen Cronjob für regelmäßige SMART-Zustandsprüfungen **und** E-Mail-Benachrichtigungen bei Problemen. Mit msmtp und einem Gmail-Konto lässt sich das [leicht einrichten](https://diskha.osoyalce.com/pages/msmtp-gmail-setup/).

Das war’s!!

Tatsächlich ist dies einer der wichtigsten Teile meiner Infrastruktur.

Also habe ich daraus eine Anwendung gemacht: [DiskHA](https://diskha.osoyalce.com).

![Architektur von DiskHA](/images/2026-10-16-diskha-architecture.png)

Die [Website](https://diskha.osoyalce.com) erklärt alles Weitere. Ich muss es hier nicht wiederholen.
