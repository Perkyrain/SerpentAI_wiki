The _InputController_ class is meant to be a gateway to sending game input in the Serpent.AI framework.

## What's in a InputController Instance?

### Responsibilities

* Exposing basic and convenience keyboard inputs
* Exposing basic and convenience mouse inputs

### Concepts

#### Backend

Serpent.AI provides different backend options to be used with input controllers They are defined in the InputControllers enum:

* `InputControllers.PYAUTOGUI`: A backend that leverages the [PyAutoGUI](https://github.com/asweigart/pyautogui) Python library. Default for Linux & macOS. 
* `InputControllers.NATIVE_WIN32`: A backend that leverages Windows' SendInput DLL function. Default for Windows.

If you want to override your platform's default backend, an entry needs to be added to your Game plugins:

```python
class SerpentNuclearThroneGame(Game, metaclass=Singleton):

    def __init__(self, **kwargs):
        kwargs["platform"] = "steam"

        kwargs["input_controller"] = InputControllers.PYAUTOGUI  # <= Specify your InputController backend here 

        kwargs["window_name"] = "Nuclear Throne"

        kwargs["app_id"] = "242680"
        kwargs["app_args"] = None

        super().__init__(**kwargs)

        self.api_class = NuclearThroneAPI
        self.api_instance = None
```

#### Game Focus

Before dispatching inputs, an InputController instance will always make sure the game still has focus to prevent accidental inputs to other applications. If the game loses focus, the desired input will be cancelled.

#### MouseButton Enum

Whenever a mouse button is expected as an argument in a function, an item from the MouseButton enum should be passed:

* `MouseButton.LEFT`: The left mouse button
* `MouseButton.MIDDLE`: The middle mouse button
* `MouseButton.RIGHT`: The right mouse button

If you plan to use mouse input, make sure to import the enum:

`from serpent.input_controller import MouseButton`

#### KeyboardKey Enum

When a keyboard key is expected as an argument in a function, an item from the KeyboardKey enum should be passed:

[KeyboardKey enum](https://github.com/SerpentAI/SerpentAI/blob/dev/serpent/input_controller.py#L10)

If you plan to use keyboard input, make sure to import the enum:

`from serpent.input_controller import KeyboardKey`

### Keyboard Actions

### `self.handle_keys`

**Method Signature** *handle_keys(self, key_collection)*

Compare `set(key_collection)` to `self.previous_key_collection_set`. Release keys that aren't there anymore and press new keys. Keep other keys pressed. Use this method for more human-like keyboard input.

* **key_collection**: A list of valid keyboard key values that should be pressed.

### `self.tap_keys`

**Method Signature** *tap_keys(self, keys, duration=0.05)*

Press *keys*. Hold down for *duration*. Release *keys*.

* **keys**: A list of valid keyboard key values that should be tapped.
* **duration**: How long to hold the keys down (in seconds).


### `self.tap_key`

**Method Signature** *tap_key(self, key, duration=0.05)*

Press *key*. Hold down for *duration*. Release *key*.

* **key**: A valid keyboard key value that should be tapped.
* **duration**: How long to hold the keys down (in seconds).

### `self.press_keys`

**Method Signature** *press_keys(self, keys)*

Press *keys*.

* **keys**: A list of valid keyboard key values that should be pressed.

### `self.press_key`

**Method Signature** *press_key(self, key)*

Press *key*.

* **key**: A valid keyboard key value that should be tapped.

### `self.release_keys`

**Method Signature** *release_keys(self, keys)*

Release *keys*.

* **keys**: A list of valid keyboard key values that should be released.

### `self.release_key`

**Method Signature** *release_key(self, key)*

Release *key*.

* **key**: A valid keyboard key value that should be released.

### `self.type_string`

**Method Signature** *type_string(self, string, duration=0.05)*

Type *string* characters with a *duration* interval between.

* **string**: A string of characters to type.
* **duration**: How long to wait before pressing the next character (in seconds).

## Mouse Actions

### `self.move`

**Method Signature** *move(self, x=None, y=None, duration=0.25, absolute=True)*

Move the mouse cursor to (*x*, *y*) over *duration*. If *absolute* is True, (*x*, *y*) refers to coordinates in pixels starting from the top left of the display, otherwise they are considered offsets from the current mouse cursor position.

* **x**: The X pixel coordinate.
* **y**: The Y pixel coordinate.
* **duration**: Time to take to move the cursor to (*x*, *y*) (in seconds).
* **absolute**: Whether to use absolute or relative coordinates.

### `self.click_down`

**Method Signature** *click_down(self, button=MouseButton.LEFT)*

Press the mouse button corresponding to *button*.

* **button**: An item from the MouseButton enum.

### `self.click_up`

**Method Signature** *click_up(self, button=MouseButton.LEFT)*

Release the mouse button corresponding to *button*.

* **button**: An item from the MouseButton enum.

### `self.click`

**Method Signature** *click(self, button=MouseButton.LEFT, duration=0.25)*

Click the mouse button corresponding to *button*. The time between the press and release is defined by *duration*.

* **button**: An item from the MouseButton enum.
* **duration**: Time between press and release (in seconds).

### `self.click_screen_region`

**Method Signature** *click_screen_region(self, button=MouseButton.LEFT, screen_region=None)*

Move the mouse cursor to the center of *screen_region* and click using *button*.

* **button**: An item from the MouseButton enum.
* **screen_region**: Label of a valid screen region defined in the InputController's Game instance.

### `self.click_sprite`

**Method Signature** *click_sprite(self, button=MouseButton.LEFT, sprite=None, game_frame=None)*

Attempt to locate *sprite* in *game_frame*. If found, click the center of the location using *button*

* **button**: An item from the MouseButton enum.
* **sprite**: A Sprite instance.
* **game_frame**: A GameFrame instance.

### `self.click_string`

**Method Signature** *click_string(self, query_string, button=MouseButton.LEFT, game_frame=None, fuzziness=2, ocr_preset=None)*

Attempt to locate *query_string* in *game_frame* using OCR with *ocr_preset*. If a match within *fuzziness* is found, click the center of the location using *button*.

* **query_string**: The string to match in the game frame.
* **button**: An item from the MouseButton enum.
* **game_frame**: A GameFrame instance.
* **fuzziness**: Maximum Damerau–Levenshtein distance from the query string.
* **ocr_preset**: Label of a valid OCR preset defined in the InputController's Game instance.

### `self.drag`

**Method Signature** *drag(self, button=MouseButton.LEFT, x0=None, y0=None, x1=None, y1=None, duration=0.25)*

Drag using *button* from (*x0*, *y0*) to (*x1*, *y1*).

* **button**: An item from the MouseButton enum.
* **x0**: Initial X pixel coordinate.
* **y0**: Initial Y pixel coordinate.
* **x1**: Destination X pixel coordinate.
* **y1**: Destination Y pixel coordinate.
* **duration**: Time to take going from initial to destination pixel coordinates (in seconds).

### `self.drag_screen_region_to_screen_region`

**Method Signature** *drag_screen_region_to_screen_region(self, button=MouseButton.LEFT, start_screen_region=None, end_screen_region=None, duration=0.25)*

Drag using *button* from the center of *start_screen_region* to the center of *end_screen_region* over *duration*.

* **button**: An item from the MouseButton enum.
* **start_screen_region**: Label of a valid screen region defined in the InputController's Game instance.
* **end_screen_region**: Label of a valid screen region defined in the InputController's Game instance.
* **duration**: Time to take going from start to end screen regions (in seconds).

### `self.scroll`

**Method Signature** *scroll(self, clicks=1, direction="DOWN")*

Scroll *clicks* going *direction*.

* **clicks**: Number of scroll clicks.
* **direction**: One of "UP", "DOWN"