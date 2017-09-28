_Note: This guide is still in development. If you encounter any issues while trying to follow it, please report it by [creating an issue](https://github.com/SerpentAI/Serpent/issues/new) or posting a message to the [Discord](https://discord.gg/9D5SuxH). Thank you!_

Installing Serpent.AI on Windows might sound a little daunting at first but it really turns out to be easier than expected. You will have less flexibility than with a Linux install, but between that and not having any Windows support at all (like so many libraries out there...), it is an acceptable trade off. _Have a pro tip? Contributions to this page are encouraged._

## Initial Requirements

* Windows 10 (for WSL)
* Visual C++ Build Tools from [Microsoft](http://landinghub.visualstudio.com/visual-cpp-build-tools) (only needed if Visual Studio is not already installed)

## Python Environment

### Python 3.6+ (with Anaconda)

Serpent.AI was developed taking full advantage of Python 3.6 so it is only natural that the Python requirement be for versions 3.6 and up.

Installing regular Python 3.6+ isn't exactly difficult but Serpent.AI relies on a good amount of scientific computing libraries that are extremely difficult / impossible to compile on your own on Windows. Thankfully, the [Anaconda Distribution](https://www.anaconda.com/distribution) exists and takes this huge weight off our collective shoulders.

#### Installing Anaconda 4.4.0 (Python 3.6)

[Download](https://www.anaconda.com/download/) the Python 3.6 version of Anaconda 4.4.0 and run the graphical installer.

The following commands are to be performed in an _Anaconda Prompt_ with elevated privileges (AKA Run as Administrator...). It is recommended to create a shortcut to this prompt because every Python and Serpent command will have to be performed from there starting now. 

#### Creating a Conda Env for Serpent.AI

`conda create --name serpent python=3.6` (_serpent_ can obviously be replaced with another name)

#### Creating a directory for your Serpent.AI projects

`mkdir SerpentAI && cd SerpentAI`

#### Activating the Conda Env

`activate serpent`

## 3rd-Party Dependencies

### Redis

Redis is used in the framework as the in-memory store for the captured frame buffers as well as the temporary storage of analytics events. It is not meant to be compatible with Windows! Microsoft used to [maintain a port](https://github.com/MicrosoftArchive/redis) but it's been abandoned since.

Luckily, with Windows 10, we have access to the Windows Subsystem for Linux (WSL) which allows us to run a Redis server in 
a _Bash on Ubuntu on Windows_ prompt. If you have not set up WSL yet, you can follow this article to get started: [https://msdn.microsoft.com/en-us/commandline/wsl/install_guide](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).

Open a _Bash on Ubuntu on Windows_ prompt and execute the following commands:

`sudo apt-get update`

`sudo apt-get install redis-server -y`

`sudo sed -i -e 's/127.0.0.1/0.0.0.0/g' /etc/redis/redis.conf`

`sudo service redis-server restart`

You now have a running Redis server. Note that even though Redis appears to be running as a service, you CANNOT close the bash prompt because it will take down WSL with it. Feel free to minimize it though.

### Tesseract

Serpent.AI includes OCR functionality so that text can be read from game frame regions and leverages Tesseract to do so. You can install Tesseract for Windows by following these steps:

1. Visit [https://github.com/UB-Mannheim/tesseract/wiki](https://github.com/UB-Mannheim/tesseract/wiki)
2. Download the .exe for version 3
3. Run the graphical installer (Remember the install path!)
4. Add the path to _tesseract.exe_ to your %PATH% environment variable

You can test your Tesseract installation by opening an Anaconda Prompt and executing `tesseract --list-langs`.

### GPU-Accelerated Tensorflow Dependencies (Optional)

You only need to install the following dependencies if:

* Your game agents will leverage deep neural networks like CNNs and RNNs and you want to get top performance from Tensorflow

AND

* You have an NVIDIA GPU that supports CUDA 3.0+ (Generally the GTX 600 series and up)

#### NVIDIA Drivers

You are on Windows, have a modern GPU and planning to use video games as an environment... Let's face it, your NVIDIA drivers are probably current and up to date. 

In case you are totally new to this:

1. Visit [http://www.nvidia.com/Download/index.aspx](http://www.nvidia.com/Download/index.aspx)
2. Find your GPU in the list and download the drivers
3. Run the downloaded graphical installer

#### CUDA

1. Make an account on [https://developer.nvidia.com/](https://developer.nvidia.com/) or log in
2. Visit [https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)
3. Select _Windows_, _x68_64_, _10_, _exe (local)_ and download the installer
4. Run the graphical installer. If you get a warning about drivers, select _Custom Install_ and untick the drivers option.
5. Add _C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\bin_ and _C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\libnvpp_ to your %PATH% environment variable

You can test your CUDA installation by opening an Anaconda Prompt and executing `nvcc --version`.

#### cuDNN

1. Make an account on [https://developer.nvidia.com/](https://developer.nvidia.com/) or log in
2. Visit [https://developer.nvidia.com/rdp/cudnn-download](https://developer.nvidia.com/rdp/cudnn-download)
3. Download _cuDNN v6.0 Library for Windows 10_
4. Extract _bin_, _include_ and _lib_ inside the ZIP archive to the CUDA installation path equivalent (like _C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0_)
5. Make sure _.DLL_ is included in your %PATHEXT% environment variable

## Installing Serpent.AI

Once all of the above had been installed and set up, you are ready to install the framework.

Go back to the directory you created earlier for your Serpent.AI projects. Make sure you are scoped in your Conda Env.

Run `pip install SerpentAI`

Then run `serpent setup` to install the remaining dependencies automatically.

#### In case `kivy.deps.sdl2` or `kivy.deps.glew` errors when installing:

You need to download them manually from the official repository (make sure to download appropriate version, e.g. cp36 for Python 3.6):

* https://pypi.python.org/pypi/kivy.deps.sdl2
* https://pypi.python.org/pypi/kivy.deps.glew

And then run `wheel install [file]` on each of them

### You are now done with the installation on Windows!
