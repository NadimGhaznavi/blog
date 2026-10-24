---
layout: single
author_profile: true
title: "Ein kleines Experiment: AI FX"
date: 2026-04-17
lang: de
locale: de
language_name: Deutsch
translation_key: hello-ai-fx
permalink: /de/journal/hello-ai-fx/
category: journal
tags:
  - AI
  - FOREX
  - Textual
---

Ich habe schon einiges mit Handelssystemen erlebt. Bei Merrill Lynch unterstützte ich das *Order Audit Trail System* ([OATS](https://www.finra.org/filing-reporting/market-transparency-reporting/order-audit-trail-system-oats-ROT)), ein von NASDAQ verlangtes Compliance-System. Seine Aufgabe ist theoretisch einfach, praktisch aber entscheidend: Finanzinstitute dürfen Kundenaktivitäten nicht für unfaire Vorteile ausnutzen. Solche Systeme sollen Muster sichtbar machen, die eigentlich nicht vorkommen dürften.

![AI FX auf PyPI](/images/2026-04-18-aifx.png)

Bei CIBC leitete ich ein Team, das das vollautomatische Devisenhandelssystem der Bank betreute. Diese geschäftskritische Plattform beeinflusste mit jeder Ausfallminute direkt den Umsatz. Laut Betriebshandbuch generierte das System etwa vier Millionen kanadische Dollar pro Stunde, bei Spitzenbelastung sogar bis zu 40 Millionen.

Diese Erfahrung säte eine Idee.

Ich beschäftigte mich eine Weile mit automatisiertem Devisenhandel und kam zu einem klaren Schluss: Dieses Spiel bevorzugt die großen Institutionen.

Nicht wegen geheimer Strategien, sondern wegen ihrer Größe und Positionierung.

- Sie handeln große Volumina, sodass kleine statistische Vorteile sich summieren.
- Ihre Systeme stehen physisch nahe an der Börseninfrastruktur, wodurch sich die Latenz verringert.

Ein Privatanleger mit begrenztem Kapital und höherer Latenz spielt ein ganz anderes Spiel.

# Die Idee erst einmal beiseitelegen

Also legte ich sie auf Eis.

Stattdessen widmete ich mich anderen Projekten, darunter [AI Hydra](https://pypi.org/project/ai-hydra/), einem Experimentierfeld für Reinforcement Learning, KI-Systeme und Hyperparameter.

# Der Geistesblitz

Vor Kurzem entdeckte ich eine ausführliche YouTube-Serie: [Forex bot & backtest system with Python for beginners](https://www.youtube.com/watch?v=zKk2iuuNJO0&list=PLZ1QII7yudbecO6a-zAI6cuGP1LLnmW8e&index=1).

Sie erklärt den Aufbau eines einfachen Tradingbots mit regelbasierten Strategien.

Und da dachte ich:

- Was wäre, wenn man diese Regeln durch ein neuronales Netz ersetzt oder ergänzt?

Ich gehe nicht mit der Erwartung daran, den Devisenmarkt zu „knacken“.

Aber bei zwei Dingen bin ich mir sicher:

- Ich werde sehr viel lernen.
- Es gibt Raum für schnelle iterative Ansätze, die nur bei hoher Zuversicht handeln.

Außerdem bietet FOREX eine riesige Menge historischer Daten als Trainingsmaterial.

# Den Samen pflanzen

Also gründete ich ein [GitHub-Repository](https://github.com/NadimGhaznavi/aifx), baute die Grundstruktur auf, fügte eine GitHub Action für Veröffentlichungen hinzu und stellte die erste Version auf [PyPI](https://pypi.org/project/aifx/) bereit.

Es ist noch sehr früh. Ich habe gesehen, wie die große Maschine funktioniert. Jetzt baue ich eine kleinere, um sie zu verstehen.

Der Samen ist gepflanzt.

Mal sehen, was daraus wächst.
