_GameAgent_ 类旨在呈现一个在 Serpent.AI 框架中与游戏交互的通用代理。

## GameAgent实例里有什么？

### 功能
* 注册并公开多个可查的机器学习模型
* 维护一个 SpriteIdentifier 实例，该实例知道所有游戏的已注册精灵
* 注册并公开多个图像帧处理程序
* 允许图像帧处理程序定义一个初始化方法
* 提供有关如何处理传入图像帧的说明
* 将所需的游戏输入命令中继到 InputController 实例
* 管理运行时在终端中显示的内容
* 在特定时间点触发分析事件


### 概念

#### 图像帧处理程序

帧处理程序是可以接收 GameFrame 实例作为输入的高级功能。你的游戏代理只会在运行时执行一个帧处理程序，但你可以创建和注册多个处理程序。其实可以将图像帧处理程序视为游戏代理的操作模式。例如，同一个游戏代理可以定义一个 _random_ 随机游戏处理程序，一个 _scripted_ 脚本游戏处理程序和一个 _machine learning_ 机器学习处理程序。他们都会从同一个游戏中接收图像帧，但会对它们产生不同的反应。帧处理程序通常最终会变得功能庞杂而且非常庞大，所以建议将它们分割成多个较小的功能。图像帧处理程序们需要在 GameAgent 的构造函数中进行注册，以便后者能够知道它们的存在。

#### 初始化图像帧处理程序

如果你的帧处理程序具有需要在接收到第一个游戏帧之前运行的代码，你可以定义一个初始化设置功能并将其注册到 GameAgent 的构造函数中。这样，您可以在为给定的帧处理程序提供个性化需求的同时，也保持着游戏代理程序构造函数的简洁性。


#### 接收 GameFrame 实例

通过调用 Game 实例的 _play_ 方法，指定的帧处理程序将进入调用的无限循环中。每当帧处理程序结束时，下一循环周期就会开始，此时最近的游戏帧将被传递给帧处理程序。

有一点很重要的事情需要说明一下，那就是框架不能保证帧与帧之间的时间延迟一致，因为这取决于一次帧处理程序的执行时间。如果对于你的游戏代理来说，保证帧之间的时间间隔一致性很重要的话，则需要确保每次帧处理程序的运行速度都高于 _Game_ 的 GameFrameLimiter 中配置的 FPS 值。只有这样才能确保在循环周期之间的时间间隔会保持一致。


### 相关信息

GameAgent（游戏代理）的 Frame Handler（帧处理器）是一张完全空白的画布，同时，我们还有一大堆功能强大的工具来辅助你，请开发者尽情使用。

* `self.game`: 游戏代理的目标游戏的游戏实例
* `self.game.api`: 指向目标游戏的 GameAPI 实例的接口
* `self.config`: 游戏代理的插件配置
* `self.machine_learning_models`: 包含已加载的机器学习模型的标签集合的字典
* `self.input_controller`: 用于调度所需游戏输入的 InputController 实例
* `self.visual_debugger`: 用于将图像数据发送到可视化调试器的 VisualDebugger 实例
* `self.game_frame_buffer`: 包含游戏代理所能看到的 GameFrame 实例的 GameFrameBuffer 实例
* `self.sprite_identifier`:预装了由游戏插件提供的精灵实例的 SpriteIdentifier 实例 
* `self.uuid`: 通用识别码（UUIDv4）每次游戏代理启动时都会有所不同
* `self.started_at`: 表示游戏代理启动时间戳的 Python *datetime* 对象

### 动作

### `self.load_machine_learning_model`

**方法签名** *load_machine_learning_model(self, file_path)*

尝试解压缩位于 *file_path* 路径的二进制文件，并返回结果。

* **file_path**: 压缩后的机器学习模型文件的位置。

*该方法不可以被插件重写*

### `self.update_game_frame`

**方法签名** *update_game_frame(self, game_frame)*

当GameAgent的框架处理程序正在处理特定的GameFrame实例时，会使用图像帧抓取器堆栈的顶部图像更新该GameFrame实例。实际上：系统会在不退出当前帧处理程序循环的情况下更新游戏帧。这对与于更复杂的UI操作很有用。

* **game_frame**: 一个游戏帧实例。

*该方法不可以被插件重写*
