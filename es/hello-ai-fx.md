---
layout: single
author_profile: true
title: "Un pequeño experimento: AI FX"
date: 2026-04-17
lang: es
locale: es
language_name: Español
translation_key: hello-ai-fx
permalink: /es/journal/hello-ai-fx/
category: journal
tags:
  - AI
  - FOREX
  - Textual
---

He trabajado cerca de sistemas de negociación financiera. En Merrill Lynch di soporte al *Order Audit Trail System* ([OATS](https://www.finra.org/filing-reporting/market-transparency-reporting/order-audit-trail-system-oats-ROT)), un sistema de cumplimiento exigido por NASDAQ. Su propósito es sencillo en teoría, pero fundamental en la práctica: evitar que las instituciones aprovechen la actividad de sus clientes para obtener ventajas injustas. Estos sistemas detectan patrones que no deberían existir.

![AI FX en PyPI](/images/2026-04-18-aifx.png)

En CIBC dirigí un equipo encargado del sistema automatizado de negociación de divisas del banco. Era una plataforma crítica donde cada interrupción afectaba los ingresos. Según el manual de operación, generaba alrededor de cuatro millones de dólares canadienses por hora y hasta cuarenta millones en momentos de gran actividad.

Aquella experiencia sembró una idea.

Investigué durante un tiempo los sistemas automáticos de negociación FOREX y llegué a una conclusión bastante directa: este juego favorece muchísimo a las instituciones.

No porque las estrategias sean secretas, sino por su escala y ubicación.

- Ejecutan operaciones de gran volumen, donde pequeñas ventajas estadísticas se acumulan.
- Sus equipos están físicamente cerca de la infraestructura bursátil, lo que reduce la latencia.

Un operador particular, con capital limitado y mayor latencia, juega a algo muy diferente.

# Dejarlo por un tiempo

Así que guardé la idea.

Me concentré en otros proyectos, incluido [AI Hydra](https://pypi.org/project/ai-hydra/), un entorno de aprendizaje por refuerzo para experimentar con sistemas de IA e hiperparámetros.

# La idea repentina

Hace poco encontré una extensa serie de YouTube: [Forex bot & backtest system with Python for beginners](https://www.youtube.com/watch?v=zKk2iuuNJO0&list=PLZ1QII7yudbecO6a-zAI6cuGP1LLnmW8e&index=1).

Explica cómo crear un robot básico de negociación con estrategias basadas en reglas.

Eso me hizo pensar:

- ¿Y si sustituyera o complementara esas reglas con una red neuronal?

No espero «descifrar» los mercados FOREX.

Pero *sí* estoy seguro de dos cosas:

- Aprenderé muchísimo.
- Puedo experimentar con enfoques rápidos e iterativos que actúen únicamente cuando exista mucha confianza en una oportunidad.

FOREX ofrece otra ventaja: enormes cantidades de datos históricos que pueden servir como entrenamiento.

# Sembrar la idea

Creé un [repositorio en GitHub](https://github.com/NadimGhaznavi/aifx), preparé una estructura básica, incorporé una GitHub Action para las publicaciones y subí el primer paquete a [PyPI](https://pypi.org/project/aifx/).

Es pronto. Muy pronto. Ya he visto cómo funciona la gran maquinaria; ahora construyo una versión pequeña para entenderla.

La semilla está plantada.

Ahora queda ver qué crece.
