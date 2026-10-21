---
layout: single
title: "Git 自动化"
date: 2026-10-21
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: git-automation
permalink: /zh-CN/journal/git-automation/
category: journal
tags:
  - Blog
  - Git
  - Release Management
  - Metadata
---

有些读者可能看过我之前写的[《使用 Git 进行发布》](/zh-CN/journal/git-battles/)。那篇文章讲的是我如何摸索并记录出一套 Git 工作流程。

接下来顺理成章的一步，就是把这套流程自动化。

我已经做到了。现在我有一个 `new_release.sh` 脚本。下面是一次运行的部分输出：

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

可以看到，脚本会自动创建下一个开发分支，并直接把我切换过去。

你可能还注意到了我使用的 `Codename`（代号）字段。这个代号也会显示在 GitHub 的 Release 页面上。

![GitHub 发布标签](/images/2026-10-21-github-release.png)

我还有一个负责*资产清单、补丁管理和元数据*的仓库，它同样会读取这些代号。比起一串版本号，代号更容易让人一眼认出对应的发布版本，也方便我查看自己环境中各个项目的整体情况。

![CMDB 软件信息](/images/2026-10-21-cmdb-info.png)

围绕 Git 发布管理，你还可以构建各种自动化钩子。能做到什么程度，就看你的想象力了！
