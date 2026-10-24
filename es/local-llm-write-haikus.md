---
layout: single
author_profile: true
title: "Un modelo de lenguaje local escribe haikus (1/4)"
date: 2026-08-29
lang: es
locale: es
language_name: Español
translation_key: local-llm-write-haikus
permalink: /es/journal/local-llm-write-haikus/
category: journal
tags:
  - AI
  - LLM
  - Qwen
  - llama.cpp
  - GGUF
---

Esta es la primera parte de una serie de cuatro artículos. Aquí descargo, instalo y configuro software para pedirle a un modelo de lenguaje local que escriba haikus…

Hace poco conseguí dos estaciones de trabajo HP Z440 idénticas, con tarjetas gráficas M4000. Investigué un poco y descubrí que podía ejecutar un modelo de lenguaje pequeño con ese hardware. Hasta entonces solo había utilizado estos modelos como usuario, desde el navegador o mi editor VS Code.

Así que descargué **Qwen3.5 4B** de Hugging Face y empecé a descubrir cómo ejecutarlo con `llama.cpp`.

---

## Preparar el modelo

Ponerlo todo en marcha requería varios pasos.

Descargué el modelo Qwen de Hugging Face y el código fuente de `llama.cpp`. Luego preparé un modelo intermedio y finalmente lo convertí al formato **GGUF** de `llama.cpp`. Fue técnico, pero bastante directo. Lo más largo fue descargar 18 GB.

Después ejecuté `llama_server`, abrí su dirección en el navegador y…

Ahí estaba.

Una página web con una caja de chat.

Había construido mi propio chatbot.

Era al mismo tiempo increíblemente chévere y poco interesante.

Ya tenía ChatGPT, que sabe y puede hacer **MUCHÍSIMO** más.

---

## Hablar con él

Por supuesto, primero empecé a hacerle preguntas.

Le pregunté sobre conocimientos generales. Conocía personajes históricos como Rudolf Steiner, geografía, ciencia, programación…

Sabía bastante.

Eso ya era impresionante. No estaba consultando Google ni llamando a un enorme servicio de IA por Internet. El modelo y sus conocimientos estaban allí mismo, en mi propio computador.

Luego le pregunté por **el tutorial de Patrick Loeber para enseñar a una IA a jugar Snake**.

Sabía de qué hablaba.

Eso captó mi atención.

El proyecto de Patrick Loeber me resultaba familiar. Mi propio proyecto **Snake Lab** nació de experimentar con una IA que aprendía a jugar Snake. De pronto dejé de preguntarle trivialidades y comencé a hablar de algo que realmente conocía.

Y el modelo respondía con inteligencia.

Eso sí que era chévere.

---

## ¿Y ahora qué?

El problema era que seguía teniendo un chatbot.

Podía hacerle preguntas y recibir respuestas. Era impresionante, pero eso no era lo que realmente me interesaba.

Quería usar el modelo de lenguaje **desde mi propio código**.

Así que escribí un bucle de conversación bastante rudimentario.

No tenía un gran plan. Solo quería que un programa enviara un prompt, recibiera una respuesta y pudiera hacer algo con ella.

Eso planteó una pregunta importante de ingeniería:

**¿Qué debería pedirle que haga?**

Al parecer mi respuesta fue:

Escribir haikus sobre números.

---

## Haikus aleatorios

El programa generaba un número al azar y le pedía al modelo que escribiera un haiku sobre ese número.

El modelo escribía el poema.

El programa guardaba el resultado en un archivo de registro.

```
Una nueva era comienza hoy
Apolo toca el rostro de la Luna
Un nuevo mundo empieza a brillar

1970
```

Después generaba otro número.

Y otro.

Y otro.

Pronto tenía un gran modelo de lenguaje funcionando en mi computador, contemplando números enteros al azar y escribiendo poesía sobre ellos.

No era precisamente útil.

Pero funcionaba.

Lo importante no era el haiku. Era el bucle:

1. Mi programa generaba datos de entrada.
2. Construía un prompt.
3. Enviaba el prompt al modelo.
4. El modelo generaba una respuesta.
5. Mi programa registraba esa respuesta.
6. El proceso se repetía.

Por primera vez, no estaba simplemente **conversando con una IA**.

Había puesto una IA **dentro de un programa**.

---

## Conclusión

Mi primer experimento con un modelo de lenguaje local no produjo absolutamente nada de valor práctico.

Escribía haikus sobre números aleatorios.

Pero respondió a la pregunta que realmente me importaba.

Podía ejecutar un modelo local, comunicarme con él desde mi software, proporcionarle datos, recibir una respuesta, guardarla y repetir todo sin estar sentado frente a una ventana de chat.

En ese momento todavía no existía **Ax3l**.

Ni siquiera tenía completamente formada la idea.

Tenía un bucle.

Y un archivo de registro lleno de haikus.

---

## Enlaces

- [Parte 2/4 de esta serie](/es/journal/the-evolution-of-snakelab/)
