---
title: "Git Branching Strategy"
layout: single
permalink: /journal/git-battles/branching-strategy/
lang: en
language_name: English
translation_key: git-branching-strategy
toc: true
---

This is the release workflow described in [Releasing with Git]({% post_url 2025-07-06-Git-Battles %}). The examples use `main` for stable releases and `dev` for integration, starting at version `v1.2.3` and preparing `v1.3.0`.

## Diagram

In the example below, the current project release is v1.2.3.

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

## Branching Strategy

* **main**: Stable production-ready code only. Tag each completed release here.
* **dev**: Integration branch for feature branches before merging into main.
* **feature/xxx**: Short-lived branches created from dev for individual features or noncritical fixes.
* **release/xxx**: Branches created from dev when preparing for a release, for final testing and fixes.
* **bugfix/xxx**: Branches created from main to fix critical bugs on production releases.

---

## Work in Dev

Feature work starts from the `dev` branch.

The dev branch already exists.

```bash
git branch | grep dev
```

Should output:

```text
dev
```

---

## Feature Branch off Dev

A new branch is created off the `dev` branch to house the feature.

```bash
git checkout dev
git pull origin dev
git checkout -b feature/foo
```

The developer switches to the *feature/foo* branch where the development work is done.

```bash
git checkout feature/foo
```

---

## Add and Commit Changes

Once the work is complete, the changes are committed into the *feature/foo* branch.

```bash
git add . -v
git commit -m "feat: implement foo feature"
git push origin feature/foo
```

See the [Git Commit Standard]({% link pages/git-battles/Git-Commit-Standard.md %}) for how Git commit messages should be formatted.

---

## Merge Feature Branch into Dev

Finally, the *feature/foo* branch is merged into the *dev* branch.

```bash
git checkout dev
git pull origin dev
git merge --no-ff feature/foo -m "feat: merge foo feature"
git push origin dev
```

---

## Release Branch

With the project version at *v1.2.3*, a *release/v1.3.0* branch is created off the *dev* branch.

```bash
git checkout dev
git pull origin dev
git checkout -b release/v1.3.0
git push origin release/v1.3.0
```

* Update the version in `pyproject.toml` to `1.3.0`.
* Update `CHANGELOG.md` to reflect the changes in the new release.
* For Db4E, also update `DB4E_VERSION_DEFAULT` in `Constants/Defaults.py` to `1.3.0`.

The commands below use Db4E's version files; adjust the paths for other projects. Run the project's tests and build checks before merging the release.

```bash
git add pyproject.toml CHANGELOG.md Constants/Defaults.py
git commit -m "chore: bump version to v1.3.0"
git push origin release/v1.3.0
```

---

## Merge Release Branch into Main

```bash
git checkout main
git pull --ff-only origin main
git merge --no-ff release/v1.3.0 -m "chore: release v1.3.0"
git tag -a v1.3.0 -m "Release v1.3.0"
git push origin main v1.3.0
```

### Handling Conflicts

If a merge stops with a conflict, edit each conflicted file to produce the intended result, remove the conflict markers, and stage the resolved files. For example, if `pyproject.toml` conflicted:

```bash
git add pyproject.toml
git commit
```

After resolving all conflicts and completing the merge, rerun the project's checks before continuing with the tag and push commands.

---

## Merge Release Branch into Dev

```bash
git checkout dev
git pull origin dev
git merge --no-ff release/v1.3.0 -m "chore: sync release v1.3.0 into dev"
git push origin dev
```

---

## Bug Fixes

For a critical production fix after `v1.3.0`, create `bugfix/v1.3.1` from `main`, rather than from `dev`:

```bash
git checkout main
git pull --ff-only origin main
git checkout -b bugfix/v1.3.1
```

Apply and test the fix, update the version files and changelog to `1.3.1`, commit the changes, and push the bugfix branch. Follow the release merge, tag, and push steps above, substituting `bugfix/v1.3.1` for `release/v1.3.0` and `v1.3.1` for `v1.3.0`. Merge the bugfix branch back into `dev` as well, preserving any newer development version when resolving version-file conflicts.

## References

* [Git merge documentation](https://git-scm.com/docs/git-merge)
* [Git tag documentation](https://git-scm.com/docs/git-tag)
* [Back to Releasing with Git]({% post_url 2025-07-06-Git-Battles %})

