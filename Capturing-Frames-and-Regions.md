One very common task in the workflow of Game Agent creation is capturing game frames or regions of said game frames. In this section we'll cover common use cases and how to tackle them with Serpent.AI.

## Capturing Full Game Frames

### Example Use Cases

* Measuring game screen regions.
* Extracting game sprites.
* Testing image processing code.

### How It's Done

1. Launch the Game through the `serpent launch` command. (example: `serpent launch SuperHexagon`)
2. Get to the section of the game you are interested in manually.
3. Run `serpent capture frame <game_name> <interval>` (example: `serpent capture frame SuperHexagon 1`)

The captured game frames will be saved to _datasets/collect\_frames_.

## Capturing Labeled Full Game Frames

### Example Use Cases

* Building a dataset for game context classification.
* Building a dataset for other machine learning training tasks.

### How It's Done

1. Launch the Game through the `serpent launch` command. (example: `serpent launch SuperHexagon`)
2. Get to the section of the game you are interested in manually.
3. Run `serpent capture context <game_name> <interval> <context_name>` (example: `serpent capture context SuperHexagon 1 main_menu`)

The captured game frames will be saved to _datasets/collect\_frames\_for\_context/<context\_name>_. Since this is mainly meant for image classification machine learning tasks, the captured game frames are downsized to half-width and half-height.

## Capturing Game Frame Regions

### Example Use Cases

* Capturing rectangular game sprites
* Collecting non-rectangular game sprites in preparation to isolate them from their backgrounds
* Collecting samples with text to establish its OCR preset.

### How It's Done

1. Define at least one screen region in your Game plugin.
2. Launch the Game through the `serpent launch` command. (example: `serpent launch SuperHexagon`)
3. Get to the section of the game you are interested in manually.
4. Run `serpent capture region <game_name> <interval> <screen_region>` (example: `serpent capture region SuperHexagon 1 MAIN_MENU_OPTIONS`)

The captured game frames will be saved to _datasets/collect\_frames/<screen\_region>_.
