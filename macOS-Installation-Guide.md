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

`mkvirtualenv --python=python3 serpent` (_serpent_ can be replaced with another name)

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

Serpent.AI includes OCR functionality so that text can be read from game frame regions and leverages Tesseract to do so. You can install Tesseract with Homebrew:

`brew install tesseract`

Language data is also required to have Tesseract return something. By default `english` is included. If you are planning to work on another language than English, you can install all languages with `brew install tesseract --all-languages` or [manually as needed](https://blog.philippklaus.de/2011/01/chinese-ocr/).

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

## Caveats

* In order for Serpent.AI to access your mac you'll have to give the terminal you're using rights. You can do this by going to System Preferences > Security & Privacy > Privacy, then place a check box beside your terminal application.

## Compiling Tensorflow from source (Optional)
I have found that having Tensorflow compiled to use all the instructions set that your CPU provides on macOS greatly speeds up the training process (**up to 5 times**).
Here are the steps to achieve this, **I recommend running a training with the pip lib first to see the warnings about operations your CPU support but that Tensforflow isn't compiled for (we need them for later)** :
```
# First we install bazel, a java build tool.
# (yes, sadly this requires having to install the Java SDK on your machine)
# For this, you will need the Java SDK 8, you can grab it here :
# http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
# You can then proceed
```
```
# We download the Bazel install script for version 0.5.4 (only this version will work for Tensorflow 1.3)
curl https://github.com/bazelbuild/bazel/releases/download/0.5.4/bazel-0.5.4-installer-darwin-x86_64.sh \
--output /tmp/bazel-install.sh
```
```
# Next we install Bazel
./tmp/bazel-install.sh
# Proceed with install
```
```
# Next, in the right virtualenv we install the dependencies for building TF
pip install six numpy wheel
```
```
# Next we clone the Tensorflow repository and checkout the version we want, here 1.3
cd ~ && \
git clone https://github.com/tensorflow/tensorflow && \
cd ~/tensorflow && \
git checkout r1.3
```
```
# We can now configure the install with
./configure
# Accepts all defaults
```
```
# Next we build with bazel, with the RIGHT CPU FLAGS ENABLED
# (https://stackoverflow.com/questions/41293077/how-to-compile-tensorflow-with-sse4-2-and-avx-instructions)
# For me it was AVX, AVX2, FMA, SSE4.2
bazel build --config=opt \
--copt=-mavx \
--copt=-mavx2 \
--copt=-mfma \
--copt=-msse4.2 \
//tensorflow/tools/pip_package:build_pip_package
# This takes a while, don't worry about the warnings...
```
```
# Next we will build the pip package with
bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
```
```
# Last step is to remove the current tensorflow lib in your Serpent AI install and replace it with our one :
cd your_serpent_install_path
pip uninstall tensorflow
# Simply tab to complete the file path
pip install /tmp/tensorflow_pkg/tensorflow-...
```
You're all set !
Try a training and see the warning not show up and your training finally being somewhat worth your time.