---
layout: posts
author_profile: true
title: "Eine LLM sortiert meine Filmsammlung"
date: 2026-09-27
lang: de
locale: de
language_name: Deutsch
translation_key: r3eling
permalink: /de/journal/r3eling/
category: journal
tags:
  - R3el
  - LLM
  - Local AI
  - Media Management
  - Automation
  - Python
  - TMDB
  - Self-Hosted
---

Unser erster Fernseher war ein kleines Schwarz-Weiß-Gerät, auf dem wir gelegentlich *Animal Kingdom* sahen. Dann kamen Beta und VHS, und man konnte Filme zu Hause anschauen! Später folgten DVDs.

Alles wurde digital. Speicherplatz wurde immer billiger, und ich begann, digitale Kopien der Filme und Serien aufzubewahren, die mir gefielen. Warum? Weil ich Dinge gerne wieder anschaue und sammle. Es erschien mir harmlos.

Ich dachte: „Großartig! Das beseitigt den ganzen Krempel. Jetzt muss ich nur noch ein paar Dateien sortieren!“ Also trennte ich mich von den DVDs und stürzte mich ins digitale Leben.

---

### Der Sumpf

Das war vor ungefähr 25 Jahren. Heute habe ich über tausend Filme, fast 200 Fernsehserien und Staffeln über Staffeln voller Episoden.

Ich versuchte, durch Umbenennen von Dateien und Ordnern Ordnung zu halten. Das funktionierte einigermaßen. Aber bei dieser Menge wurde das Suchen und Stöbern mit der Zeit immer schwieriger und schließlich fast unmöglich. Es gab einfach zu viele Verzeichnisse.

---

### Die Lösung

Die Lösung lag auf der Hand:

- Eine Oberfläche zum Durchsuchen und Stöbern erstellen.
- Zuerst jedoch die Daten aufräumen, damit das Programm sie findet.

Gleichzeitig einen Katalogeintrag anzulegen, war nur logisch.

[The Movie Database](https://tmdb.org) bietet eine kostenlose öffentliche Datenbank mit Film- und Serientiteln, Postern, Episodenbeschreibungen, Besetzung und vielem mehr. Damit könnte ich eine richtig schöne Sammlung erstellen.

---

### Datenbereinigung

Das alles **auf keinen Fall** von Hand!

Software für sämtliche Sonderfälle zu schreiben, schien entmutigend. Unzählige seltsame Dateinamen mussten mit [TMDB](https://tmdb.com) abgeglichen werden. Ehrlich gesagt traute ich mir das allein nicht zu.

Was ich brauchte, war ein Praktikant, ein Laufbursche oder ein Roboter, der sich Verzeichnis für Verzeichnis vorarbeitet.

Und genau so etwas hatte ich! Kurz zuvor hatte ich [Qwen 3.5 4B](https://huggingface.co/Qwen/Qwen3.5-4B) heruntergeladen, eine kleine LLM ähnlich ChatGPT, die auf meinem eigenen Rechner läuft. Sie war schlau genug für die Aufgabe.

---

### Die LLM als Praktikant

Gerade hatte ich [Ax3l](https://ax3l.osoyalce.com) fertiggestellt, einen autonomen KI-Agenten, der Tausende von Simulationen durchführte, um ein einfaches neuronales Netz zu optimieren. Parameter ändern, Simulation starten, Ergebnis auswerten, wieder ändern ...

Im Vergleich dazu war das neue Projekt einfach. Die LLM sollte:

- Ein Verzeichnis scannen.
- Den Inhalt einem [TMDB](https://tmdb.org)-Katalogeintrag zuordnen.
- Dateien umbenennen und an einen neuen, ordentlichen Ort verschieben.
- Einen Datenbankeintrag für die Datei erzeugen.

Ich nannte das Projekt **R3el** und begann mit dem Entwurf.

![Architektur von R3el](/images/2026-09-27-r3el-architecture.png)

Na gut, doch nicht ganz einfach. Aber mit dem Code von [Ax3l](https://ax3l.osoyalce.com) als Ausgangspunkt war es machbar.

---

### Die Steueroberfläche

Ich wusste, dass ich irgendwann einen Webkatalog mit Suche und Browse-Funktion bauen würde. Also beschloss ich, gleich auch die Steueroberfläche dort unterzubringen.

![Steueroberfläche](/images/2026-09-27-r3el-control.png)

---

### Das Ereignisprotokoll

Aus meiner Erfahrung mit LLMs wusste ich, wie wichtig detaillierte Berichte sind. Wieder verwendete ich Teile des [Ax3l](https://ax3l.osoyalce.com)-Codes und baute ein ausführliches **Ereignisprotokoll**, um den Prozess zu überwachen, anzupassen und Fehler zu finden.

![Ereignisprotokoll](/images/2026-09-27-r3el-eventlog.png)

Fast einen ganzen Tag brauchte ich, um alle interessanten Meldungen aufzulisten und zu benennen. Es war eine ausgezeichnete Investition und half mir erheblich bei der Entwicklung.

---

### Es funktionierte!!

Ein wenig Feinarbeit war nötig: zusätzliche Anwendungsfälle, bessere Mustererkennung und ein bisschen manuelle Dateibereinigung. Aber es funktionierte!!! In den folgenden Tagen stellte ich jeweils mehrere Verzeichnisse in die Warteschlange und ließ das System arbeiten.

![R3el während der Verarbeitung](/images/2026-09-27-r3el-processing.png)

- R3el scannte die Dateien.
- Es erkannte Serien und Filmtitel.
- Es benannte Dateien um und bereinigte Dateinamen.
- Es löschte leere alte Verzeichnisse.
- Es lud Poster und Informationen über Schauspieler und Produzenten herunter.
- Es legte die passenden Datenbankeinträge an.

---

### Stöbern und Suchen

Zum Schluss baute ich eine schöne Oberfläche zum Stöbern und Suchen:

![R3el-Katalog](/images/2026-09-27-r3el-catalogue.png)

Dieser Teil **war** leicht. Es gibt unzählige Beispiele für solche Benutzeroberflächen, daher ließ sich eine einfache Version schnell umsetzen.

![Serienansicht in R3el](/images/2026-09-27-r3el-tv-show.png)

---

### Fertig

Das Projekt ist fertig. R3el scannt zwar noch, aber ich denke, bis heute Abend ist es durch. Anfangs fragte ich mich, ob das nicht übertrieben sei. Doch für eine Datenbereinigung in dieser Größenordnung fällt mir immer noch kein anderer Weg ein.

Und ziemlich cool ist es auch, oder? :)

![R3el-Logo](/images/2026-09-27-r3el.png)

---

### Links

- [R3el-Projektwebsite](https://r3el.osoyalce.com)
- [R3el auf GitHub](https://github.com/NadimGhaznavi/r3el)
- [R3el-Änderungsprotokoll](https://github.com/NadimGhaznavi/r3el/blob/main/CHANGELOG.md)
