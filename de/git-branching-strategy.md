---
layout: single
author_profile: true
title: "Git-Branching-Strategie"
lang: de
locale: de
language_name: Deutsch
translation_key: git-branching-strategy
permalink: /de/journal/git-battles/branching-strategy/
toc: true
---

Das ist der Release-Ablauf aus [„Veröffentlichen mit Git“](/de/journal/git-battles/). Wir verwenden `main` für stabile Releases und `dev` für die Integration. Im Beispiel beginnen wir bei `v1.2.3` und bereiten `v1.3.0` vor.

## Diagramm

Die aktuelle Projektversion ist hier v1.2.3.

  <pre class="mermaid">
    gitGraph
    commit id: "Aktuelles Release" tag: "v1.2.3"
    branch dev
    checkout dev
    commit id: "Entwicklung"
    branch feature/foo
    checkout feature/foo
    commit id: "Feature foo"
    commit id: "Tests für foo"
    checkout dev
    merge feature/foo
    branch release/v1.3.0
    checkout release/v1.3.0
    commit id: "v1.3.0 vorbereiten"
    checkout main
    merge release/v1.3.0 tag: "v1.3.0"
    checkout dev
    merge release/v1.3.0
  </pre>
  <script type="module">
    import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
    mermaid.initialize({ startOnLoad: true });
  </script>

---

## Branching-Strategie

* **main**: Nur stabiler, produktionsreifer Code. Hier wird jedes fertige Release getaggt.
* **dev**: Integrationsbranch für neue Funktionen vor dem Merge nach `main`.
* **feature/xxx**: Kurzlebige Branches von `dev` für einzelne Features oder unkritische Korrekturen.
* **release/xxx**: Branches von `dev` für abschließende Tests und Korrekturen vor einem Release.
* **bugfix/xxx**: Branches von `main` zur Behebung kritischer Fehler in Produktivversionen.

---

## Auf Dev arbeiten

Die Entwicklung neuer Funktionen beginnt im Branch `dev`.

Dieser Branch existiert bereits.

```bash
git branch | grep dev
```

Erwartete Ausgabe:

```text
dev
```

---

## Feature-Branch von Dev erstellen

Wir legen einen Branch von `dev` für eine neue Funktion an:

```bash
git checkout dev
git pull origin dev
git checkout -b feature/foo
```

Der Entwickler wechselt zu *feature/foo* und arbeitet dort:

```bash
git checkout feature/foo
```

---

## Änderungen hinzufügen und committen

Nach Abschluss der Arbeit werden die Änderungen im Branch *feature/foo* committet.

```bash
git add . -v
git commit -m "feat: implement foo feature"
git push origin feature/foo
```

Informationen zur Formulierung von Commit-Nachrichten stehen im [Git-Commit-Standard]({% link pages/git-battles/Git-Commit-Standard.md %}).

---

## Feature-Branch nach Dev mergen

Schließlich führen wir *feature/foo* mit `dev` zusammen:

```bash
git checkout dev
git pull origin dev
git merge --no-ff feature/foo -m "feat: merge foo feature"
git push origin dev
```

---

## Release-Branch

Wenn das Projekt noch Version *v1.2.3* hat, erstellen wir aus `dev` den Branch *release/v1.3.0*.

```bash
git checkout dev
git pull origin dev
git checkout -b release/v1.3.0
git push origin release/v1.3.0
```

* Version in `pyproject.toml` auf `1.3.0` ändern.
* Die Änderungen für das neue Release in `CHANGELOG.md` dokumentieren.
* Bei Db4E zusätzlich `DB4E_VERSION_DEFAULT` in `Constants/Defaults.py` auf `1.3.0` ändern.

Die Befehle beziehen sich auf Db4E. Für andere Projekte müsst ihr die Pfade anpassen. Führt vor dem Merge Tests und Build-Prüfungen aus.

```bash
git add pyproject.toml CHANGELOG.md Constants/Defaults.py
git commit -m "chore: bump version to v1.3.0"
git push origin release/v1.3.0
```

---

## Release-Branch nach Main mergen

```bash
git checkout main
git pull --ff-only origin main
git merge --no-ff release/v1.3.0 -m "chore: release v1.3.0"
git tag -a v1.3.0 -m "Release v1.3.0"
git push origin main v1.3.0
```

### Konflikte behandeln

Wenn ein Merge wegen eines Konflikts stoppt, bearbeitet ihr jede betroffene Datei, entfernt die Konfliktmarkierungen und nehmt die korrigierten Dateien in den Index auf. Beispiel für `pyproject.toml`:

```bash
git add pyproject.toml
git commit
```

Nach dem Beheben aller Konflikte und dem Abschluss des Merges die Projekttests erneut ausführen, bevor Tags erstellt und Änderungen gepusht werden.

---

## Release-Branch zurück nach Dev mergen

```bash
git checkout dev
git pull origin dev
git merge --no-ff release/v1.3.0 -m "chore: sync release v1.3.0 into dev"
git push origin dev
```

---

## Kritische Fehler beheben

Ist nach `v1.3.0` eine wichtige Korrektur für die Produktivversion notwendig, erstellt ihr `bugfix/v1.3.1` aus `main`, nicht aus `dev`:

```bash
git checkout main
git pull --ff-only origin main
git checkout -b bugfix/v1.3.1
```

Korrigiert und testet den Fehler, aktualisiert die Versionsdateien und das Änderungsprotokoll auf `1.3.1`, commitet und pusht den Bugfix-Branch. Anschließend durchlauft ihr die oben beschriebenen Merge-, Tag- und Push-Schritte, wobei ihr `release/v1.3.0` durch `bugfix/v1.3.1` und `v1.3.0` durch `v1.3.1` ersetzt. Führt den Bugfix auch zurück nach `dev`. Bei Konflikten in Versionsdateien soll eine eventuell neuere Entwicklungsversion erhalten bleiben.

## Referenzen

* [Git-merge-Dokumentation](https://git-scm.com/docs/git-merge)
* [Git-tag-Dokumentation](https://git-scm.com/docs/git-tag)
* [Zurück zu „Veröffentlichen mit Git“](/de/journal/git-battles/)
