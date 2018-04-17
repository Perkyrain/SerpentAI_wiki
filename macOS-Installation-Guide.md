Installing Serpent.AI on macOS should be relatively easy. This install guide was built and tested against macOS Sierra but should work on El Capitan and up.

## Initial Requirements

* macOS El Capitan and up
* Homebrew ([https://brew.sh/](https://brew.sh/))

## Python Environment

### Python 3.6+

Serpent.AI was developed taking full advantage of Python 3.6 so it is only natural that the Python requirement be for versions 3.6 and up.

If your system Python meets this requirement (very unlikely) you are free to use it but it is recommended to use a Python version management system (like _pyenv_) along with a proper VirtualEnv. That way, if manage to break your Python somehow, you won't also break system tools that depend on it.

This section will assume the use of [pyenv](https://github.com/pyenv/pyenv). Installing and setting up _pyenv_ is beyond the scope of the guide but it can be achieved easily with [pyenv-installer](https://github.com/pyenv/pyenv-installer).

#### Installing Python 3.6

`pyenv install 3.6.4` (or a more recent version, if applicable)

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

You can install Redis with Homebrew `brew install redis` (recommended, [article](https://medium.com/@petehouston/install-and-config-redis-on-mac-os-x-via-homebrew-eb8df9a4f298)), run it in a [Docker container](https://hub.docker.com/_/redis/) or [install it from source](https://redis.io/download).

Once it is installed, you can verify it is up and running properly by executing `redis-cli`. Note that this won't work with the Docker container approach but if your container is running, you are good to go!

Native installs are favored over Docker containers for performance reasons.

## Installing Serpent.AI

Once all of the above had been installed and set up, you are ready to install the framework.

Go back to the directory you created earlier for your Serpent.AI projects. Your VirtualEnv should switch automatically.

Run `pip install SerpentAI`

Then run `serpent setup` to install the remaining dependencies automatically.

#### You are now done with the installation on macOS!

## Installing Optional Modules

In the spirit of keeping the initial installation on the light side, some specialized / niche components with extra dependencies have been isolated from the core. It is recommended to only focus on installing them once you reach a point where you actually need them. The framework will provide a warning when a feature you are trying to use requires one of those modules.

### OCR

A module to provide OCR functionality in your game agents.

#### Tesseract

Serpent.AI leverages Tesseract for its OCR functionality. You can install Tesseract with Homebrew:

`brew install tesseract`

Language data is also required to have Tesseract return something. By default English is included. If you are planning to work on another language than English, you can install all languages with `brew install tesseract --all-languages`.

You can test your Tesseract installation by executing `tesseract --list-langs`.

#### Installation

Once you've validated that Tesseract has been properly set up, you can install the module with `serpent setup ocr`

### GUI

A module to allow the visual debugger to run.

#### Kivy

Kivy is the GUI framework used for the visual debugger in the framework. It is notorious for being rather difficult to get working properly. The most important part is making sure the system dependencies are available:

`brew install pkg-config sdl2 sdl2_image sdl2_ttf sdl2_mixer gstreamer`

#### Installation

Once you are ready to test your Kivy, you can install the module with `serpent setup gui` and try to run `serpent visual_debugger`

### ML

A module to leverage various machine learning solutions that are packaged with the framework.

#### Tensorflow GPU Dependencies

No dependencies. Tensorflow doesn't support GPU acceleration on macOS.

#### Installation

Once you've taken care of the dependencies, you can install the module with `serpent setup ml`