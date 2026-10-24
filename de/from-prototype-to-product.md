---
layout: posts
author_profile: true
title: "Vom Prototyp zum Produkt (4/4)"
date: 2026-09-19
lang: de
locale: de
language_name: Deutsch
translation_key: from-prototype-to-product
permalink: /de/journal/from-prototype-to-product/
category: journal
tags:
  - AI
  - LLM
  - Qwen
  - llama.cpp
  - GGUF
---

Fr3d funktionierte: Es konnte mit Snake Lab sprechen, Änderungen an der RNN-Konfiguration vorschlagen, Experimente ausführen, Ergebnisse betrachten und erneut beginnen. Damit war die Idee bewiesen: Eine lokale LLM konnte als autonomer Forscher ein Reinforcement-Learning-System über Hunderte Simulationen optimieren.

![Ax3l](/images/2026-09-17-ax3l.png)

Als ich mit Fr3d fertig war, wusste ich allerdings auch genau, *wie ich die nächste Version nicht bauen wollte*.

Der Code war organisch rund um das Experiment gewachsen. Prompts steckten direkt in der Anwendung, Zuständigkeiten überschnitten sich. Logs zeigten, *was* das Programm tat, aber nicht unbedingt *warum*. Änderungen erforderten das mühsame Verfolgen von inzwischen angesammelten Abhängigkeiten.

Fr3d war ein erfolgreicher Prototyp.

**Ax3l** war die Neufassung.

---

## Zuerst das Instrumentenbrett

Bei Ax3l begann ich mit den Architekturlektionen aus Fr3d.

Das Promptsystem wurde modular und datengesteuert. Verschiedene Arten der Interaktion mit der LLM erhielten eigene Promptklassen. Experimentdaten, Promptkonstruktion, Simulationssteuerung und Berichterstattung wurden sauber getrennt.

Vor allem war Beobachtbarkeit diesmal ein Kernbestandteil der Anwendung, nicht etwas, das ich später dazubauen wollte.

Ax3l bekam einen eigenen Berichtsserver und ein neues Ereignisprotokoll mit Datenbankanbindung. Ereignisse wurden in Kategorien und Unterkategorien organisiert, und die Logs wurden interaktiv.

Statt nur:

> Simulation abgeschlossen.

konnte ich auf das Ereignis klicken und die Simulation, ihre Konfiguration, den auslösenden Prompt, die Überlegungen des Modells und die Messdaten prüfen.

Die Logs waren kein reiner Textstrom mehr.

Sie wurden zu einer Möglichkeit, durch das Experiment zu navigieren.

Das war enorm wichtig. Ax3l sollte nicht ein einziges Experiment unter meiner Aufsicht durchführen, sondern Hunderte unbeaufsichtigt über Tage und Wochen. Um die Entscheidungen der KI später zu verstehen, musste ich sie rekonstruieren können.

---

## Dann wollte ich die Schlange sehen

Irgendwann reichten Zahlenkolonnen nicht mehr.

Snake Lab wusste bereits, wann ein Spiel einen neuen Rekord erreichte. Also erweiterte ich es so, dass es in diesem Moment den Zustand des Spielfelds erfasste und zusammen mit den Simulationsergebnissen speicherte.

Anschließend stellte ich diesen Schnappschuss über die ZeroMQ-API von Snake Lab bereit.

Jetzt konnte Ax3l das Spielfeld zum besten Spiel einer Simulation abrufen und im Bericht anzeigen.

Plötzlich war ein Ergebnis von 47 nicht bloß die Zahl `47`.

Da war die Schlange.

Man konnte ihren Weg erkennen, ihre Länge, die Position des Futters und den Zustand des Spielfelds beim Rekord.

Technisch war es eine kleine Ergänzung, aber sie gab dem Projekt einen neuen Charakter. Die Experimente erzeugten nicht länger nur Datenbankeinträge. Irgendwo darin befand sich eine kleine KI-Schlange, die ihren Job immer besser machte.

Bei diesem Experiment kam keine echte Schlange zu Schaden. Viele virtuelle Schlangen trafen allerdings fragwürdige Lebensentscheidungen!

---

## Wird sie wirklich besser? Diagramme!!!

Ein einzelner Rekord ist eine miserable Antwort auf diese Frage.

Reinforcement Learning ist von Zufall geprägt. Eine Konfiguration kann einfach Glück haben. Ein spektakulärer Lauf beweist noch keine grundsätzliche Verbesserung.

Deshalb fügte ich ein Histogramm hinzu, das die Verteilung der Punktzahlen über alle Simulationen zeigt.

Dann ergänzte ich eine Überlagerung.

Das Histogramm der ältesten Hälfte des Experiments wird über das Histogramm aller Daten gelegt. Da die älteren Werte eine Teilmenge der Gesamtdaten sind, können ihre Balken niemals höher sein als die entsprechenden Balken des Gesamthistogramms.

Wenn Ax3l nichts bewirkt, sammeln sich zusätzliche Ergebnisse ungefähr in derselben Verteilung.

Falls das Experiment jedoch erfolgreich ist, passiert etwas Spannenderes:

Die Verteilung wandert langsam nach rechts.

Man kann *sehen*, wie das Experiment bessere Bereiche des Konfigurationsraums findet.

Diese Darstellung wurde für mich viel wertvoller, als nur auf eine langsam wachsende Rekordzahl zu starren.

---

## Vom Rekord 38 auf 53

Mit der ursprünglichen Snake-Konfiguration lag der Rekord bei ungefähr 38 Punkten.

Aus früheren Experimenten wusste ich, dass die RNN mehr schaffen konnte. Die Frage war, ob eine LLM mit geeigneten Werkzeugen, einer Experimenthistorie und dem richtigen Rahmen bessere Konfigurationen systematisch finden könnte.

Zum Zeitpunkt dieses Beitrags hat Ax3l bereits mehr als 500 Simulationen durchgeführt.

Der aktuelle Rekord beträgt **53**.

Noch wichtiger: Die gesamte Verteilung der Ergebnisse hat sich verschoben.

Ax3l wirft nicht nur blind Pfeile, bis einer zufällig bei 53 landet. Die Menge aller Ergebnisse hat sich in Richtung besser funktionierender Konfigurationen bewegt.

Und das Experiment läuft weiter.

---

## Live-Experimentdaten: Snake Web

Ein letztes Problem blieb.

Der Berichtsserver von Ax3l funktionierte wunderbar in meinem eigenen Netzwerk. Ich wollte das Experiment aber auch öffentlich sichtbar machen.

Nur für ein paar Diagramme wollte ich weder die Produktionsdatenbank noch den Anwendungsserver ins Internet stellen.

Also baute ich ein weiteres kleines Projekt: **Snake Web**.

Alle halbe Stunde liest Snake Web die Experimentdatenbank aus und erzeugt CSV-Dateien für eine statische GitHub-Pages-Website. Diese Dateien werden veröffentlicht, und die öffentliche Website zeigt stets den aktuellen Zustand des Experiments.

Die Rekorde sind dort.

Die Histogramme sind dort.

Die Ergebnisse einzelner Simulationen sind dort.

Sogar Seed-Wechsel sind sichtbar. Wenn Ax3l zu einem neuen Zufallsseed wechselt und die Leistung plötzlich abfällt, stürzt die Kurve ehrlich ab und klettert dann langsam wieder nach oben.

Das ist keine glatt polierte Rückschau, die nur erfolgreiche Momente zeigt.

Es ist das Experiment, mit allen Narben.

---

## Dem Experiment zusehen

Und damit ist das Projekt für mich fertig.

Nicht, weil ich nicht noch hundert Dinge hinzufügen könnte. Das könnte ich immer.

Es ist fertig, weil das System, das ich ursprünglich bauen wollte, nun existiert.

Es gibt ein eigenes Reinforcement-Learning-Labor, das kontrollierte Snake-Experimente durchführt und dokumentiert.

Es gibt einen autonomen LLM-Agenten, der Ergebnisse untersucht, darüber nachdenkt, neue Konfigurationen auswählt und weitere Experimente startet.

Es gibt eine Beobachtungsebene, auf der ich nachsehen kann, was er getan hat und warum.

Und es gibt einen öffentlichen Veröffentlichungsprozess, der Fortschritte zeigt, ohne die zugrunde liegenden Systeme freizugeben.

Irgendwo zwischen Architektur, Datenbanken, ZeroMQ-Nachrichten, Prompts, PyTorch-Tensoren, CSV-Dateien und Diagrammen lebt eine Schlange, die früher 38 Punkte erreichte.

Jetzt schafft sie 53.

Ein ziemlich guter Punkt, um aufzuhören.

---

## Links

- [Ax3l-Projekt](https://ax3l.osoyalce.com)
- [Live-Experiment von Ax3l](https://snakeweb.osoyalce.com)
- [Snake Lab Server](https://snakelabserver.osoyalce.com)

### Artikelserie

- [Teil 1/4 – Lokale LLM schreibt Haikus](/de/journal/local-llm-write-haikus/)
- [Teil 2/4 – Die Entwicklung von Snake Lab](/de/journal/the-evolution-of-snakelab/)
- [Teil 3/4 – Einführung ins Prompt Engineering](/de/journal/prompt-engineering-101/)
- Teil 4/4 ist dieser Beitrag.
