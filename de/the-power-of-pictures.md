---
layout: posts
author_profile: true
title: "Die Macht der Bilder"
date: 2025-09-19
lang: de
locale: de
language_name: Deutsch
translation_key: the-power-of-pictures
permalink: /de/journal/the-power-of-pictures/
category: journal
tags:
  - Db4E
  - Class Diagrams
  - Programming
  - Python
---

Kennt ihr das Sprichwort *„Ein Bild sagt mehr als tausend Worte“*? Beim Programmieren würde ich sagen: Ein Bild kann tausend Zeilen Code ersetzen.

---

# Das Problem

Dieser Beitrag entstand während der Arbeit an [Db4E](https://db4e.osoyalce.com/), meinem *Monero-Mining-Dashboard für Bereitstellung, Betrieb und Echtzeitanalyse*. Alles lief gut, bis ein seltsamer Fehler auftrat: Wenn ich den Datenbankcache nach einer Liste von *P2Pool-Installationen* abfragte, tauchten Duplikate auf. Das war besonders rätselhaft, weil die Liste mit `values()` aus einem Dictionary erzeugt wurde. Eigentlich dürfte es keine doppelten Einträge geben. Aber sie waren da.

---

# Die Ursache

Bei genauerem Hinsehen stellte sich heraus, dass meine Anwendung zwei oder vielleicht sogar mehr Instanzen des Cache-Objekts besaß. Ich hatte keinen zentralen Eigentümer festgelegt, sodass unterschiedliche Programmteile jeweils eigene Caches erzeugten.

---

# Den Code überarbeiten

Ich begann mit dem Refactoring: Klassen verschieben, Referenzen übergeben, Konstruktorparameter verändern. Es wurde unübersichtlich, ich verlor wiederholt den Faden und fing von vorn an.

Das Projekt hatte damals 32 Klassen, also war die Sache nicht ganz trivial. Nachdem ich noch mehr Durcheinander angerichtet hatte, trat ich einen Schritt zurück. Genauer gesagt mehrere: Ich ging einmal um den Block und dachte darüber nach.

---

# Die Lösung: Diagramme

Mir wurde klar, dass ich ein Klassendiagramm brauchte, das die Beziehungen zwischen den Klassen zeigte. Nichts Kompliziertes. Ich wollte nur sehen, ob eine Klasse in einer anderen enthalten war oder ob ich lediglich eine Referenz übergab.

![Beziehungen zwischen Anwendungsklassen](/images/2025-09-19_app-class-relationships.png)

Mit diesem Bauplan vor Augen war die Umsetzung einfach.

---

# Fazit

Bilder **sind** tausend Worte wert. Wenn ihr im Code feststeckt, versucht es mit einem Diagramm. Das ist euer Bauplan!

---

# Nachwort: Draw.io

Für das Diagramm habe ich das kostenlose Browsertool [Draw.io](https://www.drawio.com/) verwendet. Ich kann es sehr empfehlen:

* Kostenlos
* Integrierte Versionsverwaltung
* Speicherung an verschiedenen Orten, einschließlich [Google Drive](https://drive.google.com/)
* Export in zahlreiche Formate

Wenn ihr in einem Projekt feststeckt, zeichnet es auf. Manchmal führt der kürzeste Weg durch den Code-Dschungel über Bleistift und Papier.
