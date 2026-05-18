# SRACore核心模块概述

本文档介绍SRA的核心模块结构。部分内容可能过时，以源码内注释为准。

## 目录结构

```
SRACore/
├── operators/
│   ├── ioperator.py      # 抽象操作器基类
│   ├── model.py          # 数据模型定义
│   ├── operator.py       # 操作器实现
│   └── browser_operator.py  # 浏览器操作器
└── task/
    └── __init__.py       # 任务框架
```

## 导入方式

```python
from SRACore.operators.ioperator import IOperator
from SRACore.operators.model import Box, Region
from SRACore.task import BaseTask, task, get_tasks
```

## 模块列表

| 模块名 | 说明 |
|---|---|
| operators.ioperator | 屏幕操作模块抽象基类，定义标准接口 |
| operators.model | 数据模型定义，包含Box和Region数据类 |
| task | 任务框架，提供任务基类和注册机制 |
