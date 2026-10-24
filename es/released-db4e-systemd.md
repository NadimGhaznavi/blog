---
layout: single
author_profile: true
title: "Publicado Db4E SystemD"
date: 2025-06-26
lang: es
locale: es
language_name: Español
translation_key: released-db4e-systemd
permalink: /es/journal/released-db4e-systemd/
category: Python
tags:
  - Python
  - systemd
---

Hace poco publiqué un pequeño módulo de Python en PyPI y GitHub: [`db4e-systemd`](https://pypi.org/project/db4e-systemd/). Es una interfaz sencilla para `systemctl` que permite administrar servicios desde Python: iniciar, detener, activar, desactivar y consultar su estado, con código claro y mínimo.

Lo desarrollé para mi proyecto `db4e`, un gestor completo de despliegues para componentes de minería de Monero, pero pensé que la clase `Db4eSystemd` podría ser útil por sí sola. Resultó que tenía razón.

---

# ¿16 personas clonaron el repositorio… y después otras 35?

Tras preparar el paquete de PyPI y enlazarlo con GitHub, publiqué discretamente la versión `1.0.0` y seguí con otras cosas. Un día después consulté GitHub Insights por curiosidad: 16 personas diferentes habían clonado el repositorio. ¿Al día siguiente? 35. Sin hacer promoción alguna.

Parece que realmente había un vacío en el ecosistema y que `db4e-systemd` lo llena bastante bien.

---

# ¿Por qué existe este módulo?

Siempre se ha podido usar `subprocess` en Python para controlar servicios, pero me faltaba una interfaz limpia y natural para `systemctl`, sin montones de dependencias. Quería:

* Una clase que se instancia con el nombre de un servicio.
* Métodos como `.start()`, `.stop()`, `.restart()`, `.enable()`, `.disable()`, `.status()`.
* Propiedades como `.active`, `.enabled`, `.installed`, `.pid`.
* Un manejo claro de stdout y stderr.

Y eso fue exactamente lo que hice. Una sola clase, menos de 200 líneas y ninguna dependencia fuera de la biblioteca estándar.

---

# Diseñado para reutilizarse y documentado para descubrirse

Al notar que la gente realmente estaba clonando el repositorio, mejoré la distribución:

* Escribí un `README.md` con ejemplos.
* Creé un `CHANGELOG.md`.
* Preparé correctamente `setup.py` y `pyproject.toml`.
* Etiqueté versiones, publiqué en PyPI y añadí documentación a mi base de conocimientos en GitHub Pages.

También redacté una [guía de PyPI](https://kb.osoyalce.com/pages/PyPI.html) para quienes quieran publicar sus propios paquetes.

---

# Prepararse para el futuro y la idea del 1 %

Aunque por ahora `db4e-systemd` está completo, es importante en un sistema mayor, [Database 4 Everything](https://db4e.osoyalce.com/), donde registro el tiempo de actividad para implementar un modelo de donación del 1 %: cinco minutos de minería compartida por cada ocho horas de funcionamiento. Quizás esa lógica también termine en este módulo.

En el código abierto solemos pensar en grande, pero a veces las herramientas más pequeñas son las que llegan más lejos. Fue una buena lección. ¡Pienso seguir adelante!

---

Si buscan una forma sencilla de administrar servicios systemd desde Python, prueben [`db4e-systemd`](https://github.com/NadimGhaznavi/db4e-systemd). Y si les resulta útil, déjenle una estrella en GitHub. ¡Hace la diferencia!
