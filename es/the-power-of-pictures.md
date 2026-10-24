---
layout: single
author_profile: true
title: "El poder de las imágenes"
date: 2025-09-19
lang: es
locale: es
language_name: Español
translation_key: the-power-of-pictures
permalink: /es/journal/the-power-of-pictures/
category: journal
tags:
  - Db4E
  - Class Diagrams
  - Programming
  - Python
---

¿Conocen el dicho *«Una imagen vale más que mil palabras»*? En programación, yo diría que puede valer más que mil líneas de código.

---

# El problema

Este artículo nació de mi proyecto [Db4E](https://db4e.osoyalce.com/), un *tablero para desplegar, operar y analizar en tiempo real una infraestructura de minería de Monero*. Todo iba bien hasta que apareció un error extraño: al consultar la caché de mi base de datos para obtener la lista de *despliegues de P2Pool*, obtenía elementos duplicados. Era desconcertante porque la lista se creaba a partir de un diccionario usando `values()`. No deberían haber aparecido duplicados, pero allí estaban.

---

# La causa

Al investigar descubrí que la aplicación tenía dos copias, ¡quizás más!, del objeto de caché. No había centralizado la responsabilidad sobre él, así que distintas partes del programa creaban sus propias instancias.

---

# Reorganizar el código

Comencé a limpiar y refactorizar: mover clases, pasar referencias y cambiar parámetros de constructores. Era confuso y varias veces me perdí, así que tenía que volver a empezar.

En ese momento el proyecto tenía 32 clases, por lo que no era una tarea trivial. Después de crear otro desastre, decidí alejarme un rato. Di algunos pasos de verdad: caminé alrededor de la manzana mientras pensaba.

---

# La solución: diagramas

Me di cuenta de que necesitaba un diagrama de clases para mostrar las relaciones entre ellas. Algo sencillo que indicara si una clase contenía otra o si simplemente recibía una referencia.

![Relaciones entre clases de la aplicación](/images/2025-09-19_app-class-relationships.png)

Con ese plano, implementar la solución fue fácil.

---

# Conclusión

Las imágenes **sí** valen más que mil palabras. Si se quedan atascados en el código, consideren dibujar un diagrama. ¡Es como tener un plano de construcción!

---

# Epílogo: Draw.io

Para crear el diagrama utilicé una herramienta gratuita en el navegador: [Draw.io](https://www.drawio.com/). La recomiendo mucho. Entre sus características están:

* Es gratis.
* Tiene control de versiones integrado.
* Guarda archivos en muchos lugares, incluido [Google Drive](https://drive.google.com/).
* Exporta a distintos formatos.

Si se atascan con su proyecto, prueben a dibujarlo. A veces el camino más corto a través del código empieza con un lápiz y una imagen.
