The _GameAgent_ class is meant to represent a generic agent that will interact with games in the Serpent.AI framework.

## What's in a GameAgent Instance?

### Responsibilities

* Registering and exposing multiple queryable machine learning models
* Holding a SpriteIdentifier instance that is aware of all the game's registered sprites
* Registering and exposing multiple frame handlers
* Allowing frame handlers to define a setup function
* Providing instructions on how to handle incoming game frames
* Relaying desired game input commands to an InputController instance
* Managing what gets displayed in the terminal at runtime
* Triggering analytics events at specific points in time

### Concepts

#### Frame Handlers

A Frame Handler is a high-level function that can receive GameFrame instances as input. Your Game Agent will only execute one Frame Handler at runtime, but you can create and register multiple. Think of Frame Handlers as modes of operation for a Game Agent. For example, the same Game Agent can define a _random_ play handler, a _scripted_ play handler and a _machine learning_ handler. They will all receive frames from the same game, but will react differently to them. Frame Handlers will generally end up being extremely complex and long functions, so feel free to split them in multiple smaller functions. Frame Handlers need to be registered in the constructor of a GameAgent for the latter to be able to be aware of them.

#### Frame Handler Setup

If your Frame Handler has code it needs to run before receiving its first game frame, you can define a setup function and register it in the constructor of the GameAgent. This allows you to end up with a cleaner constructor for your Game Agent, while still providing for the individual needs of a given Frame Handler.

#### Receiving GameFrame Instances

Your selected Frame Handler will be invoked in an infinite loop by the Game instance's _play_ method. Every time your Frame Handler returns, the next loop cycle begins and the most recent game frame at this point in time is passed to the Frame Handler. The important thing to grasp here is that the delay between frames is not guaranteed to be consistent depending on the execution time of your Frame Handler. If consistent spacing between frames is important to your Game Agent implementation, you need to make sure that ALL executions of the Frame Handler run faster than the configured FPS value in the Game's GameFrameLimiter. This will ensure that the same amount of time elapses between loop cycles.

### Information

A GameAgent's Frame Handler is a totally blank canvas for the developer to play with. This being said, you have a bunch of powerful tools in scope to help you out. 

* `self.game`: A Game instance representing the GameAgent's target game
* `self.game.api`: A reference to the target game's GameAPI instance
* `self.config`: The GameAgent's plugin configuration
* `self.machine_learning_models`: A dictionary containing a labeled collection of loaded machine learning models
* `self.input_controller`: An InputController instance used to dispatch desired game input
* `self.visual_debugger`: A VisualDebugger instance used to send image data to the Visual Debugger
* `self.game_frame_buffer`: A GameFrameBuffer instance containing previous GameFrame instances seen by the GameAgent
* `self.sprite_identifier`: A SpriteIdentifier instance preloaded with Sprite instances provided by the Game plugin
* `self.uuid`: A UUIDv4. Will be different every time the GameAgent is started
* `self.started_at`: A Python *datetime* object representing the timestamp of when the GameAgent was started

### Actions

### `self.load_machine_learning_model`

**Method Signature** *load_machine_learning_model(self, file_path)*

Attempts to unpickle the binary file located at *file_path* and returns the result.

* **file_path**: Location of a pickled machine learning model file.

*This method CANNOT be overwritten by plugins*

### `self.update_game_frame`

**Method Signature** *update_game_frame(self, game_frame)*

While the GameAgent's frame handler is working with a specific GameFrame instance, update the latter with the GameFrame from the top of the frame grabber's stack. In essence: Update the game frame without exiting the current frame handler loop cycle. Useful for more complex UI operations.

* **game_frame**: A GameFrame instance.

*This method CANNOT be overwritten by plugins*