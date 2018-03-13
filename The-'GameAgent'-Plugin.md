The _GameAgent_ plugin is meant to represent a specific extension of the base _GameAgent_ class.

## What's in a GameAgent Plugin Package?

* A plugin definition file
* A GameAgent subclass
* Bundled helper modules
* Bundled machine learning models

## What's in a GameAgent Subclass?

The GameAgent subclass is the most important file of your project. It contains your implementations (frame handlers) that will define the logic used to tackle the target game. Here is what is to be provided:

* One or more frame handlers functions
* Zero or more frame handler setup functions
* Frame handler registrations inside `self.frame_handlers` in the constructor
* Frame handler setup registrations inside `self.frame_handler_setups` in the constructor
* An optional instance of AnalyticsClient in `self.analytics_client` in the constructor

You are free to implement more custom instance methods. If the size of the file becomes unmanageable (it happens; game agents are pretty complex!), start using extracting functions to helper modules.

## How are Frame Handlers Registered?

1. Add an instance method to the GameAgent subclass for your frame handler
2. Add a dictionary entry in `self.frame_handlers` in the constructor, mapping a name to the function.

### Example 

```python
class MyGameAgent(GameAgent):
    def __init__(self, **kwargs):
        super().init(**kwargs)

        self.frame_handlers["MY_FRAME_HANDLER"] = self.my_frame_handler

    def my_frame_handler(self, game_frame):
        pass
```

## How are Frame Handler Setups Registered?

Exactly like Frame Handlers but in `self.frame_handler_setups`. They need to share the same name as their Frame Handler counterparts.

### Example

```python
class MyGameAgent(GameAgent):
    def __init__(self, **kwargs):
        super().init(**kwargs)

        self.frame_handler_setups["MY_FRAME_HANDLER"]: self.my_frame_handler_setup

    def my_frame_handler_setup(self):
        pass
```

## How are Helper Modules Bundled?

1. Create new files in *files/helpers*
2. Populate them with the desired functions and classes
3. Import them in your GameAgent subclass. These loose files are not made available by the plugin system so a relative import is needed: `from .helpers.<my_module> import *`

### Example

In *files/helpers/utils.py*_ a *hello_world* function is defined:

```python
def hello_world():
    print("Hello World!")
```

To access it in your GameAgent subclass, you add this import statement:

```python
from .helpers.utils import hello_world
```

## How are Machine Learning Models Bundled?

Simply copy them to *files/ml_models*. Renaming them to the hold the _.model_ extension is recommended because when you make a Git repository for your plugin, _.model_ files are added using Git LFS which enables uploads of files over 100MB.

## How are Machine Learning Models Loaded?

Simply add a dictionary entry in `self.machine_learning_models`, mapping a name to the result of `self.load_machine_learning_model`.

### Global Example

To make a machine learning model available to all your frame handlers, load it in the GameAgent's constructor:

```python
class MyGameAgent(GameAgent):
    def __init__(self, **kwargs):
        super().init(**kwargs)

        self.machine_learning_models["MY_MODEL"] = self.load_machine_learning_model(
            os.path.join(os.path.dirname(__file__), "ml_models/my_model.model")
        )
```

### Frame Handler Specific Example

To limit a machine learning model to only be in scope for a single frame handler, load it in the latter's setup function:

```python
class MyGameAgent(GameAgent):
    def __init__(self, **kwargs):
        super().init(**kwargs)

        self.frame_handlers["MY_FRAME_HANDLER"]: self.my_frame_handler
        self.frame_handler_setups["MY_FRAME_HANDLER"]: self.my_frame_handler_setup

    def my_frame_handler_setup(self):
        self.machine_learning_models["MY_MODEL"] = self.load_machine_learning_model(
            os.path.join(os.path.dirname(__file__), "ml_models/my_model.model")
        )

    def my_frame_handler(self, game_frame):
        pass
```