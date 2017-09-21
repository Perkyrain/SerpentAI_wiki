The Serpent.AI framework ships with a small desktop application called the Visual Debugger. Its role is simple: Help the developer visualize in-memory image data at game agent runtime. To make an analogy, the Visual Debugger is like a `print` statement but for images!

## Launching the Visual Debugger

Like most things in the framework, launching the Visual Debugger is done through a _serpent_ command.

Run `serpent visual_debugger`

You should see an application looking similar to this open up:

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/visual1.png)

## Sending Image Data to the Visual Debugger

A Visual Debugger instance exposes a function to store image data to be consumed by the application.

### `self.store_image_data`

**Method Signature** *store_image_data(self, image_data, image_shape, bucket="debug")*

* **image_data**: A numpy ndarray of shape _(rows, cols, color_channels)_ and dtype _np.uint8_
* **image_shape**: The shape of *image_data*
* **bucket**: Which labeled bucket to display the image in in the Visual Debugger application 

#### Example

```python
import skimage.io
from serpent.visual_debugger.visual_debugger import VisualDebugger

image = skimage.io.imread("image.png")

visual_debugger = VisualDebugger()
visual_debugger.store_image_data(
    image_data = image,
    image_shape = image.shape,
    bucket = "0"
)
```

VisualDebugger instances can be instantiated anywhere in your code. Game Agents already have an instance in scope as `self.visual_debugger`. 

## Customizing the Visual Debugger's Buckets

By default, the Visual Debugger will expect image data in the following buckets: "0", "1", "2" and "3". These buckets can be customized by editing *config/config.yml*. The relevant snippet looks like this:

```YAML
visual_debugger:
    redis_key_prefix: SERPENT:VISUAL_DEBUGGER
    available_buckets:
        - "0"
        - "1"
        - "2"
        - "3"
```

Simply replace *available_buckets* with different keys. The Visual Debugger will adapt its display and allocate space for every configured bucket.