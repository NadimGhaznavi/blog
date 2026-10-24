---
layout: single
author_profile: true
title: "Automatización de Git"
date: 2026-10-21
lang: es
locale: es
language_name: Español
translation_key: git-automation
permalink: /es/journal/git-automation/
category: journal
tags:
  - Blog
  - Git
  - Release Management
  - Metadata
---

Quizás hayan leído mi artículo [«Batallas con Git»](/es/journal/git-battles/), donde cuento cómo desarrollé y documenté mi flujo de trabajo con Git.

El siguiente paso natural era automatizarlo.

Lo hice. Ahora tengo un script `new_release.sh`. Aquí hay un fragmento de su ejecución:

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

Como pueden ver, el script crea automáticamente la nueva rama y me deja trabajando en ella.

Quizás también notaron el campo `Codename`. Ese nombre en clave aparece en mi publicación de GitHub.

![Etiqueta de publicación en GitHub](/images/2026-10-21-github-release.png)

Tengo un repositorio de *inventario, parches y metadatos* que también recoge ese nombre en clave. Resulta más fácil de identificar y me da una vista de los distintos proyectos en mi entorno.

¡Pueden construir todo tipo de automatizaciones alrededor de sus publicaciones con Git! El único límite es su imaginación.
