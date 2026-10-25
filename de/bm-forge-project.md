---
layout: single
author_profile: true
title: "BMForge"
date: 2026-10-25
lang: de
locale: de
language_name: Deutsch
translation_key: bm-forge-project
permalink: /de/journal/bm-forge-project/
category: journal
tags:
  - Blog
  - BMForge
  - AI coding
  - SDLC
  - AI skills
  - Spec driven
---

Zurzeit verwende ich [Codex](https://openai.com/codex/) mit dem [Sol-6.1-Modell](https://openai.com/index/introducing-gpt-6-1-sol/) von OpenAI als Programmieragenten.

Ich bin begeistert davon. Die Geschwindigkeit, die Qualität des Codes und die schiere Leistungsfähigkeit sind erstaunlich. Es hat meine Art, Software zu entwickeln, grundlegend verändert.

Allerdings gibt es auch Probleme.

## Eigenmächtige Installation von Komponenten

Codex neigt dazu, Technologieentscheidungen selbst zu treffen. Es stößt auf eine Anforderung, wählt eine Bibliothek aus, installiert sie und baut sie in die Codebasis ein.

Manchmal ist das völlig vernünftig. Manchmal eben nicht.

Ich möchte an solchen Entscheidungen beteiligt sein. Ich habe bevorzugte Technologien, bestehende Implementierungen und bewährte Muster. Ich möchte nicht, dass einfach ein neues Framework eingeführt wird, nur weil der Agent es gerade praktisch findet.

Schließlich ist es **meine Software**.

## Mein Haus, meine Regeln

Ich entwickle schon seit langer Zeit Software und habe meine eigene Art, Dinge zu erledigen.

Programmierstandards, Namenskonventionen, Verzeichnisstrukturen, Datenbankschnittstellen, Release-Management und die Trennung von DEV-, QA- und PROD-Umgebungen gehören alle zu meinem Software Development Life Cycle (SDLC).

Codex kann das nicht von sich aus wissen. Aber ich brauche einen Agenten, der diese Praktiken einhält.

Wenn beispielsweise in der Produktion ein Fehler auftritt, sollte ein Entwickler nicht direkt auf dem Produktionsserver experimentieren. Stattdessen sammelt man Protokolle, exportiert die Datenbank, bringt die Belege zurück auf den Entwicklungsrechner und reproduziert das Problem dort.

Das gehört zum grundlegenden betrieblichen Handwerk.

Codex ist ein ausgesprochen fähiger Entwickler, weiß aber nicht unbedingt, wie es in meiner Entwicklungsumgebung zugeht.

Und wenn es improvisiert, kann daraus schnell Chaos werden.

Das geht nicht.

## Ich wiederhole mich

Codex unterstützt eine Datei namens `AGENTS.md` mit projektspezifischen Anweisungen. Das ist nützlich, aber ich ertappte mich dabei, in mehreren Projekten immer wieder dieselben Anweisungen zu geben.

Zum Beispiel:

- *„Sieh dir das new-release-Skript von bmgeoip an. Verwende dieses Muster.“*
- *„Schau dir das Projekt BMDynIP unter /opt/dev/bmdynip an. Es enthält einen Reporting-Server. Nimm ihn als Vorlage.“*
- *„Das Projekt snake-web-code verwendet einen GitPublisher. Geh von dieser Implementierung aus.“*

Davon gab es **JEDE MENGE**.

Und es war nicht nur lästig. Jede neue Implementierung bot eine weitere Gelegenheit, kleine Inkonsistenzen einzuschleppen.

Ich hätte `AGENTS.md` immer weiter ausbauen können. Stattdessen wollte ich etwas Systematischeres: wiederverwendbar, versioniert und mit der Möglichkeit, es kontinuierlich zu verbessern.

## Hier kommt BMForge

![BMForge-Architektur](/images/2026-10-25-bmforge-architecture.de.svg)

Mit [BMForge](https://bmforge.osoyalce.com) versuche ich, diese Probleme zu lösen.

Im Kern vereint BMForge drei Dinge:

**Spezifikationen + Prozesse + Software-Stack**

Es schafft eine gemeinsame technische Grundlage für die Entwicklung neuer Anwendungen mit wiederverwendbaren Komponenten und etablierten Entwicklungspraktiken.

Das Repository enthält:

- **Komponentenspezifikationen:** Sie definieren Verhalten, freigegebene Python-Module, Einschränkungen bei der Implementierung und Anforderungen an Tests.
- **Wiederverwendbare Komponenten:** Bewährte Implementierungen häufig benötigter Funktionen wie Datenbankzugriff, HTTP-Dienste, ZeroMQ-Messaging, Konfiguration, Lokalisierung und Visualisierung.
- **Entwicklungsprozesse:** Projektinitialisierung, Namenskonventionen, Verzeichnisstrukturen, Installation, Upgrades, Releases und weitere SDLC-Aktivitäten.
- **Skills für KI-Agenten:** Anweisungen, mit denen Programmieragenten lernen, diese Standards einzuhalten und meine etablierten Prozesse auszuführen.

Bei einem neuen Projekt stellt BMForge das Fundament bereit. Die Anwendung besitzt ihre eigene Implementierung und ist zur Laufzeit nicht von BMForge abhängig.

### Skills für KI-Agenten

KI-Skills waren das fehlende Puzzleteil.

Skills werden immer verbreiteter, und online gibt es bereits jede Menge davon.

**Ein Wort zur Vorsicht: Wer einen Skill eines Drittanbieters in seiner Umgebung installiert, sollte ihn genauso gründlich prüfen wie jede andere Fremdsoftware.** Skills können Agenten anweisen, Befehle auszuführen, auf Ressourcen zuzugreifen und mit der Umgebung zu interagieren. Man sollte der Quelle wirklich vertrauen können.

Statt mich auf fremde Skills zu verlassen, habe ich eigene Skills speziell für meine Entwicklungsumgebung erstellt.

Das sind nicht einfach nur Sammlungen von Prompts. Es sind betriebliche Runbooks, vergleichbar mit den Arbeitsanweisungen eines Systemadministrators oder Betriebsanalysten.

Beispielsweise bringt der **Root-Cause-Analysis-Skill (RCA)** von BMForge Codex bei, wie man Fehler in Produktionsanwendungen untersucht.

Das Verfahren ist einfach:

1. Über SSH eine Verbindung zur Produktionsumgebung herstellen.
2. Relevante Protokolle und Datenbank-Dumps sammeln, ohne das entfernte System zu verändern.
3. Die Belege auf den Entwicklungsrechner übertragen.
4. Das Problem lokal reproduzieren, untersuchen und beheben.

**Bei einer RCA wird die Produktionsumgebung niemals verändert.**

Das war's. Kein aufwendiger Workflow, keine Bürokratie. Einfach eine bewährte betriebliche Vorgehensweise, die Codex jetzt konsequent befolgen kann.

Weitere Skills beschreiben, wie Konstanten verwendet, Datenbankschnittstellen aufgebaut, HTTP-Dienste implementiert oder Releases vorbereitet werden.

Entscheidend ist, dass ich diese Abläufe nicht jedes Mal aufs Neue erklären muss.

### Einschränkungen sind ein Feature

BMForge begrenzt ganz bewusst, welche Technologien Codex einführen darf.

Die Komponentenspezifikationen legen den freigegebenen Software-Stack fest. Falls sich eine Anforderung mit den vorhandenen Komponenten und Modulen nicht erfüllen lässt, soll Codex innehalten und mit mir über die Alternativen sprechen.

Vielleicht brauchen wir eine neue Bibliothek. Vielleicht muss eine bestehende Komponente erweitert werden. Vielleicht sollten wir die Anforderung selbst überdenken.

Aber die Entscheidung wird bewusst getroffen.

**Die KI trägt zum Engineering-Prozess bei. Sie definiert ihn nicht eigenmächtig neu.**

### Aufbau der Komponentenbibliothek

BMForge ist darauf ausgelegt, mit der Zeit zu wachsen.

Einige Komponenten stammen aus bereits bereitgestellten Anwendungen. Sie wurden entwickelt und im Betrieb eingesetzt und bieten deshalb einen praktischen Ausgangspunkt.

Andere werden anhand von Spezifikationen erstellt, um Anforderungen zukünftiger Anwendungen zu erfüllen.

Während ich weitere Software entwickle, erkenne ich wiederkehrende Muster und übernehme sie als wiederverwendbare Bausteine in BMForge.

Jedes neue Projekt kann damit etwas zum nächsten beitragen.

### Rückverfolgbarkeit und kontinuierliche Verbesserung

Dieser Teil begeistert mich besonders.

BMForge-Komponenten enthalten Metadaten, die angeben, nach welcher Spezifikation und in welcher Version sie implementiert wurden.

Dadurch entsteht eine nachvollziehbare Verbindung zwischen einer bereitgestellten Anwendung und der technischen Spezifikation ihrer Komponenten.

Was passiert nun, wenn ich einen Fehler entdecke?

Ich kann die betroffene Komponente bis zu ihrer Spezifikation zurückverfolgen, feststellen, ob der Fehler in der Implementierung oder in der Spezifikation selbst liegt, und den richtigen Teil verbessern.

Dasselbe Prinzip gilt für Verbesserungen bei Qualität und Leistung.

Mit der Zeit können diese Verbesserungen in BMForge zurückfließen und zukünftigen Anwendungen zugutekommen.

**Problem finden. Implementierung verbessern. Spezifikation verbessern. Dieses Wissen weitertragen.**

Darum geht es: nicht nur Code wiederzuverwenden, sondern technisches Wissen anzusammeln.

Es ist ein schrittweiser Prozess, bei dem jede Verbesserung das Potenzial hat, die Qualität späterer Projekte zu steigern.

Der Mechanismus für die Rückverfolgbarkeit ist bereits vorhanden. Die umfangreicheren Berichte und Analysen, die damit möglich werden, liegen noch vor mir.

## KI als Mitwirkende im SDLC

Hinter BMForge steckt noch ein umfassenderer Gedanke.

Ich möchte keine völlig neue Softwareentwicklungsmethodik erfinden, nur weil jetzt KI mit im Spiel ist.

Ich habe bereits einen SDLC, der für mich funktioniert.

Ich will **die KI als weiteren Mitwirkenden in diesen bestehenden Prozess integrieren**.

Codex bringt enorme Geschwindigkeit und Umsetzungskompetenz mit. Ich liefere Anforderungen, Architektur, Standards, Betriebserfahrung und technisches Urteilsvermögen.

BMForge stellt den gemeinsamen Rahmen bereit, damit wir strukturiert zusammenarbeiten können.

Der Agent arbeitet effektiver, weil er seine Umgebung versteht. Ich werde produktiver, weil ich nicht ständig dieselben Regeln erklären und dieselbe Infrastruktur neu aufbauen muss.

Und die entstehende Software bleibt verständlich, wartbar und meine eigene.

## Erste Ergebnisse

Es ist noch früh, aber ich habe BMForge bereits verwendet, um **DevLog** zu entwickeln, eine Anwendung, die erfasst, wie viel Zeit ich mit meinen verschiedenen GitHub-Projekten verbringe.

DevLog sammelt Entwicklungsaktivitäten und stellt sie in einem einfachen Web-Dashboard dar.

Es ist eine überschaubare Anwendung, aber ein nützlicher Praxistest für meinen Ansatz.

BMForge hat die anfängliche Projekteinrichtung standardisiert und beschleunigt, etablierte Komponenten und Entwicklungskonventionen bereitgestellt und Codex dabei geholfen, eine schlüssige Implementierung zu erstellen, ohne die zugrunde liegende Infrastruktur ständig neu zu erfinden.

Sowohl bei DevLog als auch bei BMForge selbst gibt es noch viel zu verbessern.

Aber genau darum geht es ja.

## Fazit

BMForge begann als Versuch, einem Programmieragenten nicht immer wieder dasselbe erklären zu müssen.

Mittlerweile entwickelt es sich zu etwas Interessanterem: einer wachsenden Sammlung von Spezifikationen, Prozessen, wiederverwendbaren Softwarekomponenten und betrieblichem Wissen, die meine Art der Softwareentwicklung unterstützt.

Ich kann Anwendungen jetzt deutlich schneller entwickeln, ohne die Kontrolle über Architektur, Technologieentscheidungen oder Betriebspraktiken abzugeben.

Und wenn etwas nicht funktioniert, gibt es einen Mechanismus, um diese Erfahrung in das Framework zurückfließen zu lassen.

**Mit BMForge kann ich Software in KI-Geschwindigkeit entwickeln und gleichzeitig meine eigenen technischen Standards wahren.**

Für mich ist das ein Erfolg, und ich will das Framework weiter ausbauen und verbessern.

Es hat etwas angenehm Kreisförmiges, mit KI ein Framework zu entwickeln, das der KI wiederum beibringt, Software nach bewährten menschlichen Methoden zu bauen. Schön, weiterhin selbst mit am Steuer zu sitzen!
