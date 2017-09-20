One very important tool to learn and understand in order to master the framework is the _serpent_ executable. A lot of major features can be accessed by invoking the right _serpent_ command.

Try it out right now. Run `serpent`.

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/executable1.png)

You should see something similar to what's in the image above. Let's proceed to cover these commands one-by-one.

## Serpent Commands

### setup

Perform first time setup for the framework. Creates configuration files, dataset directories and installs OS-specific dependencies.

#### Usage

##### Perform first time setup

`serpent setup`

_Note: It will remain possible to invoke this command again later on. Be aware that it WILL deactivate all plugins, override configuration files and delete all datasets. You will be asked for confirmation before proceeding._

### grab_frames

Start an instance of the Frame Grabber. **Not meant for direct use**

#### Usage

##### Start an instance of the Frame Grabber.

`serpent grab_frames <width> <height> <x_offset> <y_offset>`

* _width_: Width in pixels of the game frames to capture
* _height_: Height in pixels of the game frames to capture
* _x\_offset_: X offset in pixels of the game frames to capture
* _y\_offset_: Y offset in pixels of the game frames to capture

_Note: This is executed in the background by Game objects when starting up a Game Agent._

### activate

Activate a plugin. Makes it visible and accessible through Serpent.AI code.

#### Usage

##### Activate a plugin

`serpent activate <plugin_name>`

* _plugin\_name_: The name of a plugin in your plugins directory

### deactivate

Deactivate a plugin. Makes it invisible and inaccessible through Serpent.AI code.

#### Usage

##### Deactivate a plugin

`serpent deactivate <plugin_name>`

* _plugin\_name_: The name of a plugin in your plugins directory

### plugins

List active and inactive plugins.

#### Usage

##### List plugins

`serpent plugins`

### launch

Launch a game through the appropriate active Serpent.AI plugin.

#### Usage

##### Launch a game

`serpent launch <game_name>`

* _game\_name_: The titleized name of the game (i.e. CoolGame).

### play

Play a game with the specified game agent through the appropriate Serpent.AI plugin. The game needs to have already been launched through a `serpent launch` command.

#### Usage

##### Play a game with the specified game agent

`serpent play <game_name> <game_agent_plugin_name>`

* _game\_name_: The titleized name of the game (i.e. CoolGame).
* _game\_agent\_plugin\_name_: The full name of the game agent (i.e. SerpentCoolGameGameAgent)

### generate

Generate code for Game and Game Agent Serpent.AI plugins.

#### Usage

##### Generate a Game plugin

`serpent generate game`

##### Generate a Game Agent plugin

`serpent generate game_agent`

### train

Train machine learning models based on presets shipped with Serpent.AI.

#### Usage

##### Train a context classifier

`serpent train context <epochs>`

* _epochs_: Number of epochs to train over. Optional. Defaults to 3

_Requires prior capture of context frames using `serpent capture`_

### capture

Capture game frames and screen regions. The game needs to have already been launched through a `serpent launch` command.

#### Usage

##### Capture full game frames

`serpent capture frame <game_name> <interval>`

* _game\_name_: The titleized name of the game (i.e. CoolGame).
* _interval_: The time to wait in seconds between captures. Defaults to 1 second.

_Captured game frames will be stored in 'datasets/collect\_frames'_

##### Capture full game frames with a context label

`serpent capture frame <game_name> <interval> <context_label>`

* _game\_name_: The titleized name of the game (i.e. CoolGame).
* _interval_: The time to wait in seconds between captures. Defaults to 1 second.
* _context\_label_*: The label to apply to the captured

_Captured game frames will be stored in 'datasets/collect\_frames\_for\_context/<context\_label>'_

##### Capture a predefined region of game frames

`serpent capture frame <game_name> <interval> <screen_region>`

* _game\_name_: The titleized name of the game (i.e. CoolGame).
* _interval_: The time to wait in seconds between captures. Defaults to 1 second.
* _screen\_region_*: A valid screen region name defined in the Game plugin.

_Captured game frames will be stored in 'datasets/collect\_frames/<screen\_region>'_

### visual_debugger

Launch the Visual Debugger.

#### Usage

##### Launch the Visual Debugger

`serpent visual_debugger`