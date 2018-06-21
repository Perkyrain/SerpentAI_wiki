If you are working with a sprite-based game, you'll be glad to know that the Serpent.AI framework ships with powerful modules to address this use case. 

## Working with Sprites

The _Sprite_ class is meant to represent sprite image data. Animation states and alpha channels are supported. A Sprite instance doesn't do much on its own but it is the expected input of some higher level systems. It also precomputes important metrics such as signature colors and a constellation of pixels for each piece of image data in the animation states. Sprite instances are generated in one of two ways: Automatically for bundled sprites in a Game plugin or manually at runtime.

### Auto-Generated Game Plugin Sprites

As mentioned in [The _Game_ Class](https://github.com/SerpentAI/SerpentAI/wiki/The-'Game'-Class#sprites), [bundled game sprites](https://github.com/SerpentAI/SerpentAI/wiki/The-'Game'-Plugin#how-are-game-sprites-bundled) are automatically converted to Sprite instances at runtime, complete with a `self.sprites` mapping in the Game instance. These generally end up being reference sprites for identification and comparison.

### Manual Sprite Instance Creation

SpriteIdentifier and SpriteLocator instances are operated using Sprite objects so you will have to convert your image data to benefit from that functionality.

#### Creating a Sprite from Image Data

Converting image data to a Sprite object is simple, with one gotcha: The input is expected to be an image stack and not a single image. This is because our Sprite instance supports animation states and therefore multiple images can be provided. To go from your regular (*rows*, *cols*, *channels*) image data to (*rows*, *cols*, *channels*, *animation_state*), you can use the following snippet:

```python
import numpy as np
import skimage.io
image_data = skimage.io.imread(image_file)[..., np.newaxis]
```

You then create your Sprite like this:

```python
from serpent.sprite import Sprite
sprite = Sprite("MY SPRITE", image_data=image_data)
```

That's all that needs to be done!

#### Appending Extra Image Data to a Sprite

If you have extra image data (e.g. variants, animation frames) that needs to live under the same Sprite, you can use the following snippet to append it:

```python
sprite.append_image_data(extra_image[..., np.newaxis])
```

## Identifying Sprites

One common use case in game agents is to look at a specific region of the screen and try to identify which sprite it may be. A SpriteIdentifier instance preloaded with all sprites bundled in the Game plugin is available inside any GameAgent instance. This makes for very easy identification.

In Serpent.AI, there are 2 ways of identifying sprites: *Signature Colors* and *Constellation of Pixels*.

### Identify Using Signature Colors

In most cases, this approach will be the best and should be used. Any Sprite object gets its image data analyzed when initialized and part of the analysis includes determining a set of signature colors. These are the dominant colors in the image data.

In a nutshell: A query sprite's signature colors are compared to all of a Game instance's bundled sprites' signature colors. The match with the best score over a certain threshold is returned. This is a great approach because it is resistant to scaling and transforms. The only vulnerability of this approach are sprites sharing an identical color palette.

To identify a sprite using signature colors in a GameAgent:

```python
query_sprite = Sprite("QUERY", image_data=query_image[])
sprite_name = self.sprite_identifier.identify(query_sprite, mode="SIGNATURE_COLORS")  # Will be "UNKNOWN" if no match
```

### Identify Using Constellation of Pixels

This method is provided as a backup for when using signature colors doesn't work. It it less flexible: The query sprite needs to be the exact same size as the intended match. On top of determining signature colors, constellations of pixels are generated when a Sprite object is created. A constellation of pixels is a mapping between pixel coordinates and signature colors.

In a nutshell: A query sprite's constellation of pixels is compared to all of a Game instance's bundled sprites' constellation of pixels. The match with the best score over a certain threshold is returned.

To identify a sprite using constellation of pixels in a GameAgent:

```python
query_sprite = Sprite("QUERY", image_data=query_image[])
sprite_name = self.sprite_identifier.identify(query_sprite, mode="CONSTELLATION_OF_PIXELS")  # Will be "UNKNOWN" if no match
```

## Locating Sprites in a Game Frame

The framework makes locating image data inside a GameFrame instance trivial:

```python
from serpent.sprite_locator import SpriteLocator

# Assuming the existance of "image"
sprite_to_locate = Sprite("QUERY", image_data=image[..., np.newaxis])

sprite_locator = SpriteLocator()
location = sprite_locator.locate(sprite=sprite_to_locate, game_frame=game_frame)
```

*location* will either be a (*y0*, *x0*, *y1*, *x1*) tuple or None