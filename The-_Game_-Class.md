The _Game_ class is meant to represent a generic video game in the Serpent.AI framework.

## What's in a Game Instance?

### Responsibilities

* Launching the game through the proper GameLauncher class
* Collecting and storing game window information
* Starting/stopping instances of the FrameGrabber class
* Feeding GameFrame instances to a given GameAgent instance in a loop
* Throttling the speed at which GameFrame instances are fed to the GameAgent instance
* Expose the game-specific API implemented by a plugin
* Register and expose all sprites following the name convention inside a plugin's data directory
* Provide information about regions of interest in game frames
* Provide information about OCR presets

### Concepts

#### Game Frame Limiter

When feeding GameFrame instances to a game agent, the rate at which frames are dispatched is throttled so it can't go over a specific FPS value. This concept was added after gaining experience building game agents with the framework. The main reason for this throttle is to ensure a realistic amount of APM (actions per minute) and to normalize the interval between actions from the agent. While it may sound great to have a game agent play the game at 1000 APM, the resulting gameplay ends up looking quite unnatural and it very often confuses the agent more than anything. The default limit is set to 2 FPS which is a very generous theoretical maximum of 120 APM. The threshold can be changed in _config/config.plugins.yml_.

#### API

Game Plugins can optionally ship with their own GameAPI subclass containing game-specific, reusable functions that are expected to be useful to ANY game agent that leverages it. The provided functions can cover anything from mere utilities to advanced UI operations or image processing routines.

#### Sprites

Game Plugins can optionally ship with pre-extracted game sprites in their _files/data/sprites_ directory. Provided the file follow the naming convention (_sprite\_<sprite_name>\_<animation\_state\_index>.png_), they will automatically be registered and instantiated as Sprite objects and stored in `self.sprites`. This is useful for sprite identification / location in game agents later on.

#### Screen Regions

A basic way of defining rectangular image ROIs (regions of interest). Defined as a hardcoded property in a plugin, they allow a game agent to conveniently look at a precise region of a game frame.

#### OCR Presets

A basic way of defining presets for OCR settings (preprocessing options, mostly) and tie them to categories of UI textual elements. Defined as a hardcoded property in a plugin, they allow a game agent to conveniently pick the best OCR settings for a given text category.

### Information

#### Game Window

* `self.window_id`: An OS-specific ID of the detected game window. (Linux and Windows only)
* `self.window_name`: The title of the window containing the game. Used by the window controller to locate the proper window.
* `self.window_geometry`: A dictionary containing the following keys with geometry information: _width_, _height_, _x\_offset_, _y\_offset_

#### Game Specific

* `self.api`: A GameAPI instance provided by a plugin
* `self.sprites`: A collection of registered Sprite instances
* `self.screen_regions`: A dictionary containing Name => Bounding Box Coordinates key-value pairs
* `self.ocr_presets`: A dictionary containing Name => OCR Settings key-value pairs

#### Flags

* `self.is_launched`: Whether or not the game is running
* `self.is_focused`: Whether or not the game window has focus

#### Misc

* `self.kwargs`: Extra parameters passed by a plugin

### Actions

### `self.launch`

**Method Signature** *launch(self, dry_run=False)*

Run the *before_launch* callback, launch the game through the proper GameLaunch and run the *after_launch* callback. 

* **dry_run**: When True, only run the callbacks and don't launch the game. Defaults to False.

*This method CANNOT be overwritten by plugins*

### `self.play`

**Method Signature** *play(self, game_agent_class_name=None, frame_handler=None, **kwargs)*

Try to find an active plugin for *game_agent_class_name* and instantiate it. Start a frame grabber process with the game window geometry. Wait for the first frames to appear in the frame grabber's stack. Start passing game frames in a loop to the game agent's frame handler matching *frame_handler*. 

* **game_agent_class_name**: The class name of the GameAgent that should be instantiated. Must be an active plugin.
* **frame_handler**: The name of a registered frame handler in the GameAgent instance. Optional. Falls back on the value in _config/config.plugins.yml_
* **\*\*kwargs**: Bag of extra arguments that are passed down to the GameAgent when instantiating it.

*This method CANNOT be overwritten by plugins*

### Callbacks

### `self.before_launch`

**Method Signature** *before_launch(self)*

Block of code to run before the game is launched.

*This method CAN be overwritten by plugins*

### `self.after_launch`

**Method Signature** *after_launch(self)*

Block of code to run after the game is launched. The base implementation of this method will set the `self.is_launched` flag, wait for the game to launch and locate the game window and store its geometry information.

*This method CAN be overwritten by plugins but a call to super().after_launch() is required*