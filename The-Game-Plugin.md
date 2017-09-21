The _Game_ plugin is meant to represent a game-specific extension of the base _Game_ class.

## What's in a Game Plugin Package?

* A plugin definition file
* A game-specific Game subclass
* A game-specific GameAPI subclass
* Bundled game sprites

## What's in a Game Subclass?

This subclass is lean by design; Most of the critical functionality is provided by the parent _Game_ class. Here is what is to be provided:

* An import and assignment to `self.api_class` of the GameAPI subclass in the constructor
* `self.platform`, `self.window_name` and other keyword argument assignments before the call to `super().__init__()`
* A property containing a mapping of all coordinates for screen regions of interest. Example:
```python
    @property
    def screen_regions(self):
        regions = {
            "MAIN_MENU_NEW_GAME": (626, 348, 708, 428),
            "MAIN_MENU_LOAD_GAME": (626, 429, 708, 510),
            "MAIN_MENU_MULTIPLAYER": (626, 511, 708, 592),
            "MAIN_MENU_GAME_TOOLS": (626, 593, 708, 674),
            "MAIN_MENU_QUIT": (704, 983, 768, 1023),
            "MAIN_MENU_SCENARIO_TAB_RCT": (234, 148, 267, 239),
            "MAIN_MENU_SCENARIO_TAB_RCT_CF": (234, 239, 267, 330),
            "MAIN_MENU_SCENARIO_TAB_RCT_LL": (234, 330, 267, 421),
            "MAIN_MENU_SCENARIO_TAB_RCT2": (234, 421, 267, 512),
            "MAIN_MENU_SCENARIO_TAB_RCT2_WW": (234, 512, 267, 603),
            "MAIN_MENU_SCENARIO_TAB_RCT2_TT": (234, 603, 267, 694),
            "MAIN_MENU_SCENARIO_TAB_RCT_RP": (234, 694, 267, 785),
            "MAIN_MENU_SCENARIO_TAB_RCT_OP": (234, 785, 267, 876),
            "GAME_PAUSE_BUTTON": (0, 0, 27, 30),
            "GAME_SPEED_BUTTON": (0, 30, 27, 60),
            "GAME_FLOPPY_BUTTON": (0, 60, 27, 90)
        }

        return regions
```
* A property containing OCR presets for different text block types in the game. Example:
```python
    @property
    def ocr_presets(self):
        presets = {
            "SCENARIO_TEXT": {
                "extract": {
                    "gradient_size": 3,
                    "closing_size": 10
                },
                "perform": {
                    "scale": 16,
                    "order": 3,
                    "horizontal_closing": 1,
                    "vertical_closing": 1
                }
            }
        }

        return presets
```

You are free to implement more custom instance methods; they will be callable from a game agent since they hold a reference to an instance of this subclass.

If the game has special extra steps that need to be taken during the launch process, you can extend the `self.before_launch` and/or `self.after_launch` callbacks. Don't forget the calls to `super()` though, the callbacks are meant to be extended, not overridden!

## What's in a GameAPI Subclass?

This is where the bulk of the development work will happen in a _Game_ plugin. It is a totally freeform class where you are expected to implement reusable functions to perform deterministic operations in the game or with the game data.

The file's location is: *files/api/api.py*

### Namespacing

Since your Game API can grow to be quite large, it is possible to namespace your functions for better organization. It was debated whether or not to use submodules or nested classes to provide such namespacing, and in the end, nested classes won. They aren't as pretty and as pythonic as submodules, but require way less intervention from the developer and are easier to explain.

A Game API created by generating a _Game_ plugin using `serpent generate game` will have provide an example of how to namespace your functions.

### Example Functions

* Performing the UI operations to start a new game
* Performing the UI operations to select a level
* Reading values from the HUD
* Locating game sprites

Your GameAPI functions can exist

## How are Game Sprites Bundled?

To properly bundle sprites along with your _Game_ plugin, you should add them to the *files/data/sprites* directory. The PNG format is expected for all sprites. Alpha channels and animated sprites are supported.

### Naming Convention

A naming convention has been established for game sprites. While optional, it is recommended to take advantage of it since all sprites adhering to it will automatically be discovered, labeled and registered by the _Game_ class at runtime.

`sprite_<label>_<animation_state_index>.png`

#### Example 

*sprite_npc_priest_0.png* will turn into a Sprite object with label *SPRITE_NPC_PRIEST*

*sprite_npc_priest_1.png* will append another frame of image data to existing Sprite object with label *SPRITE_NPC_PRIEST*