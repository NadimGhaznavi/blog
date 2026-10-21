---
title: "Git Commit Standard"
layout: single
permalink: /journal/git-battles/commit-standard/
lang: en
language_name: English
toc: true
---

## Introduction

This document outlines the commit-message convention used by [Db4E (Database 4 Everything)](https://github.com/NadimGhaznavi/Db4E). It accompanies the [Git Branching Strategy]({% link pages/git-battles/Git-Branching-Strategy.md %}) described in [Releasing with Git]({% post_url 2025-07-06-Git-Battles %}) and is based on the [Conventional Commits Specification](https://www.conventionalcommits.org/en/v1.0.0/).

This convention dovetails with [Semantic Versioning](https://semver.org/) by describing features, fixes, and breaking changes in commit messages. The length and writing-style guidelines below are project conventions rather than requirements of Conventional Commits.

---

## Imperative Mood

Write messages like a command:

* **Good:** `feat: add user authentication`
* **Bad:** `feat: added user authentication` or `feat: adding user authentication`

---

## Limit Subject to 50 Chars

Keep it short and sweet, concise enough to read in one glance.

---

## Multi-Line Format

Separate the subject from the body with a blank line. Sample message:

```text
feat: support remote monerod deployment

Refactor the installer to handle remote nodes by:
- Passing SSH keys securely
- Adding retries on connection failures
- Documenting usage in the README
```

---

## Limit Lines to 72 Chars

This improves readability in terminals and git tools.

---

## Why, not Just What

The *diff* shows *what* changed. The message should give context on *why*.

---

## Use Bullets or Paragraphs

Using bullets or paragraphs helps organize details, especially for complex changes.

---

## Reference Issues, Tickets, or PRs

Where applicable, include reference numbers to project issues, tickets or PRs. For example:

```text
fix: race condition on startup (#42)
```

---

## Prefix Categories

Use the following prefixes:

- `feat:` A new feature.
- `fix:` A bug fix.
- `perf:` A performance improvement.
- `docs:` Documentation only changes.
- `style:` Formatting changes, not code changes.
- `refactor:` Code changes that neither fix a bug nor add a feature.
- `test:` Adding or refactoring tests.
- `chore:` Build process or auxiliary tool changes.

For example:

```text
feat: add support for remote monerod deployment

This adds the ability to deploy monerod nodes on remote machines
via SSH, including secure key handling and connection retries.
```

---

## Breaking Changes

Use `!` after the type or optional scope, or add a `BREAKING CHANGE:` footer, to identify an incompatible change. For example:

```text
feat(config)!: require an explicit database URL

BREAKING CHANGE: set DB4E_DATABASE_URL before starting Db4E.
```

For releases at version `1.0.0` or above, `fix` corresponds to a patch release, `feat` to a minor release, and breaking changes to a major release. Other prefixes do not imply a version bump on their own. Semantic Versioning treats `0.y.z` as initial development, where the public API is not yet stable.

## Links

* [Conventional Commits](https://www.conventionalcommits.org/)
* [Semantic Versioning](https://semver.org/)
* [Git Branching Strategy]({% link pages/git-battles/Git-Branching-Strategy.md %})
