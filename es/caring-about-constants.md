---
layout: single
author_profile: true
title: "La importancia de las constantes"
date: 2025-09-11
lang: es
locale: es
language_name: Español
translation_key: caring-about-constants
permalink: /es/journal/caring-about-constants/
category: journal
tags:
  - Constants
  - Python
  - Db4E
---

Este artículo trata sobre el uso de constantes en el código: por qué son importantes, cuáles son sus beneficios y cómo utilizarlas bien. Lamentablemente, aprendí estas lecciones a la fuerza. Al principio casi no usaba constantes.

Como en muchos proyectos, dependía de cadenas mágicas. Eran rápidas y cómodas, y estaban por todas partes.

```python
form_data = {
    "name": "Update Operation",
    "to_module": "DeplMgr",
    "to_method": "post_job","
    "op": "update",
    "element_type": "monerod_remote",
    "element": self.monerod,
}
```

Una *cadena mágica* es un texto escrito directamente en el código cuyo valor influye considerablemente en el comportamiento o la lógica del programa. Se le dice «mágica» porque su significado puede no ser evidente para quien lee el código.

---

# Fase I: cadenas mágicas

Mi primer paso fue estandarizar los nombres de las constantes. Llevaba unas ocho horas refactorizando cuando me topé con el problema de cómo nombrarlas. Después de mirar la pantalla, inventar nombres malos y maldecir, hice lo que debí hacer desde el principio: investigar.

## Nombres estándar

Una convención es fundamental. Uso la siguiente clasificación, que me ha dado buenos resultados:

Tipo | Ejemplo de nombre | Ejemplo de valor
---|---|---
Campo | P2POOL_VER_FIELD | p2pool_ver
Etiqueta | P2POOL_VER_LABEL | "P2Pool version"
Predeterminado | P2POOL_VER_DEFAULT | "4.10.1"

Finalmente comprendí que las cadenas mágicas eran *un problema*, y llegó el día de la verdad: una maratón de refactorización de 30 horas, con seis horas adicionales. Mi objetivo era reemplazarlas todas por constantes. ¡Fue durísimo!

Al final me dolían las muñecas y temía estar desarrollando el síndrome del túnel carpiano. Luego descubrí que era porque apretaba constantemente *Shift* para escribir las constantes en mayúsculas. Al empezar a usar *Caps Lock*, todo mejoró.

```python
form_data = {
    NAME_FIELD: UPDATE_OPERATION_LABEL,
    TO_MODULE_FIELD: DEPLOYMENT_MGR_DEFAULT,
    TO_METHOD_FIELD: POST_JOB_FIELD,
    OP_FIELD: UPDATE_FIELD,
    ELEMENT_TYPE_FIELD: MONEROD_REMOTE_FIELD,
    ELEMENT_FIELD: self.monerod,
}
```

---

# Problemas con las cadenas mágicas

## Propensas a errores

Los errores pueden producir efectos inesperados difíciles de rastrear. Quizás en un sitio escribas `p2pool_ver` y en otro `p2pool_version`. No aparece un error en tiempo de ejecución, pero el programa se comporta de forma extraña.

## Feas

Uso **Visual Studio Code**, que resalta el código con colores. Las cadenas mágicas no aprovechan todas las ventajas del editor.

---

# Fase II: grupos inteligentes

Con el tiempo comprendí que una gran montaña de constantes tampoco era la solución. Estaban dispersas, era difícil encontrarlas y seguían apareciendo en el código de maneras poco claras. Había cambiado *cadenas mágicas* por *constantes mágicas*.

Una mejora, pero…

Inicié otra refactorización, esta vez para organizar las constantes en grupos lógicos. Cada constante debería pertenecer a su propio dominio y el código de aplicación debería referirse a ella directamente lo menos posible.

## Soporte mediante clases virtuales

Utilizo `ConstGroup.py`, que me permite especificar el tipo de datos de cada constante de forma limpia y legible.

```python
class MetaConst(type):
    """Metaclass that collects public attributes into a dictionary-like mapping."""
    def __new__(mcs, name, bases, namespace):
        cls = super().__new__(mcs, name, bases, namespace)
        cls._constants = {
            k: v for k, v in namespace.items()
            if not k.startswith("_") and not callable(v)
        }
        return cls

    def __getitem__(cls, key):
        return cls._constants[key]

    def keys(cls):
        return cls._constants.keys()

    def values(cls):
        return cls._constants.values()

    def items(cls):
        return cls._constants.items()

    def __iter__(cls):
        return iter(cls._constants)

    def __contains__(cls, item):
        return item in cls._constants

    def __repr__(cls):
        return f"<ConstGroup {cls.__name__}: {cls._constants!r}>"

class ConstGroup(metaclass=MetaConst):
    """Base class for constant groups (dict + namespace)."""
    pass
```

## Constantes por dominio

Estos son algunos fragmentos de mis archivos actuales:

```python
from db4e.Modules.ConstGroup import ConstGroup

class DField(ConstGroup):
    COLORTERM_ENVIRON : str = "COLORTERM"
    COMPONENT : str = "component"
    COMPONENTS : str = "components"
    CONFIG_FILE : str = "config_file"
    DATA : str = "data"
    DATA_DIR : str = "data_dir"
```

```python
from db4e.Modules.ConstGroup import ConstGroup
from db4e.Constants.DField import DField

class DDir(ConstGroup):
    API : str = "api_dir"
    BIN : str = "bin_dir"
    BLOCKCHAIN : str = "blockchain_dir"
    DATA : str = DField.DATA_DIR
    DB4E : str = "db4e_dir"
    DEV : str = "dev_dir"    ...
```

---

# Problemas al tener muchas constantes

## Duplicados

Es difícil evitar duplicados si solo hay tres categorías: *campos*, *etiquetas* y *valores predeterminados*. Aunque los ordenaba alfabéticamente, se me escapaban nombres repetidos, como `ver_p2pool` y `p2pool_ver`.

## Siguen siendo feas

El código estaba lleno de constantes. **¡POR TODAS PARTES!** Y con los sufijos `_FIELD`, `_LABEL` y `_DEFAULT`, los nombres se volvieron larguísimos. `LOCAL_SOFTWARE_SYSTEM_FIELD` y `MAX_BACKUPS_DEFAULT` no son precisamente cómodos.

## Demasiados archivos

Intenté organizarlos en dominios, pero tener archivos como `Jobs_Fields.py`, `Jobs_Defaults.py` y `Jobs_Labels.py` aumentó la posibilidad de duplicados.

---

# Beneficios

Empecé a notar las ventajas inmediatamente.

## Autocompletado

Al usar constantes agrupadas, VS Code ofrece sugerencias mientras escribes. Eso reduce bastante el trabajo. La tecla *Tab* es mi amiga y disminuye la posibilidad de errores.

## Código más agradable

Con constantes el código simplemente *se ve mejor*.

## Significado claro

Además se entiende mejor. `DStatus.ERROR`, `DDef.MONEROD_VER` y `DDir.RUN` expresan su propósito.

## Referencias cruzadas y alias

La solución también admite alias. Por ejemplo, `DDir.DATA` es un alias de `DField.DATA_DIR`, lo cual aclara el código:

```python
form_data = {
    DField.NAME: DLabel.UPDATE_OPERATION,
    DField.TO_MODULE: DModule.DEPLOYMENT_MGR,
    DField.TO_METHOD: DMethod.POST_JOB,
    DField.OP: DJob.UPDATE,
    DField.ELEMENT_TYPE: DElem.MONEROD_REMOTE,
    DField.ELEMENT: self.monerod,
}
```

También definí `DJob.OP` como alias: `OP : str = DField.OP`. Sin embargo, usar `DJob.OP` en el diccionario no me parecía correcto.

---

# Conclusión

- **Usen constantes agrupadas** y comiencen desde el principio, ¡no hagan lo que hice yo!
- **No esperen**: definan una estrategia de constantes al iniciar el proyecto.
- **Los nombres importan**: elijan una convención y manténganla.
- **Agrupen con criterio**: los grupos por dominio escalan mejor que una lista plana.
- **Los alias aclaran** el sentido de una constante en diferentes contextos.
- **Refactorizar vale la pena**: aunque duela, una buena estructura acelera el desarrollo futuro y reduce errores.
