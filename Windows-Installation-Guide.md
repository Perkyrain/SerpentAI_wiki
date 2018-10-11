Installing Serpent.AI on Windows might sound a little daunting at first but it really turns out to be easier than expected. _Have a pro tip? Contributions to this page are encouraged._

Prefer a video? A thorough install guide is available on [YouTube](https://www.youtube.com/watch?v=5d4Ceq1L8hg).

## Initial Requirements

* Windows 7 and up

## Python Environment

### Python 3.6+ (with Anaconda)

Serpent.AI was developed taking full advantage of Python 3.6 so it is only natural that the Python requirement be for versions 3.6 and up.

Installing regular Python 3.6+ isn't exactly difficult but Serpent.AI relies on a good amount of scientific computing libraries that are extremely difficult / impossible to compile on your own on Windows. Thankfully, the [Anaconda Distribution](https://www.anaconda.com/distribution) exists and takes this huge weight off our collective shoulders.

#### Installing Anaconda 5.2.0 (Python 3.6)

[Download](https://www.anaconda.com/download/) the Python 3.6 version of Anaconda 5.2.0 and run the graphical installer.

The following commands are to be performed in an _Anaconda Prompt_ with elevated privileges (Right click and **Run as Administrator**). It is recommended to create a shortcut to this prompt because every Python and Serpent command will have to be performed from there starting now. 

#### Creating a Conda Env for Serpent.AI

`conda create --name serpent python=3.6` ('serpent' can be replaced with another name)

#### Creating a directory for your Serpent.AI projects

`mkdir SerpentAI && cd SerpentAI`

#### Activating the Conda Env

`conda activate serpent`

## 3rd-Party Dependencies

### Redis

Redis is used in the framework as the in-memory store for the captured frame buffers as well as the temporary storage of analytics events. It is not meant to be compatible with Windows! Microsoft used to [maintain a port](https://github.com/MicrosoftArchive/redis) but it's been abandoned since. This being said, that Redis version is sufficient and it outperforms stuff like running it in WSL on Windows 10. It will install as a Windows Service. Make sure you set it to start automatically.

Alternatively, you can run it in a [Docker container](https://hub.docker.com/_/redis/).

### Build Tools for Visual Studio 2017

Some of the packages that will be installed alongside Serpent.AI are not pre-compiled binaries and will be need to be built from source. This is a little more problematic for Windows but with the correct C++ Build Tools for Visual Studio it all goes down smoothly.

You can get the proper installer by visiting [https://www.visualstudio.com/downloads/](https://www.visualstudio.com/downloads/) and scrolling down to the _Build Tools for Visual Studio 2017_ download. Download, run, select the _Visual C++ build tools_ section and make sure the following components are checked:

* Visual C++ Build Tools core features
* VC++ 2017 version 15.7 v14.14 latest v141 tools
* Visual C++ 2017 Redistributable Update
* VC++ 2015.3 v14.00 (v140) toolset for desktop
* Universal CRT

## Installing Serpent.AI

Once all of the above had been installed and set up, you are ready to install the framework. Remember that PATH changes in Windows are not reflected in your command prompts that were opened while you made the changes. Open a fresh Anaconda prompt before continuing to avoid installation issues.

Go back to the directory you created earlier for your Serpent.AI projects. Make sure you are scoped in your Conda Env.

Run `pip install SerpentAI`

Then run `serpent setup` to install the remaining dependencies automatically.

### You are now done with the installation on Windows!

## Installing Optional Modules

In the spirit of keeping the initial installation on the light side, some specialized / niche components with extra dependencies have been isolated from the core. It is recommended to only focus on installing them once you reach a point where you actually need them. The framework will provide a warning when a feature you are trying to use requires one of those modules.

### OCR

A module to provide OCR functionality in your game agents.

#### Tesseract

Serpent.AI leverages Tesseract for its OCR functionality. You can install Tesseract for Windows by following these steps:

1. Visit [https://github.com/UB-Mannheim/tesseract/wiki](https://github.com/UB-Mannheim/tesseract/wiki)
2. Download the .exe for version 3
3. Run the graphical installer (Remember the install path!)
4. Add the path to _tesseract.exe_ to your %PATH% environment variable

You can test your Tesseract installation by opening an Anaconda Prompt and executing `tesseract --list-langs`.

#### Installation

Once you've validated that Tesseract has been properly set up, you can install the module with `serpent setup ocr`

### GUI

A module to allow Serpent.AI desktop app to run.

#### Kivy

Kivy is the GUI framework used in the framework.

Once you are ready to test your Kivy, you can install the module with `serpent setup gui` and try to run `serpent visual_debugger`

### ML

A module to leverage various machine learning solutions that are packaged with the framework.

#### Tensorflow GPU Dependencies (Optional)

You only need to install the following dependencies if:

* Your game agents will leverage deep neural networks like CNNs and RNNs and you want to get top performance from Tensorflow

AND

* You have an NVIDIA GPU that supports compute capability 3.0+ (Generally the GTX 600 series and up)

##### NVIDIA Drivers

You are on Windows, have a modern GPU and planning to use video games as an environment... Let's face it, your NVIDIA drivers are probably current and up to date. 

In case you are totally new to this:

1. Visit [http://www.nvidia.com/Download/index.aspx](http://www.nvidia.com/Download/index.aspx)
2. Find your GPU in the list and download the drivers
3. Run the downloaded graphical installer

##### CUDA

1. Make an account on [https://developer.nvidia.com/](https://developer.nvidia.com/) or log in
2. Visit [https://developer.nvidia.com/cuda-toolkit-archive](https://developer.nvidia.com/cuda-toolkit-archive)
3. Select _CUDA Toolkit 9.0_
4. Select _Windows_, _x86_64_, _10_, _exe (local)_ and download the installer
5. Run the graphical installer. Select _Custom Install_ and untick everything except CUDA.

You can test your CUDA installation by opening an Anaconda Prompt and executing `nvcc --version`.

##### cuDNN

1. Make an account on [https://developer.nvidia.com/](https://developer.nvidia.com/) or log in
2. Visit [https://developer.nvidia.com/rdp/cudnn-archive](https://developer.nvidia.com/rdp/cudnn-archive)
3. Click _Download cuDNN v7.0.5 (Dec 5, 2017), for CUDA 9.0_
4. Download _cuDNN v7.0.5 Library for Windows 10_
4. Extract _bin_, _include_ and _lib_ inside the ZIP archive to the CUDA installation path equivalent (like _C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0_)
5. Make sure _.DLL_ is included in your %PATHEXT% environment variable

#### Installation

Once you've taken care of the dependencies, you can install the module with `serpent setup ml`. The stable release of the framework installs TensorFlow 1.4.0 which is only compatible with CUDA 8. We installed 9.0 to be compatible with newer releases of TensorFlow which means you will have to uninstall 1.4.0 `pip uninstall tensorflow-gpu`. If you want a good balance of library support and recent enough TensorFlow version, it is currently recommended to install 1.5.1 `pip install tensorflow-gpu==1.5.1`. Installing versions that are too recent breaks compatibility with libraries that build on top of TensorFlow.

You can test your GPU Tensorflow installation by going to `ipython` and entering `import tensorflow`. If it loads without any exceptions, you are good to go!