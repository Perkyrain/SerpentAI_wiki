_提示: 这个教程还在不断完善中，如果按照此教程进行配置遇到任何问题，请[创建一个问题](https://github.com/SerpentAI/Serpent/issues/new)或者在 [Discord](https://discord.gg/9D5SuxH) 中发信息提问，或直接 @Serpent.AI，非常感谢。_

在 linux 平台上部署 Serpent.AI 相对简单一些，本安装指南是在 [Antergos](https://antergos.com/) 平台( Arch架构 )上进行编写和测试的，但经过少量的修改应该可以适配所有其他发行版。_我们鼓励在此页添加任何针对其他发行版的改进。_

## 初始要求

* 基于 X11 的桌面环境，现阶段还不支持 Wayland，抱歉我的老伙计 - Serpent.AI 是基于 Cinnamon DE 开发的。

## Python 环境

### Python 3.6+

Serpent.AI 的开发利用了很多 Python 3.6 的特征功能，所以 Python 的版本要求最低为 3.6。

如果你的系统本身的 Python 已经符合了这一需求( Arch-架构的基本都符合，其他的不尽然)，你可以随意使用本框架。但是我们推荐使用诸如 pyenv 的 Python 版本管理软件，同时使用一个正确配置的虚拟环境 (VirtualEnv)。一旦因为操作不当导致 Python 本身出现严重问题，这样可以避免导致依赖 Python 的系统工具的运行崩溃。

本节说明建立在使用 [pyenv](https://github.com/pyenv/pyenv) 的基础上，虽然本教程不会详细介绍如何配置 pyenv ，但是这里有一份[关于 pyenv-installer 的详细文档](https://github.com/pyenv/pyenv-installer)你可以查阅。

#### 安装 Python 3.6

`pyenv install 3.6.2` (如果有更新的版本，也可以)

在基于Debian的系统上，如果Python编译失败了，尝试使用下面的方法：

`sudo apt-get install libexpat1-dev libssl-dev zlib1g-dev libncurses5-dev libbz2-dev liblzma-dev libsqlite3-dev libffi-dev tcl-dev libgdbm-dev libreadline-dev tk tk-dev openssl` (Python 编译依赖包)

#### 为 Serpent.AI 创建一个虚拟运行环境 (VirtualEnv)

`pyenv virtualenv 3.6.2 serpent` (当然如果你愿意的话，_serpent_ 这个名字可以随便改)

#### 为 Serpent.AI 项目创建一个目录

`mkdir SerpentAI && cd SerpentAI`

#### 为虚拟环境 (VirtualEnv) 分配目录

`pyenv local serpent`

这样会创建一个 _.python-version_ 文件，这样一旦进入这个目录，就可以自动启动虚拟环境了。

## 第三方软件依赖

### Redis

Redis 用来实现所捕获帧的内存内存储，同时也是分析事件的临时容器。最低版本要求 3.0.0。

你可以使用包管理器安装 Redis ( Arch架构: _redis_, Debian架构: _redis-server_)。你可以选择[从源代码处进行安装](https://redis.io/download)或者[使用 Docker 容器](https://hub.docker.com/_/redis/)。

一旦安装成功，你可以通过运行`redis-cli`命令来测试其是否正确安装。注意该命令不适用于Docker容器安装的方法，不过如果你的容器已经运行起来了，你就可以继续下一步了！

出于运行效率表现考虑，我们更推荐使用原生安装，不推荐使用 Docker 安装。


### Tesseract

Serpent.AI 内含有一些 OCR 功能，这样我们就可以从游戏帧中读取文字了，我们把这项重任交给 Tesseract 去完成。你可以使用包管理器安装 Tesseract ( Arch-架构: _tesseract_, Debian-架构: _tesseract-ocr_)。Tesseract 需要对应的语言包进行工作，同样你也可以使用包管理器获取语言包，以英文为例  ( Arch-架构: _tesseract-data-eng_, Debian-架构: _tesseract-ocr-eng_)。如果你还需要识别游戏中的其他语言，比如中文，那么就把 _eng_ 改成 _chi_sim_。

使用`tesseract --list-langs`可以测试 Tesseract 是否成功安装，同时也可以查看已经安装的语言包。

在 Debian-架构的发行版中，在编译Python包时有些系统也需要一些特定的库: _libtesseract-dev libleptonica-dev_。


### Kivy 依赖

##### Arch-架构 系统

安装以下包: _sdl2_
Install the following package: _sdl2_

##### Debian-架构 系统

`sudo add-apt-repository ppa:kivy-team/kivy`

`sudo apt-get update`

`sudo apt-get install python3-kivy`

要注意在有些发行版中 (比如 Ubuntu 16.04)，为了保障 Kivy 的正确安装，你必须手动定义所安装的 Cython 的版本。举例来说 `pip install Cython==0.25.2`, 参考自 [StackOverflow](https://stackoverflow.com/questions/13485364/cant-install-kivy-cython-gcc-error)。

### GPU-加速的Tensorflow 依赖 (可选)

只有当如下情况时你才需要安装:

* 你设计的游戏代理使用了诸如卷积神经网络CNN、或者循环神经网络RNN等深度神经网络DNN，而且想要充分利用Tensorflow和你的显卡(们)的运算能力。


* 你拥有一张支持CUDA 3.0+的显卡，一般来说GTX 600系列之上的都行。

#### NVIDIA 英伟达驱动

在安装任何其他依赖之前，请务必确认你的系统已经针对你的GPU进行了正确的配置，安装了合适的显卡驱动程序。若想测试是否正确安装，最简单的方式是运行`nvidia-smi`。如果你没有看到任何问题，而且明确的看到了你的显卡版本信息，你就可以继续了。

##### Arch-架构 系统

利用包管理器安装驱动应该是最简单的安装方式了。请在包管理器中安装如下包: _nvidia_ & _nvidia-installer_. Optionally: _nvidia-settings_ & _nvidia-utils_。可选包: _nvidia-settings_ & _nvidia-utils_。请确认不要安装 _xf86-video-nouveau_ 包，因为安装它会导致驱动冲突错误。

运行 `nvidia-installer`，然后跟着屏幕上的说明进行就可以了。

##### Debian-架构 系统

1. 进入 TTY 终端 (Ctrl + Alt + F1)
2. 运行 `sudo apt-get purge nvidia-*`
3. 运行 `sudo apt-add-repository ppa:graphics-drivers/ppa`
4. 运行 `sudo apt-get update`
5. 运行 `sudo apt-get install nvidia-384`
6. 运行 `sudo reboot`

#### CUDA

##### Arch-架构 系统

安装如下包: _cuda_

##### Debian-架构 系统

运行 `sudo apt-get install nvidia-cuda-dev nvidia-cuda-toolkit nvidia-nsight` (Ubuntu 17.04)

#### cuDNN

##### Arch-架构 系统

安装如下包: _cudnn_

##### Debian-架构 系统

1. 注册一个账户 [https://developer.nvidia.com/](https://developer.nvidia.com/)
2. 访问 [https://developer.nvidia.com/cudnn](https://developer.nvidia.com/cudnn)
3. 下载 _cuDNN v6.0 Library for Linux_
4. 在命令行中进入下载文件的路径，运行 `tar -xvzf cudnn-8.0-linux-x64-v6.0.tgz`
5. 运行 `sudo mv cuda /usr/local/`
6. 往 _.bashrc_ 中添加环境变量 `export LD_LIBRARY_PATH=/usr/local/cuda/lib64/` 然后 source 它。

## 开始安装 Serpent.AI

一旦上面的所有组件都被正确安装与配置，你就可以开始安装 Serpent.AI 框架了。

返回你原来为自己创建的 Serpent.AI 项目目录，你的虚拟环境 (VirtualEnv) 应该会自动切换好。

运行 `pip install SerpentAI`

然后 运行 `serpent setup` 来自动安装余下的依赖包。

### 恭喜你在Linux系统上安装成功！