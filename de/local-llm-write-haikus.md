---
layout: posts
author_profile: true
title: "Eine lokale LLM schreibt Haikus (1/4)"
date: 2026-08-29
lang: de
locale: de
language_name: Deutsch
translation_key: local-llm-write-haikus
permalink: /de/journal/local-llm-write-haikus/
category: journal
tags:
  - AI
  - LLM
  - Qwen
  - llama.cpp
  - GGUF
---

Dies ist der erste Teil einer vierteiligen Artikelserie. Darin lade ich Software herunter, installiere und konfiguriere sie, damit ein lokal laufendes Sprachmodell Haikus schreiben kann.

Vor Kurzem habe ich zwei identische HP-Z440-Workstations mit M4000-Grafikkarten bekommen. Nach etwas Recherche stellte ich fest, dass auf dieser Hardware eine kleine lokale LLM laufen kann. Bisher hatte ich Sprachmodelle nur als Endnutzer im Browser oder in VS Code verwendet.

Also lud ich das Modell **Qwen3.5 4B** von Hugging Face herunter und versuchte, es mit `llama.cpp` zu starten.

---

## Das Modell vorbereiten

Dafür waren einige Schritte nötig.

Ich lud das Qwen-Modell und den Quellcode von `llama.cpp` herunter, erzeugte ein Zwischenmodell und konvertierte es schließlich in das Format **GGUF**, das `llama.cpp` verwendet. Technisch, aber überschaubar. Am längsten dauerte der Download von 18 GB.

Dann startete ich `llama_server`, öffnete die Adresse im Browser und ...

Da war sie.

Eine Webseite mit einem Chatfeld.

Ich hatte meinen eigenen Chatbot gebaut.

Das war gleichzeitig wahnsinnig cool und nicht besonders interessant.

Schließlich hatte ich bereits ChatGPT, das **VIEL** mehr weiß und kann.

---

## Mit dem Modell sprechen

Natürlich stellte ich ihm zuerst einige Fragen.

Ich fragte nach Allgemeinwissen. Es kannte historische Persönlichkeiten wie Rudolf Steiner, Geografie, Naturwissenschaften und Programmierung.

Es wusste erstaunlich viel.

Das beeindruckte mich. Das Modell rief nicht Google oder einen riesigen KI-Dienst im Internet auf. Alles Wissen steckte lokal auf meinem eigenen Rechner.

Dann fragte ich nach **Patrick Loebers Tutorial, in dem eine KI Snake spielen lernt**.

Das Modell wusste, wovon ich sprach.

Jetzt wurde es interessant.

Mit Patrick Loebers Projekt kannte ich mich gut aus. Mein eigenes **Snake Lab** war aus Experimenten mit einer KI für das Snake-Spiel entstanden. Plötzlich stellte ich keine Quizfragen mehr, sondern konnte mich über etwas unterhalten, das ich wirklich verstand.

Und das Modell antwortete intelligent.

Das war cool.

---

## Und jetzt?

Das Problem war nur: Ich hatte immer noch einen Chatbot.

Ich konnte Fragen stellen, und er konnte antworten. Beeindruckend, aber nicht das, was mich wirklich interessierte.

Ich wollte die LLM **aus meinem eigenen Programmcode heraus verwenden**.

Also schrieb ich eine primitive Gesprächsschleife.

Einen großen Plan gab es nicht. Ich wollte einfach ein Programm, das einen Prompt sendet, eine Antwort erhält und etwas damit macht.

Damit stellte sich eine wichtige technische Frage:

**Was sollte ich das Modell tun lassen?**

Offenbar lautete meine Antwort:

Haikus über Zahlen schreiben.

---

## Zufällige Haikus

Mein Programm erzeugte eine Zufallszahl und bat die LLM per Prompt, ein Haiku über diese Zahl zu verfassen.

Die LLM schrieb das Haiku.

Das Programm speicherte das Ergebnis in einer Logdatei.

```
A new age starts today
Apollo touches the moon's face
New world begins to shine

1970
```

Dann erzeugte das Programm die nächste Zahl.

Und noch eine.

Und noch eine.

Bald saß auf meinem Computer ein lokal betriebenes großes Sprachmodell, das über zufällige ganze Zahlen nachdachte und Gedichte darüber schrieb.

Praktisch nützlich war das nicht unbedingt.

Aber es funktionierte.

Entscheidend war nicht das Haiku, sondern die Schleife:

1. Das Programm erzeugte Eingabedaten.
2. Es baute einen Prompt.
3. Es schickte den Prompt an die LLM.
4. Die LLM erzeugte eine Antwort.
5. Das Programm zeichnete die Antwort auf.
6. Alles begann von vorn.

Zum ersten Mal **sprach ich nicht bloß mit einer KI**.

Ich hatte **eine KI in ein Programm eingebaut**.

---

## Fazit

Mein erstes Experiment mit einer lokal betriebenen LLM produzierte absolut nichts von praktischem Wert.

Es schrieb Haikus über Zufallszahlen.

Aber es beantwortete die Frage, die mich wirklich beschäftigte.

Ich konnte eine LLM lokal ausführen, sie von eigener Software aus ansprechen, Informationen übergeben, Antworten erhalten, speichern und den Ablauf wiederholen, ohne selbst vor einem Chatfenster zu sitzen.

Damals hatte ich noch kein **Ax3l**.

Nicht einmal die Idee war vollständig ausgereift.

Ich hatte eine Schleife.

Und eine Logdatei voller Haikus.

---

## Links

- [Teil 2/4 dieser Artikelserie](/de/journal/the-evolution-of-snakelab/)
