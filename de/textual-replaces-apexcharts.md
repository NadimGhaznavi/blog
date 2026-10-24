---
layout: posts
author_profile: true
title: "Textual ersetzt ApexCharts"
date: 2025-06-29
lang: de
locale: de
language_name: Deutsch
translation_key: textual-replaces-apexcharts
permalink: /de/journal/textual-replaces-apexcharts/
category: journal
tags:
  - db4e
  - Textual
---

Mein ursprüngliches Berichtssystem bestand aus statischen Dashboards mit GitHub Pages, JavaScript und ApexCharts. Über Logparser und CSV-Exporte sammelte es Daten aus meiner Mining-Umgebung und zeichnete übersichtliche interaktive Diagramme. Schnell prototypisiert, kaum Wartung, optisch gelungen. Aber es blieb vom Kernsystem getrennt: eher Beiwagen als Cockpit.

---

# Warum ich weitergezogen bin

Diese Architektur war nicht zufällig zerbrechlich, sondern von Anfang an. Sie beruhte auf lose gekoppelten Skripten, zeit- oder ereignisgesteuerten Updates und einem Veröffentlichungsmodell, das **viel zu viel** von der Mining-Infrastruktur preisgab. Öffentliches HTML und JavaScript, CSV-Dateien aus dem internen Netz: Dahinter stand die Annahme, dass die Darstellung außerhalb der operativen Ebene liegen sollte. Für Monero ist diese Transparenz zu viel. Unsere Community legt großen Wert auf Privatsphäre.

Für eine kleine Hobbyinstallation mag das in Ordnung sein. Nicht jedoch für *db4e*, wo Bereitstellung, Logs und Messwerte zusammengehören und der Zugriff gezielt begrenzt werden muss.

---

# Der Wechsel zu Textual

Ich bin nicht zu Textual gewechselt, weil die Browserlösung versagte, sondern weil ich damit alles in einer Oberfläche zusammenführen konnte. Ich wollte keine Trennung zwischen Frontend und Backend verwalten. Ich wollte eine Live-Systemoberfläche direkt auf dem Rechner, mit unmittelbarem Zugriff auf Logs, Laufzeitstatus und Bereitstellungssteuerung.

Genau das bietet Textual. Es läuft im Terminal und ist trotzdem modern. Statusansichten, Logpanels und historische Diagramme können unmittelbar neben den Schaltern und Knöpfen zur Systemsteuerung stehen. Anders als ein Browserdashboard verrät es nichts, was ich nicht verraten möchte.

---

# Auf Wiedersehen, ApexCharts

ApexCharts bleibt großartig, doch ich brauche es nicht mehr. Was es bisher notdürftig gelöst hat, kann ich nun eleganter im Terminal erledigen. Falls ich später Browseransichten benötige, setze ich sie hinter authentifizierte APIs. Bis dahin bleiben die Telemetriedaten dort, wo sie hingehören: in der Bedienerkonsole.

---

# Auf Wiedersehen, GitHub Pages

Die *Textual*-Oberfläche kann auch Berichte anzeigen, für die ich zuvor GitHub Pages verwendet habe. Auf Wiedersehen, GitHub Pages.

---

# Ein System, eine Denkweise

Mit Textual gelang mir, was mit Urwid nie richtig funktionierte: Bereitstellung, Überwachung und Berichte in einer einzigen, konsistenten Anwendung zusammenzuführen. Erst dieser Wechsel von zusammengeflickten Dashboards zu einem zentralen TUI lässt db4e wie eine echte Plattform wirken.
