*Note: This class is still pretty rudimentary and needs to be expanded as of beta. Massive improvements are planned for 1.0*

The _GameFrame_ class is meant to represent a game frame (_no way!_) in the Serpent.AI framework.

## What's in a GameFrame Instance?

### Responsibilities

* Holding the image data of the game frame
* Tracking game frame metadata
* Exposing lazily-evaluated, cached frame variants
* Performing measurements against other GameFrame instances

### Concepts

#### Frame Variants

Frame Variants are transformations performed on the GameFrame instance's image data. They are exposed as properties. For performance reasons, Frame Variants are not precomputed but rather lazily-evaluated when a property is first requested. The result is cached so the next time this property is accessed, you will get instant results.

### Information

* `self.offset_x`: The X offset in pixels if the GameFrame instance is a subset of a larger GameFrame. Defaults to 0.
* `self.offset_y`: The Y offset in pixels if the GameFrame instance is a subset of a larger GameFrame. Defaults to 0.
* `self.resize_order`: The order of the spline interpolation to use when resizing the image data. Defaults to 1.

### Frame Variants

* `self.frame`: Full resolution image data
* `self.half_resolution_frame`: 1/4 resolution image data (width / 2, height / 2)
* `self.quarter_resolution_frame`: 1/8 resolution image data (width / 4, height / 4)
* `self.eighth_resolution_frame`: 1/16 resolution image data (width / 8, height / 8)
* `self.grayscale_frame`: Grayscale, full resolution image data
* `self.ssim_frame`: 100x100 grayscale image data to be used for SSIM computation

### Actions

### `self.compare_ssim`

**Method Signature** *compare_ssim(self, previous_game_frame)*

Return the structural similarity score (SSIM) between 2 GameFrame instances' *ssim_frame* variantsé

* **previous_game_frame**: A different GameFrame instance

## Where are GameFrame Instances Used?

GameFrame instances can technically be used anywhere, but the main usage is as the *game_frame* argument of game agents' frame handlers. In Serpent.AI, it is generally preferred to pass around GameFrame instances vs raw image data.