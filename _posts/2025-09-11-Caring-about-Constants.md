---
title: "Caring About Constants"
date: 2025-09-11
category: journal
tags: 
  - Constants
  - Python
  - Db4E
---

# Introduction

This post is about using constants in your code. Why it matters, what the benefits are and how you can do it well. I'm sad to say, I learned this the hard way. Initially I didn't use many constants in my code.

I didn’t think much about constants. Like most projects, I leaned heavily on magic strings. They were quick, convenient, and everywhere.

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

The term *magic string* refers to a string literal that is hardcoded directly into the application's source code and has a significant impact on the program's behavior or logic. The term "magic" implies that the string's purpose or meaning might not be immediately clear to someone reading the code, as its significance is not explicitly defined or explained.

---

# Phase I - Magic Strings

My first step was to standardize the names of constants. I was about eight hours into my refactoring marathon, when I started struggling with how to name the constants. After staring at the screen, naming things badly, and cursing under my breath, I did what I should have done from the start: Research.

## Standard Names

Using a standard for your constants is key. I use the following standard and it has served me well. I break constants down into three categories as shown in the table below

Type    | Example Name       | Example Value
--------|--------------------|----------------
Field   | P2POOL_VER_FIELD   | p2pool_ver
Label   | P2POOL_VER_LABEL   | "P2Pool version"
Default | P2POOL_VER_DEFAULT | "4.10.1" 

Eventually, I realized that magic strings were a *bad thing* and then came the day (plus six hours) of reckoning: a 30-hour refactoring marathon. My goal was simple — wipe out magic strings and replace them with constants. It was grueling. 

Towards the end my wrists started to hurt and I was scared that *carpal tunnel syndrome* was creeping in. Then I realized it was because I was constantly using the *shift* key since my constants were all uppercase. When I switched to using the *caps lock* key, things got a lot better.

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

# Issues with Magic Strings

## Error Prone

Errors can creep in and cause unexpected side effects and are hard to trace. Maybe you use `p2pool_ver` in one spot and `p2pool_version` in another spot. There are no runtime errors, the code just fails in weird ways.

## Ugly

I use **Visual Studio Code** as an IDE. It's colorized. When you use magic strings you aren't reaping the full benefits of VS Code.

---

# Phase II - Smart Groups 

Time passed. I slowly realized that having a big pile of constants wasn’t the endgame. Constants were scattered, hard to navigate, and still leaked into the code in ways that didn’t feel clean. I realized I’d just traded *magic strings* for *magic constants.*

An upgrade, but....

I started another refactoring — not to eliminate magic strings, but to organize constants into logical groups. The idea was simple: constants should live in their own domain, and application code should almost never reference them directly.

## Virtual Class Support

I use this `ConstGroup.py`. It allows me to specify the data type of a constant in the constant file, keeping things clean and readable.

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

## Domain Specific Constants

Here are some snippets from the constants files I currently use:

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

# Issues with Many Constants

## Duplicates

It's hard to avoid duplicates if all you have is three buckets; *fields*, *labels* and *defaults*. I ordered them alphabetically, but dupes still crept in when I forgot that I had a constant defined already e.g. `ver_p2pool` and `p2pool_ver`.

## Still Ugly

My code was littered with constants: They were **EVERYWHERE** and since I'd adoped the `_FIELD`, `_LABEL`, `_DEFAULT` convention, they weren't short. `LOCAL_SOFTWARE_SYSTEM_FIELD` is a mouthful, so is `MAX_BACKUPS_DEFAULT`. Something was missing.

## Multiple Files

I tried to get a handle on things by breaking things into domains, but that made things worse. Having a `Jobs_Fields.py`, `Jobs_Defaults.py` and `Jobs_Labels.py`, increased the chance of dupes.

---

# Benefits

I immediately began to reap the rewards of using constants.

## Auto-Complete

When you use grouped constants, VS Code provides auto-complete suggestions as you type. This actually makes a big difference: You do way less typing (the *tab key* is my friend) and -again- there's much less chance of errors (see [issues](#issues-with-magic-strings) above).

## Prettier

With constants the code just *looks* better in the editor. 

## Meaning

Not only did my code look better, but it was clearerL `DStatus.ERROR`, `DDef.MONEROD_VER`, `DDir.RUN` convey meaning.

## Cross-Referencing and Aliases

My new solution also supported cross referencing. You probably noticed the `DDir.DATA` is an alias for `DField.DATA_DIR`. This not only provides an alias, but also helps to clarify the code itself. For example

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

I have a `DJob.OP` constant definition, but it's an alias `OP : str = DField.OP`. Using `DJob.OP` in the dictionary just seemed wrong.

---

# Conclusion

- **Use group constants** and don't do what I did, do it from the start!!! 
- **Don’t wait**: define your constants strategy at the start of a project.  
- **Naming matters**: adopt a convention early and stick to it.  
- **Group wisely**: domain-specific constant groups scale much better than flat lists.  
- **Aliases help clarity**: sometimes the same constant has different meanings in different contexts.  
- **Refactoring pays off**: even if it’s painful, investing in structure makes future coding faster and less error-prone.  
