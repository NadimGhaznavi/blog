---
layout: posts
author_profile: true
title: "Der Wechsel zu Textual"
date: 2025-06-27
lang: de
locale: de
language_name: Deutsch
translation_key: pivot-to-textual
permalink: /de/journal/pivot-to-textual/
category: journal
tags:
  - Textual
  - db4e
---

Ich stecke tief in meinem Projekt **Db4E**: Monero-Mining-Infrastruktur, MongoDB-Integration und eine Menge Orchestrierungslogik. Eine Zeit lang versuchte ich, eine Oberfläche auf Basis von Urwid darüberzustülpen.

Sagen wir es so: Technisch funktionierte es. Aber je mehr ich daran arbeitete, desto mehr Widerstand leistete es.

---

# Urwid-Müdigkeit

Ihr kennt dieses Gefühl: Man merkt, dass eine Layoutänderung bedeutet, ein Knäuel verschachtelter Widgets und Spaghetti-Logik zu entwirren. Darstellung und Daten geraten durcheinander. Ein Workaround folgt dem anderen, nur damit alles richtig ausgerichtet ist. Sobald ich etwas änderte, ging an anderer Stelle etwas kaputt.

Die Benutzeroberfläche wurde zum Engpass. Und sie war auch noch hässlich.

---

# Bühne frei für Textual

Dann entdeckte ich [Textual](https://textual.textualize.io/). Seht euch diese *Terminaloberfläche* an. Genau, ein TUI ohne Gtk, Qt oder Tk. Das ist **Textual**:

![Die Textual-Anwendung Dolphie](/images/2025-06-27-textual-app.png)

Ich war skeptisch, schließlich hatte ich mich an TUI-Frameworks schon öfter verbrannt. Aber nach weniger als einer Stunde gestaltete ich Widgets mit einer CSS-ähnlichen Syntax, reagierte intuitiv auf Ereignisse und bekam schön formatierte Tracebacks samt lokalen Variablen direkt im Terminal.

![Traceback in Textual](/images/2025-06-27-textual-traceback.png)

Es fühlte sich an, als hätte jemand die Regeln der Terminalentwicklung neu geschrieben. Zum ersten Mal war das Terminal kein Kompromiss mehr.

---

# Liebe auf den ersten Debug-Konsolenblick

Der entscheidende Moment? Ich startete die Debug-Konsole und sah saubere, gefilterte Logmeldungen und strukturierte Ausgaben, ohne dass der Bildschirm durcheinandergeriet. Kein `print("FOO")` mehr, das meine Logs und die Datenbank zumüllt. Kein vorsichtiges Herumtänzeln um kaputte Layouts.

![Textual-Konsole](/images/2025-06-27-textual-console.png)

Nur sauberer Code, eine übersichtliche Oberfläche und endlich wieder Freude an der Entwicklung.

---

# Wie geht es weiter?

Ich schreibe das TUI von db4e mit Textual komplett neu. Und nicht nur das: Vielleicht hole ich auch meinen Snake-KI-Simulator wieder hervor und portiere ihn. Dieses Framework macht selbst Nebenprojekte wieder spannend.

Fortsetzung folgt.

– Nadim
