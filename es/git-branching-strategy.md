---
layout: single
author_profile: true
title: "Estrategia de ramas de Git"
lang: es
locale: es
language_name: Español
translation_key: git-branching-strategy
permalink: /es/journal/git-battles/branching-strategy/
toc: true
---

Este es el proceso de publicación descrito en [«Publicar versiones con Git»](/es/journal/git-battles/). Los ejemplos usan `main` para versiones estables y `dev` para integración, partiendo de `v1.2.3` y preparando `v1.3.0`.

## Diagrama

En el siguiente ejemplo, la versión actual del proyecto es `v1.2.3`.

  <pre class="mermaid">
    gitGraph
    commit id: "Versión actual" tag: "v1.2.3"
    branch dev
    checkout dev
    commit id: "Desarrollo"
    branch feature/foo
    checkout feature/foo
    commit id: "Agregar función foo"
    commit id: "Pruebas de foo"
    checkout dev
    merge feature/foo
    branch release/v1.3.0
    checkout release/v1.3.0
    commit id: "Preparar v1.3.0"
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

## Estrategia de ramas

* **main**: únicamente código estable y listo para producción. Aquí se etiqueta cada versión terminada.
* **dev**: rama de integración para incorporar cambios antes de fusionarlos con `main`.
* **feature/xxx**: ramas de corta duración creadas a partir de `dev` para funcionalidades individuales o correcciones no críticas.
* **release/xxx**: ramas creadas desde `dev` para preparar una publicación, realizar pruebas finales y correcciones.
* **bugfix/xxx**: ramas creadas desde `main` para corregir errores críticos en producción.

---

## Trabajar en Dev

Las funcionalidades nuevas comienzan en la rama `dev`.

La rama `dev` ya existe.

```bash
git branch | grep dev
```

La salida esperada es:

```text
dev
```

---

## Crear una rama de funcionalidad desde Dev

Se crea una rama a partir de `dev` para trabajar en una funcionalidad.

```bash
git checkout dev
git pull origin dev
git checkout -b feature/foo
```

El desarrollador cambia a *feature/foo*, donde realiza el trabajo.

```bash
git checkout feature/foo
```

---

## Agregar los cambios y hacer commit

Cuando termina el trabajo, los cambios se guardan en la rama *feature/foo*.

```bash
git add . -v
git commit -m "feat: implement foo feature"
git push origin feature/foo
```

Consulten el [estándar de commits de Git]({% link pages/git-battles/Git-Commit-Standard.md %}) para saber cómo escribir los mensajes de commit.

---

## Fusionar la rama de funcionalidad con Dev

Finalmente, se fusiona *feature/foo* con `dev`.

```bash
git checkout dev
git pull origin dev
git merge --no-ff feature/foo -m "feat: merge foo feature"
git push origin dev
```

---

## Rama de publicación

Si el proyecto está en la versión *v1.2.3*, creamos `release/v1.3.0` a partir de `dev`.

```bash
git checkout dev
git pull origin dev
git checkout -b release/v1.3.0
git push origin release/v1.3.0
```

* Actualizar la versión en `pyproject.toml` a `1.3.0`.
* Actualizar `CHANGELOG.md` con los cambios de la nueva versión.
* En Db4E, actualizar también `DB4E_VERSION_DEFAULT` en `Constants/Defaults.py` a `1.3.0`.

Los comandos siguientes usan archivos de versión de Db4E. Ajusten las rutas para otros proyectos. Ejecuten las pruebas y comprobaciones de compilación antes de fusionar la rama de publicación.

```bash
git add pyproject.toml CHANGELOG.md Constants/Defaults.py
git commit -m "chore: bump version to v1.3.0"
git push origin release/v1.3.0
```

---

## Fusionar la rama de publicación con Main

```bash
git checkout main
git pull --ff-only origin main
git merge --no-ff release/v1.3.0 -m "chore: release v1.3.0"
git tag -a v1.3.0 -m "Release v1.3.0"
git push origin main v1.3.0
```

### Resolver conflictos

Si la fusión se detiene por un conflicto, editen cada archivo afectado para conseguir el resultado deseado, eliminen los marcadores de conflicto y agreguen al índice los archivos corregidos. Por ejemplo, si hay un conflicto en `pyproject.toml`:

```bash
git add pyproject.toml
git commit
```

Después de resolver todos los conflictos y finalizar la fusión, vuelvan a ejecutar las verificaciones del proyecto antes de continuar con el etiquetado y el push.

---

## Fusionar la rama de publicación con Dev

```bash
git checkout dev
git pull origin dev
git merge --no-ff release/v1.3.0 -m "chore: sync release v1.3.0 into dev"
git push origin dev
```

---

## Corrección de errores

Para corregir un error crítico de producción después de `v1.3.0`, creen `bugfix/v1.3.1` desde `main`, no desde `dev`:

```bash
git checkout main
git pull --ff-only origin main
git checkout -b bugfix/v1.3.1
```

Apliquen y prueben la corrección, actualicen los archivos de versión y el registro de cambios a `1.3.1`, hagan commit y publiquen la rama de corrección. Después sigan los pasos anteriores para fusionar, etiquetar y publicar, sustituyendo `release/v1.3.0` por `bugfix/v1.3.1` y `v1.3.0` por `v1.3.1`. Fusionen también la corrección de vuelta a `dev`, conservando cualquier versión de desarrollo más reciente al resolver conflictos en los archivos de versiones.

## Referencias

* [Documentación de Git merge](https://git-scm.com/docs/git-merge)
* [Documentación de Git tag](https://git-scm.com/docs/git-tag)
* [Volver a «Publicar versiones con Git»](/es/journal/git-battles/)
