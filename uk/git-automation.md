---
layout: single
title: "Автоматизація Git"
date: 2026-10-21
lang: uk
locale: uk
language_name: Українська
translation_key: git-automation
permalink: /uk/journal/git-automation/
category: journal
tags:
  - Blog
  - Git
  - Release Management
  - Metadata
---

Дехто з вас, можливо, читав мій допис [«Битви з Git»](/uk/journal/git-battles/), де я розповідав, як розробив і задокументував власний робочий процес Git.

Логічним наступним кроком стала автоматизація.

І я це зробив. Тепер у мене є сценарій `new_release.sh`. Ось уривок одного запуску:

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

Як бачите, сценарій автоматично створює нову гілку та одразу переводить мене до неї.

Можливо, ви також помітили поле `Codename`, яким я користуюся. Ця кодова назва також з’являється у випуску на GitHub.

![Позначка випуску на GitHub](/images/2026-10-21-github-release.png)

У мене є *система інвентаризації, оновлень і метаданих*, яка теж зчитує кодові назви. Їх значно легше сприймати, ніж самі номери версій. Завдяки цьому я отримую зріз стану різних проєктів у своєму середовищі.

![Інформація про програмне забезпечення в CMDB](/images/2026-10-21-cmdb-info.png)

На основі керування випусками Git можна побудувати безліч автоматичних дій і хуків. Межа лише у вашій уяві!
