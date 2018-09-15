Before setting out to create the framework, one of the requirements that was established was portability and ease of distribution of created experiments. This applies to pieces of code for both game support and game agents. As such, Serpent.AI has been designed to be entirely plugin-based to fulfill this specification.

## Plugin Engine

Under the hood, the framework leverages the [Offshoot Plugin Engine](https://github.com/SerpentAI/offshoot) for Python 3.5+. Offshoot implements a very clear and easy to use plugin workflow and has useful features like unified plugin configuration access, dependency management and callbacks. This being said, you won't have to worry about having to learn Offshoot, as it is already implemented in the framework. Feel free to check it out though!

## Plugin Management

For now, plugin management is pretty rudimentary: You can activate/deactivate plugins through *serpent* commands to respectively make plugins visible/invisible to your code base.

## Plugin Types

### Game Plugin

* **Implementation Difficulty**: Easy / Medium
* **Intended Developer Audience**: Python developers. Image Processing experience will be useful.

#### Sample Development Tasks

* Configuring the plugin so the game launches correctly
* Defining screen regions of interest in various areas of the game
* Experimenting with OCR settings on different text blocks in the game and defining presets
* Building an API to perform common detection and UI operations in the game 

Game plugin development can be done totally independently from game agents. If you want to make the best plugin out there for you favorite game, you are encouraged to do so!

### Game Agent Plugin

* **Implementation Difficulty**: Medium / Hard
* **Intended Developer Audience**: Python developers with an understanding of Machine Learning & AI and/or Computer Vision & Image Processing concepts.

#### Sample Development Tasks

* Developing various frame handlers that approach the game or subsections of game differently (example: Random play, scripted play and machine learning model play)
* Training a context classifier to help identify various areas and menus of the game
* Training and registering machine learning models to handle precise tasks
* Combining all the tools offered by the framework and the game-specific API to create a loopable gameplay flow.

A Game Agent plugin always relies on a Game plugin. You can implement your own or browse the ones made available by the community.

## The Future

A plugin repository where developers can publish their creations is planned. You will be able to login through GitHub and select the repository that contains your plugin and register it. It will then be possible for people to directly `serpent install` your plugin.