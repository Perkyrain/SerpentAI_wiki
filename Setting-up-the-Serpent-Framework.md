## GNU/Linux

Cloning the package via git Console:

```git
$ git clone https://github.com/SerpentAI/Serpent.git
```

#### Requirements

###### Python
It is recommended to use a Virtual environment with python3. This can be created from a native python3 install via
```
python3 -m venv /path/to/new/virtual/environment
```
or 
```
python -m venv /path/to/new/virtual/environment
```
if your default python version is python 3 (i.e. ArchLinux,...)
Refer to [venv documentation](https://docs.python.org/3/library/venv.html) for more information.

Alternatively [pyenv](https://github.com/pyenv/pyenv) can be used.

###### Python packages
Following a list of the required python packages
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

All these can be installed via pip and the [requirements.txt](https://github.com/SerpentAI/Serpent/blob/master/requirements.txt) from the Serpent project folder.

```
$ pip install -r requirements.txt
```

###### None Python dependencies
Other dependencies that can't be installed via pip
* Steam and the respective game your Agent wants to play
* ...and more

## Windows

Coming soon

## Mac OS X

Coming soon