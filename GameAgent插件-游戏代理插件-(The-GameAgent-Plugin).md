_GameAgent_ 插件旨在表示一个基础 _GameAgent_ 类的定制化的扩展。

## 游戏代理插件包内有什么？

* 插件定义文件
* GameAgent 子类
* 预打包的助理模块📦
* 预打包的机器学习模型📦

## GameAgent 子类里有什么？

GameAgent子类是你项目中最重要的文件。它包含了你的项目实现（图像帧处理程序），这些处理程序定义了用于处理目标游戏的逻辑。该子类提供了如下功能:

* 一个或者更多的帧处理器实现方法
* 零个或者更多的帧处理器初始化方法
* 构造函数中`self.frame_handlers`内帧处理程序的注册
* 构造函数中`self.frame_handler_setups`内帧处理程序初始设置的注册
* 构造函数中`self.analytics_client`内可选的 AnalyticsClient 实例

现在你可以自由实现更多的自定义实例方法了。如果文件的大小变得超出控制（这种情况真的会发生，游戏代理可以非常复杂！），建议使用提取功能来帮助构建模块。

## 如何注册帧处理程序？

1. 为你的帧处理程序的 GameAgent 子类添加一个实例方法
2. 在构造函数中的`self.frame_handlers`中添加字典条目，将名称映射到函数

### 样例 

```python
class MyGameAgent(GameAgent):
    def __init__(self, **kwargs):
        super().init(**kwargs)

        self.frame_handlers["MY_FRAME_HANDLER"] = self.my_frame_handler

    def my_frame_handler(self, game_frame):
        pass
```

## 如何注册帧处理程序初始设置？

除了在`self.frame_handler_setups`处不同，其他都跟帧处理程序的一样。他们需要与他们对应的帧处理程序伙伴使用相同的名称。

### 样例

```python
class MyGameAgent(GameAgent):
    def __init__(self, **kwargs):
        super().init(**kwargs)

        self.frame_handler_setups["MY_FRAME_HANDLER"]: self.my_frame_handler_setup

    def my_frame_handler_setup(self):
        pass
```

## 如何打包助理模块？

1. 在 *files/helpers* 路径内创建文件
2. 使用所需的功能和类填充它们
3. 将它们导入你的 GameAgent 子类。这些松散的文件还不能由插件系统进行管理，所以需要相对路径的导入: `from .helpers.<my_module> import *`

### 样例

在 *files/helpers/utils.py*_ 中定义了一个 *hello_world* 方法:

```python
def hello_world():
    print("Hello World!")
```

要在你的 GameAgent 子类中访问它的话，你需要添加此 import 语句:

```python
from .helpers.utils import hello_world
```

## 如何打包机器学习模型？

只需将它们复制到*files/ml_models*路径下就好。我们建议将这些文件重命名为以 _.model_ 为扩展名的文件。因为当为插件创建 Git 库时，使用 Git LFS 添加 _.model_ 文件后缀，这样就可以上传超过 100MB 的文件了。

## 如何载入机器学习模型？

只需在`self.machine_learning_models`中添加一个字典条目，将名称映射到`self.load_machine_learning_model`的输出就行了。

### 通用示例

要使机器学习模型可用于所有的帧处理程序，请将其加载到 GameAgent 的构造函数中：

```python
class MyGameAgent(GameAgent):
    def __init__(self, **kwargs):
        super().init(**kwargs)

        self.machine_learning_models["MY_MODEL"] = self.load_machine_learning_model(
            os.path.join(os.path.dirname(__file__), "ml_models/my_model.model")
        )
```

### 帧处理程序具体示例

为了将机器学习模型限制为仅在单帧处理程序的范围内，请将其加载到后者的初始设置函数中:

```python
class MyGameAgent(GameAgent):
    def __init__(self, **kwargs):
        super().init(**kwargs)

        self.frame_handlers["MY_FRAME_HANDLER"]: self.my_frame_handler
        self.frame_handler_setups["MY_FRAME_HANDLER"]: self.my_frame_handler_setup

    def my_frame_handler_setup(self):
        self.machine_learning_models["MY_MODEL"] = self.load_machine_learning_model(
            os.path.join(os.path.dirname(__file__), "ml_models/my_model.model")
        )

    def my_frame_handler(self, game_frame):
        pass
```