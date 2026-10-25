---
layout: single
author_profile: true
title: "Proyecto BMForge"
date: 2026-10-25
lang: es
locale: es
language_name: Español
translation_key: bm-forge-project
permalink: /es/journal/bm-forge-project/
category: journal
tags:
  - Blog
  - BMForge
  - AI coding
  - SDLC
  - AI skills
  - Spec driven
---

En esta entrada presento mi nuevo proyecto [BMForge](https://bmforge.osoyalce.com).

![Logotipo de BMForge](/images/2026-10-25-bmforge.png)

Actualmente utilizo [Codex](https://openai.com/codex/) con el modelo [Sol 6.1](https://openai.com/index/introducing-gpt-6-1-sol/) de OpenAI como agente de programación.

Me encanta. La velocidad, la calidad del código y su enorme capacidad son impresionantes. Ha cambiado por completo mi forma de desarrollar software.

Pero hay algunos problemas serios.

## Instalación de componentes por su cuenta

Codex tiene la costumbre de tomar decisiones tecnológicas sin consultarme. Se encuentra con un requisito, elige una biblioteca, la instala y empieza a incorporarla al código.

A veces tiene todo el sentido del mundo. Otras veces, no.

Quiero participar en esas decisiones. Tengo tecnologías preferidas, implementaciones existentes y patrones ya establecidos. No quiero que aparezca un framework nuevo simplemente porque al agente le pareció conveniente.

Al fin y al cabo, es **mi software**.

## Mi casa, mis reglas

Llevo mucho tiempo desarrollando software y tengo mi propia manera de hacer las cosas.

Los estándares de programación, las convenciones de nombres, las estructuras de directorios, las interfaces de bases de datos, la gestión de versiones y la separación de los entornos DEV, QA y PROD forman parte de mi ciclo de vida de desarrollo de software (SDLC).

No hay razón para que Codex conozca todo eso de manera instintiva. Pero necesito que siga esas prácticas.

Por ejemplo, cuando se investiga un problema en producción, un desarrollador no debería ponerse a experimentar en el servidor de producción. Hay que recopilar los registros, exportar la base de datos, llevar las pruebas al equipo de desarrollo y reproducir allí el problema.

Eso es disciplina operativa básica.

Codex es un desarrollador muy capaz, pero no necesariamente sabe cómo funciona mi entorno de trabajo.

Y cuando improvisa, las cosas pueden volverse caóticas.

Eso no puede ser.

## Repetirme una y otra vez

Codex admite un archivo `AGENTS.md` con instrucciones específicas para cada proyecto. Es útil, pero me encontraba repitiendo las mismas indicaciones en distintos proyectos.

Cosas como:

- *«Mira el script new-release de bmgeoip. Usa ese patrón».*
- *«Echa un vistazo al proyecto BMDynIP en /opt/dev/bmdynip. Tiene un servidor de informes. Úsalo como guía».*
- *«El proyecto snake-web-code utiliza un GitPublisher. Empieza por esa implementación».*

Había **MUCHÍSIMO** de eso.

Y no era solo una cuestión de repetición. Cada implementación nueva ofrecía otra oportunidad de introducir pequeñas inconsistencias.

Podría haber seguido ampliando `AGENTS.md`, pero quería algo más sistemático: reutilizable, versionado y capaz de mejorar con el tiempo.

## Llega BMForge

![Arquitectura de BMForge](/images/2026-10-25-bmforge-architecture.es.svg)

[BMForge](https://bmforge.osoyalce.com) es mi intento de resolver estos problemas.

En esencia, BMForge reúne tres cosas:

**Especificaciones + Procesos + Pila de software**

Proporciona una base de ingeniería común para crear nuevas aplicaciones utilizando componentes reutilizables y prácticas de desarrollo consolidadas.

El repositorio contiene:

- **Especificaciones de componentes:** Definen el comportamiento, los módulos de Python autorizados, las restricciones de implementación y las expectativas de las pruebas.
- **Componentes reutilizables:** Implementaciones probadas de funciones habituales, como acceso a bases de datos, servicios HTTP, mensajería ZeroMQ, configuración, localización y visualización.
- **Procesos de desarrollo:** Inicialización de proyectos, convenciones de nombres, estructuras de directorios, instalación, actualizaciones, versiones y otras actividades del SDLC.
- **Skills para agentes de IA:** Instrucciones que enseñan a los agentes de programación a respetar estos estándares y ejecutar mis procesos establecidos.

Al comenzar un proyecto nuevo, BMForge proporciona los cimientos. La aplicación es dueña de su implementación y no depende de BMForge durante la ejecución.

### Skills para agentes de IA

Los skills de IA eran la pieza que faltaba en el rompecabezas.

Cada vez son más habituales, y hay muchísimos disponibles en Internet.

**Una advertencia: instalar un skill de IA de terceros en tu entorno merece el mismo cuidado que instalar software de terceros.** Los skills pueden indicar a los agentes que ejecuten comandos, accedan a recursos e interactúen con tu entorno. Asegúrate de confiar en la fuente.

En lugar de depender de skills de terceros, creé algunos específicamente para mi propio entorno de desarrollo.

No son simples colecciones de prompts. Son manuales de procedimientos operativos, parecidos a los que seguiría un administrador de sistemas o un analista de operaciones.

Por ejemplo, el **skill de análisis de causa raíz (RCA)** de BMForge enseña a Codex cómo investigar los problemas de las aplicaciones en producción.

El procedimiento es sencillo:

1. Conectarse a producción mediante SSH.
2. Recopilar los registros pertinentes y volcados de la base de datos sin modificar el sistema remoto.
3. Transferir las pruebas al equipo de desarrollo.
4. Reproducir, investigar y corregir el problema localmente.

**Nunca modificar producción durante un RCA.**

Eso es todo. Sin procedimientos elaborados ni burocracia. Simplemente una práctica operativa establecida que ahora Codex puede seguir de forma consistente.

Otros skills definen cómo utilizar constantes, construir interfaces de bases de datos, implementar servicios HTTP o preparar una versión.

Lo importante es que ya no tengo que explicar estos procedimientos desde cero cada vez.

### Las restricciones son una ventaja

BMForge limita deliberadamente las tecnologías que Codex puede introducir.

Las especificaciones de componentes identifican la pila de software aprobada. Si los componentes y módulos disponibles no permiten satisfacer un requisito, se espera que Codex se detenga y analice conmigo las alternativas.

Tal vez necesitemos una biblioteca nueva. Tal vez debamos ampliar un componente existente. Tal vez haya que reconsiderar el propio requisito.

Pero la decisión será deliberada.

**La IA contribuye al proceso de ingeniería. No lo redefine unilateralmente.**

### Construir la biblioteca de componentes

BMForge también está diseñado para crecer.

Algunos componentes se extraen de aplicaciones existentes que ya están desplegadas. Se han construido y utilizado, así que ofrecen un punto de partida práctico.

Otros se crean a partir de especificaciones para satisfacer las necesidades de futuras aplicaciones.

A medida que desarrollo más software, identifico patrones comunes y los incorporo a BMForge como componentes reutilizables.

Cada proyecto nuevo puede aportar algo útil al siguiente.

### Trazabilidad y mejora continua

Esta es la parte que más me entusiasma.

Los componentes de BMForge incluyen metadatos que identifican la especificación y la versión a partir de las cuales se implementaron.

Eso crea una relación trazable entre una aplicación desplegada y la especificación de ingeniería que sustenta sus componentes.

Pensemos en qué ocurre cuando descubro un defecto.

Puedo rastrear el componente afectado hasta su especificación, determinar si el problema está en la implementación o en la propia especificación y mejorar la parte correspondiente.

El mismo principio se aplica a las mejoras de calidad y rendimiento.

Con el tiempo, las mejoras pueden volver a BMForge y quedar disponibles para futuras aplicaciones.

**Encontrar un problema. Mejorar la implementación. Mejorar la especificación. Transmitir ese conocimiento.**

Ese es el objetivo: no solo reutilizar código, sino acumular conocimiento de ingeniería.

Es un proceso gradual, escalón a escalón, en el que cada mejora puede elevar la calidad de los proyectos posteriores.

El mecanismo de trazabilidad ya está implementado. Los informes y análisis más completos que hará posibles todavía están por llegar.

## La IA como participante en el SDLC

Hay una idea más amplia detrás de BMForge.

No intento inventar una metodología de desarrollo completamente nueva solo porque ahora intervenga la IA.

Ya tengo un SDLC que funciona para mí.

Lo que hago es **integrar la IA en ese proceso como un participante más**.

Codex aporta una velocidad extraordinaria y una gran capacidad de implementación. Yo aporto los requisitos, la arquitectura, los estándares, la experiencia operativa y el criterio de ingeniería.

BMForge ofrece el marco común que nos permite trabajar juntos de forma coherente.

El agente se vuelve más eficaz porque comprende el entorno en el que trabaja. Yo soy más productivo porque ya no tengo que explicar las mismas reglas ni reconstruir constantemente la misma infraestructura.

Y el software resultante sigue siendo comprensible, fácil de mantener y mío.

## Primeros resultados

Todavía estamos empezando, pero ya he utilizado BMForge para crear **DevLog**, una aplicación que registra cuánto tiempo dedico a trabajar en mis distintos proyectos de GitHub.

DevLog recopila la actividad de desarrollo y la presenta mediante un sencillo panel web.

Es una aplicación modesta, pero ha servido como una prueba práctica muy útil del enfoque.

BMForge estandarizó y aceleró la configuración inicial del proyecto, proporcionó componentes y convenciones de desarrollo consolidados y ayudó a Codex a producir una implementación coherente sin reinventar una y otra vez la infraestructura subyacente.

Todavía hay mucho margen de mejora, tanto en DevLog como en el propio BMForge.

Pero precisamente de eso se trata.

## Conclusión

BMForge comenzó como una manera de dejar de repetirme ante un agente de programación.

Se está convirtiendo en algo más interesante: una colección creciente de especificaciones, procesos, componentes reutilizables y conocimiento operativo que respalda mi forma de desarrollar software.

Ahora puedo crear aplicaciones mucho más rápido sin renunciar al control sobre la arquitectura, las decisiones tecnológicas o las prácticas operativas.

Y cuando algo no funciona, tengo un mecanismo para incorporar esa experiencia al framework.

**BMForge me permite desarrollar software a la velocidad de la IA sin perder mi propia disciplina de ingeniería.**

Lo considero un éxito y pienso seguir ampliándolo y mejorándolo.

Hay una curiosa satisfacción circular en utilizar la IA para construir un framework que enseña a la IA a desarrollar software siguiendo métodos humanos de eficacia demostrada. ¡Qué bueno es seguir formando parte del proceso!
