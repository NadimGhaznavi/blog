---
layout: single
author_profile: true
title: "Git-Automatisierung"
date: 2026-10-21
lang: de
locale: de
language_name: Deutsch
translation_key: git-automation
permalink: /de/journal/git-automation/
category: journal
tags:
  - Blog
  - Git
  - Release Management
  - Metadata
---

Vielleicht habt ihr meinen Beitrag [„Veröffentlichen mit Git“](/de/journal/git-battles/) gelesen. Darin habe ich beschrieben, wie ich meinen Git-Arbeitsablauf entwickelt und dokumentiert habe.

Der naheliegende nächste Schritt war, ihn zu automatisieren.

Das habe ich getan. Jetzt gibt es ein Skript namens `new_release.sh`. Hier ein Ausschnitt aus einem Lauf:

```sh
dan@sally:/opt/dev/cmdb$ ./scripts/new-release.sh 1.6.0 "Carbon"
[ PASSED ] Validate release arguments
[ PASSED ] Check local release prerequisites
[ PASSED ] Fetch refs from origin
[ PASSED ] Check remote refs and branch ancestry

CMDB release summary:
  Source:       feat/maint-1.5.1
  Tag:          v1.6.0
  Message:      Release 1.6.0: Carbon
  Codename:     Carbon
  Next branch:  feat/maint-1.6.1
  Remote:       git@github.com:NadimGhaznavi/cmdb

Create and push this release? [y/N] y
[ PASSED ] Confirm release
Switched to branch 'dev'
[ PASSED ] Switch to dev
Merge made by the 'ort' strategy.
 CHANGELOG.md                          |  13 +++++
 cmdb/activity/InventoryCoordinator.py |   7 ++-
 cmdb/interface/Network.py             |  20 +++++++-
 index.md                              |   2 +
  .
  .
  .
[ PASSED ] Push release refs atomically
Switched to a new branch 'feat/maint-1.6.1'
[ PASSED ] Create next feature branch feat/maint-1.6.1
[ PASSED ] CMDB release v1.6.0 published successfully.
dan@sally:/opt/dev/cmdb$
```

Wie ihr seht, erstellt das Skript automatisch den nächsten Branch und wechselt gleich hinein.

Vielleicht ist euch auch das Feld `Codename` aufgefallen. Dieser Codename erscheint auch beim Release auf GitHub.

![GitHub-Release-Tag](/images/2026-10-21-github-release.png)

Ich habe ein Repository für *Inventarisierung, Updates und Metadaten*, das ebenfalls den Codenamen übernimmt. Das ist für Menschen einfacher zu lesen und bietet einen Überblick über die verschiedenen Projekte in meiner Umgebung.

![CMDB-Softwareübersicht](/images/2026-10-21-cmdb-info.png)

Rund um das Release-Management mit Git kann man allerlei Hooks und Automatisierungen bauen. Der Fantasie sind keine Grenzen gesetzt!
