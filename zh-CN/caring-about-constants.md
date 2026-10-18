---
layout: posts
title: "认真对待常量"
date: 2025-09-11
category: journal
lang: zh-CN
locale: zh-CN
language_name: 简体中文
translation_key: caring-about-constants
permalink: /zh-CN/journal/caring-about-constants/
tags:
  - Constants
  - Python
  - Db4E
---

这篇文章谈的是如何在代码中使用常量：为什么重要、有什么好处、怎样才能用得好。遗憾的是，我是吃了苦头才学会的。最初，我在代码里并没有使用多少常量。

我不怎么重视常量。和许多项目一样，我大量使用“魔法字符串”。写起来又快又方便，到处都是。

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

所谓*魔法字符串*，就是直接硬编码在应用源代码里，并对程序行为或逻辑有重要影响的字符串字面量。“魔法”意味着阅读代码的人不一定立刻明白它的用途或含义，因为它的重要性没有被明确命名或说明。

---

# 第一阶段：魔法字符串

第一步是统一常量命名。我已经连续重构约八个小时，开始为常量该怎么命名而头疼。盯着屏幕、起了一堆糟糕名字、低声抱怨了一阵后，我终于做了从一开始就应该做的事：查资料。

## 标准命名

常量采用一致的命名标准非常重要。我使用下面这套规范，效果很好。常量分为三类：

类型 | 示例名称 | 示例值
-----|----------|-------
字段 | P2POOL_VER_FIELD | p2pool_ver
标签 | P2POOL_VER_LABEL | "P2Pool version"
默认值 | P2POOL_VER_DEFAULT | "4.10.1"

最终，我意识到魔法字符串确实是个*坏东西*，于是迎来了清算日：整整一天再加六小时，总计 30 小时的重构马拉松。目标很简单：清除魔法字符串，全部替换为常量。过程非常艰苦。

快结束时，我的手腕开始疼，甚至担心自己是不是要患上*腕管综合征*。后来才发现是因为常量都是大写字母，我一直不停按 Shift 键。改用 Caps Lock 之后，情况好多了。

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

# 魔法字符串的问题

## 容易出错

错误可能悄悄混进来，产生难以追踪的副作用。比如这里写 `p2pool_ver`，另一个地方却写 `p2pool_version`。程序不会报运行时错误，只会以奇怪的方式失败。

## 不好看

我用 **Visual Studio Code** 作为 IDE，它有语法高亮。使用魔法字符串时，你根本没有充分发挥 VS Code 的优势。

---

# 第二阶段：智能分组

时间过去了。我慢慢意识到，把常量堆成一大堆不是终点。它们散落各处，很难查找，应用代码里依然到处直接引用，感觉并不整洁。我不过是把*魔法字符串*换成了*魔法常量*。

虽然有进步，但……

于是我开始另一次重构。这次不是为了消灭魔法字符串，而是把常量组织成有逻辑的分组。想法很简单：常量应该归属于自己的领域，应用代码几乎不应直接接触底层常量定义。

## 虚拟类支持

我使用下面的 `ConstGroup.py`。它允许在常量文件中指定数据类型，同时保持代码整洁易读。

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

## 特定领域的常量

以下是我目前使用的常量文件中的一些片段：

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

# 常量太多带来的问题

## 重复

如果只有*字段*、*标签*和*默认值*三个大桶，就很难避免重复。我把它们按字母顺序排列，但还是会忘记已经定义过某个常量，例如 `ver_p2pool` 和 `p2pool_ver`。

## 还是难看

代码里到处都是常量，真的**无处不在**。而且采用了 `_FIELD`、`_LABEL`、`_DEFAULT` 命名约定后，它们还特别长。`LOCAL_SOFTWARE_SYSTEM_FIELD` 已经够拗口，`MAX_BACKUPS_DEFAULT` 也好不到哪里去。显然还有什么没解决。

## 文件太多

我尝试按领域拆分常量，但反而更糟。每个领域都有 `Jobs_Fields.py`、`Jobs_Defaults.py`、`Jobs_Labels.py`，让重复定义更容易发生。

---

# 收获

使用常量的好处很快就显现出来。

## 自动补全

采用分组常量后，VS Code 会在输入时提供自动补全建议。区别真的很明显：需要打的字少了很多（*Tab 键*是我的好朋友），出错机会也进一步降低。

## 更美观

使用常量以后，代码在编辑器里就是*更好看*。

## 表达含义

不只是视觉上的改善，代码也更清晰。`DStatus.ERROR`、`DDef.MONEROD_VER`、`DDir.RUN` 一眼就能表达意思。

## 交叉引用和别名

新方案还支持交叉引用。你可能注意到 `DDir.DATA` 是 `DField.DATA_DIR` 的别名。这不只是缩短名称，也有助于让代码更清楚。例如：

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

我虽然定义了 `DJob.OP`，但它只是 `OP : str = DField.OP` 的别名。在这个字典里使用 `DJob.OP` 总觉得不合适。

---

# 结论

- **使用分组常量**，千万别像我一样，应该从一开始就这么做！！！
- **别等**：在项目开始时就确定常量策略。
- **命名重要**：尽早采用统一约定，并坚持遵循。
- **合理分组**：按领域组织的常量分组，比平铺式列表更能适应项目增长。
- **别名有助于清晰表达**：同一个值在不同上下文中可能有不同含义。
- **重构值得投入**：虽然过程痛苦，但良好的结构会让今后的开发更快、更不容易出错。
