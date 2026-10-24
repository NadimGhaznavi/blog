---
layout: posts
author_profile: true
title: "AI Snake Lab"
date: 2025-10-16
lang: de
locale: de
language_name: Deutsch
translation_key: ai-snake-lab
permalink: /de/journal/ai-snake-lab/
category: journal
tags:
  - AI Snake Lab
  - Project Description
  - Programming
  - Python
  - PyTorch
  - AI
  - Textual
---

**AI Snake Lab** ist eine interaktive Umgebung für Experimente mit Reinforcement Learning in der klassischen Snake-Spielwelt. Sie bietet eine live aktualisierte Textual-Terminaloberfläche, eine flexible Datenbank für Replay Memory und modular definierte Modelle.

![AI Snake Lab](/images/2025-10-16-ai-snake-lab.png)

---

# 🚀 Funktionen

- 🐍 **Klassisches Snake-Spiel** mit anpassbarem Spielfeld und Regeln
- 🧠 **KI-Agentenschnittstelle** für verschiedene Architekturen (linear, RNN, CNN)
- 🎮 **Simulator auf Textual-Basis** mit Live-Darstellung und Messwerten
- 💾 **Replay Memory in SQLite** für Frames, Episoden und Läufe
- 🧩 **Metadaten für Experimente** wie Modelle, Hyperparameter und Versionen der Zustandsabbildung
- 📊 **Integrierte Diagramme** für Hashrate, Punktzahlen und Lernfortschritt

---

# 🧰 Technologie

| Komponente | Beschreibung |
|------------|--------------|
| **Python 3.11+** | Hauptprogrammiersprache |
| **Textual** | Framework für Terminaloberflächen |
| **SQLite3** | Leichter Speicher für Erfahrungen und Experimente |
| **PyTorch** *(optional)* | Deep-Learning-Backend |
| **Plotext / Matplotlib** | Visualisierungswerkzeuge |

---

# 🔄 Dynamisches Training

Die Option *Dynamic Training* steigert mit zunehmender Epochenzahl den Umfang des *Langzeittrainings* am Ende jeder *Epoche*. Der Code `AIAgent:train_long_memory()` wird dabei immer häufiger aufgerufen, höchstens aber `MAX_ADAPTIVE_TRAINING_LOOPS` Mal. Das aktuelle Maximum liegt gemäß der folgenden Rechnung bei `16`:

```
    if self.dynamic_training():
        loops = max(
            1, min(self.epoch() // 250, DAIAgent.MAX_DYNAMIC_TRAINING_LOOPS)
        )
    else:
        loops = 1

    while loops > 0:
        loops -= 1
        self.train_long_memory()
```

---

# 🧩 Epsilon N

Die Klasse *Epsilon N* wurde von Richard Suttons [The Bitter Lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html) inspiriert und ersetzt den üblichen *Epsilon-Greedy*-Algorithmus direkt. Für jede Punktzahl wird eine eigene Instanz von Epsilon Greedy verwendet. Hat der Agent sieben Punkte, kommt beispielsweise `self._epsilons[7]` zum Einsatz.

Das folgende Diagramm zeigt die *Highscores* mit dem herkömmlichen Epsilon-Greedy-Algorithmus und aktiviertem [dynamischem Training](#dynamisches-training). Er konvergiert schnell, macht aber ungleichmäßige Fortschritte. Die sprunghaften Rekorde deuten darauf hin, dass die Erkundung nachlässt, bevor der Agent die jeweilige Spielphase vollständig beherrscht.

![Traditionelles Epsilon Greedy](/images/2025-10-16-highscores-epsilon.png)

Das nächste Diagramm zeigt die Rekorde mit *Epsilon N* und ohne [dynamisches Training](#dynamisches-training). Weil jede Punktestufe ihr eigenes Epsilon hat, bleibt die lokale Erkundung erhalten. Die Rekordkurve wird dadurch gleichmäßiger und annähernd linear: Der Agent festigt sein Können auf jeder Stufe, bevor er weiterkommt.

![Epsilon N](/images/2025-10-16-highscores-epsilon-n.png)

Die traditionelle Methode verbessert sich langsamer und erreicht früh ein Plateau. Epsilon N ermöglicht dagegen einen beständigeren Fortschritt.

---

# 💻 Installation

Das Projekt ist auf [PyPI](https://pypi.org/project/ai-snake-lab/) verfügbar und kann mit `pip` installiert werden.

## Virtuelle Umgebung erstellen

```shell
python3 -m venv snake_venv
. snake_venv/bin/activate
```

## AI Snake Lab installieren

Nach dem Aktivieren der Umgebung:

```shell
pip install ai-snake-lab
```

---

# ▶️ AI Snake Lab starten

Innerhalb der virtuellen Umgebung:

```shell
ai-snake-lab
```

---

# ⚠️ Einschränkungen

Da die Simulation in der Textual-Terminaloberfläche läuft, können Änderungen an der Terminalgröße oder das Neuzeichnen von Diagrammen das Timing geringfügig beeinflussen. Deshalb können Rekorde bei wiederholten Läufen in etwas unterschiedlichen Spielen auftreten. Das ist erwartetes Verhalten und kein Fehler in der KI-Logik.

Wer vollständig reproduzierbare Simulationen benötigt, kann sich bei mir melden. Ich könnte einen **Headless-Modus** implementieren, bei dem die Simulation in einem eigenen Prozess außerhalb von Textual läuft.

---

# 🙏 Danksagung

Der ursprüngliche Code basiert auf Patrick Loebers YouTube-Tutorial [Python + PyTorch + Pygame Reinforcement Learning – Train an AI to Play Snake](https://www.youtube.com/watch?v=L8ypSXwyBds). Den Originalcode gibt es auf [GitHub](https://github.com/patrickloeber/snake-ai-pytorch). Danke, Patrick!!! Du bist großartig!!!! Dieses Projekt ist eine Portierung der Pygame- und Matplotlib-Lösung.

Vielen Dank auch an Will McGugan und das [Textual-Team](https://textual.textualize.io/). Textual ist fantastisch. Von Pygame und Matplotlib auf Textual zu wechseln, dauerte weniger als einen Tag. Das nenne ich *Rapid Application Development*!

---

# 🌟 Inspiration

Einen KI-Agenten zu entwickeln, freizulassen und sein Verhalten zu beobachten, ist faszinierend. Ein bisschen wie Kinder großziehen, nur in einem viel, viel, viel kleineren Maßstab. Zumindest heute! Ich kann der KI beim Snake-Spielen stundenlang zusehen und denke ständig darüber nach, wie sie besser werden könnte. Patrick Loeber hat mir ein großartiges Projekt zum Erkunden künstlicher Intelligenz gegeben.

Ich habe den Großteil meiner Karriere als Linux-Systemadministrator verbracht. Auf der Kommandozeile fühle ich mich zu Hause. Ich habe nie beruflich als Programmierer gearbeitet, geschweige denn als Frontend-Entwickler. [Textual](https://textual.textualize.io/) ist daher genau meine Art von Werkzeug. Als ich [Dolphie](https://github.com/charles-001/dolphie) entdeckte, war ich begeistert: eingebaute Echtzeitdiagramme von MySQL-Metriken!

Auch Richard S. Sutton inspiriert mich. Seine Gedanken zu *Reinforcement Learning* sind eine Revolution in Zeitlupe. Seine Kritik daran, KI für einzelne Aufgaben zu entwickeln und die Arbeit danach für erledigt zu erklären, trifft einen wunden Punkt. Seine Vision eines Agenten, der kontinuierlich und nichtlinear lernt, ist eine wichtige Grenze auf dem Weg zur *allgemeinen künstlichen Intelligenz*.

---

# 📚 Technische Dokumentation

- [Projektwebsite](https://snakelab.osoyalce.com/)
- [Projekt auf GitHub](https://github.com/NadimGhaznavi/ai-snake-lab)
- [Dateisystemstruktur](https://snakelab.osoyalce.com/pages/project_layout.html)
- [Datenbankschema](https://snakelab.osoyalce.com/pages/db_schema.html)
- [Organisation und Benennung von Konstanten](https://snakelab.osoyalce.com/pages/constants.html)
- [Git-Commit-Standards](https://snakelab.osoyalce.com/pages/git_commit_standards.html)
- [Git-Branching-Strategie](https://snakelab.osoyalce.com/pages/git_branching_strategy.html)
- [Änderungsprotokoll](https://snakelab.osoyalce.com/CHANGELOG.md)

---

# 🔗 Weitere Links

- [YouTube-Tutorial von Patrick Loeber](https://www.youtube.com/watch?v=L8ypSXwyBds)
- [Textual](https://textual.textualize.io/) von Will McGugan
- [Dolphie](https://github.com/charles-001/dolphie): *Echtzeitanalyse für MySQL/MariaDB und ProxySQL auf einem Bildschirm*
- [Richard Suttons Homepage](http://www.incompleteideas.net/)
- [Zitate und Materialien von Richard Sutton](https://snakelab.osoyalce.com/pages/richard_sutton.html)
- [Hilfreiche Diagramme zur Diagnose neuronaler Netze](https://medium.com/data-science/useful-plots-to-diagnose-your-neural-network-521907fa2f45) von George V Jose
- [Lernkurven im maschinellen Lernen](https://wandb.ai/mostafaibrahim17/ml-articles/reports/A-Deep-Dive-Into-Learning-Curves-in-Machine-Learning--Vmlldzo0NjA1ODY0) von Mostafa Ibrahim
