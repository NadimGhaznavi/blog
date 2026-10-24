---
layout: posts
author_profile: true
title: "Warum Konstanten wichtig sind"
date: 2025-09-11
lang: de
locale: de
language_name: Deutsch
translation_key: caring-about-constants
permalink: /de/journal/caring-about-constants/
category: journal
tags:
  - Constants
  - Python
  - Db4E
---

Dieser Beitrag handelt davon, wie man Konstanten im Code verwendet, warum das wichtig ist und welche Vorteile es bringt. Leider musste ich es auf die harte Tour lernen. Anfangs benutzte ich nur wenige Konstanten.

Ich dachte kaum über Konstanten nach. Wie in vielen Projekten verwendete ich jede Menge magische Strings. Sie waren schnell geschrieben, praktisch und überall.

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

Ein *magischer String* ist ein direkt im Quellcode stehender Zeichenkettenwert, dessen Inhalt das Verhalten oder die Logik des Programms wesentlich beeinflusst. „Magisch“ bedeutet, dass seine Bedeutung nicht unmittelbar erkennbar ist, weil sie nirgendwo ausdrücklich definiert oder erklärt wird.

---

# Phase I: Magische Strings

Als Erstes vereinheitlichte ich die Namen der Konstanten. Ungefähr acht Stunden nach Beginn meines Refactoring-Marathons kämpfte ich mit der Namensgebung. Ich starrte auf den Bildschirm, dachte mir schlechte Namen aus und fluchte leise vor mich hin. Schließlich tat ich, was ich von Anfang an hätte tun sollen: recherchieren.

## Einheitliche Namen

Ein Namensstandard ist entscheidend. Ich verwende einen Standard, der sich bewährt hat. Meine Konstanten unterteile ich in drei Kategorien:

Typ | Beispielname | Beispielwert
---|---|---
Feld | P2POOL_VER_FIELD | p2pool_ver
Beschriftung | P2POOL_VER_LABEL | "P2Pool version"
Standardwert | P2POOL_VER_DEFAULT | "4.10.1"

Irgendwann erkannte ich, dass magische Strings *schlecht* waren. Dann kam der Tag der Abrechnung, der einschließlich sechs weiterer Stunden schließlich zu einem 30-stündigen Refactoring-Marathon wurde. Das Ziel war einfach: magische Strings entfernen und durch Konstanten ersetzen. Es war anstrengend.

Gegen Ende schmerzten meine Handgelenke, und ich befürchtete schon ein Karpaltunnelsyndrom. Dann wurde mir klar, dass ich ständig die *Umschalttaste* drückte, weil alle Konstanten großgeschrieben waren. Mit der *Feststelltaste* ging es viel besser.

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

# Probleme mit magischen Strings

## Fehleranfällig

Fehler schleichen sich ein und führen zu unerwarteten Nebenwirkungen, die schwer aufzuspüren sind. Vielleicht steht an einer Stelle `p2pool_ver` und an einer anderen `p2pool_version`. Es gibt keine Laufzeitfehlermeldung, doch das Programm verhält sich merkwürdig.

## Unschön

Ich verwende **Visual Studio Code** als Entwicklungsumgebung. Mit seinen Syntaxfarben kann es viel helfen. Bei magischen Strings verschenkt man einen Teil dieser Vorteile.

---

# Phase II: Intelligente Gruppen

Mit der Zeit merkte ich, dass ein riesiger Haufen Konstanten nicht das Ziel sein konnte. Sie lagen verstreut, waren schwer zu finden und gelangten auf unschöne Weise in den Anwendungscode. Ich hatte *magische Strings* lediglich durch *magische Konstanten* ersetzt.

Ein Fortschritt, aber ...

Ich begann ein neues Refactoring. Diesmal ging es nicht darum, die Strings loszuwerden, sondern Konstanten logisch zu gruppieren. Die Idee: Jede Konstante gehört in ihre Domäne, und der Anwendungscode sollte sie möglichst selten direkt referenzieren.

## Unterstützung durch virtuelle Klassen

Ich verwende die Datei `ConstGroup.py`, mit der ich die Datentypen meiner Konstanten direkt in der Konstantendatei angeben und alles gut lesbar halten kann.

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

## Domänenspezifische Konstanten

Einige Ausschnitte aus meinen aktuellen Konstantendateien:

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

# Probleme mit vielen Konstanten

## Duplikate

Mit nur drei großen Kategorien, *Felder*, *Beschriftungen* und *Standardwerte*, lassen sich Duplikate schwer vermeiden. Ich sortierte alphabetisch, aber manchmal vergaß ich bestehende Definitionen, etwa `ver_p2pool` und `p2pool_ver`.

## Immer noch unübersichtlich

Mein Code war voller Konstanten. **ÜBERALL**. Die Suffixe `_FIELD`, `_LABEL` und `_DEFAULT` machten sie auch noch lang. `LOCAL_SOFTWARE_SYSTEM_FIELD` und `MAX_BACKUPS_DEFAULT` sind nicht gerade handlich. Es fehlte etwas.

## Zu viele Dateien

Ich versuchte, sie auf Domänen zu verteilen, aber das machte es zunächst schlimmer. Dateien wie `Jobs_Fields.py`, `Jobs_Defaults.py` und `Jobs_Labels.py` erhöhten die Gefahr von Dubletten.

---

# Vorteile

Die Vorteile der Konstanten zeigten sich sofort.

## Automatische Vervollständigung

Bei gruppierten Konstanten bietet VS Code beim Tippen passende Vorschläge. Das macht einen echten Unterschied. Man tippt deutlich weniger, die *Tabulatortaste* ist mein Freund, und die Fehlergefahr sinkt.

## Schönere Darstellung

Mit Konstanten sieht der Code im Editor einfach *besser* aus.

## Verständlichkeit

Mein Code wurde nicht nur hübscher, sondern klarer. `DStatus.ERROR`, `DDef.MONEROD_VER` und `DDir.RUN` vermitteln Bedeutung.

## Querverweise und Aliasse

Die neue Lösung ermöglicht auch Querverweise. `DDir.DATA` ist beispielsweise ein Alias für `DField.DATA_DIR`. Das macht den Code verständlicher:

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

Ich habe auch `DJob.OP` definiert, aber nur als Alias: `OP : str = DField.OP`. Im Dictionary fühlte sich die Verwendung von `DJob.OP` trotzdem falsch an.

---

# Fazit

- **Verwendet gruppierte Konstanten** und beginnt damit gleich zu Anfang!
- **Wartet nicht**: Legt die Konstantenstrategie beim Projektstart fest.
- **Namen zählen**: Wählt früh eine Konvention und haltet euch daran.
- **Sinnvoll gruppieren**: Domänenspezifische Gruppen sind besser skalierbar als flache Listen.
- **Aliasse schaffen Klarheit**: Eine Konstante kann je nach Kontext unterschiedliche Bedeutungen haben.
- **Refactoring lohnt sich**: Auch wenn es schmerzt, macht eine gute Struktur die weitere Entwicklung schneller und weniger fehleranfällig.
