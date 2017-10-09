_Game_ 插件旨在表示一个基础 _Game_ 类的针对特定游戏的扩展。

## 游戏插件包内有什么？

* 插件定义文件
* 游戏特定的 Game 子类
* 游戏特定的 GameAPI 子类
* 配套捆绑的游戏精灵

## Game 子类里有什么？

这个子类是精简设计的。大多数关键功能都由父类 _Game_ 类提供，该子类提供了如下功能:

* 在构造函数中，提供了 GameAPI 子类的`self.api_class`的导入和赋值
* 在调用`super().__init__()`之前，对`self.platform`, `self.window_name`等一系列关键变量的赋值
* 一个包含感兴趣区域的所有坐标的映射的属性，例子如下:
```python
    @property
    def screen_regions(self):
        regions = {
            "MAIN_MENU_NEW_GAME": (626, 348, 708, 428),
            "MAIN_MENU_LOAD_GAME": (626, 429, 708, 510),
            "MAIN_MENU_MULTIPLAYER": (626, 511, 708, 592),
            "MAIN_MENU_GAME_TOOLS": (626, 593, 708, 674),
            "MAIN_MENU_QUIT": (704, 983, 768, 1023),
            "MAIN_MENU_SCENARIO_TAB_RCT": (234, 148, 267, 239),
            "MAIN_MENU_SCENARIO_TAB_RCT_CF": (234, 239, 267, 330),
            "MAIN_MENU_SCENARIO_TAB_RCT_LL": (234, 330, 267, 421),
            "MAIN_MENU_SCENARIO_TAB_RCT2": (234, 421, 267, 512),
            "MAIN_MENU_SCENARIO_TAB_RCT2_WW": (234, 512, 267, 603),
            "MAIN_MENU_SCENARIO_TAB_RCT2_TT": (234, 603, 267, 694),
            "MAIN_MENU_SCENARIO_TAB_RCT_RP": (234, 694, 267, 785),
            "MAIN_MENU_SCENARIO_TAB_RCT_OP": (234, 785, 267, 876),
            "GAME_PAUSE_BUTTON": (0, 0, 27, 30),
            "GAME_SPEED_BUTTON": (0, 30, 27, 60),
            "GAME_FLOPPY_BUTTON": (0, 60, 27, 90)
        }

        return regions
```


* 一个包含游戏中不同文本块类型的OCR预设信息的属性。例子如下:
```python
    @property
    def ocr_presets(self):
        presets = {
            "SCENARIO_TEXT": {
                "extract": {
                    "gradient_size": 3,
                    "closing_size": 10
                },
                "perform": {
                    "scale": 16,
                    "order": 3,
                    "horizontal_closing": 1,
                    "vertical_closing": 1
                }
            }
        }

        return presets
```

你随意实现更多的自定义的实例方法，他们将可以从游戏代理程序中被调用，因为它们持有对该子类的实例的引用。

如果游戏在启动过程中需要采取特殊的额外步骤，可以选择扩展`self.before_launch`以及`self.after_launch`回调。不要忘记`super()`调用。记住，回调函数是用来扩展的，不是用来被重写覆盖的！

## GameAPI Subclass 子类里有什么？

这是在 _Game_ 插件中大部分开发工作将发生的地方。 它是一个完全自定义的类，在这里你可以尽情编写可重用的强大功能，并用它们在游戏中进行操作或者处理游戏相关的数据。

文件路径为: *files/api/api.py*

### 命名空间

考虑到你开发的游戏 API有可能增长成庞然大物，我们建议利用命名空间的功能更好的组织整体代码。我们讨论过究竟是使用子模块还是嵌套类来提供这样的命名空间，最终我们选择了嵌套类。虽然它们不像子模块的实现那样漂亮优雅，没有那么pythonic，但嵌套类对于开发人员维护依赖性更小，也更容易解释说明。

通过使用 `serpent generate game` 生成 _Game_ 插件创建的 Game API 将提供如何命名您的功能的示例。


### 示例功能

* 执行UI操作来开始一个新的游戏
* 执行UI操作来选择一个难度
* 从状态栏读取数值
* 定位游戏精灵

Your GameAPI functions can exist

## 游戏精灵是如何匹配的?

想要将精灵与你的插件正确的匹配捆绑的话，你需要将它们添加到*files/data/sprites*目录中。所有精灵都应该是PNG格式的，同时也支持Alpha通道和动画精灵。


### 命名规则

我们已经为游戏精灵建立了一个命名规则。虽然可以自定义，但我们建议使用该规则。因为在 _Game_ 类运行时，所有遵循该规则的精灵都会被自动发现、标记并注册。


`sprite_<label>_<animation_state_index>.png`

#### 样例

*sprite_npc_priest_0.png* 会转变为一个标签为 *SPRITE_NPC_PRIEST* 的 Sprite 对象

*sprite_npc_priest_1.png* 将为现有的Sprite对象填加另一帧图像数据，标签为 *SPRITE_NPC_PRIEST*