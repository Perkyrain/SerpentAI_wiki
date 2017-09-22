_Note: This guide is still in development. If you encounter any issues while trying to follow it, please report it by [creating an issue](https://github.com/SerpentAI/Serpent/issues/new) or posting a message to the [Discord](https://discord.gg/9D5SuxH). Thank you!_

Installing Serpent.AI on macOS should be relatively easy. This install guide was built and tested against [macOS Sierra](https://apple.com/) (Darwin) but should work on all later versions and macOS El Capitan.

## Initial Requirements

* macOS Sierra Version 10.12.6 or above (may work on previous version, but not tested)
* Homebrew ([see here](https://brew.sh/))

## Python Environment

### Python 3.6+

Serpent.AI was developed taking full advantage of Python 3.6 so it is only natural that the Python requirement be for versions 3.6 and up.

If you have Python 3.6 installed (unlikely on macOS by default) you may continue, otherwise install it with

`brew install python3`

It is recommended to use a proper VirtualEnv with Python 3.6. That way, if you manage to break your Python somehow, you won't also break system tools that depend on it.

This section will assume the use of [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/index.html). Installing and setting up _virtualenvwrapper_ is beyond the scope of the guide but it can be easily achieved with this [Pipenv & Virtual Environment Guide](http://docs.python-guide.org/en/latest/dev/virtualenvs/#virtualenvironments-ref) (_pipenv is optional_).

#### Installing Python 3.6 and a VirtualEnv for Serpent.AI

`mkvirtualenv --python=python3 serpent` (_serpent_ can obviously be replaced with another name)

#### Creating a directory for your Serpent.AI projects

`mkdir SerpentAI && cd SerpentAI`

#### Work in Virtual Environment

`workon serpent`


_Alternatively, you can make a project, which creates the virtual environment, and also a project directory inside `$PROJECT_HOME`, which is cd -ed into when you `workon serpent`._

_`mkproject serpent`_

## 3rd-Party Dependencies

### Redis

Redis is used in the framework as the in-memory store for the captured frame buffers as well as the temporary storage of analytics events. Minimum version is 3.0.0.

You can install Redis with Homebrew `brew install redis` (recommended), run it in a [Docker container](https://hub.docker.com/_/redis/) or [install it from source](https://redis.io/download).

Once it is installed, run `redis-server` ([how-to](https://medium.com/@petehouston/install-and-config-redis-on-mac-os-x-via-homebrew-eb8df9a4f298)) and verify it is up and running properly by executing `redis-cli`. Note that this won't work with the Docker container approach but if your container is running, you are good to go!

Native installs are favored over Docker containers for performance reasons.

### Tesseract

Serpent.AI includes OCR functionality so that text can be read from game frame regions and leverages Tesseract to do so. You can install Tesseract with Homebrew `brew install tesseract`. Language data is also required to have Tesseract return something. By default `english` is included. If you are planning to work on another language than English, you can install all languages with `brew install tesseract --all-languages` or [manually as needed](https://blog.philippklaus.de/2011/01/chinese-ocr/).

You can test your Tesseract installation by executing `tesseract --list-langs`.

### Kivy Dependencies

To be able to build and install Kivy, you need to install various libraries.

`brew install pkg-config sdl2 sdl2_image sdl2_ttf sdl2_mixer gstreamer`
`brew cask install xquartz`

(More info to be added)

### GPU-Accelerated Tensorflow Dependencies

Tensorflow > 1.2 doesn't support GPU acceleration on macOS :(

## Installing Serpent.AI

Once all of the above had been installed and set up, you are ready to install the framework.

Go back to the directory you created earlier for your Serpent.AI projects and activate your virtual environment
`workon serpent`

Run `pip install SerpentAI`

Then run `serpent setup` to install the remaining dependencies automatically.

### You are now done with the installation on macOS!
