---
title: "Стратегія роботи з гілками Git"
layout: single
permalink: /uk/journal/git-battles/branching-strategy/
lang: uk
locale: uk
language_name: Українська
translation_key: git-branching-strategy
toc: true
---

Це процес випуску нових версій, описаний у статті [«Випуски програм із Git»](/uk/journal/git-battles/). У прикладах використовується гілка `main` для стабільних випусків і `dev` для інтеграції. Початкова версія — `v1.2.3`, готуємо випуск `v1.3.0`.

## Діаграма

У цьому прикладі поточний випуск проєкту — v1.2.3.

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

## Стратегія гілок

* **main**: лише стабільний код, готовий до використання. Кожний завершений випуск позначайте тут тегом.
* **dev**: інтеграційна гілка, куди зливаються функціональні гілки перед випуском.
* **feature/xxx**: короткоживучі гілки від `dev` для окремих функцій або некритичних виправлень.
* **release/xxx**: гілки від `dev` для завершального тестування та виправлень перед випуском.
* **bugfix/xxx**: гілки від `main` для виправлення критичних помилок у виробничих випусках.

---

## Робота в Dev

Розробка функціональності починається з гілки `dev`.

Гілка dev уже існує.

```bash
git branch | grep dev
```

Очікуваний вивід:

```text
dev
```

---

## Функціональна гілка від Dev

Для розробки нової функції створюємо гілку від `dev`.

```bash
git checkout dev
git pull origin dev
git checkout -b feature/foo
```

Розробник переходить до *feature/foo*, де й виконує роботу.

```bash
git checkout feature/foo
```

---

## Додавання та коміт змін

Після завершення роботи зміни комітяться в гілці *feature/foo*.

```bash
git add . -v
git commit -m "feat: implement foo feature"
git push origin feature/foo
```

Формат повідомлень комітів описаний у [стандарті Git-комітів]({% link pages/git-battles/Git-Commit-Standard.md %}).

---

## Злиття функціональної гілки з Dev

Нарешті зливаємо *feature/foo* до `dev`.

```bash
git checkout dev
git pull origin dev
git merge --no-ff feature/foo -m "feat: merge foo feature"
git push origin dev
```

---

## Гілка випуску

Коли поточна версія проєкту — *v1.2.3*, від `dev` створюємо гілку *release/v1.3.0*.

```bash
git checkout dev
git pull origin dev
git checkout -b release/v1.3.0
git push origin release/v1.3.0
```

* Оновіть номер версії в `pyproject.toml` до `1.3.0`.
* Оновіть `CHANGELOG.md`, додавши зміни нового випуску.
* Для Db4E також оновіть `DB4E_VERSION_DEFAULT` у `Constants/Defaults.py` до `1.3.0`.

Наведені нижче команди використовують файли версій Db4E; для інших проєктів змініть шляхи. Перед злиттям гілки випуску запустіть тести та перевірки складання.

```bash
git add pyproject.toml CHANGELOG.md Constants/Defaults.py
git commit -m "chore: bump version to v1.3.0"
git push origin release/v1.3.0
```

---

## Злиття гілки випуску з Main

```bash
git checkout main
git pull --ff-only origin main
git merge --no-ff release/v1.3.0 -m "chore: release v1.3.0"
git tag -a v1.3.0 -m "Release v1.3.0"
git push origin main v1.3.0
```

### Розв’язання конфліктів

Якщо злиття зупинилося через конфлікт, відредагуйте кожний конфліктний файл так, щоб отримати потрібний результат, видаліть маркери конфлікту та додайте виправлені файли до індексу. Наприклад, якщо конфлікт виник у `pyproject.toml`:

```bash
git add pyproject.toml
git commit
```

Після розв’язання всіх конфліктів і завершення злиття знову запустіть перевірки проєкту, а вже потім створюйте тег і надсилайте зміни.

---

## Злиття гілки випуску назад до Dev

```bash
git checkout dev
git pull origin dev
git merge --no-ff release/v1.3.0 -m "chore: sync release v1.3.0 into dev"
git push origin dev
```

---

## Виправлення критичних помилок

Щоб виправити критичну проблему у виробничій версії після `v1.3.0`, створіть гілку `bugfix/v1.3.1` від `main`, а не від `dev`:

```bash
git checkout main
git pull --ff-only origin main
git checkout -b bugfix/v1.3.1
```

Внесіть і протестуйте виправлення, оновіть файли версій та журнал змін до `1.3.1`, зробіть коміт і надішліть гілку виправлень. Далі повторіть описані вище кроки злиття, створення тегу й надсилання, замінивши `release/v1.3.0` на `bugfix/v1.3.1`, а `v1.3.0` на `v1.3.1`. Обов’язково злийте гілку виправлень також до `dev`. Під час розв’язання конфліктів у файлах версій збережіть новіший номер версії розробки, якщо він уже змінений.

## Довідкові матеріали

* [Документація git merge](https://git-scm.com/docs/git-merge)
* [Документація git tag](https://git-scm.com/docs/git-tag)
* [Повернутися до статті «Випуски програм із Git»](/uk/journal/git-battles/)
