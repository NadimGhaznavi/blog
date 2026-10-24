---
layout: single
author_profile: true
title: "Publicar versiones con Git"
date: 2025-07-06
lang: es
locale: es
language_name: Español
translation_key: git-battles
permalink: /es/journal/git-battles/
category: journal
tags:
  - Git
  - Db4E
---

Voy a ser sincero: Git me ha dado más golpes de los que puedo contar. Cada vez que creo tener un proceso de publicación perfecto, Git grita «¡No tan rápido!» y, después de un par de golpes, un gancho de izquierda y un uppercut, acabo en el suelo preguntándome qué pasó. Ramas desincronizadas, conflictos apareciendo por todas partes y versiones que parecen números de lotería. ¡Un desastre!

Después de muchos experimentos, `stash` y lecciones aprendidas a golpes, por fin encontré un proceso en el que confío. Y qué bien se siente saber que el código que publico en PyPI y GitHub es exactamente el que pretendía publicar, no un Frankenstein armado a última hora.

Así llegué hasta aquí.

---

# Los primeros problemas

Al empezar el proyecto no tenía una estrategia de ramas. Todo ocurría directamente en `main`, lo que significaba que cualquiera que clonara el repositorio podía terminar con código roto.

Soy arquitecto de sistemas y valoro mucho Git y sus capacidades. Simplemente no lograba usarlo bien. Cuando entré en el maravilloso mundo de las *ramas*, *ramas creadas desde otras ramas* y *fusiones*, digamos que no me fue muy bien. Las cosas se complicaban y la versión 0.15.0 terminaba convertida en 0.15.8 antes de resolver el problema.

---

# El punto de inflexión

Estaba claro que necesitaba un plan, algo sencillo y repetible. Así que escribí una [estrategia de ramas](/es/journal/git-battles/branching-strategy/): `main` para versiones estables, `dev` para integración, ramas de funcionalidades para cambios nuevos, ramas de publicación para los preparativos finales y ramas de corrección desde `main`. No es solo una descripción general: incluye *todos* los comandos que necesito. Sinceramente, está lista en un 95 %.

Documenté el proceso tanto en el repositorio como en mi base de conocimientos. Mi consejo: no documenten la gestión de versiones si documentarla significa tener que publicar nuevas versiones. ¡Simplemente no lo hagan!

---

# Lecciones de la vida real

La gran revelación fue el famoso conflicto al incrementar la versión en `pyproject.toml`. `main` y la rama de publicación tenían versiones distintas y Git me obligó a elegir. Aprendí a reconocer y resolver esos conflictos, y ahora solo modifico la versión en la rama de publicación.

Además, `stash` es mi mejor amigo. Si `main` estaba hecho un desastre, guardaba temporalmente los cambios, hacía la fusión y luego descartaba el stash porque en realidad no necesitaba esos cambios locales. No era elegante, pero más de una vez me salvó.

---

# La victoria

Finalmente simulé todo el proceso de publicación: rama de funcionalidad, fusión a `dev`, rama de publicación, cambio de versión, fusión a `main`, etiqueta y push. Todo terminó publicado en GitHub y PyPI sin sorpresas.

¿Lo mejor? Crear un entorno virtual desde cero, instalar el paquete de PyPI y comprobar que tenía exactamente el código que quería publicar. ¡Eso sí que da gusto!

---

# Consejos para otros desarrolladores

* Empiecen por lo sencillo y mejoren el proceso gradualmente.
* Mantengan la lista de verificación de publicación fuera del repositorio.
* No teman las fusiones ni `stash`: Git es una herramienta, no un tirano.
* Automatícenlo solo cuando el proceso manual sea completamente fiable.

Y no tengan vergüenza de pedir ayuda a alguien con experiencia en Git.

---

# Reflexiones finales

Probablemente Git seguirá pegándome, pero ya no estoy huyendo. Tengo un plan, un proceso y cierta **sensación** de control. Si están pasando por lo mismo, no se rindan. ¡Hay luz al final del túnel de Git!

¿Tienen sus propias historias de batallas con Git? Me encantaría escucharlas.

---

# Seguimiento

Más adelante publiqué [otro artículo sobre Git](/es/journal/git-automation/) en el que automatizo este proceso.
