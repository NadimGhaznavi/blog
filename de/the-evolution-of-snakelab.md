---
layout: posts
author_profile: true
title: "Die Entwicklung von Snake Lab (2/4)"
date: 2026-09-05
lang: de
locale: de
language_name: Deutsch
translation_key: the-evolution-of-snakelab
permalink: /de/journal/the-evolution-of-snakelab/
category: journal
tags:
  - AI
  - LLM
  - Qwen
  - llama.cpp
  - GGUF
---

Im [letzten Beitrag](/de/journal/local-llm-write-haikus/) habe ich eine lokale LLM auf einer alten HP-Z440-Workstation eingerichtet und eine einfache Gesprächsschleife geschrieben. Ich erzeugte Zufallszahlen, übergab sie der LLM und bat sie, Haikus darüber zu verfassen.

![Snake-Lab-Logo](/images/2026-09-05-snake-lab.png)

Es funktionierte.

In meinem Keller stand ein Computer und schrieb Gedichte über Zufallszahlen.

Die nächste Frage lag auf der Hand: **Was kann ich diese Maschine noch machen lassen?**

Beim Experimentieren stellte ich fest, dass das Modell Patrick Loebers KI-Snake-Experiment kannte. Das weckte meine Aufmerksamkeit, denn ich kannte dieses Projekt sehr gut und hatte den Code schon lange untersucht und verändert.

Und da hatte ich eine Idee.

Was wäre, wenn ich der LLM statt Zufallszahlen die Ergebnisse einer Snake-Simulation übergeben würde?

Ich könnte ihr die Konfiguration samt Ergebnissen zeigen, sie darüber nachdenken lassen und um eine bessere Konfiguration bitten. Dann würde ich diese im Simulator ausführen, die Ergebnisse sammeln, an die LLM zurückgeben und den Ablauf wiederholen.

Anders gesagt:

**Könnte ich eine LLM eine andere KI optimieren lassen?**

Aus dieser Idee wurde später **Fr3d**.

Aber es gab ein Problem.

---

## Das alte Projekt AI Snake Lab

Ich hatte Patrick Loebers Tutorial umgesetzt und danach weiterentwickelt. Ich portierte es auf **Textual** und ersetzte die alten Matplotlib-Diagramme durch integrierte **Textual-Plot**-Darstellungen. Die Anwendung wurde in Client- und Serverprozesse aufgeteilt, verbunden durch ZeroMQ. Simulationsergebnisse landeten in einer Datenbank.

Der Textual-Client konnte Simulationen konfigurieren, die Konfiguration an den Server senden, einen Lauf starten und ihn überwachen.

Das war schon weit mehr als ein Snake-Spiel.

Aber es blieb eine *Anwendung*, deren Architektur davon ausging, dass ein Mensch vor dem Client saß und Experimente durchführte.

Fr3d änderte diese Annahme.

---

## Das Labor bauen

Ich trennte den bestehenden Snake-Code ab und gründete ein neues unabhängiges Projekt: **Snake Lab Server**.

Die Grundidee war einfach: Dem Server sollte es egal sein, wer das Experiment durchführt.

Ein Client sendet eine JSON-Konfiguration über ZeroMQ. Der Server validiert sie, stellt das Experiment in eine Warteschlange, führt die Simulation aus, sammelt Messwerte und speichert die Ergebnisse in MariaDB.

Der Server selbst läuft als systemd-Dienst.

Damit kann eine LLM ihn verwenden. Oder ein Python-Skript. Oder eine Terminalanwendung. Oder etwas, an das ich noch gar nicht gedacht habe.

Der Simulator muss das nicht wissen.

Ich portierte auch den Textual-Client. Schließlich wollte ich weiterhin selbst Experimente starten und vor allem beobachten können.

Es ist viel befriedigender, die Schlange über das Feld kriechen zu sehen, als Zahlen in einer Logdatei vorbeiziehen zu lassen.

Aber jetzt war die Textual-Oberfläche nur noch **ein Client unter mehreren**.

Das war ein wichtiger Unterschied.

---

## Ein Experiment ist mehr als ein Rekord

Außerdem sollte Snake Lab Server streng festlegen, was ein Experiment ausmacht.

Die Konfiguration wird geprüft, bevor eine Simulation angenommen wird. Die verwendete Konfiguration und die Ergebnisse werden gespeichert. Während der Simulation werden Messwerte gesammelt, auch für einzelne Spiele und Episoden.

Denn ich wollte nicht nur fragen:

> Hat diese Schlange mehr Punkte erreicht?

Ich wollte fragen können:

> Was genau haben wir ausgeführt und was ist dabei passiert?

Wenn eine automatisierte Anlage irgendwann Hunderte Experimente durchführt, muss sich die Frage auch lange nach deren Abschluss beantworten lassen.

Sonst hätte ich kein Labor.

Sondern einen Spielautomaten.

---

## Bereit für den Wissenschaftler

Das war also Snake Lab Server.

Fr3d gab es noch nicht.

Noch fehlte ein autonomer LLM-Wissenschaftler, der Ergebnisse betrachtete, über Konfigurationen nachdachte und den nächsten Versuch auswählte. Diesen Teil musste ich erst bauen.

Doch nun gab es einen Arbeitsplatz für ihn.

Angefangen hatte ich mit einer KI, die Snake spielen lernte. Daraus wurde eine Client-Server-Anwendung für Experimente. Jetzt war sie erneut gewachsen, zu einem Dienst, der ausdrücklich dafür geschaffen wurde, dass **andere Programme kontrollierte Experimente mit der Snake-KI durchführen können**.

Das Labor war fertig.

Jetzt musste ich den Wissenschaftler bauen.

---

## Links

- [Projektwebsite von Snake Lab Server](https://snakelabserver.osoyalce.com)
- [Teil 1/4 der Artikelserie](/de/journal/local-llm-write-haikus/)
- Teil 2/4 ist dieser Beitrag.
- [Teil 3/4 der Artikelserie](/de/journal/prompt-engineering-101/)
