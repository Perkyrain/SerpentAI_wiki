Installing Serpent.AI on Linux platforms should be relatively easy. This install guide was built and tested against [Antergos](https://antergos.com/) (Arch-based) but should work on all other distros with minimal changes. _Distro-specific gotcha contributions to this page are encouraged._

## Initial Requirements

* A X11-Based Desktop Environment (sorry Wayland; for now) - Serpent.AI was Developed on Cinnamon DE

## Python Environment

### Python 3.6+

Serpent.AI was developed taking full advantage of Python 3.6 so it is only natural that the Python requirement be for versions 3.6 and up.

If your system Python meets this requirement (likely on Arch-based distros; unlikely on others) you are free to use it but it is recommended to use a Python version management system (like _pyenv_) along with a proper VirtualEnv. That way, if manage to break your Python somehow, you won't also break system tools that depend on it.

This section will assume the use of [pyenv](https://github.com/pyenv/pyenv). Installing and setting up _pyenv_ is beyond the scope of the guide but it can be achieved easily with [pyenv-installer](https://github.com/pyenv/pyenv-installer).

#### Installing Python 3.6

`pyenv install 3.6.4` (or a more recent version, if applicable)

On Debian-based systems, if the Python build fails, try the following first:

`sudo apt-get install libexpat1-dev libssl-dev zlib1g-dev libncurses5-dev libbz2-dev liblzma-dev libsqlite3-dev libffi-dev tcl-dev libgdbm-dev libreadline-dev tk tk-dev openssl` (Python build dependencies)

#### Creating a VirtualEnv for Serpent.AI

`pyenv virtualenv 3.6.4 serpent` (_serpent_ can be replaced with another name)

#### Creating a directory for your Serpent.AI projects

`mkdir SerpentAI && cd SerpentAI`

#### Assigning the VirtualEnv to the directory

`pyenv local serpent`

This will create a _.python-version_ file that will automatically set the active VirtualEnv upon entering the directory

## 3rd-Party Dependencies

### Redis

Redis is used in the framework as the in-memory store for the captured frame buffers as well as the temporary storage of analytics events. Minimum version is 3.0.0.

You can (likely) install Redis from your package manager (Arch-based: _redis_, Debian-based: _redis-server_), run it in a [Docker container](https://hub.docker.com/_/redis/) or [install it from source](https://redis.io/download).

Once it is installed, you can verify it is up and running properly by executing `redis-cli`. Note that this won't work with the Docker container approach but if your container is running, you are good to go!

Native installs are favored over Docker containers for performance reasons.

## Installing Serpent.AI

Once all of the above had been installed and set up, you are ready to install the framework.

Go back to the directory you created earlier for your Serpent.AI projects. Your VirtualEnv should switch automatically.

Run `pip install SerpentAI`

Then run `serpent setup` to install the remaining dependencies automatically.

#### You are now done with the installation on Linux!

## Installing Optional Modules

In the spirit of keeping the initial installation on the light side, some specialized / niche components with extra dependencies have been isolated from the core. It is recommended to only focus on installing them once you reach a point where you actually need them. The framework will provide a warning when a feature you are trying to use requires one of those modules.

### OCR

A module to provide OCR functionality in your game agents.

#### Tesseract

Serpent.AI leverages Tesseract for its OCR functionality. You can install Tesseract from your package manager (Arch-based: _tesseract_, Debian-based: _tesseract-ocr_). Language data is also required to have Tesseract return something. You can get language data from the package manager again (Arch-based: _tesseract-data-eng_, Debian-based: _tesseract-ocr-eng_). If you are planning to work with another language than English, make sure to also install the language data for it.

You can test your Tesseract installation by executing `tesseract --list-langs`.

On Debian-based distros, some system libraries will also be required when building the Python package: _libtesseract-dev libleptonica-dev_

#### Installation

Once you've validated that Tesseract has been properly set up, you can install the module with `serpent setup ocr`

### GUI

A module to allow the visual debugger to run.

#### Kivy

Kivy is the GUI framework used for the visual debugger in the framework. It is notorious for being rather difficult to get working properly. It is recommended to draw inspiration from [https://kivy.org/docs/installation/installation-linux.html#installation-in-a-virtual-environment](https://kivy.org/docs/installation/installation-linux.html#installation-in-a-virtual-environment) if you get errors while trying to install / use it.

#### Installation

Once you are ready to test your Kivy, you can install the module with `serpent setup gui` and try to run `serpent visual_debugger`

### ML

A module to leverage various machine learning solutions that are packaged with the framework.

#### Tensorflow GPU Dependencies (Optional)

You only need to install the following dependencies if:

* Your game agents will leverage deep neural networks like CNNs and RNNs and you want to get top performance from Tensorflow

AND

* You have an NVIDIA GPU that supports CUDA 3.0+ (Generally the GTX 600 series and up)

##### NVIDIA Drivers

Before installing any other dependencies, you need to make sure you are running on NVIDIA's proprietary drivers for your GPU. One easy way to test this is to run `nvidia-smi`. If you get no errors and see a driver version, you can skip to the next block.

###### Arch-Based Systems

The easiest way to install the drivers is probably through the package manager. You should install the following packages: _nvidia_ & _nvidia-installer_. Optionally: _nvidia-settings_ & _nvidia-utils_. Make sure the _xf86-video-nouveau_ package is not installed as it will conflict with the drivers.

Run `nvidia-installer` and follow the on-screen instructions.

###### Debian-Based Systems

1. Enter the TTY (Ctrl + Alt + F1)
2. Run `sudo apt-get purge nvidia-*`
3. Run `sudo apt-add-repository ppa:graphics-drivers/ppa`
4. Run `sudo apt-get update`
5. Run `sudo apt-get install nvidia-384`
6. Run `sudo reboot`

##### CUDA

###### Arch-Based Systems

Install the following package: _cuda_.

###### Debian-Based Systems

Run `sudo apt-get install nvidia-cuda-dev nvidia-cuda-toolkit nvidia-nsight` (Ubuntu 17.04)

##### cuDNN

###### Arch-Based Systems

Install the following package: _cudnn_.

###### Debian-Based Systems

1. Make an account on [https://developer.nvidia.com/](https://developer.nvidia.com/)
2. Visit [https://developer.nvidia.com/cudnn](https://developer.nvidia.com/cudnn)
3. Download _cuDNN v6.0 Library for Linux_
4. Go to the file in the Terminal and run `tar -xvzf cudnn-8.0-linux-x64-v6.0.tgz`
5. Run `sudo mv cuda /usr/local/`
6. Add `export LD_LIBRARY_PATH=/usr/local/cuda/lib64/` to _.bashrc_ and source it.

#### Installation

Once you've taken care of the dependencies, you can install the module with `serpent setup ml`