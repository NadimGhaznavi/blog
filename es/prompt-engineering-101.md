---
layout: single
author_profile: true
title: "Introducción a la ingeniería de prompts (3/4)"
date: 2026-09-12
lang: es
locale: es
language_name: Español
translation_key: prompt-engineering-101
permalink: /es/journal/prompt-engineering-101/
category: journal
tags:
  - AI
  - LLM
  - Qwen
  - llama.cpp
  - GGUF
---

[Snake Lab](https://snakelabserver.osoyalce.com) ya era un servidor de verdad, con API ZeroMQ, validación de configuraciones, base de datos y capacidad de ejecutar experimentos independientemente del cliente. La siguiente pregunta era evidente:

![Fr3d](/images/2026-09-12-fr3d.png)

¿Qué pasaría si dejara que un modelo de lenguaje lo dirigiera?

Así nació **Fr3d**.

Fue mi primer intento de crear un agente autónomo alrededor de Snake Lab. La idea era sencilla: darle suficiente información sobre el experimento, la configuración actual y los resultados previos, y pedirle que eligiera una configuración potencialmente mejor.

Snake Lab ejecutaría el experimento.

Fr3d examinaría el resultado.

Luego volvería a intentarlo.

Por fin había conectado mi pequeño experimento de modelo local con algo capaz de realizar trabajo real.

---

## Darle un experimento a la IA

Comencé a escribir prompts que explicaban al modelo el experimento de Snake.

El prompt describía la red neuronal, los parámetros disponibles, sus rangos válidos, la configuración actual y el historial relevante. Fr3d escogía un valor y enviaba un nuevo experimento mediante la API de Snake Lab.

Todo formaba un bucle:

**prompt → LLM → configuración → Snake Lab → resultado → prompt**

Y sorprendentemente pronto funcionó.

No de manera elegante.

Pero funcionó.

Ya tenía una configuración capaz de conseguir unos 38 puntos de récord y sabía, después de meses experimentando manualmente con la RNN, que existían configuraciones mejores. Fr3d tenía un problema real que resolver.

No se trataba solo de pedir a un modelo que escribiera JSON convincente.

Tenía que tomar decisiones basadas en resultados experimentales reales.

---

## Un pequeño experimento con visión

Por aquella época probé brevemente **Qwen 2.5 VL**, una versión multimodal capaz de procesar imágenes.

La idea era atractiva.

En vez de describirlo todo con números, quizás podría mostrarle diagramas o imágenes de las simulaciones y permitir que razonara visualmente. Técnicamente funcionó, pero apareció un problema más importante.

El modelo con visión no razonaba tan bien como Qwen 3.5. Para esta aplicación, esa diferencia importaba mucho más que las capacidades visuales.

Qwen 3.5 también mostraba su proceso de razonamiento, algo muy útil al diseñar prompts. Podía ver cómo entendía mis instrucciones y a veces prácticamente me decía:

*«Este prompt no tiene ningún sentido».*

No literalmente, pero casi.

Por ejemplo, le pedía elegir un valor de parámetro que todavía no se hubiera usado y descubría en su razonamiento que ya se habían probado todos los valores válidos. Omitía parte del historial que claramente necesitaba. O presentaba los datos en una estructura que me parecía lógica, pero que dificultaba identificar cuáles resultados podían compararse.

Observar el razonamiento hizo visibles esos fallos.

Probablemente esa fue mi primera gran lección sobre ingeniería de prompts: una mala respuesta no siempre es culpa del modelo. A veces simplemente le hemos planteado una mala pregunta.

---

## La ingeniería de prompts es ingeniería de software

Antes de Fr3d veía los prompts como instrucciones. Al terminar el proyecto, los consideraba más bien interfaces.

El modelo solo puede analizar la información que recibe. Importa cómo se estructura, la terminología, las restricciones y el historial incluido.

Si le pedimos escoger un valor de un espacio de búsqueda, debe conocer ese espacio. Si no puede repetir valores, necesita un registro fiable de los ya utilizados. Si los resultados con distintas semillas no se pueden comparar directamente, el prompt debe explicarlo. Y si las instrucciones se contradicen, el modelo enfrenta el mismo problema que un programador con una especificación contradictoria.

Los prompts se convirtieron en parte de la arquitectura.

---

## El problema del prototipo

Después de aproximadamente una semana, Fr3d funcionaba.

- Podía comunicarse con Snake Lab.
- Podía enviar experimentos.
- Recibía resultados.
- Analizaba esos resultados y elegía nuevas configuraciones.
- Ejecutaba un ciclo autónomo completo.

Y el código era horrible.

No porque cada pieza fuera muy complicada. Simplemente había crecido orgánicamente mientras descubría cómo debía funcionar el sistema y cómo utilizar sus componentes de bajo nivel.

- Los prompts estaban incrustados directamente en el código.
- La lógica del experimento se mezclaba con la comunicación genérica con el servidor LLM.
- Había duplicación.
- El estado estaba disperso.
- Entender una ejecución exigía revisar archivos de registro y consultas SQL improvisadas.
- Cambiar una pieza significaba perseguir dependencias por un programa bastante monolítico.

Había llegado a esa etapa típica del prototipo: el software funcionaba, pero cada vez era más difícil entender *por qué*. Y entonces comprendí otro de mis errores.

---

## Los informes debían ser una función central

Traté los registros y los informes como extras que podía añadir después. Era al revés. En un sistema autónomo de experimentación, la observabilidad forma parte del experimento. Necesitaba responder fácilmente:

- ¿Qué información recibió el modelo?
- ¿Qué prompt provocó la decisión?
- ¿Cuál fue su razonamiento?
- ¿Qué configuración envió?
- ¿Qué resultado obtuvo?
- ¿Qué cambió respecto al experimento anterior?
- ¿Cuál es la mejor configuración actual?

Una montaña de registros podría responder técnicamente todas esas preguntas. Pero responderlas en teoría y tener un sistema comprensible son cosas muy distintas. La próxima versión debía diseñarse alrededor de los informes desde el principio.

---

## Encontrar los límites

Fr3d me enseñó dónde estaban las fronteras naturales de los módulos.

Algunas partes eran genéricas:

- Comunicarse con el servidor local del modelo.
- Administrar conversaciones.
- Enviar solicitudes.
- Guardar el estado.
- Procesar respuestas.
- Registrar eventos.

Otras eran específicas del experimento:

- Parámetros de configuración de Snake Lab.
- Rangos válidos.
- Historial experimental.
- Prompts.
- Lógica de comparación.
- Reglas para elegir el siguiente valor.

En Fr3d ambas categorías estaban enredadas. Al separarlas conceptualmente, la arquitectura de la siguiente versión se volvió mucho más clara.

- Los prompts debían basarse en datos.
- Su construcción necesitaba una abstracción propia.
- Los informes tenían que formar parte de la arquitectura.
- La lógica de cada experimento debía separarse de los mecanismos genéricos de comunicación con la LLM.

---

## Empezar de nuevo

Siempre existe la tentación de seguir reparando un prototipo que funciona. Refactorizar una pieza, mover una función, extraer una clase, limpiar un prompt o quitar duplicados.

Decidí no hacerlo.

Fr3d ya había cumplido su misión. Demostró que la idea funcionaba y, aún más importante, reveló dónde estaban los problemas difíciles. Esa información es tremendamente valiosa y casi imposible de conseguir diseñándolo todo perfectamente de antemano. Así que conservé las lecciones y descarté la arquitectura.

Mi conclusión no fue *«hay que diseñarlo todo bien antes de empezar»*. Casi lo contrario:

- Construyan un prototipo rudimentario.
- Hagan funcionar el sistema de extremo a extremo.
- Observen dónde duele.
- Encuentren dónde se separan naturalmente las responsabilidades.
- Descubran qué información deberían haber registrado desde el principio.
- Después, comiencen de nuevo.

Fr3d era un desastre, pero funcionaba. Gracias a eso por fin sabía qué construir a continuación.

---

## Enlaces

- [Parte 1/4: una LLM local escribe haikus](/es/journal/local-llm-write-haikus/)
- [Parte 2/4: la evolución de Snake Lab](/es/journal/the-evolution-of-snakelab/)
- La parte 3/4 es esta publicación.
- [Parte 4/4: del prototipo al producto](/es/journal/from-prototype-to-product/)
