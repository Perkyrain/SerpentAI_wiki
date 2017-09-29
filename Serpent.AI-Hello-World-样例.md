_注意: 在开始阅读此指南之前，请确认你已经针对你的操作系统完成了全部的安装指南步骤，包括运行 'serpent setup'。_

在本节中，我们将会创建一个初始的、最小的 SerpentAI 项目: 一个 Hello World 样例。样例将会使用 Steam 版本的 [Super Hexagon](http://store.steampowered.com/app/221640/Super_Hexagon/) 游戏作为演示，当然如果你没有这个游戏的话你也可以换其他的。

## 游戏准备

如果你打算为一个新的游戏开发代理插件的化，无论什么时候你都需要做一些准备工作:

1. 购买、下载、安装游戏
2. 运行游戏
3. 调整视频选项:
    * 运行模式选窗口模式
    * 选一个适合你的分辨率 (如果可选的话)
4. 记下窗口的识别名称:
    * **在 Linux 或者 Windows平台**: 游戏的窗口名。
    * **在 macOS 平台**: 游戏的进程名。由于进程名并不是一眼就能看到，而且获取起来还有点麻烦，所以我们提供了一个功能来解决这个问题。在你的项目目录中运行命令 `serpent window_name` 然后跟着指示做就行了。

## 创建一个游戏插件

在 Serpent.AI 中，想要为一个新游戏提供支持的话，你必须为它创建一个插件。_serpent_ 程序为创建插件任务提供了一个简洁、完美的方案

运行 `serpent generate game`

你将会在命令行中看到类似下面的界面:

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/hello1.png)

依次填入关键词:

* SuperHexagon(游戏名)
* steam

你会看到一些插件安装过程中的输出，它们会以这句结尾:
 _SerpentSuperHexagonGamePlugin was installed successfully!_

如果你去插件的文件路径下看一看，你会发现我们已经为你生成了为 Super Hexagon 游戏量身定做的插件模版文件，像这样

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/hello2.png)

## 配置游戏插件

为了通过 Serpent.AI 启动 Super Hexagon 游戏，你需要在生成的游戏插件文件中进行几处小修改。

编辑 _plugins/SerpentSuperHexagonGamePlugin/files/serpent_SuperHexagon_game.py_。结构体应该看起来像下面这样:

```python
    def __init__(self, **kwargs):
        kwargs["platform"] = "steam"

        kwargs["window_name"] = "WINDOW_NAME"

        kwargs["app_id"] = "APP_ID"
        kwargs["app_args"] = None


        super().__init__(**kwargs)

        self.api_class = SuperHexagonAPI
        self.api_instance = None
```

* 用你之前记下的窗口名替换掉 _WINDOW\_NAME_ 字段。在这个简单的例子里，请使用 _Super Hexagon_ 替换掉 _WINDOW_NAME_。
* 用 Steam 的 游戏编号 (APPID) 取代 _APP\_ID_ 字段。在这个例子里，请使用  _221640_ 替换掉 APP_ID。

小提示: 一你可以在[SteamDB](https://steamdb.info/search/?a=app)很快速的找到 Steam 游戏的游戏编号。

### 其他游戏

如果你在用其他 *可执行游戏* 来开发这个 Hello World 样例的话，举街霸X来说:


```python
   def __init__(self, **kwargs):

        kwargs["platform"] = "executable"

        kwargs["window_name"] = "WINDOW_NAME"

        kwargs["executable_path"] = "EXECUTABLE_PATH"

        super().__init__(**kwargs)

        self.api_class = StreetFighterXMegaManAPI
        self.api_instance = None
```

* 用你之前记下的窗口名替换掉 _WINDOW\_NAME_ 字段。
* 用这个游戏的环境变量中的路径或该游戏的绝对路径取代 _EXECUTABLE\_PATH_ 字段。

## 启动游戏

运行 `serpent launch SuperHexagon`

游戏将会开始运行。如果你的游戏被移动到了屏幕左上角的话，这说明 Serpent.AI 可以定位到这个游戏窗口。

让游戏在那里运行着不要管它。

## 创建游戏代理插件

在 Serpent.AI 中，如果你想从你的游戏中获取画面帧并与之交互的话，首先必须创建一个游戏代理插件。_serpent_ 程序也为创建代理插件任务提供了一个高效、快速的解决方案


运行 `serpent generate game_agent`

你将会在命令行中看到类似下面的界面:

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/hello3.png)

填入游戏名称关键词:

* SuperHexagon

你会看到一些插件安装过程中的输出，它们会以这句结尾: _SerpentSuperHexagonGameAgentPlugin was installed successfully!_

如果你去插件的文件路径下看一看，你会发现我们已经为你生成了为 Super Hexagon 游戏量身定做的代理插件模版文件，像这样

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/hello4.png)

## Hello World 你好，世界！

编辑 _plugins/SerpentSuperHexagonGameAgentPlugin/files/serpent_SuperHexagon_game_agent.py_.

更改 _handle\_play_ 函数内容，使之看起来像这样:

```python
    def handle_play(self, game_frame):
        print("Hello World!")
```

运行 `serpent play SuperHexagon SerpentSuperHexagonGameAgent`

一切顺利的话，你会在你的终端中开始看到一连串输出的 "Hello World!" 字样。


## 更加可视化的方式

在刚才的样例中，每次你的终端中出现一个新的 "Hello World!" 字段，你的游戏代理程序都获取到了来自游戏的最新的一帧图像。你有可能已经发现了这个流传输并不是特别的快，这是因为程序设置了一个限制器，它会限制每秒钟代理程序能获取和处理图像帧的数量。这个参数是可以自行定义的 ( 查看 _config/config.plugins.yml_ )，我们将默认参数设置为 `2 FPS` 是有原因的，这就意味着相当于 120 APM ( 每分钟操作次数 )的操作手速了，这已经快到足够应对绝大多数市面上的游戏了。除非你在开发纯粹处理模拟控制的游戏 ( 举例来说，赛车游戏 )，否则以一个极高的 APM 去处理普通的游戏的话，游戏操作本身会变得 _不那么自然_ ，而且很多情况下你的代理模型会反应过激。

我们先就此打住，让我们在输出 "Hello World!" 的基础上更直观可视化一些。Serpent.AI 中内嵌了一个名为可视化调试器的应用，它可以在代理运行的同时让你可以方便的检测图像信息。

打开另一个终端，运行 `serpent visual_debugger`

你应该会看到一个类似这样的界面:

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/hello5.png)

如果你需要的话，调整大小然后放在游戏界面的下面。

现在把 _handle\_play_ 改为如下这样:

```python
    def handle_play(self, game_frame):
        print("Hello World!")

        for i, game_frame in enumerate(self.game_frame_buffer.frames):
            self.visual_debugger.store_image_data(
                game_frame.frame,
                game_frame.frame.shape,
                str(i)
            )
```

进入 Super Hexagon 游戏的难度选择界面。

运行 `serpent play SuperHexagon SerpentSuperHexagonGameAgent`

这样你应该就已经能在可视化调试器中看到循环显示的逐帧画面了，差不多跟终端中显示新的 "Hello World!" 字段的节奏一致。

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/hello6.png)

游戏代理会自动维护一些从游戏本身获取的图像帧，在本例中代理程序可以把这些图像帧传递给可视化调试器。


## 更有交互性的尝试

作为本 Serpent.AI 样例的最后一步，让我们来添加一些键盘交互吧。

按如下方式修改 _handle\_play_ 函数:

```python
    def handle_play(self, game_frame):
        print("Hello World!")

        for i, game_frame in enumerate(self.game_frame_buffer.frames):
            self.visual_debugger.store_image_data(
                game_frame.frame,
                game_frame.frame.shape,
                str(i)
            )

        self.input_controller.tap_key("right")
```

你能猜到会发生什么吗？

自己动手试试吧！运行 `serpent play SuperHexagon SerpentSuperHexagonGameAgent`

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/hello7.gif)

#### 以上就是 Serpent.AI Hello World 样例的全部内容了，这对于我们能实现的全部功能来说只是沧海一粟，希望这个例子能引起你足够的兴趣继续探索研究下去！