---
layout: single
author_profile: true
title: "Simulando la red eléctrica"
date: 2026-04-26
lang: es
locale: es
language_name: Español
translation_key: the-grid
permalink: /es/journal/the-grid/
category: journal
tags:
  - pgm-ds
  - Power Grid Modeling Data Science
  - Simulation
---

Un posible cliente me recomendó el artículo [Empowering the Energy Transition Through Fast and Flexible Network Simulation](https://lfenergy.org/empowering-the-energy-transition-through-fast-and-flexible-network-simulation/). Describe un proyecto de código abierto llamado [Power Grid Model DS](https://power-grid-model-ds.readthedocs.io/en/stable/) (`pgm-ds`), donde *DS* significa *Data Science*.

![Logotipo de PGM DS](/images/2026-04-26-pgm-ds-logo.png)

`pgm-ds` amplía [Power Grid Model](https://power-grid-model.readthedocs.io/en/stable/), una biblioteca para analizar redes eléctricas de distribución en estado estacionario. Su modelo incluye:

- Nodos
- Ramas
- Equipos
- Sensores
- Fallas
- Reguladores

y mucho más. Se usa para modelar flujos de potencia, estimar estados y analizar cortocircuitos.

*Power Grid Model DS* añade una API clara para modelar y simular el comportamiento de la red. Está pensada para desarrollar aplicaciones de ciencia de datos, análisis y simulación. Define una clase de datos `Grid` que mantiene la coherencia de toda la red y permite ampliar sus estructuras.

---

# Ejecutar la demostración

La mecánica era sencilla. Comprender el significado costó más.

`pgm-ds` **sí** es fácil de usar. Convertir su documentación en cuadernos Jupyter en un script de demostración fue trivial. Entender qué estaba viendo fue otra historia.

El paquete incluye una visualización de la red mediante una interfaz WSGI desarrollada con [Flask](https://flask.palletsprojects.com/en/stable/). Los gráficos usan [Dash Cytoscape](https://dash.plotly.com/cytoscape), un potente motor de visualización de grafos perfecto para este tipo de trabajo.

La imagen era bonita, pero los datos importantes aparecían como tablas en la terminal. Aquí tienen ejemplos:

## Nodos


 id  | u_rated | node_type | feeder_branch_id | feeder_node_id |  u  |  x_coor |  y_coor
 ----|---------|-----------|------------------|----------------|-----|---------|-------
  1  | 10500.0 |     0     |        64        |       61       | 0.0 |125.848..|245.522..
  2  | 10500.0 |     0     |        65        |       61       | 0.0 |385.322..|175.70..
  61 | 10500.0 |     1     |   -2147483648    |  -2147483648   | 0.0 |134.732..|145.515..

## Líneas

  id | from_node | to_node | from_status | to_status | feeder_branch_id | feeder_node_id | is_feeder |   r1  |   x1  |  c1 | tan1 |   i_n   | i_from 
----|-----------|---------|-------------|-----------|------------------|----------------|-----------|-------|-------|-----|------|---------|---------
 63 |     61    |    12   |      1      |     1     |        63        |       61       |    True   |0.411..|0.023..| 0.0 | 0.0  |110.013..|  0.0   
 74 |     20    |    8    |      1      |     1     |        63        |       61       |   False   |0.585..|0.000..| 0.0 | 0.0  |227.694..|  0.0   
 125 |     96    |    94   |      1      |     1     |        63        |       61       |   False   |0.036..|0.04..| 0.0 | 0.0  |362.904..|  0.0   

 ## Transformadores

   id | from_node | to_node | from_status | to_status | feeder_branch_id | feeder_node_id | is_feeder |    u1   |   u2   |     sn     | tap_size |   uk  |    pk    |  i0 |  p0 | winding_from | winding_to | clock | tap_side | tap_pos | tap_min | tap_max | tap_nom
-----|-----------|---------|-------------|-----------|------------------|----------------|-----------|---------|--------|------------|----------|-------|----------|-----|-----|--------------|------------|-------|----------|---------|---------|---------|----------- 
 123 |     5     |    95   |      1      |     1     |        64        |       61       |   False   | 10500.0 | 3000.0 | 30000000.0 |   0.0    | 0.203 | 100000.0 | 0.0 | 0.0 |      0       |     0      |   12  |    0     |    0    |    0    |    0    |    0   
 124 |     12    |    96   |      1      |     1     |        63        |       61       |   False   | 10500.0 | 3000.0 | 30000000.0 |   0.0    | 0.203 | 100000.0 | 0.0 | 0.0 |      0       |     0      |   12  |    0     |    0    |    0    |    0    |    0   


---

# Comprender los datos

Preparar y ejecutar el programa **fue** sencillo. Entender los resultados y sus implicaciones no. Mi experiencia es en administración de sistemas Linux y programación Python; no soy ingeniero eléctrico.

Empecé examinando estructuras de datos, funciones y la clara [documentación de la API](https://power-grid-model-ds.readthedocs.io/en/stable/api_reference/power_grid_model_ds.html), además de investigar por mi cuenta.

Siempre imaginé la red eléctrica como un circuito complicado, pero relativamente estático. Descubrí que se entiende mejor como un sistema físico dinámico, donde propiedades como la reactancia y la potencia reactiva influyen en su comportamiento de formas poco intuitivas. Casi parece un ser vivo.

Uno de los primeros valores que observé fue la **reactancia por kilómetro**, representada por el atributo `x1` de las líneas. La reactancia fue probablemente lo más difícil de entender. Terminé viendo videos de YouTube y conversando sobre ella con ChatGPT. La **potencia reactiva** es fundamental para comprender por qué la red es tan complicada de gestionar. Explicarla a fondo supera el alcance de este artículo.

No estudié todos los atributos. Solo quería hacerme una idea de qué hacía el paquete y cómo representaba el mundo real.

---

# Modelar la red

Cuando entendí la complejidad del sistema, su escala creció varias órdenes de magnitud en mi cabeza. Como mencioné, vengo del mundo Linux y trabajé en operaciones de CIBC, Merrill Lynch, Moneris Solutions y SiGe Semiconductor. Conozco esos enormes tableros llenos de detalles técnicos. Busqué uno de operaciones de Hydro One, el proveedor eléctrico de Ontario.

![Operaciones de Hydro One](/images/2026-04-26-hydro-one-ops.png)

Es apenas una parte del sistema, pero demuestra la densidad de información que manejan los operadores.

Con `pgm-ds` se puede modificar dinámicamente la red, añadir y quitar componentes, simular fallas y desviar la energía por enlaces redundantes. También permite modelar cambios, predecir la demanda futura y estimar sus consecuencias.

En un sistema tan complejo, la simulación proporciona una base natural para el aprendizaje automático. Herramientas como `pgm-ds` permiten generar escenarios estructurados para predicción, detección de anomalías y apoyo a decisiones.

Ampliar y gestionar una red eléctrica es complicado y lento. Construir centrales, represas, centros de datos, fábricas o barrios enteros lleva tiempo.

---

# Conclusión

Es un campo técnicamente fascinante y muy importante. Estas herramientas no solo modelan la infraestructura: también ayudan a determinar cómo evoluciona. Eso hace que el trabajo sea intelectualmente interesante y valioso.

---

# Enlaces

## Empresas y organizaciones

- [LF Energy](https://lfenergy.org/)
- [Alliander](https://www.alliander.com/en/)

## Power Grid Model DS

- [Descripción del conjunto de herramientas](https://lfenergy.org/empowering-the-energy-transition-through-fast-and-flexible-network-simulation/)
- [Diapositivas de una presentación](https://archive.fosdem.org/2025/events/attachments/fosdem-2025-4662-empowering-the-energy-transition-through-fast-and-flexible-network-simulation/slides/238824/FOSDEM_Pr_gNLHXgk.pdf)
- [Guía de inicio rápido](https://power-grid-model-ds.readthedocs.io/en/stable/quick_start.html#modifying-the-grid)
- [Ejemplo: conectar un nuevo consumidor](https://power-grid-model-ds.readthedocs.io/en/stable/demos/connecting_new_consumer.html)

## Documentación de API

- [Power Grid Model DS](https://power-grid-model-ds.readthedocs.io/en/stable/)
- [Power Grid Model](https://power-grid-model.readthedocs.io/en/stable/)

## Repositorios de GitHub

- [Organización Power Grid Model](https://github.com/PowerGridModel/)
  - [power-grid-model](https://github.com/PowerGridModel/power-grid-model)
  - [power-grid-model-ds](https://github.com/PowerGridModel/power-grid-model-ds)
  - [power-grid-model-io](https://github.com/PowerGridModel/power-grid-model-io)
- [Dash Cytoscape](https://github.com/plotly/dash-cytoscape)
- [Lista de correo Power Grid Model](mailto:powergridmodel+subscribe@lists.lfenergy.org)

## Conceptos fundamentales

- [RustworkX](https://www.rustworkx.org/tutorial/introduction.html)
- [Explicación de potencia reactiva](https://www.youtube.com/watch?v=2mD3UbSW7ho)
