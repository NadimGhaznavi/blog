---
layout: single
author_profile: true
title: "Animando a Ax3l mientras juega Snake"
date: 2026-10-11
lang: es
locale: es
language_name: Español
translation_key: animating-ax3l
permalink: /es/journal/animating-ax3l/
category: journal
tags:

---

Llevo un tiempo experimentando con diferentes versiones del tutorial de Patrick Loeber, [Train an AI to Play Snake](https://www.youtube.com/watch?v=L8ypSXwyBds&t=4s). Es mi proyecto favorito para relajarme: divertido, fascinante y educativo. Mientras yo he pasado horas viendo simulaciones del juego Snake, ustedes solo han podido ver capturas de pantalla. ¡Eso acaba de cambiar!

![Animación del juego Snake](/images/2026-10-11-snake-game.gif)

¡Visiten el [sitio en vivo de Ax3l](https://snakeweb.osoyalce.com) para ver más! :)

## Detalles técnicos

- **Captura:** [Snake Lab](https://snakelabserver.osoyalce.com), el servidor de las simulaciones, registra el estado del juego después de cada movimiento: posición de la serpiente, ubicación de la comida, puntuación y configuración del tablero. Son los datos fotograma por fotograma.
- **Selección:** Entre todos los juegos de una simulación se conserva el que obtiene la puntuación más alta.
- **Almacenamiento:** Cuando termina la simulación, los fotogramas de ese juego se guardan como registros JSON en MariaDB.

El proyecto [Snake Web](https://snakeweb.osoyalce.com) muestra el estado actual del [experimento Ax3l](https://ax3l.osoyalce.com). Cada 30 minutos actualiza el [sitio web](https://snakeweb.osoyalce.com) con nuevos resultados, incluidas las animaciones.

- **Recuperación:** [Snake Web](https://snakeweb.osoyalce.com) solicita los fotogramas almacenados a [Snake Lab](https://snakelabserver.osoyalce.com) mediante el protocolo de petición y respuesta de ZeroMQ, identificando la simulación por su ID único.
- **Renderizado:** La biblioteca Pillow de Python convierte los fotogramas en un GIF animado, con los gráficos del tablero, la puntuación y una pequeña animación de la comida que recorre el cuerpo de la serpiente.
- **Publicación:** [Snake Web](https://snakeweb.osoyalce.com) selecciona los mejores juegos del día, genera los GIF y automáticamente hace commit y push al repositorio GitHub del sitio, donde GitHub Pages los publica como imágenes estáticas.
