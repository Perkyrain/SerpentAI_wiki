_InputController_ 类旨在成为在 Serpent.AI 框架中传输游戏输入的介质。

## InputController 实例里有什么？

### 功能

* 提供简明基本的键盘输入接口
* 提供简明基本的鼠标输入接口

### 概念

#### 后端

Serpent.AI 提供了与输入控制器一起使用的不同的后端接口选项。在 InputControllers 枚举类型中定义了它们:

* `InputControllers.PYAUTOGUI`: 利用 [PyAutoGUI](https://github.com/asweigart/pyautogui) Python库的后端。Linux & macOS 操作系统的默认配置。 
* `InputControllers.NATIVE_WIN32`: 利用 Windows 的 SendInput DLL 的方法。Windows 操作系统的默认配置。

如果要重写平台的默认后端，你需要在游戏插件中添加一个条目:

```python
class SerpentNuclearThroneGame(Game, metaclass=Singleton):

    def __init__(self, **kwargs):
        kwargs["platform"] = "steam"

        kwargs["input_controller"] = InputControllers.PYAUTOGUI  # <= Specify your InputController backend here 

        kwargs["window_name"] = "Nuclear Throne"

        kwargs["app_id"] = "242680"
        kwargs["app_args"] = None

        super().__init__(**kwargs)

        self.api_class = NuclearThroneAPI
        self.api_instance = None
```

#### 聚焦游戏窗口

在调度输入行为之前，InputController 实例将始终确保游戏仍然将焦点固定在游戏上，以防止意外输入到其他应用程序。如果游戏失去焦点，则既定的输入将被取消。

#### 鼠标按钮枚举类型

每当需要一个鼠标按钮操作要作为函数中的输入参数时，你都应该传递一个属于 MouseButton 枚举类型的项:

* `MouseButton.LEFT`: 鼠标左键
* `MouseButton.MIDDLE`: 鼠标中键
* `MouseButton.RIGHT`: 鼠标右键

如果你想使用鼠标输入，请确保先导入对应的枚举类型:

`from serpent.input_controller import MouseButton`

#### 键盘枚举类型

每当需要一个键盘操作要作为函数中的输入参数时，你都应该传递一个属于 KeyboardKey 枚举类型的项:

[KeyboardKey enum](https://github.com/SerpentAI/SerpentAI/blob/dev/serpent/input_controller.py#L10)

如果你想使用键盘输入，请确保先导入对应的枚举类型:

`from serpent.input_controller import KeyboardKey`

### 键盘操作

### `self.handle_keys`

**方法签名** *handle_keys(self, key_collection)*

比较一下 `set(key_collection)` 和 `self.previous_key_collection_set`。 释放不再需要按下的键，然后再按新的键。或者是按住其他的键。利用这种方式可以让输入更像人类。

* **key_collection**: 应该按下的有效的键盘键值列表。

### `self.tap_keys`

**方法签名** *tap_keys(self, keys, duration=0.05)*

按下 *keys* 按键。持续 *duration* 时间。松开 *keys* 按键。

* **keys**: 应当点击的有效键盘键值的列表。
* **duration**: 按住键的持续时间（单位为秒）。

### `self.tap_key`

**方法签名** *tap_key(self, key, duration=0.05)*

按下 *key* 按键。持续 *duration* 时间。松开 *keys* 按键。

* **key**: 应该点击的有效的键盘键值。
* **duration**: 按住键的持续时间（单位为秒）。

### `self.press_keys`

**方法签名** *press_keys(self, keys)*

按下 *keys* 们。

* **keys**: 应当点击的有效键盘键值的列表。

### `self.press_key`

**方法签名** *press_key(self, key)*

按下 *key*。

* **key**: 应该点击的有效的键盘键值。

### `self.release_keys`

**方法签名** *release_keys(self, keys)*

松开 *keys* 们。

* **keys**: 应该释放的有效键盘键值的列表。

### `self.release_key`

**方法签名** *release_key(self, key)*

松开 *key*.

* **key**: 应该释放的有效的键盘键值。

### `self.type_string`

**方法签名** *type_string(self, string, duration=0.05)*

键入 *string* 字符串，键入的时间间隔为 *duration* 。

* **string**: 待输入的字符串。
* **duration**: 按下下一个字符之前等待的时间间隔（单位为秒）。

## 鼠标操作

### `self.move`

**方法签名** *move(self, x=None, y=None, duration=0.25, absolute=True)*

在持续时长 *duration* 内将鼠标指针移动到 (*x*, *y*) 位置。如果 *absolute* 值为真的话，（*x*, *y*)指代的是从显示器左上角开始的像素坐标，否则它们被视为与当前鼠标光标位置的偏移量。

* **x**: X 像素坐标。
* **y**: Y 像素坐标。
* **duration**: 鼠标移动到 (*x*, *y*) 位置花费的时间 (单位为秒)。
* **absolute**: 使用绝对坐标还是相对坐标。

### `self.click_down`

**方法签名** *click_down(self, button=MouseButton.LEFT)*

按下对应 *button* 按钮的鼠标按键。

* **button**: MouseButton 枚举的一个子项。

### `self.click_up`

**方法签名** *click_up(self, button=MouseButton.LEFT)*

松开对应 *button* 按钮的鼠标按键。

* **button**: MouseButton 枚举的一个子项。

### `self.click`

**方法签名** *click(self, button=MouseButton.LEFT, duration=0.25)*

按下对应 *button* 按钮的鼠标按键。按下和松开的时间间隔由 *duration* 变量定义。

* **button**: MouseButton 枚举的一个子项。
* **duration**: 按下和松开该键的持续时间（单位为秒）。

### `self.click_screen_region`

**方法签名** *click_screen_region(self, button=MouseButton.LEFT, screen_region=None)*

将鼠标光标移动到 *screen_region* 屏幕区域的中心，然后单击 *button* 按钮。

* **button**: MouseButton 枚举的一个子项。
* **screen_region**: 在 InputController 的 Game 实例中定义的有效屏幕区域的标签。

### `self.click_sprite`

**方法签名** *click_sprite(self, button=MouseButton.LEFT, sprite=None, game_frame=None)*

尝试在 *game_frame* 游戏帧中定位 *sprite* 精灵。如果找到的话，用 *button* 按钮点击位置的中心。

* **button**: MouseButton 枚举的一个子项。
* **sprite**: 一个 Sprite 精灵实例。
* **game_frame**: 一个 GameFrame 游戏图像帧实例。

### `self.click_string`

**方法签名** *click_string(self, query_string, button=MouseButton.LEFT, game_frame=None, fuzziness=2, ocr_preset=None)*

尝试使用预设置方案为 *ocr_preset* 的 OCR 方法在 *game_frame* 游戏帧中定位 *query_string* 目标字符串。如果发现了一个在可接受的 *fuzziness* 模糊程度内的目标，则用 *button* 按钮点击位置的中心。

* **query_string**: 在游戏帧中待匹配的字符串。
* **button**: MouseButton 枚举的一个子项。
* **game_frame**: 一个 GameFrame 游戏图像帧实例。
* **fuzziness**: 与待查询字符串的最大 Damerau–Levenshtein [编辑距离](https://zh.wikipedia.org/wiki/編輯距離)。
* **ocr_preset**: 在 InputController 的 Game 实例中定义的有效 OCR 预设的标签。

### `self.drag`

**方法签名** *drag(self, button=MouseButton.LEFT, x0=None, y0=None, x1=None, y1=None, duration=0.25)*

使用 *button* 按钮以 *duration* 时间间隔将目标从 (*x0*, *y0*) 到 (*x1*, *y1*)。

* **button**: MouseButton 枚举的一个子项。
* **x0**: 初始 X 像素坐标。
* **y0**: 初始 Y 像素坐标。
* **x1**: 目标 X 像素坐标。
* **y1**: 目标 Y 像素坐标。
* **duration**: 从初始位置到目标像素坐标所用的时间（单位为秒）。

### `self.drag_screen_region_to_screen_region`

**方法签名** *drag_screen_region_to_screen_region(self, button=MouseButton.LEFT, start_screen_region=None, end_screen_region=None, duration=0.25)*

使用 *button* 按钮以 *duration* 时间间隔将目标从起始屏幕区域 *start_screen_region* 的中心拖动到目标屏幕区域  *end_screen_region* 的中心。

* **button**: MouseButton 枚举的一个子项。
* **start_screen_region**: 在 InputController 的 Game 实例中定义的有效屏幕区域的标签。
* **end_screen_region**: 在 InputController 的 Game 实例中定义的有效屏幕区域的标签。
* **duration**: 从初始屏幕区域到目标屏幕区域所用的时间（单位为秒）。

### `self.scroll`

**方法签名** *scroll(self, clicks=1, direction="DOWN")*

向 *direction* 方向滚动 *clicks* 次数。

* **clicks**: 滚轮滚动的次数。
* **direction**: "UP" 上或者 "DOWN" 下之一。