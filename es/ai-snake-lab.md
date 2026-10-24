---
layout: single
author_profile: true
title: "AI Snake Lab"
date: 2025-10-16
lang: es
locale: es
language_name: Español
translation_key: ai-snake-lab
permalink: /es/journal/ai-snake-lab/
category: journal
tags:
  - AI Snake Lab
  - Project Description
  - Programming
  - Python
  - PyTorch
  - AI
  - Textual
---

**AI Snake Lab** es un entorno interactivo de aprendizaje por refuerzo para experimentar con agentes de inteligencia artificial en el clásico juego Snake. Incluye una interfaz de terminal en vivo con Textual, una base de datos flexible de memoria de experiencias y definiciones modulares de modelos.

![AI Snake Lab](/images/2025-10-16-ai-snake-lab.png)

---

# 🚀 Características

- 🐍 **Entorno clásico de Snake** con tablero y reglas configurables.
- 🧠 **Interfaz de agentes de IA** compatible con arquitecturas lineales, RNN y CNN.
- 🎮 **Simulador basado en Textual** con visualización y métricas en vivo.
- 💾 **Memoria de experiencias en SQLite** para almacenar fotogramas, episodios y ejecuciones.
- 🧩 **Metadatos de experimentos**: modelos, hiperparámetros y versiones del mapa de estado.
- 📊 **Gráficos integrados** de tasa de hash, puntuaciones y progreso de aprendizaje.

---

# 🧰 Tecnologías

| Componente | Descripción |
|---|---|
| **Python 3.11+** | Lenguaje principal |
| **Textual** | Framework de interfaz de terminal |
| **SQLite3** | Almacenamiento de experiencias y experimentos |
| **PyTorch** *(opcional)* | Motor de aprendizaje profundo |
| **Plotext / Matplotlib** | Herramientas de visualización |

---

# 🔄 Entrenamiento dinámico

La opción *Dynamic Training* incrementa el número de entrenamientos prolongados al final de cada *época*. La función `AIAgent:train_long_memory()` se llama cada vez más veces a medida que crece la cantidad de épocas, hasta un máximo de `MAX_ADAPTIVE_TRAINING_LOOPS`, actualmente `16`, según este cálculo:

```
    if self.dynamic_training():
        loops = max(
            1, min(self.epoch() // 250, DAIAgent.MAX_DYNAMIC_TRAINING_LOOPS)
        )
    else:
        loops = 1

    while loops > 0:
        loops -= 1
        self.train_long_memory()
```

---

# 🧩 Epsilon N

La clase *Epsilon N*, inspirada por [The Bitter Lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html) de Richard Sutton, reemplaza al tradicional algoritmo *Epsilon Greedy*. Crea una instancia independiente de Epsilon Greedy para cada puntuación. Por ejemplo, cuando la IA tiene siete puntos, utiliza `self._epsilons[7]`.

La siguiente captura muestra el gráfico de *récords* con Epsilon Greedy tradicional, de una sola instancia, y con [entrenamiento dinámico](#entrenamiento-dinámico) activado. El algoritmo converge rápidamente, pero progresa de forma irregular. Los saltos repentinos indican que la exploración disminuye antes de que la IA domine cada fase del juego.

![Epsilon Greedy tradicional](/images/2025-10-16-highscores-epsilon.png)

El siguiente gráfico utiliza *Epsilon N* sin [entrenamiento dinámico](#entrenamiento-dinámico). Mantener un epsilon independiente por puntuación prolonga la exploración a ese nivel. El resultado es una curva de récords más uniforme y casi lineal mientras el agente consolida lo aprendido.

![Epsilon N](/images/2025-10-16-highscores-epsilon-n.png)

En resumen, Epsilon Greedy tradicional mejora más lentamente y alcanza pronto una meseta. Epsilon N permite un avance más constante.

---

# 💻 Instalación

El proyecto está disponible en [PyPI](https://pypi.org/project/ai-snake-lab/). Pueden instalarlo con `pip`.

## Crear un entorno virtual

```shell
python3 -m venv snake_venv
. snake_venv/bin/activate
```

## Instalar AI Snake Lab

Después de activar el entorno:

```shell
pip install ai-snake-lab
```

---

# ▶️ Ejecutar AI Snake Lab

Desde el entorno virtual:

```shell
ai-snake-lab
```

---

# ⚠️ Limitaciones

Como la simulación se ejecuta dentro de una interfaz Textual, cambiar el tamaño de la terminal o volver a dibujar los gráficos puede afectar ligeramente los tiempos. Por eso los récords pueden aparecer en diferentes partidas durante ejecuciones repetidas. Es un comportamiento esperado, no un error de la lógica de IA.

Si necesitan simulaciones totalmente reproducibles, escríbanme. Podría crear un **modo sin interfaz gráfica**, donde la simulación se ejecute en un proceso independiente de Textual.

---

# 🙏 Agradecimientos

El código original está basado en el tutorial de YouTube de Patrick Loeber, [Python + PyTorch + Pygame Reinforcement Learning – Train an AI to Play Snake](https://www.youtube.com/watch?v=L8ypSXwyBds). Pueden consultar el [código original en GitHub](https://github.com/patrickloeber/snake-ai-pytorch). ¡Muchas gracias, Patrick! ¡Eres increíble! Este proyecto es una adaptación de su solución con Pygame y Matplotlib.

También agradezco a Will McGugan y al equipo de [Textual](https://textual.textualize.io/). Textual es fantástico. ¡Eso sí que es desarrollo rápido de aplicaciones! Migrar de Pygame y Matplotlib a Textual tomó menos de un día.

---

# 🌟 Inspiración

Crear un agente inteligente, dejarlo suelto y observar su comportamiento es fascinante. Es un poco como tener hijos, pero a una escala muchísimo menor, al menos por ahora. Ver jugar Snake a una IA resulta hipnótico. Siempre se me ocurren nuevas formas de mejorarla. Agradezco a Patrick Loeber por darme un proyecto tan divertido para explorar la inteligencia artificial.

He trabajado gran parte de mi carrera administrando sistemas Linux. Mi lugar cómodo es la línea de comandos. Nunca trabajé profesionalmente como programador y mucho menos como desarrollador frontend. [Textual](https://textual.textualize.io/), para construir interfaces de terminal enriquecidas, encaja perfectamente conmigo. Cuando descubrí [Dolphie](https://github.com/charles-001/dolphie), quedé impresionado: ¡gráficos integrados en tiempo real de métricas de MySQL!

También me inspira Richard S. Sutton. Sus ideas sobre el *aprendizaje por refuerzo* representan una revolución lenta. Su crítica a crear una IA específica para cada problema y luego declarar el trabajo terminado es acertada. Su visión de agentes capaces de aprender continuamente y de manera no lineal es una frontera hacia la *inteligencia artificial general*.

---

# 📚 Documentación técnica

- [Sitio del proyecto](https://snakelab.osoyalce.com/)
- [Proyecto en GitHub](https://github.com/NadimGhaznavi/ai-snake-lab)
- [Estructura de archivos](https://snakelab.osoyalce.com/pages/project_layout.html)
- [Esquema de base de datos](https://snakelab.osoyalce.com/pages/db_schema.html)
- [Organización y nombres de constantes](https://snakelab.osoyalce.com/pages/constants.html)
- [Normas de commits de Git](https://snakelab.osoyalce.com/pages/git_commit_standards.html)
- [Estrategia de ramas de Git](https://snakelab.osoyalce.com/pages/git_branching_strategy.html)
- [Registro de cambios](https://snakelab.osoyalce.com/CHANGELOG.md)

---

# 🔗 Enlaces

- [Tutorial de Patrick Loeber](https://www.youtube.com/watch?v=L8ypSXwyBds)
- [Textual de Will McGugan](https://textual.textualize.io/), framework de desarrollo rápido
- [Dolphie](https://github.com/charles-001/dolphie), panel de análisis para MySQL/MariaDB y ProxySQL
- [Página de Richard Sutton](http://www.incompleteideas.net/)
- [Citas y materiales de Richard Sutton](https://snakelab.osoyalce.com/pages/richard_sutton.html)
- [Gráficos para diagnosticar redes neuronales](https://medium.com/data-science/useful-plots-to-diagnose-your-neural-network-521907fa2f45), George V Jose
- [Análisis de curvas de aprendizaje](https://wandb.ai/mostafaibrahim17/ml-articles/reports/A-Deep-Dive-Into-Learning-Curves-in-Machine-Learning--Vmlldzo0NjA1ODY0), Mostafa Ibrahim
