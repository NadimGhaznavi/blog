---
layout: single
author_profile: true
title: "Una IA para organizar mis películas"
date: 2026-09-27
lang: es
locale: es
language_name: Español
translation_key: r3eling
permalink: /es/journal/r3eling/
category: journal
tags:
  - R3el
  - LLM
  - Local AI
  - Media Management
  - Automation
  - Python
  - TMDB
  - Self-Hosted
---

Nuestro primer televisor era una pequeña caja en blanco y negro en la que, muy de vez en cuando, veíamos *Animal Kingdom*. Después llegaron Beta y VHS, ¡y podíamos ver películas en casa! Luego vinieron los DVD.

Todo se volvió digital. El costo del almacenamiento cayó y empecé a conservar copias digitales de mis películas y series favoritas. ¿Por qué? Porque me gusta volver a verlas y coleccionar cosas. Parecía algo inofensivo.

Pensé: «¡Perfecto! Ahora habrá menos desorden en mi vida. ¡Solo tengo que organizar unos cuantos archivos!». Me deshice de los DVD y adopté la vida digital.

---

### El pantano

Eso fue hace unos 25 años. Ahora tengo más de mil películas, casi doscientas series y temporadas y temporadas de episodios.

Intenté mantener el orden renombrando archivos y carpetas. Me fue bastante bien, pero con tantos elementos, buscar o explorar la colección se volvió cada vez más difícil, hasta casi imposible. Había demasiados directorios.

---

### La solución

La respuesta era evidente:

- Crear una interfaz para explorar y buscar.
- Primero, limpiar los datos para que el programa pudiera encontrarlos.

Crear registros de catálogo al mismo tiempo parecía natural.

[The Movie Database](https://tmdb.org) ofrece una base de datos pública y gratuita de películas y series, con afiches, descripciones de episodios, repartos y más. Podía usarla para elaborar fichas bonitas de toda mi colección.

---

### Limpiar los datos

**De ninguna manera** iba a hacerlo a mano.

Programar todos los casos especiales era desalentador. Había una infinidad de nombres extraños y relacionarlos con registros de [TMDB](https://tmdb.com) sería muy difícil. Francamente, estaba fuera de mi alcance.

Lo que necesitaba era un pasante, un ayudante o un robot que revisara los directorios uno por uno.

¡Y tenía exactamente eso! Acababa de descargar [Qwen 3.5 4B](https://huggingface.co/Qwen/Qwen3.5-4B), un pequeño modelo de lenguaje parecido a ChatGPT que corre en mi propio computador. Era suficientemente inteligente para hacerlo.

---

### Un modelo de lenguaje como pasante

Acababa de terminar [Ax3l](https://ax3l.osoyalce.com), un agente autónomo de IA que ejecutaba miles de simulaciones para optimizar una red neuronal sencilla. Ajustaba un parámetro, simulaba, estudiaba resultados, volvía a ajustar…

En comparación, este proyecto era simple. El modelo debía:

- Examinar un directorio.
- Relacionar su contenido con un registro de [TMDB](https://tmdb.org).
- Renombrar y mover el archivo a una ubicación ordenada.
- Crear una ficha en la base de datos.

Lo llamé **R3el** y empecé a diseñarlo.

![Arquitectura de R3el](/images/2026-09-27-r3el-architecture.png)

Bueno, quizás no tan sencillo, pero sí algo que podía crear partiendo del código de [Ax3l](https://ax3l.osoyalce.com).

---

### Interfaz de control

Sabía que terminaría creando un catálogo web para explorar la colección, así que aproveché y construí allí la interfaz de control.

![Interfaz de control](/images/2026-09-27-r3el-control.png)

---

### Registro de eventos

Mi experiencia con modelos de lenguaje me enseñó la importancia de informes detallados. Volví a aprovechar el código de [Ax3l](https://ax3l.osoyalce.com) y creé un **registro de eventos** para monitorear, ajustar y diagnosticar el proceso.

![Registro de eventos](/images/2026-09-27-r3el-eventlog.png)

Me tomó casi todo un día enumerar y etiquetar los mensajes que me interesaban. Fue una inversión excelente que ayudó enormemente al desarrollo.

---

### ¡¡¡Funcionó!!!

Tuve que ajustar algunos casos, mejorar la identificación y hacer una pequeña limpieza manual de archivos. ¡Pero funcionó! Durante los siguientes días agregaba lotes de directorios a la cola y dejaba trabajar al sistema.

![R3el procesando archivos](/images/2026-09-27-r3el-processing.png)

- R3el examinaba los archivos.
- Identificaba series y títulos.
- Renombraba y limpiaba los nombres.
- Eliminaba directorios vacíos.
- Descargaba afiches e información del elenco y los productores.
- Creaba los registros de base de datos que unían toda la información.

---

### Explorar y buscar

El último paso fue desarrollar una interfaz cómoda para explorar y buscar:

![Catálogo R3el](/images/2026-09-27-r3el-catalogue.png)

Esto **sí** fue fácil. Hay muchos ejemplos de interfaces de este tipo y crear una sencilla resultó bastante directo.

![Serie en R3el](/images/2026-09-27-r3el-tv-show.png)

---

### Listo

El proyecto está terminado. R3el todavía está examinando archivos, pero creo que finalizará hoy. Al principio me preguntaba si construir todo esto era exagerado; sin embargo, sinceramente, no se me ocurre otra forma de abordar una limpieza masiva de datos.

Y es bastante chévere, ¿no? :)

![Logotipo de R3el](/images/2026-09-27-r3el.png)

---

### Enlaces

- [Página principal de R3el](https://r3el.osoyalce.com)
- [R3el en GitHub](https://github.com/NadimGhaznavi/r3el)
- [Historial de cambios de R3el](https://github.com/NadimGhaznavi/r3el/blob/main/CHANGELOG.md)
