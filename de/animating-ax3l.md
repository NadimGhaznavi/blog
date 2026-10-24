---
layout: posts
author_profile: true
title: "Ax3l beim Snake-Spielen animieren"
date: 2026-10-11
lang: de
locale: de
language_name: Deutsch
translation_key: animating-ax3l
permalink: /de/journal/animating-ax3l/
category: journal
tags:

---

Seit einiger Zeit experimentiere ich mit verschiedenen Varianten von Patrick Loebers Tutorial [Train an AI to Play Snake](https://www.youtube.com/watch?v=L8ypSXwyBds&t=4s). Es ist mein Lieblingsprojekt zum Entspannen: unterhaltsam, faszinierend und lehrreich. Während ich stundenlang Snake-Simulationen beobachtet habe, konntet ihr bisher nur Screenshots sehen. Damit ist jetzt Schluss:

![Animiertes Snake-Spiel](/images/2026-10-11-snake-game.gif)

Auf der [Live-Website von Ax3l](https://snakeweb.osoyalce.com) gibt es mehr zu sehen! :)

## Technische Details

- **Aufzeichnung:** [Snake Lab](https://snakelabserver.osoyalce.com), der Simulationsserver, speichert nach jedem Zug den Spielzustand einschließlich Schlangenposition, Position des Futters, Punktzahl und Spielfeldkonfiguration. So entstehen die Einzelbilder.
- **Auswahl:** Von den vielen Spielen einer Simulation wird das mit der höchsten Punktzahl behalten.
- **Speicherung:** Am Ende der Simulation werden die Einzelbilder des besten Spiels als JSON-Datensätze in MariaDB abgelegt.

Das Projekt [Snake Web](https://snakeweb.osoyalce.com) zeigt den aktuellen Stand des [Ax3l-Experiments](https://ax3l.osoyalce.com). Alle 30 Minuten werden neue Daten und Animationen auf der [Website](https://snakeweb.osoyalce.com) veröffentlicht.

- **Abruf:** [Snake Web](https://snakeweb.osoyalce.com) fragt die gespeicherten Einzelbilder über das Request-Reply-Protokoll von ZeroMQ bei [Snake Lab](https://snakelabserver.osoyalce.com) ab. Dafür verwendet es die eindeutige Lauf-ID der Simulation.
- **Darstellung:** Die Python-Bibliothek Pillow erzeugt daraus ein animiertes GIF, zeichnet das Spielfeld und die Punktzahl und ergänzt die kleine Animation, bei der das Futter durch den Schlangenkörper wandert.
- **Veröffentlichung:** [Snake Web](https://snakeweb.osoyalce.com) wählt die besten Spiele des Tages aus, erzeugt die GIF-Dateien und commitet und pusht sie automatisch in das GitHub-Repository der Website. GitHub Pages liefert sie als statische Bilder aus.
