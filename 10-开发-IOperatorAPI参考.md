# IOperator屏幕操作API参考

ioperator是屏幕操作模块的抽象基类，提供图像识别、OCR文字识别、鼠标点击、按键操作等自动化控制功能。

## 类概述

```python
class IOperator(ABC)
```

IOperator是抽象基类，定义屏幕自动化操作的标准接口。开发者需实现其抽象方法以适配不同窗口平台。

### 初始化参数

| 参数 | 类型 | 说明 |
|---|---|---|
| stop_event | threading.Event or None | 停止线程事件，用于中断操作 |

### 类属性

| 属性 | 类型 | 说明 |
|---|---|---|
| ocr_engine | RapidOCR or None | OCR引擎单例实例 |

## IOperator属性列表

| 属性名 | 类型 | 说明 |
|---|---|---|
| type | str | 操作器类型，默认"Local" |
| tm_confidence | float | 模板匹配置信度阈值 |
| ocr_confidence | float | OCR识别置信度阈值 |
| top | int | 窗口顶部坐标 |
| left | int | 窗口左侧坐标 |
| width | int | 窗口宽度 |
| height | int | 窗口高度 |
| is_developer_mode | bool | 是否启用开发者模式 |
| is_save_ocr_image | bool | 是否保存OCR图像 |
| stop_event | threading.Event or None | 停止事件 |

## IOperator抽象方法

### is_window_active() -> bool

检查目标窗口是否为当前活动窗口。

### screenshot() -> Image

截取屏幕截图。

**参数：**

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| from_x | float or None | None | 起始点X坐标比例(0-1) |
| from_y | float or None | None | 起始点Y坐标比例(0-1) |
| to_x | float or None | None | 结束点X坐标比例(0-1) |
| to_y | float or None | None | 结束点Y坐标比例(0-1) |

### click_point() -> bool

点击指定坐标位置。

**参数：**

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| x | int or float | - | X坐标 |
| y | int or float | - | Y坐标 |
| x_offset | int or float | 0 | X偏移量 |
| y_offset | int or float | 0 | Y偏移量 |
| after_sleep | float | 0 | 点击后等待时间(秒) |
| tag | str | "" | 日志标记 |
| trace | bool | False | 是否打印日志 |

### press_key() -> bool

按下按键。

**参数：**

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| key | str | - | 按键名称 |
| presses | int | 1 | 按下次数 |
| interval | float | 0 | 按键间隔时间(秒) |
| wait | float | 0 | 首次按下前等待时间(秒) |
| trace | bool | True | 是否打印调试信息 |

### hold_key() -> bool

按住按键一段时间。

**参数：**

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| key | str | - | 按键名称 |
| duration | float | 0 | 按住时间(秒) |

### copy() -> None

将文本复制到剪贴板。

### paste() -> None

从剪贴板粘贴文本。

### move_rel() -> bool

相对当前位置移动光标。

**参数：**

| 参数 | 类型 | 说明 |
|---|---|---|
| x_offset | int | X轴偏移量 |
| y_offset | int | Y轴偏移量 |

### move_to() -> bool

将鼠标移动到指定位置。

**参数：**

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| x | int or float | - | X坐标 |
| y | int or float | - | Y坐标 |
| duration | float | 0.0 | 移动持续时间(秒) |
| trace | bool | True | 是否打印调试信息 |

### mouse_down() -> bool

按下鼠标按钮。

**参数：**

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| x | int or float | - | X坐标 |
| y | int or float | - | Y坐标 |
| trace | bool | True | 是否打印调试信息 |

### mouse_up() -> bool

释放鼠标按钮。

**参数：**

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| x | int or float or None | None | X坐标 |
| y | int or float or None | None | Y坐标 |
| trace | bool | True | 是否打印调试信息 |

### scroll() -> bool

滚动鼠标滚轮。参数：distance(int)，滚动距离。

## IOperator图像识别方法

### locate() -> Box or None

在窗口内查找模板图片位置。

```python
def locate(self, template: str, *, from_x=None, from_y=None, to_x=None, to_y=None, confidence=None, trace=True) -> Box | None
```

**参数：**

| 参数 | 类型 | 默认值 | 说明 |
|---|---|---|---|
| template | str | - | 模板图片路径 |
| from_x | float or None | None | 起始点X坐标比例(0-1) |
| from_y | float or None | None | 起始点Y坐标比例(0-1) |
| to_x | float or None | None | 结束点X坐标比例(0-1) |
| to_y | float or None | None | 结束点Y坐标比例(0-1) |
| confidence | float or None | None | 匹配置信度阈值(0-1) |
| trace | bool | True | 是否打印调试信息 |

返回：找到的Box对象或None。

### locate_all() -> list[Box] or None

查找所有匹配的图片位置。参数同locate，返回所有匹配的Box列表或None。

### locate_any() -> tuple[int, Box | None]

查找任意一张图片位置。

```python
def locate_any(self, templates: list[str], *, from_x=None, from_y=None, to_x=None, to_y=None, confidence=None, trace=True) -> tuple[int, Box | None]
```

**参数：** templates为模板图片路径列表，其余同locate。

返回：(图片索引, Box对象)，未找到返回(-1, None)。

## IOperator OCR识别方法

### ocr() -> list[Any] or None

执行OCR文字识别。可指定识别区域(from_x, from_y, to_x, to_y)，返回OCR引擎原始结果列表或None。

### ocr_match() -> Box or None

OCR识别并匹配指定文本，返回文本位置。

```python
def ocr_match(self, text: str, confidence=None, *, from_x=None, from_y=None, to_x=None, to_y=None, trace=True) -> Box | None
```

**参数：** text为要识别的文本，confidence为识别置信度（默认使用ocr_confidence），其余同locate。

### ocr_match_any() -> tuple[int, Box | None]

OCR识别并匹配任意指定文本。

```python
def ocr_match_any(self, texts: list[str], confidence=None, *, from_x=None, from_y=None, to_x=None, to_y=None, trace=True) -> tuple[int, Box | None]
```

返回：(文本索引, Box对象)，未找到返回(-1, None)。

## IOperator等待方法

### wait_img() -> Box or None

等待模板图片出现。参数：template(路径)，timeout(超时秒数，默认10)，interval(检查间隔秒数，默认0.5)。返回找到的Box或None。

### wait_any_img() -> tuple[int, Box | None]

等待任意一张图片出现。参数：templates(路径列表)，timeout(默认10)，interval(默认0.5)。返回(索引, Box)或(-1, None)。

### wait_ocr() -> Box or None

等待OCR识别到指定文本。参数：text，confidence，interval(默认0.2)，timeout(默认10)。

### wait_ocr_any() -> tuple[int, Box | None]

等待OCR识别到任意指定文本。参数同wait_ocr但texts为列表。

### wait_any() -> tuple[int, Box | None] (静态方法)

等待任意条件满足。参数：conditions(条件函数列表)，timeout(默认10)，interval(默认0.5)。

## IOperator点击与输入方法

### click_box() -> bool

点击Box区域中心。参数：box(Box对象)，x_offset(默认0)，y_offset(默认0)，after_sleep(默认0)。

### click_img() -> bool

查找图片并点击其中心位置。参数：template(模板路径)，x_offset，y_offset，after_sleep。

## IOperator鼠标操作方法

### drag_to() -> bool

拖动鼠标到指定位置。参数：from_x，from_y，to_x，to_y，duration(默认0.5)，trace(默认True)。

## IOperator工具方法

### sleep() -> None (静态方法)

线程休眠。参数：seconds(float)。

### do_while() -> bool (静态方法)

在满足条件时重复执行操作。参数：action(操作函数)，condition(条件函数)，interval(默认0.1)，max_iterations(默认50)。因不再满足条件退出返回True，达到最大迭代次数返回False。
