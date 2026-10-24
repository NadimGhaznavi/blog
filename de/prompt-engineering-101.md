---
layout: posts
author_profile: true
title: "Einführung ins Prompt Engineering (3/4)"
date: 2026-09-12
lang: de
locale: de
language_name: Deutsch
translation_key: prompt-engineering-101
permalink: /de/journal/prompt-engineering-101/
category: journal
tags:
  - AI
  - LLM
  - Qwen
  - llama.cpp
  - GGUF
---

[Snake Lab](https://snakelabserver.osoyalce.com) war zu einem richtigen Server geworden, mit ZeroMQ-API, Konfigurationsvalidierung, Datenbank und Experimenten unabhängig vom Client. Die nächste Frage lag auf der Hand:

![Fr3d](/images/2026-09-12-fr3d.png)

Was geschieht, wenn ich eine LLM diesen Server steuern lasse?

So entstand **Fr3d**.

Fr3d war mein erster Versuch, einen autonomen Agenten auf Snake Lab aufzubauen. Die Grundidee: Das Modell bekommt genügend Informationen über das Experiment, die aktuelle Konfiguration und die Ergebnisse früherer Läufe. Dann soll es eine neue Konfiguration vorschlagen, die vielleicht besser funktioniert.

Snake Lab führt das Experiment aus.

Fr3d betrachtet das Ergebnis.

Dann versucht es etwas Neues.

Zum ersten Mal hatte ich das kleine LLM-Experiment aus dem vorherigen Beitrag mit einer Aufgabe verbunden, die tatsächlich Arbeit verrichtete.

---

## Der KI ein Experiment geben

Ich begann, Prompts zu erstellen, die dem Modell das Snake-Experiment erklärten.

Der Prompt beschrieb das neuronale Netz, verfügbare Parameter, gültige Wertebereiche, aktuelle Einstellungen und passende frühere Ergebnisse. Fr3d wählte einen Parameterwert und übermittelte eine neue Konfiguration über die Snake-Lab-API.

So entstand eine Schleife:

**Prompt → LLM → Konfiguration → Snake Lab → Ergebnis → Prompt**

Und erstaunlich schnell funktionierte sie.

Nicht besonders schön.

Aber sie funktionierte.

Ich kannte bereits eine Snake-Konfiguration mit ungefähr 38 Punkten als Rekord und wusste aus monatelangen RNN-Experimenten, dass bessere möglich waren. Fr3d hatte also ein echtes Problem zu lösen.

Es ging nicht darum, bloß plausibel aussehendes JSON zu erzeugen.

Die Entscheidungen mussten sich an echten Messwerten bewähren.

---

## Ein kurzer Ausflug in die Bildverarbeitung

Damals experimentierte ich auch kurz mit **Qwen 2.5 VL**, der Vision-Language-Variante.

Die Idee war reizvoll.

Vielleicht könnte ich dem Modell statt Zahlen Diagramme oder Bilder aus den Snake-Experimenten zeigen und es daraus Schlussfolgerungen ziehen lassen. Technisch funktionierte das, doch ein anderes Problem war wichtiger.

Das multimodale Modell konnte nicht so gut schlussfolgern wie Qwen 3.5. Für diesen Einsatz war das deutlich wichtiger als seine Bildfähigkeiten.

Qwen 3.5 zeigte außerdem seinen Denkprozess, was mir beim Entwickeln der Prompts enorm half. Ich sah, wie das Modell meine Aufgaben verstand. Manchmal sagte es mir indirekt sehr deutlich:

*„Dieser Prompt ist Unsinn.“*

Nicht wortwörtlich, aber sinngemäß.

Ich bat etwa darum, einen unbenutzten Parameterwert auszuwählen, und sah dann, dass alle gültigen Werte schon getestet waren. Oder ich ließ Informationen aus der Experimenthistorie weg, die das Modell offensichtlich brauchte. Oder ich strukturierte Daten so, dass sie für mich logisch wirkten, das Modell aber kaum erkennen konnte, welche Werte vergleichbar waren.

Der Denkprozess machte diese Fehler sichtbar.

Das war wahrscheinlich meine erste echte Lektion im Prompt Engineering: Eine schlechte Antwort ist nicht zwangsläufig ein Modellproblem. Manchmal hat man einfach eine schlechte Frage gestellt.

---

## Prompt Engineering ist Software Engineering

Vor Fr3d betrachtete ich Prompts vor allem als Anweisungen. Am Ende dachte ich eher an Schnittstellen.

Das Modell kann nur die Informationen analysieren, die es bekommt. Ihre Struktur ist wichtig, ebenso Terminologie, Einschränkungen und die enthaltene Historie.

Soll das Modell einen Wert aus einem Suchraum auswählen, muss es diesen Suchraum kennen. Soll es bereits getestete Werte vermeiden, braucht es eine zuverlässige Liste. Sind Ergebnisse unterschiedlicher Seeds nicht direkt vergleichbar, muss der Prompt das deutlich machen. Und widersprüchliche Anforderungen verursachen für eine LLM dieselben Schwierigkeiten wie für einen Programmierer mit widersprüchlicher Spezifikation.

Die Prompts wurden Teil der Systemarchitektur.

---

## Das Problem des Prototyps

Nach ungefähr einer Woche funktionierte Fr3d.

- Es konnte mit Snake Lab kommunizieren.
- Experimente übermitteln.
- Ergebnisse empfangen.
- Die Ergebnisse analysieren und neue Konfigurationen auswählen.
- Es war eine autonome Experimentierschleife von Anfang bis Ende.

Und der Code war schrecklich.

Nicht weil einzelne Teile besonders schwierig waren, sondern weil das Programm organisch gewachsen war, während ich noch herausfand, was es werden sollte und wie ich die Low-Level-Funktionen benutzen musste.

- Prompts waren direkt in den Anwendungscode eingebettet.
- Experimentspezifische Logik war mit allgemeiner LLM-Server-Logik vermischt.
- Es gab doppelte Implementierungen.
- Der Zustand war verstreut.
- Um ein Experiment nachzuvollziehen, musste ich Logs lesen und spontane SQL-Abfragen ausführen.
- Jede Änderung verlangte, Abhängigkeiten durch ein ziemlich monolithisches Programm zu verfolgen.

Das war die typische Prototypphase: Die Software funktionierte zwar, aber zu verstehen, *warum* sie funktionierte, wurde immer schwieriger. Dabei bemerkte ich einen weiteren Fehler.

---

## Reporting hätte von Anfang an dazugehören müssen

Ich hatte Logging und Berichterstattung als etwas behandelt, das man nachträglich um Experimente herum baut. Genau das war falsch. Für eine autonome Experimentieranlage ist Beobachtbarkeit Teil des Experiments. Ich wollte grundlegende Fragen einfach beantworten:

- Was sah das Modell?
- Welcher Prompt führte zu seiner Entscheidung?
- Wie argumentierte es?
- Welche Konfiguration reichte es ein?
- Was war das Ergebnis?
- Was änderte sich gegenüber dem vorherigen Experiment?
- Welche Konfiguration gilt aktuell als beste?

Mit genügend Logdateien könnte man das technisch beantworten. Aber eine Antwort irgendwo ausfindig machen zu können und ein verständliches System zu haben, sind zwei verschiedene Dinge. Die nächste Version musste von Anfang an auf Reporting ausgelegt sein.

---

## Die Grenzen erkennen

Fr3d half mir auch, die natürlichen Modulgrenzen zu sehen.

Einige Funktionen waren allgemein:

- Mit dem lokalen LLM-Server kommunizieren
- Gespräche verwalten
- Anfragen versenden
- Zustand speichern
- Antworten verarbeiten
- Ereignisse protokollieren

Andere waren spezifisch für dieses Experiment:

- Konfigurationsparameter von Snake Lab
- Gültige Wertebereiche
- Experimenthistorie
- Prompts
- Vergleichslogik
- Regeln für die nächste Auswahl

In Fr3d war das alles miteinander verknüpft. Als ich diese Unterscheidung verstand, wurde die Architektur der nächsten Version deutlich klarer.

- Prompts sollten datengesteuert werden.
- Ihre Konstruktion brauchte eine eigene Abstraktion.
- Reporting gehörte in die Architektur.
- Experimentspezifische Logik musste von der allgemeinen LLM-Kommunikation getrennt werden.

---

## Neu anfangen

Bei einem funktionierenden Prototyp möchte man immer weiter daran herumschrauben: eine Funktion verschieben, eine Klasse extrahieren, den Prompt säubern, Duplikate entfernen.

Ich entschied mich dagegen.

Fr3d hatte seinen Zweck erfüllt. Es hatte bewiesen, dass die Idee funktioniert. Vor allem hatte es mir gezeigt, wo die eigentlichen Schwierigkeiten lagen. Das ist enorm wertvolles Wissen, das man durch perfektes Vorausplanen kaum bekommt. Ich behielt die Erkenntnisse und warf die Architektur weg.

Die Lehre war nicht *„Entwirf alles gleich richtig“*. Fast das Gegenteil:

- Baut einen groben Prototyp.
- Bringt das ganze System zum Laufen.
- Beobachtet, wo es wehtut.
- Erkennt die natürlichen Verantwortungsgrenzen.
- Fragt euch, welche Daten ihr von Anfang an hättet speichern sollen.
- Fangt dann neu an.

Fr3d war chaotisch. Aber es funktionierte. Und deswegen wusste ich endlich, was ich als Nächstes bauen musste.

---

## Links

- [Teil 1/4 – Eine lokale LLM schreibt Haikus](/de/journal/local-llm-write-haikus/)
- [Teil 2/4 – Die Entwicklung von Snake Lab](/de/journal/the-evolution-of-snakelab/)
- Teil 3/4 ist dieser Beitrag.
- [Teil 4/4 – Vom Prototyp zum Produkt](/de/journal/from-prototype-to-product/)
