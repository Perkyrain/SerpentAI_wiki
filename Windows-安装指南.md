_提示: 这个教程还在不断完善中，如果按照此教程进行配置遇到任何问题，请[创建一个问题](https://github.com/SerpentAI/Serpent/issues/new)或者在 [Discord](https://discord.gg/9D5SuxH) 中发信息提问，或直接 @Serpent.AI，非常感谢。_

在 Windows 平台上部署 Serpent.AI 听起来是块硬骨头，可是后来我们发现并没有想象中那么难。与 Linux 版本相比你会丢失一些灵活性，但是与其放弃对 Windows 版本的支持(有很多很多项目都直接放弃了对windows的适配)，我们更愿意向那些在 Windows 系统上的开发者展示我们的诚意。如果你有任何建议，我们鼓励您参与本项目的建设。


## 初始要求

* Windows 10 (for WSL(Windows Subsystem for Linux))
* Visual C++ Build Tools 2015 来自 [Microsoft](http://landinghub.visualstudio.com/visual-cpp-build-tools) (仅在没有安装 Visual Studio 的情况下)

## Python 环境

### Python 3.6+ (通过 Anaconda)

Serpent.AI 的开发利用了很多 Python 3.6 的特征功能，所以 Python 的版本要求最低为 3.6。

其实安装普通的 Python 3.6+版本并不是很难，但是 Serpent.AI 需要一系列的科学计算模块和库，这些库本身在你的电脑上编译起来极其困难甚至是不可能成功的。令人庆幸的是我们可以直接使用 [Anaconda Distribution](https://www.anaconda.com/distribution) 帮我们解决这些问题，它为我们减轻了极大的负担。

#### 安装 Anaconda 4.4.0 (Python 3.6)

[官方下载地址](https://www.anaconda.com/download/) 配置有 Python 3.6 版本的 Anaconda 4.4.0 可以通过图形化安装器安装。

以下命令请使用管理员权限运行 _Anaconda Prompt_，我们强烈建议为此添加一个快捷链接，因为从现在开始基本上以后你所有的操作命令都需要在这里运行了，无论是 Python 的还是 Serpent 的。

#### 为 Serpent.AI 创建一个 Conda 虚拟环境

`conda create --name serpent python=3.6` (当然如果你愿意，_serpent_ 这个名字可以随便改)

#### 为 Serpent.AI 项目创建一个目录

`mkdir SerpentAI && cd SerpentAI`

#### 启动 Conda 虚拟环境

`activate serpent`

## 第三方软件依赖

### Redis

Redis 用来实现所捕获帧的内存内存储，同时也是分析事件的临时容器。Redis 并不原生适配 Windows 的！微软曾经维护过一个[适配方案](https://github.com/MicrosoftArchive/redis)，但是后来就被废弃了。

但幸运的是，有了 windows 10，我们可以通过使用 Windows 下的 Linux 子系统 (WSL) 来访问 Linux 兼容内核接口。这样我们就可以在 Windows 上的 Ubuntu 的 Bash 命令提示符中运行 Redis 服务器了。如果你还没有配置 WSL，你可以通过[参考这篇文章](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide)开始配置。

打开一个 Bash 命令提示符 (Windows 下的 Ubuntu核心的)，然后运行以下命令:

`sudo apt-get update`

`sudo apt-get install redis-server -y`

`sudo sed -i -e 's/127.0.0.1/0.0.0.0/g' /etc/redis/redis.conf`

`sudo service redis-server restart`

现在 Redis 服务器就已经运行起来了。这里要注意一点，尽管 Redis 看起来是以一个进程进行运行的，但是请 __不要__ 关闭这个 Bash 提示符，关闭它你就关闭了全部 WSL。你可以把它最小化藏起来。

### Build Tools for Visual Studio 2017
一些伴随着Serpent.AI安装所需的包尚未预编译，所以将需要从源代码生成。虽然安装过程有点麻烦，但是使用来自Visual Studio正确的C++ Build Tools 会使其更简单。

你可以从这个网址获取正确的安装器 [htitps://www.visualstudio.com/downloads/](https://www.visualstudio.com/downloads/)  找到 _Build Tools for Visual Studio 2017_ download. 找到并下载，在安装器中确保以下的选项被勾上:

* Visual C++ Build Tools core features
* VC++ 2017 version 15.7 v14.14 latest v141 tools
* Visual C++ 2017 Redistributable Update
* VC++ 2015.3 v14.00 (v140) toolset for desktop
* Universal CRT
### Tesseract

Serpent.AI 内含有一些 OCR 功能，这样我们就可以从游戏帧中读取文字了，我们把这项重任交给 Tesseract 去完成。你可以按照以下步骤为 Windows 安装 Tesseract:

1. 访问 [https://github.com/UB-Mannheim/tesseract/wiki](https://github.com/UB-Mannheim/tesseract/wiki)
2. 下载版本为 __3__ 的 .exe 可执行文件
3. 运行图形化安装器 (务必记录下安装路径！)
4. 在你的环境变量 %PATH% 中为 _tesseract.exe_ 添加刚才你记录的路径

打开 Anaconda 提示符，使用`tesseract --list-langs`可以测试 Tesseract 是否成功安装，同时也可以查看已经安装的语言包。

### GPU-加速的Tensorflow 依赖 (可选)

只有当如下情况时你才需要安装:

* 你设计的游戏代理使用了诸如卷积神经网络CNN、或者循环神经网络RNN等深度神经网络DNN，而且想要充分利用Tensorflow和你的显卡(们)的运算能力。


* 你拥有一张支持CUDA 3.0+的显卡，一般来说GTX 600系列之上的都行。

#### NVIDIA 英伟达驱动

你在 Windows 平台上，有张炫酷的显卡，还打算用游戏来学习这所有炫酷的技术...好吧既然这样，我们估计你的英伟达 (NVIDIA) 显卡驱动已经是最新的了。

如果你是刚开始了解显卡及显卡驱动的话:

1. 访问 [http://www.nvidia.com/Download/index.aspx](http://www.nvidia.com/Download/index.aspx)
2. 在列表中找到你的显卡的型号并下载对应的显卡驱动程序F
3. 运行图形化安装界面

#### CUDA 计算统一设备架构加速

1. 注册一个账户 [https://developer.nvidia.com/](https://developer.nvidia.com/) or log in
2. 访问 [https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)
3. 选择 _Windows_, _x68_64_, _10_, _exe (local)_ 然后下载安装包
4. 运行图形界面安装程序。如果有关于驱动的警告，选择 _Custom Install_ 自定义安装然后取消驱动选项的打勾。
5. 添加以下环境变量 _C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\bin_ 和 _C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\libnvpp_ 到 %PATH% 中

打开 Anaconda 提示符，使用`nvcc --version`可以测试 CUDA 是否成功安装，同时也可以查看已经安装的 CUDA 的版本信息。


#### cuDNN

1. 注册一个账户 [https://developer.nvidia.com/](https://developer.nvidia.com/) or log in
2. 访问 [https://developer.nvidia.com/rdp/cudnn-download](https://developer.nvidia.com/rdp/cudnn-download)
3. 下载 _cuDNN v6.0 Library for Windows 10_
4. 解压缩 ZIP 压缩文件内的 _bin_, _include_ 和 _lib_ 三个文件夹到你安装 CUDA 的同一路径下 (例如 _C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0_)
5. 确定你的环境变量 %PATHEXT% 中包含 _.DLL_

## 开始安装 Serpent.AI

一旦上面的所有组件都被正确安装与配置，你就可以开始安装 Serpent.AI 框架了。

返回你原来为自己创建的 Serpent.AI 项目目录，确保你在 Conda 虚拟环境下。

运行 `pip install SerpentAI`

然后运行 `serpent setup` 来自动安装余下的依赖包。

### 恭喜你在 Windows 系统上安装成功！