_提示: 这个教程还在不断完善中，如果按照此教程进行配置遇到任何问题，请[创建一个问题](https://github.com/SerpentAI/Serpent/issues/new)或者在 [Discord](https://discord.gg/9D5SuxH) 中发信息提问，或直接 @Serpent.AI，非常感谢。_

在 macOS 平台上部署 Serpent.AI 相对简单一些，本安装指南是在 [macOS Sierra](https://apple.com/)  平台( Darwin )上进行编写和测试的，应该可以适配macOS El Capitan和任何更新的版本。

## 初始要求

* macOS Sierra Version 10.12.6 或者更高的版本 (有可能在之前的版本也能运行但是还没有经过测试)
* Homebrew ([查看详情](https://brew.sh/))

## Python 环境

### Python 3.6+

Serpent.AI 的开发利用了很多 Python 3.6 的特征功能，所以 Python 的版本要求最低为 3.6。

如果你已经安装了 Python 3.6 (原生系统并没有安装，默认安装2.7)，你可以继续下个步骤了。如果没有安装的话，可以用以下命令安装:

`brew install python3`

我们推荐使用诸如 pyenv 的 Python 版本管理软件，同时使用一个正确配置的虚拟环境 (VirtualEnv)。一旦因为操作不当导致 Python 本身出现严重问题，这样可以避免导致依赖 Python 的系统工具的运行崩溃。

本节说明建立在使用 [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/index.html) 的基础上，虽然本教程不会详细介绍如何配置 _virtualenvwrapper_ ，但是这里有一份[关于 Pipenv & Virtual Environment Guide 的详细文档](http://docs.python-guide.org/en/latest/dev/virtualenvs/#virtualenvironments-ref)你可以查阅。(你也可以选则使用 _pipenv_ )


#### 为  Serpent.AI 安装 Python 3.6 和 虚拟环境 (VirtualEnv)

`mkvirtualenv --python=python3 serpent` (当然如果你愿意，_serpent_ 这个名字可以随便改)

#### 为 Serpent.AI 项目创建一个目录

`mkdir SerpentAI && cd SerpentAI`

#### 进入虚拟环境开始工作

`workon serpent`

_另外有一种方式，你可以在创建项目的同时，创建对应的虚拟环境，而且项目目录隶属于 `$PROJECT_HOME`，这样下次使用 `workon serpent`就可以直接进入这个目录开始工作。这种方式的命令为:_

_`mkproject serpent`_

## 第三方软件依赖

### Redis

Redis 用来实现所捕获帧的内存内存储，同时也是分析事件的临时容器。最低版本要求 3.0.0。

你可以使用 Homebrew 安装 Redis， `brew install redis` (推荐方法)。你也可以选择[从源代码处进行安装](https://redis.io/download)或者[使用 Docker 容器](https://hub.docker.com/_/redis/)。

一旦安装成功，运行`redis-server`([参考资料](https://medium.com/@petehouston/install-and-config-redis-on-mac-os-x-via-homebrew-eb8df9a4f298))。你可以通过运行`redis-cli`命令来测试其是否正确安装。注意该命令不适用于Docker容器安装的方法，不过如果你的容器已经运行起来了，你就可以继续下一步了！

出于运行效率表现考虑，我们更推荐使用原生安装，不推荐使用 Docker 安装。

### Tesseract

Serpent.AI 内含有一些 OCR 功能，这样我们就可以从游戏帧中读取文字了，我们把这项重任交给 Tesseract 去完成。你可以使用 Homebrew 包管理器安装 Tesseract (`brew install tesseract`)。Tesseract 需要对应的语言包进行工作，默认的语言包为英语，同样你也可以使用包管理器获取语言包，命令`brew install tesseract --all-languages`可以安装所有语言，想要安装中文或指定语言请[参考这里](https://blog.philippklaus.de/2011/01/chinese-ocr/)。


使用`tesseract --list-langs`可以测试 Tesseract 是否成功安装，同时也可以查看已经安装的语言包。

### Kivy 依赖

想要正确的编译并安装 Kivy，你需要安装一系列不同的库。

`brew install pkg-config sdl2 sdl2_image sdl2_ttf sdl2_mixer gstreamer`

`brew cask install xquartz`

(相关说明有待补充)

### GPU-加速的Tensorflow 依赖 (可选)

高于 1.2 版本的 Tensorflow 在 macOS 上不支持 GPU 加速 :(

## 开始安装 Serpent.AI

一旦上面的所有组件都被正确安装与配置，你就可以开始安装 Serpent.AI 框架了。

返回你原来为自己创建的 Serpent.AI 项目目录，使用以下命令进入你的虚拟开发环境 (VirtualEnv) 
`workon serpent`

运行 `pip install SerpentAI`

然后运行 `serpent setup` 来自动安装余下的依赖包。

### 恭喜你在 macOS 系统上安装成功！