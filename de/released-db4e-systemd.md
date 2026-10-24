---
layout: posts
author_profile: true
title: "Db4E SystemD veröffentlicht"
date: 2025-06-26
lang: de
locale: de
language_name: Deutsch
translation_key: released-db4e-systemd
permalink: /de/journal/released-db4e-systemd/
category: Python
tags:
  - Python
  - systemd
---

Vor Kurzem habe ich ein kleines Python-Modul auf PyPI und GitHub veröffentlicht: [`db4e-systemd`](https://pypi.org/project/db4e-systemd/). Es ist eine schlanke Hülle um `systemctl`, mit der man Systemdienste direkt aus Python heraus starten, stoppen, aktivieren, deaktivieren und ihren Status abfragen kann.

Ursprünglich habe ich es für mein Projekt `db4e` geschrieben, einen umfassenden Bereitstellungsmanager für Monero-Mining-Komponenten. Dann dachte ich, dass die Klasse `Db4eSystemd` vielleicht auch für sich genommen nützlich sein könnte. Offenbar hatte ich recht.

---

# 16 Klone … und dann weitere 35?

Nachdem ich das PyPI-Paket eingerichtet und das GitHub-Repository verlinkt hatte, veröffentlichte ich still und leise Version `1.0.0`. Einen Tag später sah ich neugierig in GitHub Insights: 16 verschiedene Nutzer hatten das Repository geklont. Am nächsten Tag waren es 35. Ganz ohne Werbung.

Offenbar gab es tatsächlich eine Lücke im Ökosystem, die `db4e-systemd` gut füllt.

---

# Warum dieses Modul existiert

Mit Python und subprocess konnte man Systemdienste schon immer irgendwie steuern. Aber mir fehlte eine saubere, idiomatische `systemctl`-Schnittstelle ohne einen Berg von Abhängigkeiten. Ich wollte:

* Eine Klasse, die man mit dem Dienstnamen instanziiert
* Methoden wie `.start()`, `.stop()`, `.restart()`, `.enable()`, `.disable()`, `.status()`
* Eigenschaften wie `.active`, `.enabled`, `.installed`, `.pid`
* Eine saubere Behandlung von stdout und stderr

Genau das habe ich gebaut: eine Klasse mit weniger als 200 Zeilen und keinerlei Abhängigkeiten außerhalb der Standardbibliothek.

---

# Zur Wiederverwendung gebaut, zum Entdecken dokumentiert

Als ich sah, dass Leute das Repository tatsächlich klonten, habe ich die Paketierung aufgeräumt:

* Eine `README.md` mit Anwendungsbeispielen geschrieben
* Eine `CHANGELOG.md` angelegt
* `setup.py` und `pyproject.toml` ordentlich eingerichtet
* Releases getaggt, auf PyPI hochgeladen und die Dokumentation auf meiner GitHub-Pages-Wissensdatenbank veröffentlicht

Für alle, die eigene Pakete veröffentlichen möchten, habe ich sogar eine [PyPI-Anleitung](https://kb.osoyalce.com/pages/PyPI.html) geschrieben.

---

# Zukunftssicherheit und die Ein-Prozent-Idee

Obwohl `db4e-systemd` vorerst fertig ist, spielt es eine wichtige Rolle im größeren System [Database 4 Everything](https://db4e.osoyalce.com/). Dort verfolge ich die Betriebszeit, um ein „Ein-Prozent-Spendenmodell“ einzuführen: fünf Minuten gemeinsames Mining pro acht Stunden Laufzeit. Wahrscheinlich wird auch diese Logik im Modul landen.

Bei Open Source denken wir oft groß. Aber manchmal haben die kleinsten Werkzeuge die größte Reichweite. Das war eine schöne Erinnerung daran, und ich werde auf jeden Fall dranbleiben.

---

Wenn ihr systemd-Dienste aus Python heraus einfach verwalten möchtet, probiert [`db4e-systemd`](https://github.com/NadimGhaznavi/db4e-systemd) aus. Und wenn es hilft, lasst einen Stern auf GitHub da. Das macht einen Unterschied!
