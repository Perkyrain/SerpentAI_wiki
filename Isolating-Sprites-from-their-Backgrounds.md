The Serpent.AI framework ships with a utility function that attempts to isolate sprites from their backgrounds.

## The Problem

We want to register game sprites from the main menu of [OpenRCT2](https://github.com/OpenRCT2/OpenRCT2). The region of interest looks like this:

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/isolate1.png)

The sample may seem perfectly suitable, but if you pay attention you'll notice there is reduced opacity in the green background area of buttons. This means that the pixels from the background will change colors as the animated intro plays along. The provided sprite location / identification methods are vulnerable to this scenario and will fail most of the time.

## The Solution

The proper way to handle sprites like these is to isolate the static pixels from the dynamic ones which will yield a PNG with transparency. This resulting PNG can then be bundled with the game plugin and you will be able to detect / identify that sprite without issues.

Let's walk through the required steps to achieve this!

### Add a Screen Region for the Button in the Game Plugin

The first thing to do is to, as usual, add a screen region delimiting the button in the game plugin. Let's use the _Load Game_ button for this example. We measure the bounding box coordinates and add this entry to the screen regions:

`"MAIN_MENU_LOAD_GAME": (626, 429, 708, 510)`

### Capture the Button Screen Region

Next, we want to capture a few samples of that screen region. Make sure you have started the game through the `serpent launch` command and run the following command:

`serpent capture region OpenRCT2 1 MAIN_MENU_LOAD_GAME`

Let the capture run for around 30 seconds and interrupt it with Ctrl-C.

Navigate to *datasets/collect_frames/MAIN_MENU_LOAD_GAME* and you should see something similar to this:

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/isolate2.png)

### Isolate the Button Sprite

In IPython or the Jupyter Notebook, write the following code:

```python
import serpent.cv

serpent.cv.isolate_sprite("datasets/collect_frames/MAIN_MENU_LOAD_GAME", "sprite_main_menu_load_game_0.png")
```

### Admire the Results

Open *sprite_main_menu_load_game_0.png* and you should see something like this:

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/isolate3.png)

Voilà!

You can now bundle the transparent sprite in your game plugin.

