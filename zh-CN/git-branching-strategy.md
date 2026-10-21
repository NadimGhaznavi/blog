---
title: "Git 分支策略"
layout: single
permalink: /zh-CN/journal/git-battles/branching-strategy/
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: git-branching-strategy
toc: true
---

这是我在[《使用 Git 进行发布》](/zh-CN/journal/git-battles/)中介绍的发布流程。示例使用 `main` 作为稳定发布分支，`dev` 作为集成分支，从 `v1.2.3` 开始，准备发布 `v1.3.0`。

## 流程图

在下面的示例中，项目当前的发布版本是 v1.2.3。

  <pre class="mermaid">
    gitGraph
    commit id: "Current release" tag: "v1.2.3"
    branch dev
    checkout dev
    commit id: "Development"
    branch feature/foo
    checkout feature/foo
    commit id: "Add foo feature"
    commit id: "Add foo tests"
    checkout dev
    merge feature/foo
    branch release/v1.3.0
    checkout release/v1.3.0
    commit id: "Prepare v1.3.0"
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

## 分支策略

* **main**：只存放稳定、可用于生产环境的代码。每次正式发布都在这里打标签。
* **dev**：集成分支。功能分支先合并到这里，再进入正式发布流程。
* **feature/xxx**：从 `dev` 创建的短期分支，用于开发单个功能或修复非关键问题。
* **release/xxx**：准备发布时从 `dev` 创建，用于最后的测试和修复。
* **bugfix/xxx**：从 `main` 创建，用于修复生产版本中的严重问题。

---

## 在 Dev 分支上工作

功能开发从 `dev` 分支开始。

`dev` 分支已经存在。

```bash
git branch | grep dev
```

预期输出：

```text
dev
```

---

## 从 Dev 创建功能分支

从 `dev` 创建一个新分支，用来开发某项功能。

```bash
git checkout dev
git pull origin dev
git checkout -b feature/foo
```

开发者切换到 *feature/foo* 分支，并在这里进行开发。

```bash
git checkout feature/foo
```

---

## 添加并提交修改

完成功能开发后，将修改提交到 *feature/foo* 分支。

```bash
git add . -v
git commit -m "feat: implement foo feature"
git push origin feature/foo
```

关于 Git 提交信息的格式，请参阅 [Git 提交规范]({% link pages/git-battles/Git-Commit-Standard.md %})。

---

## 将功能分支合并到 Dev

最后，把 *feature/foo* 分支合并到 `dev`。

```bash
git checkout dev
git pull origin dev
git merge --no-ff feature/foo -m "feat: merge foo feature"
git push origin dev
```

---

## 发布分支

项目当前版本是 *v1.2.3*，现在从 `dev` 创建 *release/v1.3.0* 分支。

```bash
git checkout dev
git pull origin dev
git checkout -b release/v1.3.0
git push origin release/v1.3.0
```

* 将 `pyproject.toml` 中的版本号更新为 `1.3.0`。
* 更新 `CHANGELOG.md`，记录新版本的变更。
* 对于 Db4E，还需要把 `Constants/Defaults.py` 中的 `DB4E_VERSION_DEFAULT` 更新为 `1.3.0`。

下面的命令使用 Db4E 的版本文件；其他项目需要相应调整文件路径。在合并发布分支之前，运行项目的测试和构建检查。

```bash
git add pyproject.toml CHANGELOG.md Constants/Defaults.py
git commit -m "chore: bump version to v1.3.0"
git push origin release/v1.3.0
```

---

## 将发布分支合并到 Main

```bash
git checkout main
git pull --ff-only origin main
git merge --no-ff release/v1.3.0 -m "chore: release v1.3.0"
git tag -a v1.3.0 -m "Release v1.3.0"
git push origin main v1.3.0
```

### 处理合并冲突

如果合并因为冲突而中断，需要编辑每个冲突文件，保留正确的最终内容，删除冲突标记，然后暂存已经解决的文件。例如，如果 `pyproject.toml` 出现冲突：

```bash
git add pyproject.toml
git commit
```

解决全部冲突、完成合并之后，应重新运行项目检查，再继续执行打标签和推送命令。

---

## 将发布分支合并回 Dev

```bash
git checkout dev
git pull origin dev
git merge --no-ff release/v1.3.0 -m "chore: sync release v1.3.0 into dev"
git push origin dev
```

---

## 修复严重问题

如果在 `v1.3.0` 发布后需要修复生产环境中的严重问题，应从 `main` 而不是 `dev` 创建 `bugfix/v1.3.1` 分支：

```bash
git checkout main
git pull --ff-only origin main
git checkout -b bugfix/v1.3.1
```

完成并测试修复，将版本文件和变更日志更新为 `1.3.1`，提交修改并推送修复分支。然后按照上文的发布合并、打标签和推送步骤操作，只需将 `release/v1.3.0` 替换为 `bugfix/v1.3.1`，将 `v1.3.0` 替换为 `v1.3.1`。

还要把修复分支合并回 `dev`。如果版本文件发生冲突，解决冲突时应保留开发分支上可能已经更新的版本号。

## 参考资料

* [Git merge 官方文档](https://git-scm.com/docs/git-merge)
* [Git tag 官方文档](https://git-scm.com/docs/git-tag)
* [返回《使用 Git 进行发布》](/zh-CN/journal/git-battles/)
