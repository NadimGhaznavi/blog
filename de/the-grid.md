---
layout: posts
author_profile: true
title: "Das Stromnetz simulieren"
date: 2026-04-26
lang: de
locale: de
language_name: Deutsch
translation_key: the-grid
permalink: /de/journal/the-grid/
category: journal
tags:
  - pgm-ds
  - Power Grid Modeling Data Science
  - Simulation
---

Ein potenzieller Kunde machte mich auf den Artikel [Empowering the Energy Transition Through Fast and Flexible Network Simulation](https://lfenergy.org/empowering-the-energy-transition-through-fast-and-flexible-network-simulation/) aufmerksam. Darin wird das Open-Source-Projekt [Power Grid Model DS](https://power-grid-model-ds.readthedocs.io/en/stable/) (`pgm-ds`) beschrieben. *DS* steht für *Data Science*.

![Logo von PGM DS](/images/2026-04-26-pgm-ds-logo.png)

`pgm-ds` erweitert das [Power Grid Model](https://power-grid-model.readthedocs.io/en/stable/). Laut Dokumentation ist das eine Bibliothek zur stationären Analyse elektrischer Verteilnetze. Das Modell enthält Komponenten für:

- Knoten
- Zweige
- Geräte
- Sensoren
- Fehler
- Regler

und mehr. Damit lassen sich Leistungsflüsse, Zustandsabschätzungen und Kurzschlüsse modellieren.

*Power Grid Model DS* baut darauf auf und bietet eine übersichtliche API zum Modellieren und Simulieren des Netzverhaltens. Es richtet sich an datenwissenschaftliche Anwendungen, darunter Netzwerkanalysen und Simulationen. Eine `Grid`-Datenklasse hält das gesamte Netz konsistent und ermöglicht die Erweiterung der Datenstruktur.

---

# Die Demo ausführen

Die technischen Schritte waren einfach. Die Bedeutung der Ergebnisse zu verstehen, kostete mehr Mühe.

`pgm-ds` **ist** wirklich benutzerfreundlich. Aus der Jupyter-Notebook-Dokumentation ein Skript für die Demo zu machen, war keine große Sache. Zu begreifen, was ich betrachtete, war schwieriger.

Das Paket enthält eine Netzvisualisierung mit einem WSGI-Frontend auf Basis von [Flask](https://flask.palletsprojects.com/en/stable/), die im Browser angezeigt werden kann. Die Grafiken verwendet [Dash Cytoscape](https://dash.plotly.com/cytoscape), eine elegante und leistungsfähige Visualisierung von Graphen.

So hübsch das Bild war, die wirklich aufschlussreichen Daten standen als Tabellen in der Konsole. Ein paar Beispiele:


 id  | u_rated | node_type | feeder_branch_id | feeder_node_id |  u  |  x_coor |  y_coor
 ----|---------|-----------|------------------|----------------|-----|---------|-------
  1  | 10500.0 |     0     |        64        |       61       | 0.0 |125.848..|245.522..
  2  | 10500.0 |     0     |        65        |       61       | 0.0 |385.322..|175.70..
  61 | 10500.0 |     1     |   -2147483648    |  -2147483648   | 0.0 |134.732..|145.515..

## Leitungen

  id | from_node | to_node | from_status | to_status | feeder_branch_id | feeder_node_id | is_feeder |   r1  |   x1  |  c1 | tan1 |   i_n   | i_from 
----|-----------|---------|-------------|-----------|------------------|----------------|-----------|-------|-------|-----|------|---------|---------
 63 |     61    |    12   |      1      |     1     |        63        |       61       |    True   |0.411..|0.023..| 0.0 | 0.0  |110.013..|  0.0   
 74 |     20    |    8    |      1      |     1     |        63        |       61       |   False   |0.585..|0.000..| 0.0 | 0.0  |227.694..|  0.0   
 125 |     96    |    94   |      1      |     1     |        63        |       61       |   False   |0.036..|0.04..| 0.0 | 0.0  |362.904..|  0.0   

## Transformatoren

   id | from_node | to_node | from_status | to_status | feeder_branch_id | feeder_node_id | is_feeder |    u1   |   u2   |     sn     | tap_size |   uk  |    pk    |  i0 |  p0 | winding_from | winding_to | clock | tap_side | tap_pos | tap_min | tap_max | tap_nom
-----|-----------|---------|-------------|-----------|------------------|----------------|-----------|---------|--------|------------|----------|-------|----------|-----|-----|--------------|------------|-------|----------|---------|---------|---------|----------- 
 123 |     5     |    95   |      1      |     1     |        64        |       61       |   False   | 10500.0 | 3000.0 | 30000000.0 |   0.0    | 0.203 | 100000.0 | 0.0 | 0.0 |      0       |     0      |   12  |    0     |    0    |    0    |    0    |    0   
 124 |     12    |    96   |      1      |     1     |        63        |       61       |   False   | 10500.0 | 3000.0 | 30000000.0 |   0.0    | 0.203 | 100000.0 | 0.0 | 0.0 |      0       |     0      |   12  |    0     |    0    |    0    |    0    |    0   


---

# Die Daten verstehen

Den Code einzurichten und auszuführen **war** einfach. Die Bedeutung der Ausgabe und die Fähigkeiten der Software zu begreifen, war es nicht. Ich komme aus der Linux-Systemadministration und Python-Programmierung, nicht aus der Elektrotechnik.

Also untersuchte ich Datenstrukturen und Funktionen, las die gut strukturierte [API-Dokumentation](https://power-grid-model-ds.readthedocs.io/en/stable/api_reference/power_grid_model_ds.html) und recherchierte die Grundlagen.

Ich hatte mir das Stromnetz immer als komplizierten, aber weitgehend statischen Schaltkreis vorgestellt. Doch besser versteht man es als dynamisches physikalisches System, dessen Verhalten unter anderem von Blindwiderstand und Blindleistung beeinflusst wird, oft auf wenig intuitive Weise. Fast wie ein lebendiger Organismus.

Gleich zu Beginn fiel mir der Wert **Blindwiderstand pro Kilometer** auf, der bei den Leitungen als Attribut `x1` auftaucht. Blindwiderstand war für mich besonders schwierig zu verstehen. Ich sah mir YouTube-Videos an und sprach mit ChatGPT darüber. **Blindleistung** ist zentral dafür, zu begreifen, warum Stromnetze so schwer zu verwalten sind. Eine vollständige Erklärung sprengt diesen Beitrag.

Ich habe nicht jedes einzelne Attribut untersucht. Ich wollte zunächst ein Gefühl dafür bekommen, was das Paket leistet und wie es die reale Welt abbildet.

---

# Das Netz modellieren

Als ich begriff, wie kompliziert das Stromnetz tatsächlich ist, wuchs das Ausmaß der Aufgabe in meiner Vorstellung um mehrere Größenordnungen. Ich habe in Betriebsteams bei CIBC, Merrill Lynch, Moneris Solutions und SiGe Semiconductor gearbeitet und große Dashboards voller technischer Details gesehen. Dann suchte ich nach einem Betriebsdashboard von Hydro One in Ontario.

![Betriebsdashboard von Hydro One](/images/2026-04-26-hydro-one-ops.png)

Das ist nur ein Ausschnitt, vermittelt aber einen Eindruck von der Informationsdichte und Komplexität, mit der Netzbetreiber arbeiten.

Mit `pgm-ds` kann man das Netz dynamisch verändern, Komponenten hinzufügen oder entfernen, Fehler und Ausfälle simulieren und Strom über redundante Verbindungen umleiten. Man kann Veränderungen modellieren, Nachfrage prognostizieren und deren Folgen untersuchen.

Bei dieser Komplexität bilden Simulationen eine natürliche Grundlage für maschinelles Lernen. Werkzeuge wie `pgm-ds` ermöglichen strukturierte Szenarien für Prognosen, Anomalieerkennung und Entscheidungsunterstützung.

Ein Stromnetz auszubauen und zu betreiben ist außerdem ein langsamer Prozess. Kraftwerke, Staudämme, Rechenzentren, Fabriken und Wohnviertel entstehen nicht über Nacht.

---

# Fazit

Dieses Gebiet ist technisch faszinierend und zugleich von großer praktischer Bedeutung. Solche Werkzeuge modellieren Infrastruktur nicht nur, sondern können auch dabei helfen, ihre Weiterentwicklung zu gestalten. Das macht die Arbeit intellektuell spannend und sinnvoll.

---

# Links

## Unternehmen und Organisationen

- [LF Energy](https://lfenergy.org/)
- [Alliander](https://www.alliander.com/en/)

## Power Grid Model DS

- [Überblick über Power Grid Model Data Science](https://lfenergy.org/empowering-the-energy-transition-through-fast-and-flexible-network-simulation/)
- [Folien einer Präsentation](https://archive.fosdem.org/2025/events/attachments/fosdem-2025-4662-empowering-the-energy-transition-through-fast-and-flexible-network-simulation/slides/238824/FOSDEM_Pr_gNLHXgk.pdf)
- [Schnellstart](https://power-grid-model-ds.readthedocs.io/en/stable/quick_start.html#modifying-the-grid)
- [Vollständiges Beispiel: Einen neuen Verbraucher anschließen](https://power-grid-model-ds.readthedocs.io/en/stable/demos/connecting_new_consumer.html)

## API-Dokumentation

- [Power Grid Model DS](https://power-grid-model-ds.readthedocs.io/en/stable/)
- [Power Grid Model](https://power-grid-model.readthedocs.io/en/stable/)

## GitHub-Repositories

- [PowerGridModel](https://github.com/PowerGridModel/)
  - [power-grid-model](https://github.com/PowerGridModel/power-grid-model)
  - [power-grid-model-ds](https://github.com/PowerGridModel/power-grid-model-ds)
  - [power-grid-model-io](https://github.com/PowerGridModel/power-grid-model-io)
- [Dash Cytoscape](https://github.com/plotly/dash-cytoscape)
- [Mailingliste von Power Grid Model](mailto:powergridmodel+subscribe@lists.lfenergy.org)

## Grundbegriffe

- [RustworkX](https://www.rustworkx.org/tutorial/introduction.html)
- [Blindleistung erklärt](https://www.youtube.com/watch?v=2mD3UbSW7ho)
