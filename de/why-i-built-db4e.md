---
layout: posts
author_profile: true
title: "Warum ich Db4E entwickelt habe"
date: 2025-07-09
lang: de
locale: de
language_name: Deutsch
translation_key: why-i-built-db4e
permalink: /de/journal/why-i-built-db4e/
category: journal
tags:
  - Db4E
---

Als ich anfing, mit Monero-Mining und selbst betriebener Infrastruktur zu experimentieren, tat ich das Übliche: Ich lud Monero, P2Pool und XMRig herunter. Nach Experimenten mit Konfigurationsdateien, eigenen Startskripten und Definitionen für Systemdienste konnte ich endlich minen.

Schnell merkte ich aber, dass **jeder Dienst sein eigenes Konfigurationsformat, eigene Besonderheiten bei der Einrichtung und eigene Anforderungen an die Verwaltung seines Lebenszyklus hatte**.

---

# Keine Analysen

Ich beobachtete die Logs und meine Monero-Wallet. Schon bald kam die erste Auszahlung. Später folgten weitere, aber Trends oder künftige Einnahmen waren kaum einzuschätzen. Hatte meine letzte Änderung an XMRig etwas gebracht?

Besonders fiel mir das *Fehlen von Analysen* auf. Bestenfalls exportierte ich Wallet-Transaktionen, importierte sie in eine Tabelle und zeichnete ein Diagramm. Mühsam, improvisiert und vor allem immer wieder dieselbe Handarbeit.

Für Programmierer ist das eine offensichtliche Gelegenheit zur Automatisierung.

---

# Datenquellen

Die Monero-Wallet ist komplexe, auf Sicherheit ausgelegte Software. Einen programmatischen Zugriff auf meine Transaktionen fand ich nicht. Aber im P2Pool-Log konnte ich die Transaktionen **und** viele weitere interessante Angaben sehen. Statt die Wallet abzufragen, könnte ich also die P2Pool-Protokolle auswerten, um die Leistung meiner Miningfarm über die Zeit zu analysieren.

---

# Dauerhafte Speicherung

Ich legte los. So begann **Db4E**.

Ich baute einen persistenten Datenspeicher. Erst SQLite, dann ZODB und schließlich MongoDB. Ein Log-Watcher überwachte das P2Pool-Protokoll auf Transaktionen. Nebenbei sammelte ich auch *Share Found*, *Block Found* und *Hashrate* für meinen Miner, die Mini-Sidechain und die Haupt-Blockchain. Einfach weil es möglich war.

---

# Analyse, Phase I

Danach schrieb ich ein Dashboard mit Urwid, das MongoDB minütlich abfragte. Das war nicht einfach: Urwid ist ein **Monster**. Im Jahr 2012 war es modern, aber 2024 war die Entwicklung damit mühsam. Trotzdem blieb ich dran.

Nach einigen Monaten hatte ich ein einfaches Dashboard. Noch keine Diagramme, aber aktuelle *XMR-Auszahlungen* und *gefundene Shares* waren sichtbar. Ein Anfang.

---

# Betriebsüberwachung

Ich ergänzte Hashrate-Daten der Miner, der Sidechain und der Haupt-Blockchain. Ich versah die Daten mit Zeitstempeln, um Ausfälle zu erkennen. Langsam sah das ordentlich aus, blieb aber zerbrechlich und schwer erweiterbar. Es gab keine Bedienelemente, nur minütliche Aktualisierungen. Immerhin erkannte ich auf einen Blick: *Alles in Ordnung*.

---

# Analyse, Phase II

Diagramme fehlten weiterhin. Transaktionen exportierte ich noch immer manuell, konnte sie aber inzwischen mit einem Skript in ein tabellenfreundliches CSV-Format umwandeln. Immer noch lästig.

Ich überlegte, Tk/Tcl, Gtk oder Qt einzusetzen. Mit keinem davon hatte ich Erfahrung. Alle Optionen bedeuteten eine steile Lernkurve, eine grafische Desktopumgebung und viele Voraussetzungen. Das schien mir übertrieben.

Dann dachte ich: *„Warum nicht das Web?“*

Der Browser wäre die Oberfläche, und GitHub Pages würde sie kostenlos hosten. Nach etwas Recherche entschied ich mich für JavaScript und ApexCharts. Ich schrieb Code, der Daten aus Mongo extrahierte, CSV- und Markdown-Dateien generierte und sie per Git zu GitHub Pages pushte.

Endlich hatte ich Diagramme und konnte tägliche und kumulierte Einnahmen verfolgen.

---

# Nachfrage entsteht von selbst

Ich lehnte mich zurück und genoss den Erfolg. Ich hatte eine Pipeline gebaut:

**P2Pool → Mongo → CSV → GitHub Pages → ApexCharts + Markdown.**

Kompliziert, viele Technologien, aber es funktionierte.

Eines Tages veröffentlichte ich beim Stöbern in r/MoneroMining auf Reddit einfach ein paar [Screenshots](https://www.reddit.com/r/MoneroMining/comments/1kvk3fc/my_mining_farm_dashboard_im_on_the_mini_sidechain/). Das Interesse war überwältigend.

Der Beitrag erhielt über hundert Upvotes und hat inzwischen **27.000 Aufrufe**. Kommentare kamen herein:

> „Was ist das für eine Oberfläche? Sieht gut aus.“
> „Sieht prima aus. Vielleicht probiere ich sie aus.“
> „Wie heißt diese GUI?“
> „Ziemlich cool.“

Dieser Moment veränderte alles.

---

# Bereitstellungswerkzeug

Eigentlich logisch: Jeder Miner kennt diese Probleme.

Doch Db4E war nicht für die Installation verpackt. Es war ein Hobbyprojekt. Wer mein GitHub-Repository klonte, musste trotzdem den Monero-Daemon, P2Pool, XMRig und Db4E selbst einrichten. Keine Kleinigkeit.

Um meine Arbeit teilen zu können, brauchte ich ein **Bereitstellungswerkzeug**.

Also setzte ich mich wieder an die Tastatur.

Ich verabschiedete mich von Urwid und wechselte zu [Textual](), einem Python-Framework für *Rapid Application Development* mit *Terminal User Interface*.

Jetzt habe ich eine saubere Architektur und einen vollständig funktionierenden Ablauf für die **Erstinstallation**. Das Schwierigste ist geschafft.

---

# Wie geht es weiter?

Ich arbeite inzwischen mit formalen Releases, strengen Git-Branch- und Commit-Regeln und einer Testsuite.

Heute möchte ich die Bereitstellung des Monero-Daemons auf einem entfernten Rechner fertigstellen. Danach kommen lokales Monero, P2Pool auf lokalen und entfernten Rechnern sowie XMRig. Damit wäre der gesamte Bereitstellungsstack abgedeckt.

Bei der Arbeit mit Textual beschloss ich, **GitHub Pages und ApexCharts** für historische Trends wieder aufzugeben. Zu öffentlich: Es verrät Miningaktivitäten, fast als würde man seine Bitcoin-Wallet übertragen. Stattdessen werde ich die Diagramme direkt im Terminal mit **plotext** darstellen.

Ja, **alles im Terminal**.

Bevor ihr urteilt, schaut euch [Textual](https://textual.textualize.io), besonders [Dolphie](https://github.com/Textualize/dolphie), und dieses [plotext-Beispiel](https://github.com/piccolomo/plotext#examples) mit Wetterdiagrammen an.

Und warum nicht gleich den Betriebszustand überwachen? Ich plane Zustandsprüfungen für monerod, P2Pool und Miner, mit Warnmeldungen wie:

> *„Die Hashrate deines Miners ist in der letzten Woche um 20 % gesunken.“*

---

# Warum ist das wichtig?

Für den Betrieb eigener Infrastruktur sollte man keinen Doktortitel in Linux-Interna brauchen, kein Dutzend improvisierter Skripte und keine drei offenen Terminals. **Db4E** ermöglicht einen zuverlässigen Betrieb und die Steuerung mit wenigen Klicks.

---

# Links

👀 [GitHub-Repository ansehen](https://github.com/NadimGhaznavi/Db4E)
📦 [Von PyPI installieren](https://pypi.org/project/db4e/)
📬 Fragen oder Feedback? [Erstellt ein Issue](https://github.com/NadimGhaznavi/Db4E/issues) oder schreibt mir.
