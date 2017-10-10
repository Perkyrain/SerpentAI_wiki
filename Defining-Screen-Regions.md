*Note: This guide uses [spritex](https://github.com/codetorex/spritex) to measure screen regions. A valid Serpent.AI install should have all the dependencies to `pip install spritex`*

Defining Screen Regions in your Game plugin enables a lot of useful features for your GameAPI and GameAgent instances. In the following guide we will go over the procedure for measuring and defining Screen Regions. We will use [OpenRCT2](https://github.com/OpenRCT2/OpenRCT2) as our game but the procedure is the same for every game.

## Launch the Game

Run `serpent launch OpenRCT2`

## Capture Frames from the Game

1. Manually get to the area of the game containing the screen region of interest
2. Capture a few frames using `serpent capture frame OpenRCT2 1` (Your captures will be available in *datasets/collect_frames*)
3. Repeat for other areas of the game, if needed.

## Measure your Screen Regions in *spritex* and Register them in the Game Plugin

### Open a Captured Frame in *spritex*

`spritex datasets/collect_frames/<frame_file_name>.png`

You should see something similar to this:

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/screenregion1.png)

### Measure the Screen Region

1. Click the *Toggle Grid* button.
2. Zoom in with the mouse wheel until you see the pixel grid appear.
3. Click the *Select Region* button.
4. Drag a selection around the pixels encompassing your screen region

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/screenregion2.png)

5. Click the *Copy Region to Clipboard* button.

### Register the Screen Region in the Game Plugin

1. With your cursor positioned for the next entry in `screen_regions`, paste from the clipboard. You should see something like this:

```python
    @property
    def screen_regions(self):
        regions = {
            "REGION": (626, 348, 708, 428)
        }
```

2. Rename *REGION* to a proper label.

### You are Done! Repeat for Other Screen Regions as Needed