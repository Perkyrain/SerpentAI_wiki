The _InputController_ class is meant to be a gateway to sending game input in the Serpent.AI framework.

## What's in a InputController Instance?

### Responsibilities

* Exposing basic and convenience keyboard inputs
* Exposing basic and convenience mouse inputs

### Concepts

#### Game Focus

Before dispatching inputs, an InputController instance will always make sure the game still has focus to prevent accidental inputs to other applications. If the game loses focus, the desired input will be cancelled.

#### MouseButton Enum

Whenever a mouse button is expected as an argument in a function, an item from the MouseButton enum should be passed:

* `MouseButton.LEFT`: The left mouse button
* `MouseButton.MIDDLE`: The middle mouse button
* `MouseButton.RIGHT`: The right mouse button

If you plan to use mouse input, make sure to import the enum:

`from serpent.input_controller import MouseButton`

#### Keyboard Key Values

When a keyboard key is expected as an argument in a function, one of the following values should be passed:

```python
['\t', '\n', '\r', ' ', '!', '"', '#', '$', '%', '&', "'", '(',
')', '*', '+', ',', '-', '.', '/', '0', '1', '2', '3', '4', '5', '6', '7',
'8', '9', ':', ';', '<', '=', '>', '?', '@', '[', '\\', ']', '^', '_', '`',
'a', 'b', 'c', 'd', 'e','f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o',
'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', '{', '|', '}', '~',
'accept', 'add', 'alt', 'altleft', 'altright', 'apps', 'backspace',
'browserback', 'browserfavorites', 'browserforward', 'browserhome',
'browserrefresh', 'browsersearch', 'browserstop', 'capslock', 'clear',
'convert', 'ctrl', 'ctrlleft', 'ctrlright', 'decimal', 'del', 'delete',
'divide', 'down', 'end', 'enter', 'esc', 'escape', 'execute', 'f1', 'f10',
'f11', 'f12', 'f13', 'f14', 'f15', 'f16', 'f17', 'f18', 'f19', 'f2', 'f20',
'f21', 'f22', 'f23', 'f24', 'f3', 'f4', 'f5', 'f6', 'f7', 'f8', 'f9',
'final', 'fn', 'hanguel', 'hangul', 'hanja', 'help', 'home', 'insert', 'junja',
'kana', 'kanji', 'launchapp1', 'launchapp2', 'launchmail',
'launchmediaselect', 'left', 'modechange', 'multiply', 'nexttrack',
'nonconvert', 'num0', 'num1', 'num2', 'num3', 'num4', 'num5', 'num6',
'num7', 'num8', 'num9', 'numlock', 'pagedown', 'pageup', 'pause', 'pgdn',
'pgup', 'playpause', 'prevtrack', 'print', 'printscreen', 'prntscrn',
'prtsc', 'prtscr', 'return', 'right', 'scrolllock', 'select', 'separator',
'shift', 'shiftleft', 'shiftright', 'sleep', 'space', 'stop', 'subtract', 'tab',
'up', 'volumedown', 'volumemute', 'volumeup', 'win', 'winleft', 'winright', 'yen',
'command', 'option', 'optionleft', 'optionright']
```

An enum is planned for consistency.

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

### `self.click`

**Method Signature** *click(self, button=MouseButton.LEFT, y=None, x=None, duration=0.25)*

Move the mouse cursor to (*y*, *x*) over *duration* and click using *button*.

* **button**: An item from the MouseButton enum.
* **y**: The Y pixel coordinate.
* **x**: The X pixel coordinate.
* **duration**: Time to take to move the cursor to (*y*,*x*).

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

**Method Signature** *drag(self, button=MouseButton.LEFT, y0=None, x0=None, y1=None, x1=None, duration=0.25)*

Drag using *button* from (*y0*, *x0*) to (*y1*, *x1*).

* **button**: An item from the MouseButton enum.
* **y0**: Initial Y pixel coordinate.
* **x0**: Initial X pixel coordinate.
* **y1**: Destination Y pixel coordinate.
* **x1**: Destination X pixel coordinate.
* **duration**: Time to take going from initial to destination pixel coordinates (in seconds).

### `self.drag_screen_region_to_screen_region`

**Method Signature** *drag_screen_region_to_screen_region(self, button=MouseButton.LEFT, start_screen_region=None, end_screen_region=None, duration=0.25)*

Drag using *button* from the center of *start_screen_region* to the center of *end_screen_region* over *duration*.

* **button**: An item from the MouseButton enum.
* **start_screen_region**: Label of a valid screen region defined in the InputController's Game instance.
* **end_screen_region**: Label of a valid screen region defined in the InputController's Game instance.
* **duration**: Time to take going from start to end screen regions (in seconds).

### `self.scroll`

**Method Signature** *scroll(self, y=None, x=None, clicks=1, direction="DOWN")*

Position the mouse cursor at (*y*, *x*) and scroll *clicks* going *direction*.

* **y**: The Y pixel coordinate.
* **x**: The X pixel coordinate.
* **clicks**: Number of scroll clicks.
* **direction**: One of "UP", "DOWN"