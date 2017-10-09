在 Serpent.AI 框架中，_Game_ 类旨在表示通用的视频游戏。

## Game实例中都有什么？

### 功能

* 通过适当的 GameLauncher 类启动游戏
* 收集和存储游戏窗口提供的信息
* 启动/停止 FrameGrabber （帧获取）类的实例
* 将 GameFrame 实例循环提供给指定的 GameAgent 实例
* 限制 GameFrame 实例进入 GameAgent 实例的速度
* 为游戏插件实现的指定游戏的 API 提供调用接口
* 根据插件数据目录的命名规则注册并公开所有精灵
* 提供游戏帧内有关感兴趣区域的信息
* 提供有关 OCR 预设的信息


### 概念

#### 帧捕捉器

当在游戏实例中调用*play*命令时，FrameGrabber 实例在后台会作为独立的进程启动。帧捕捉器的作用是保持精确的捕获率（默认为30 FPS），并将图像数据存储在内存中。当主进程结束时，帧捕捉器会自动关闭。它在后台运行的原因非常显而易见：防止游戏代理程序的执行干扰图像帧的捕获。

#### 游戏帧限流器

当将 GameFrame 实例提供给游戏代理程序时，为了控制其不超过指定的FPS值，调度帧的速率被限制了。这个概念是在使用框架构建游戏代理时，根据实际经验总结后添加的。 这个限流器的主要原因是为了保证实际APM（每分钟的动作）的有效化，并使来自代理的动作间隔正常化。 虽然如果能让游戏代理以 1000 APM 玩游戏的话听起来很棒，但是这样玩游戏的效果会看起来非常不自然，并且会让游戏代理更加混乱。默认限制设置为 2 FPS，这个设定已经非常宽泛，默认为 120 APM。该阈值可以在 _config/config.plugins.yml_ 中进行更改。

#### API

游戏插件可以随意与自己的 GameAPI 子类相搭配，其中包含游戏相关的，可重用的，对任何使用它的游戏都有帮助的功能。框架原生提供的功能可以涵盖从底层应用函数到高级UI操作或图像处理例程的任何内容。

#### 精灵

游戏插件可以选择在 _files/data/sprites_ 目录中调用预先提取好的游戏精灵。
如果按照如下命名规范提供指定的图片文件的话(_sprite\_<sprite_name>\_<animation\_state\_index>.png_)，框架会自动识别并将这些精灵自动注册并实例化为Sprite对象，存储在“self.sprites”中。这对于接下来游戏代理中精灵的识别与定位很有用。


#### 屏幕区域

最基本的对于矩形感兴趣区域（ROI）的定义。被定义为插件中的硬编码属性，它们允许游戏代理方便地查看游戏框架的指定精确区域。

#### OCR 预设

定义OCR预置设置（基本预处理选项）的基本方法，并将其与UI文本元素的类别相关联。
被定义为插件中的硬编码属性，它们允许游戏代理方便地为给定的文本类别选择最佳的OCR设置。


### 相关信息

#### 游戏窗口

* `self.window_id`: 操作系统特定的检测到的游戏窗口的ID。(仅限 Linux 和 Windows 操作系统)
* `self.window_name`: 包含游戏的窗口的标题，由窗口控制器用于查找正确的窗口。
* `self.window_geometry`: 包含以下键的几何信息的字典: _width_, _height_, _x\_offset_, _y\_offset_

#### 游戏相关

* `self.api`: 由插件提供的 GameAPI 实例
* `self.sprites`: 已注册精灵实例的集合
* `self.screen_regions`: 含有名称及其对应边界坐标键值对的字典
* `self.ocr_presets`: 含有名称及其对应OCR设置键值对的字典

#### 标识

* `self.is_launched`: 游戏是否在运行
* `self.is_focused`: 游戏窗口是否已经聚焦

#### 杂项

* `self.kwargs`: 插件传递的额外参数

### 动作

### `self.launch`

**Method Signature** *launch(self, dry_run=False)*

调用 *before_launch* 回调，通过 GameLaunch 正确的启动游戏，并调用 *after_launch* 回调。

* **dry_run**: 当置为真时，仅仅调用回调函数但是不启动游戏，默认为假。

*该方法不可以被插件重写*

### `self.play`

**方法签名** *play(self, game_agent_class_name=None, frame_handler=None, **kwargs)*

尝试为 *game_agent_class_name* 找到一个激活的插件，并将其实例化。启动一个帧抓取器开始逐帧抓取图像。等待游戏图像的第一帧出现在帧抓取器的堆栈中。开始将游戏帧循环传递给匹配 *frame_handler* 的游戏代理的帧处理程序。

* **game_agent_class_name**: 应该实例化的 GameAgent 的类名。必须是一个已激活的插件。
* **frame_handler**: GameAgent 实例中注册的帧处理程序的名称，可选。来自 _config/config.plugins.yml_。
* **\*\*kwargs**: 一些额外的参数，在实例化时传递给 GameAgent。

*该方法不可以被插件重写*

### 回调

### `self.before_launch`

**方法签名** *before_launch(self)*

在游戏启动之前运行的代码块。

*该方法可以被插件重写*

### `self.after_launch`

**方法签名** *after_launch(self)*

在游戏启动后运行的代码块。该方法的基本功能实现是设置“self.is_launched”标志，等待游戏启动并定位游戏窗口并存储其几何信息。

*该方法可以被插件重写，但是必须调用 super().after_launch()*
