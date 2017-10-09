如果你想彻底掌握整个框架的化，有一个非常重要的工具你一定不能错过，有了它你就可以学习并理解整个框架，他就是 _serpent_ 命令。通过合理的调用 _serpent_ 命令就可以灵活使用各种框架内的主要特征功能。

现在就来试试吧，运行命令 `serpent`。


![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/executable1.png)

你将会看到如上图所示的输出，下面让我们来依次介绍一下这些命令。


## Serpent 命令

### setup（安装）

执行框架的初始化安装步骤，创建配置文件，文件目录，并根据指定操作系统安装依赖。

#### 使用方法

##### 初始化首次安装

`serpent setup`

_提示: 其实以后你也可以再次调用该命令，注意再次调用该命令会导致所有插件失效，同时也会初始化覆盖原有的配置文件，以及清空全部数据文件。在进行操作之前系统会再次提醒你确认是否执行。_

### grab_frames （获取帧）

启动一个Frame Grabber（帧捕捉）实例。**请不要直接使用**

#### 使用方法

##### 启动一个Frame Grabber（帧捕捉）实例。

`serpent grab_frames <width> <height> <x_offset> <y_offset>`

* _width_: 游戏内待捕捉帧的像素宽度
* _height_: 游戏内待捕捉帧的像素高度
* _x\_offset_: 游戏内待捕捉帧图像的像素横向偏移值
* _y\_offset_: 游戏内待捕捉帧图像的像素纵向偏移值

_提示: 当游戏代理启动时，该命令会被Game object（游戏体）类在后台调用。_

### activate（激活）

激活一个插件，使其在Serpent.AI代码内可见并可被访问到。

#### 使用方法

##### 激活一个插件

`serpent activate <plugin_name>`

* _plugin\_name_: 你插件文件路径下的插件名

### deactivate（停用）

停用一个插件，使其在 Serpent.AI 代码内不可见且不可被访问。

#### 使用方法

##### 停用一个插件

`serpent deactivate <plugin_name>`

* _plugin\_name_: 你插件文件路径下的插件名

### plugins（插件）

列出所有已激活及未激活的插件。

#### 使用方法

##### 列出所有插件

`serpent plugins`

### launch（启动）

使用恰当的活跃的 Serpent.AI 插件启动游戏

#### 使用方法

##### 启动游戏

`serpent launch <game_name>`

* _game\_name_: 待启动游戏的名称 (举例来说，CoolGame)。

### play（玩）

通过适当的 Serpent.AI 插件与指定的游戏代理玩游戏。游戏必须已经使用 `serpent launch` 命令启动。


#### 使用方法

##### 用制定的游戏代理插件来玩游戏。

`serpent play <game_name> <game_agent_plugin_name> <frame_handler>`

* _game\_name_: 待启动游戏的名称 (举例来说，CoolGame)。
* _game\_agent\_plugin\_name_: 游戏代理插件的全称。(举例来说， SerpentCoolGameGameAgent)
* _frame\_handler_: 帧处理程序标签，可选。默认将依靠插件配置。

### generate（生成）

为游戏插件及游戏代理插件生成 Serpent.AI 插件代码。

#### 使用方法

##### 生成一个游戏插件

`serpent generate game`

##### 生成一个游戏代理插件

`serpent generate game_agent`

### train（训练）

训练基于 Serpent.AI 预设的机器学习模型。

#### 使用方法

##### 训练一个上下文分类器

`serpent train context <epochs>`

* _epochs_: 训练的轮数，可选，默认为3。

_需要使用`serpent capture`先捕获上下文帧。_

### capture（捕捉）

捕捉游戏图像帧和屏幕区域图像。游戏需要首先通过`serpent launch`命令保证已启动。

#### 使用方法

##### 捕捉完整的游戏图像帧

`serpent capture frame <game_name> <interval>`

* _game\_name_: 待启动游戏的名称 (举例来说，CoolGame)。
* _interval_: 图像帧捕获的时间间隔（秒），默认为1。

_所捕获的游戏帧图像将会存储在 'datasets/collect\_frames' 路径下_

##### 捕捉完整的带上下文标签的游戏图像帧，

`serpent capture context <game_name> <interval> <context_label>`

* _game\_name_: 待启动游戏的名称 (举例来说，CoolGame)。
* _interval_: 图像帧捕获的时间间隔（秒），默认为1。
* _context\_label_*: 应用于捕获图像的标签。

_所捕获的游戏帧图像将会存储在 'datasets/collect\_frames\_for\_context/<context\_label>' 路径下_

##### 捕获预定义的游戏帧区域

`serpent capture region <game_name> <interval> <screen_region>`

* _game\_name_: 待启动游戏的名称 (举例来说，CoolGame)。
* _interval_: 图像帧捕获的时间间隔（秒），默认为1。
* _screen\_region_*: 在游戏插件内预定义好的屏幕区域。

_所捕获的游戏帧图像将会存储在  'datasets/collect\_frames/<screen\_region>' 路径下_

### visual_debugger（可视化调试器）

启动可视化调试器。

#### 使用方法

##### 使用默认配置启动可视化调试器

`serpent visual_debugger`

##### 使用自定义配置启动可视化调试器

`serpent visual_debugger custom_1 custom_2`

### window_name（窗口名称）

启动一个 CLI 实例以帮助确定游戏插件中窗口名称的正确变量名。主要用于 MacOS 平台，因为在 Linux 和 Windows 系统上窗口标题可以直接使用。

#### 使用方法

##### 启动实例

`serpent window_name`