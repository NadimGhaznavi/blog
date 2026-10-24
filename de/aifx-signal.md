---
layout: posts
author_profile: true
title: "Vom Samen zum Signal: AI FX"
date: 2026-05-18
lang: de
locale: de
language_name: Deutsch
translation_key: aifx-signal
permalink: /de/journal/aifx-signal/
category: journal
tags:
  - AI
  - FOREX
  - Qt
  - OANDA
  - ZeroMQ
---

Im ersten Beitrag war AI FX noch kaum mehr als ein Samen: ein Repository, ein PyPI-Paket und die Idee, automatisierte Devisenhandelswerkzeuge als Lernlabor zu verwenden. Inzwischen hat dieser Samen Wurzeln geschlagen. Das Projekt hat eine funktionierende Broker-Client-Architektur. Live-Daten von OANDA fließen in eine In-Memory-Datenbank, werden über ZeroMQ veröffentlicht und von einer Qt-Oberfläche als Echtzeit-Kerzenchart dargestellt. Es ist noch früh, aber AI FX ist nicht mehr nur ein Gerüst. Es läuft.

![Live-Ansicht von EUR/CAD in AI FX](/images/2026-05-18-aifx-live-eurcad.png)

---

# Der Aufbau des Systems

Die aktuelle Architektur besteht aus drei Teilen:

- Ein Broker-Prozess, der den zentralen Datenfluss verwaltet
- Ein OANDA-Manager für die Kommunikation mit der externen OANDA-API
- Ein Qt-Client, der den Datenstrom visualisiert und Verbindungslatenzen anzeigt

Der Broker ist der Mittelpunkt. Er ruft Marktdaten von OANDA ab, speichert Kurskerzen in einer SQLite-Datenbank im Arbeitsspeicher und verteilt Aktualisierungen über ZeroMQ.

![Broker-Client-Architektur von AI FX](/images/2026-05-18-aifx-architecture.png)

Der Qt-Client verbindet sich mit dem Broker, abonniert ein Thema für Kurskerzen und aktualisiert die Oberfläche bei neuen Daten. So entsteht eine kleine Live-Marktkonsole: aktuelle Kerzen als Tabelle, darunter ein Kerzenchart, Auswahl des Instruments sowie Latenzen für Broker und OANDA.

Das ist wichtig. Es geht nicht bloß darum, ein Diagramm zu bewegen. Ich möchte ein System, das sich selbst beobachten kann.

---

# Warum es einen Broker gibt

Ich wollte vermeiden, dass die grafische Oberfläche zur eigentlichen Handelsplattform wird.

Eine GUI eignet sich für Auswahl, Darstellung, Größenänderung, Tabellen, Charts und Schaltflächen. Aber dort sollte nicht der Datenstrom verwaltet werden.

Der Broker sorgt für eine klare Trennung:

- Der Zugriff auf OANDA liegt hinter einem Manager.
- Marktdaten werden normalisiert, bevor Clients sie sehen.
- Die In-Memory-Datenbank dient als lokaler Arbeitsdatensatz.
- ZeroMQ übernimmt den Transport zwischen Broker- und Client-Prozessen.
- Clients können sich anmelden, ohne die Datenpipeline zu besitzen.

Diese Trennung zahlt sich aus. Der Qt-Client kann die Verbindung verlieren und wiederherstellen, ohne zur Quelle der Wahrheit zu werden. Der Broker macht Broker-Sachen. Die GUI macht GUI-Sachen.

---

# Meilenstein: Die grafische Oberfläche

Der neueste Meilenstein ist die Qt-Oberfläche. Ursprünglich wollte ich Textual verwenden, aber nachdem ich die Kerzencharts von Plotly gesehen hatte, wollte ich unbedingt diese einsetzen.

Der AIFX-Client verwendet [Qt for Python](https://doc.qt.io/qtforpython-6/), stellt [Plotly-Diagramme](https://plotly.com/python/basic-charts/) in [QtWebEngineCore](https://doc.qt.io/qtforpython-6/PySide6/QtWebEngineCore/index.html) dar und ...

Beim Start sind historische Kerzen bereits geladen. Der Bildschirm bleibt also nicht leer, während neue Daten eintreffen. Der Instrumentenwechsel funktioniert, die Tabelle wird aktualisiert, ebenso der Chart. Statusanzeigen zeigen die Verzögerung beim Broker und bei OANDA-Aufrufen.

Das klingt kosmetisch, verändert aber das gesamte Gefühl.

Ein Konsolenlog sagt, die Maschine lebt. Ein Live-Chart macht es sichtbar.

Und Fehler lassen sich schneller erkennen: fehlende Kerzen, veraltete Abonnements, Latenzspitzen, Formatierungsprobleme oder Besonderheiten beim Timing werden offensichtlich. Diese GUI ist nicht nur hübsch, sondern ein Diagnosecockpit.

---

# Kurze Kerzen, schnelles Feedback

Aktuell konzentriert sich AI FX auf Fünf-Sekunden-Kerzen.

Dieses Intervall ist kurz genug, damit sich die Anwendung lebendig anfühlt, aber klar genug strukturiert, um mit diskreten Daten zu arbeiten. Jede Kerze fasst einen winzigen Zeitraum zusammen: Eröffnung, Höchstkurs, Tiefstkurs, Schlusskurs, Volumen und Zeitstempel.

Noch ist das Projekt in der Infrastrukturphase. Ich behaupte nicht, dass das System schon handelstauglich ist. Jetzt werden erst die Grundlagen geschaffen:

- Daten zuverlässig abrufen
- Sie sauber speichern
- Sie vorhersagbar veröffentlichen
- Sie verständlich anzeigen
- Testen, statt zu raten

Die Handelslogik kommt später. Zuerst müssen Leitungen und Rohre liegen.

---

# Tests, Releases und die unromantischen Dinge

Erstaunlich viel Fortschritt stammt aus den wenig glamourösen Teilen.

Inzwischen gibt es Unit-Tests für zentrale Datenklassen wie Instrumente und Kurskerzen. Außerdem existieren ein Release-Prozess, Linting- und Typprüfungen sowie ein Smoke-Test für frische Installationen außerhalb des Entwicklungsverzeichnisses.

Das ist wichtig, weil ein solches Projekt schnell zu einem Aquarium voller Spaghetti werden kann.

Marktdaten, Netzwerkkommunikation, asynchrone Schleifen, GUI-Ereignisse, Serialisierung, Diagramme, Paketierung und API-Aufrufe wollen sich alle miteinander verknoten. Tests und Release-Disziplin sind kleine Zäune, die die Ziegen aus dem Serverraum halten.

---

# Immer noch eine kleine Maschine

AI FX ist weiterhin eine kleine Maschine.

Keine institutionelle Handelsplattform, kein magischer Geldautomat. Es versucht nicht, Server direkt neben Börsen zu überholen oder die Latenz von Privatanlegern wegzudiskutieren.

Aber es entwickelt sich zu einem nützlichen Labor.

Die Idee war von Anfang an, etwas zu bauen, das klein genug zum Verstehen und flexibel genug zum Experimentieren ist. Das ist weiterhin das Ziel.

Der Samen steckt nicht mehr einfach nur im Boden.

Jetzt gibt es ein Signal.
