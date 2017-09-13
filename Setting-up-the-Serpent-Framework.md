# WORK IN PROGRESS
## GNU/Linux

Cloning the package via git Console:

```git
$ git clone https://github.com/SerpentAI/Serpent.git
```

### Requirements

#### Python 3
It is recommended to use a virtual environment with python3. This can be created from a native python3 install via
```
$ python3 -m venv /path/to/new/virtual/environment
```
or
```
$ python -m venv /path/to/new/virtual/environment
```
if your default python version is python 3 (i.e. ArchLinux,...).
Refer to [venv documentation](https://docs.python.org/3/library/venv.html) for more information.

Alternatively [pyenv](https://github.com/pyenv/pyenv) can be used, to create the virtual environment

#### Python packages requirements.txt
Following a list of the required python packages:

* offshoot==0.1.1
* numpy==1.12.1
* scikit-image==0.13.0
* redis==2.10.5
* scikit-learn==0.18.1
* editdistance==0.3.1
* invoke==0.16.3
* PyUserInput==0.1.11
* h5py==2.7.0
* Keras==2.0.4
* keras-rl==0.3.0
* tensorflow-gpu==1.1.0
* Cython==0.25.2
* Kivy==1.10.0
* termcolor==1.1.0
* pyperclip==1.5.27

All these can be installed via _pip_ and the [requirements.txt](https://github.com/SerpentAI/Serpent/blob/master/requirements.txt) from the Serpent project folder.

Make sure to check, that you are installing into the virtual environment, by checking the active python environment first. Further info [here](https://www.dabapps.com/blog/introduction-to-pip-and-virtualenv-python/).

```
$ which python
$ pip install -r requirements.txt
```

If _pip_ is not already installed on your system, you can do so via

```
$ wget https://bootstrap.pypa.io/get-pip.py
$ python get-pip.py
```
Refer to the pip [documentation](https://pip.pypa.io/en/stable/installing/) for more information.

#### Other python requirements
* __PyGObject__, please find installation guidelines for PyGObject [here](http://pygobject.readthedocs.io/en/latest/getting_started.html)

#### None Python dependencies

* __Steam__ and the game your Agent wants to play
* __redis-server__, please find installation guidelines for redis [here](https://redis.io/download#installation)
* __xdotool__, please find installation guidelines for xdotool [here](http://semicomplete.com/projects/xdotool/)
* __cuda__, ```$ pacman -S cuda``` if you are on arch or [check the docs](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#axzz4k7BQOTHS) for different installation options

## Windows

Coming soon

## Mac OS X

Coming soon
