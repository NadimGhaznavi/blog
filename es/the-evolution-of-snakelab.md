---
layout: single
author_profile: true
title: "La evolución de Snake Lab (2/4)"
date: 2026-09-05
lang: es
locale: es
language_name: Español
translation_key: the-evolution-of-snakelab
permalink: /es/journal/the-evolution-of-snakelab/
category: journal
tags:
  - AI
  - LLM
  - Qwen
  - llama.cpp
  - GGUF
---

En mi [artículo anterior](/es/journal/local-llm-write-haikus/) puse en marcha un modelo de lenguaje local en un viejo HP Z440 y escribí un pequeño bucle de conversación. Generaba números aleatorios, se los pasaba al modelo y le pedía haikus.

![Logotipo de Snake Lab](/images/2026-09-05-snake-lab.png)

Funcionaba.

Tenía una máquina en el sótano escribiendo poesía sobre números al azar.

Naturalmente, la siguiente pregunta era: **¿qué más podría conseguir que hiciera?**

Mientras experimentaba con el modelo descubrí que conocía el proyecto de IA para Snake de Patrick Loeber. Eso me llamó la atención porque yo conocía bien ese experimento. Llevaba bastante tiempo trabajando con ese código y modificándolo.

Y tuve una idea.

¿Y si, en vez de pedir haikus, le diera los resultados de una simulación de Snake?

Podría mostrarle la configuración que produjo los resultados, pedirle que los analizara y que propusiera una nueva configuración. Ejecutaría esa configuración, recopilaría los resultados, se los devolvería al modelo y repetiría el proceso.

En otras palabras:

**¿Podría lograr que un modelo de lenguaje optimizara otra IA?**

Con el tiempo, esa idea se convertiría en **Fr3d**.

Pero había un problema.

---

## El viejo proyecto AI Snake Lab

Había seguido el tutorial de Patrick Loeber y continué desarrollándolo. Lo adapté a **Textual** y cambié los gráficos de Matplotlib por gráficos integrados de **Textual Plot**. Separé la aplicación en cliente y servidor conectados con ZeroMQ. Los resultados de las simulaciones se guardaban en una base de datos.

El cliente Textual podía configurar simulaciones, enviar la configuración al servidor, iniciar ejecuciones y observarlas mientras ocurrían.

Ya era mucho más que un juego de Snake.

Pero seguía siendo una *aplicación*, diseñada bajo la suposición de que una persona estaba frente al cliente realizando los experimentos.

Fr3d cambió esa suposición.

---

## Construir el laboratorio

Tomé el código existente de Snake y creé un proyecto independiente: **Snake Lab Server**.

La idea era simple. Al servidor no debería importarle quién realizara el experimento.

Un cliente enviaría una configuración JSON por ZeroMQ. El servidor la validaría, pondría el experimento en cola, ejecutaría la simulación, recopilaría métricas y guardaría los resultados en MariaDB.

El servidor funcionaría como un servicio systemd.

Así podría usarlo un modelo de lenguaje. O un script Python. O una aplicación de terminal. O algún programa que todavía no se me ocurra.

El simulador no tendría por qué saberlo.

También adapté mi cliente Textual al nuevo sistema. Seguía queriendo iniciar experimentos yo mismo y, sobre todo, verlos en acción.

Hay algo mucho más satisfactorio en observar a la serpiente moviéndose por el tablero que en ver números pasar por un archivo de registro.

Pero ahora Textual era simplemente **uno de los clientes**.

Esa diferencia era fundamental.

---

## Un experimento es más que un récord

También quería que Snake Lab Server fuera estricto respecto a qué constituye un experimento.

La configuración se valida antes de aceptar una simulación. Se guarda exactamente qué configuración se utilizó, junto con los resultados. Se recopilan métricas durante toda la ejecución, incluidos datos por juego y episodio.

Esto importa porque no quería preguntar simplemente:

> ¿Consiguió esta serpiente una mejor puntuación?

Quería preguntar:

> ¿Qué ejecutamos exactamente y qué ocurrió?

Si un sistema autónomo iba a realizar cientos de experimentos, necesitaba poder responder mucho después de que terminaran.

De lo contrario no tendría un laboratorio.

Tendría una máquina tragamonedas.

---

## Listo para el científico

Y así nació Snake Lab Server.

Todavía no era Fr3d.

Aún faltaba un científico autónomo basado en un modelo de lenguaje que examinara los resultados, analizara configuraciones y decidiera qué probar después. Había que construirlo.

Pero ahora existía un lugar donde podría trabajar.

Comencé con una IA que aprendía a jugar Snake. Eso evolucionó a una aplicación cliente-servidor para experimentar. Después evolucionó nuevamente a un servicio diseñado para que **otros programas pudieran realizar experimentos controlados con la IA de Snake**.

El laboratorio estaba listo.

Ahora necesitaba construir al científico.

---

## Enlaces

- [Sitio de Snake Lab Server](https://snakelabserver.osoyalce.com)
- [Parte 1/4 de la serie](/es/journal/local-llm-write-haikus/)
- La parte 2/4 es esta publicación.
- [Parte 3/4 de la serie](/es/journal/prompt-engineering-101/)
