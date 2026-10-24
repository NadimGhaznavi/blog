---
layout: single
author_profile: true
title: "Del prototipo al producto (4/4)"
date: 2026-09-19
lang: es
locale: es
language_name: Español
translation_key: from-prototype-to-product
permalink: /es/journal/from-prototype-to-product/
category: journal
tags:
  - AI
  - LLM
  - Qwen
  - llama.cpp
  - GGUF
---

Fr3d funcionaba: podía comunicarse con Snake Lab, proponer cambios en la configuración de la RNN, ejecutar experimentos, analizar sus resultados y repetir el proceso. Demostró que un modelo de lenguaje local podía actuar como investigador autónomo y ajustar un sistema de aprendizaje por refuerzo a lo largo de cientos de simulaciones.

![Ax3l](/images/2026-09-17-ax3l.png)

Pero cuando terminé Fr3d también sabía exactamente cómo *no* quería construir la siguiente versión.

El código había crecido alrededor del experimento. Los prompts estaban incrustados en la aplicación, las responsabilidades se superponían y los registros me decían qué hacía el programa, pero no necesariamente *por qué*. Cada cambio requería revisar código acumulado durante el desarrollo.

Fr3d fue un prototipo exitoso.

**Ax3l** fue la reescritura.

---

## Primero construir el panel de instrumentos

Con Ax3l empecé aplicando las lecciones de arquitectura de Fr3d.

El sistema de prompts se volvió modular y basado en datos. Distintos tipos de interacción con la LLM se convirtieron en clases de prompts independientes. Datos experimentales, construcción de prompts, control de simulaciones e informes pasaron a ser responsabilidades separadas.

Más importante aún, traté la observabilidad como parte de la aplicación y no como un añadido posterior.

Ax3l recibió su propio servidor de informes y un sistema de eventos guardados en base de datos. Los eventos se organizaron por categorías y subcategorías, y los registros se volvieron interactivos.

En vez de ver solamente:

> Simulación completada.

podía hacer clic y examinar la simulación, su configuración, el prompt que la produjo, el razonamiento del modelo y los resultados.

Los registros dejaron de ser una corriente de texto.

Se convirtieron en una forma de navegar por los experimentos.

Eso era muy importante. Ax3l no iba a realizar un experimento mientras yo observaba. Haría cientos, sin supervisión, a lo largo de días o semanas. Para entender las decisiones de la IA, necesitaba poder reconstruirlas después.

---

## Después quise ver a la serpiente

En cierto momento, las filas de números dejaron de ser suficientes.

Snake Lab ya sabía cuándo una partida alcanzaba un nuevo récord, así que lo modifiqué para capturar el estado del tablero en ese momento y guardarlo con los resultados.

Luego publiqué esa imagen a través de la API ZeroMQ de Snake Lab.

Ahora Ax3l podía recuperar el tablero correspondiente a la mejor partida de una simulación y mostrarlo en el informe.

De pronto una puntuación de 47 no era simplemente `47`.

Ahí estaba la serpiente.

Se podía observar su recorrido, su longitud, la ubicación de la comida y el estado del tablero cuando consiguió el récord.

Fue una mejora técnica pequeña, pero cambió la personalidad del proyecto. Los experimentos ya no producían solo registros de base de datos: había una pequeña serpiente de IA aprendiendo a hacer mejor su trabajo.

Ninguna serpiente real sufrió durante este experimento, aunque muchas serpientes virtuales tomaron decisiones de vida bastante cuestionables.

---

## ¿Está mejorando de verdad? ¡Gráficos!

Una puntuación máxima por sí sola es una forma terrible de responder esa pregunta.

El aprendizaje por refuerzo tiene mucho ruido. Una configuración puede tener suerte. Una ejecución espectacular no significa necesariamente que el sistema haya mejorado.

Por eso añadí un histograma que muestra la distribución de puntuaciones de las simulaciones.

Luego añadí una superposición.

Dibujo el histograma de la mitad más antigua de los experimentos encima del histograma completo. Como los datos antiguos son un subconjunto del total, sus barras solo pueden ser igual de altas o más bajas.

Si Ax3l no consigue nada, los nuevos resultados simplemente se acumularán siguiendo aproximadamente la misma distribución.

Pero si el experimento funciona, sucede algo mucho más interesante:

La distribución empieza a desplazarse hacia la derecha.

Se puede *ver* cómo el experimento encuentra regiones mejores del espacio de configuraciones.

Esta visualización resultó mucho más útil que observar cómo sube un único número de récord.

---

## De 38 a 53 puntos

La configuración original de Snake conseguía alrededor de 38 puntos.

Sabía por experimentos anteriores que la RNN podía superar esa cifra. La pregunta era si un modelo de lenguaje, con las herramientas, los datos históricos y el entorno adecuados, podría encontrar configuraciones mejores de forma sistemática.

Al escribir este artículo, Ax3l había superado las 500 simulaciones.

El récord actual era **53**.

Más importante aún, la distribución de puntuaciones se había desplazado.

Ax3l no estaba lanzando dardos a ciegas hasta acertar por suerte con 53. En conjunto, los resultados se habían movido hacia configuraciones de mayor rendimiento.

Y el experimento seguía en marcha.

---

## Datos públicos del experimento: Snake Web

Quedaba un último problema.

El servidor de informes de Ax3l funcionaba muy bien dentro de mi red, pero quería que el experimento también fuera visible públicamente.

No quería exponer a Internet la base de datos ni el servidor de aplicaciones de producción solo para publicar gráficos.

Por eso creé otro pequeño proyecto: **Snake Web**.

Cada media hora, Snake Web consulta la base de datos del experimento y genera archivos CSV con los datos que necesita un sitio estático de GitHub Pages. Los publica automáticamente y la web muestra el estado más reciente.

Están los récords.

Están los histogramas.

Están los resultados de cada simulación.

Incluso se ven los cambios de semillas aleatorias. Cuando Ax3l pasa a una nueva semilla y el rendimiento cae repentinamente, el gráfico cae por un precipicio y vuelve a subir.

No es una retrospectiva maquillada que muestra solo los éxitos.

Es el experimento, con todas sus cicatrices.

---

## Observar el experimento

Con eso creo que el proyecto está terminado.

No porque no pudiera agregar otras cien cosas. Siempre podría.

Está terminado porque ya existe el sistema que imaginé originalmente.

Hay un laboratorio de aprendizaje por refuerzo que ejecuta experimentos controlados con Snake y registra los resultados.

Hay un agente LLM autónomo que analiza esos datos, razona sobre ellos, escoge nuevas configuraciones y propone otros experimentos.

Hay una capa de observabilidad para entender qué hizo y por qué.

Y una cadena de publicación que muestra los avances al público sin exponer la infraestructura interna.

En algún lugar entre la arquitectura, las bases de datos, los mensajes ZeroMQ, los prompts, los tensores PyTorch, los CSV y los gráficos, hay una serpiente que antes lograba 38 puntos.

Ahora llega a 53.

Me parece un buen momento para parar.

---

## Enlaces

- [Proyecto Ax3l](https://ax3l.osoyalce.com)
- [Experimento de Ax3l en vivo](https://snakeweb.osoyalce.com)
- [Snake Lab Server](https://snakelabserver.osoyalce.com)

### Serie del blog

- [Parte 1/4: una LLM local escribe haikus](/es/journal/local-llm-write-haikus/)
- [Parte 2/4: la evolución de Snake Lab](/es/journal/the-evolution-of-snakelab/)
- [Parte 3/4: ingeniería de prompts]( /es/journal/prompt-engineering-101/)
- La parte 4/4 es esta publicación.
