---
layout: single
author_profile: true
title: "Veröffentlichen mit Git"
date: 2025-07-06
lang: de
locale: de
language_name: Deutsch
translation_key: git-battles
permalink: /de/journal/git-battles/
category: journal
tags:
  - Git
  - Db4E
---

Ich sage es ganz offen: Git hat mir öfter eine verpasst, als ich zählen kann. Immer wenn ich dachte, ich hätte einen reibungslosen Release-Prozess, rief Git „Nicht so schnell!“, und nach zwei schnellen Schlägen, einem linken Haken und einem Aufwärtshaken lag ich auf der Matte. Nicht synchronisierte Branches, Merge-Konflikte überall, Versionsnummern wie Lottozahlen: ein einziges Durcheinander.

Nach viel Ausprobieren, Stashing und schmerzhaften Lektionen habe ich endlich einen Ablauf gefunden, dem ich vertrauen kann. Es fühlt sich großartig an zu wissen, dass auf PyPI und GitHub genau der Code landet, den ich veröffentlichen wollte, und kein in letzter Minute zusammengeflicktes Frankenstein-Monster.

So bin ich dorthin gekommen.

---

# Die ersten Schwierigkeiten

Am Anfang hatte ich überhaupt keine Branching-Strategie. Alles geschah direkt in `main`. Wer das Repository klonte, konnte leicht einen kaputten Stand erwischen.

Als Systemarchitekt schätze ich Git und seine Möglichkeiten sehr. Aber ich wusste nicht, wie ich sie effektiv einsetzen sollte. Branches, Branches von Branches, Merges: Sagen wir einfach, dass es schlecht lief. Bis ich alles sortiert hatte, war aus Version 0.15.0 schon 0.15.8 geworden.

---

# Der Wendepunkt

Ich brauchte einen klaren, wiederholbaren Plan. Also schrieb ich eine [Branching-Strategie](/de/journal/git-battles/branching-strategy/): `main` für stabile Releases, `dev` für Integration, Feature-Branches für neue Funktionen, Release-Branches für letzte Vorbereitungen und Bugfix-Branches von `main`. Darin stehen sogar *alle* Befehle, die ich benötige. Zu etwa 95 Prozent passt das schon.

Ich dokumentierte den Ablauf sowohl im Repository als auch in meiner unabhängigen Wissensdatenbank. Mein Rat: Wenn man zum Dokumentieren des Release-Managements neue Releases erstellen muss, dann sollte man es lieber lassen.

---

# Lektionen aus der Praxis

Der große Augenöffner war der Konflikt beim Erhöhen der Version in `pyproject.toml`. `main` und der Release-Branch hatten unterschiedliche Versionsnummern, und Git zwang mich zur Entscheidung. Jetzt erkenne und behebe ich solche Konflikte schnell und ändere Versionsnummern nur noch im Release-Branch.

Auch `stash` wurde zu meinem besten Freund. Wenn `main` durcheinandergeraten war, parkte ich die Änderungen, führte den Merge aus und verwarf anschließend den Stash, weil ich die lokalen Änderungen eigentlich gar nicht wollte. Nicht elegant, aber mehr als einmal die Rettung.

---

# Der Sieg

Nachdem ich den ganzen Prozess simuliert hatte, vom Feature-Branch über den Merge nach `dev` und den Release-Branch bis zu Versionsänderung, Merge nach `main`, Tag und Push, gelang die Veröffentlichung auf GitHub und PyPI ohne Überraschungen.

Der schönste Moment? Eine frische virtuelle Umgebung erstellen, mein Paket von PyPI installieren und erkennen, dass es genau der gewünschte Code ist. Perfekt.

---

# Tipps für andere Entwickler

* Fangt einfach an und verbessert den Ablauf schrittweise.
* Bewahrt eure Release-Checkliste außerhalb des Repositories auf.
* Habt keine Angst vor Merges oder Stashes. Git ist ein Werkzeug, kein Tyrann.
* Automatisiert erst, wenn der manuelle Ablauf wirklich zuverlässig funktioniert.

Und scheut euch nicht, einen erfahrenen Git-Nutzer zur Seite zu holen.

---

# Zum Schluss

Git wird mich wahrscheinlich weiter verprügeln, aber ich laufe nicht mehr davon. Ich habe einen Plan, einen Ablauf und das **Gefühl**, die Kontrolle zu haben. Wenn es euch ähnlich geht, bleibt dran: Am Ende des Git-Tunnels ist Licht.

Habt ihr auch Git-Kriegsgeschichten? Ich würde sie gerne hören.

---

# Nachtrag

Später habe ich einen [weiteren Git-Beitrag](/de/journal/git-automation/) geschrieben, in dem ich diesen Ablauf automatisiert habe.
