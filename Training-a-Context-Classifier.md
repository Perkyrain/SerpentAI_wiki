A Context Classifier is special type of machine learning model that can be trained by only using tools that ship with the Serpent.AI framework. Having a properly trained Context Classifier is essential to manage gameplay flows in your game agents. In this guide, we will build a Context Classifier from scratch using [Super Hexagon](http://store.steampowered.com/app/221640/Super_Hexagon/).

## Analyzing the Game

To build a great Context Classifier, you need to spend some time playing/studying the game; breaking it down in different sections (i.e. contexts). With Super Hexagon, we will want our game agent to be able to differentiate between 4 contexts: *main_menu*, *level_select*, *game* and *game_over*.

## Launch the Game

`serpent launch SuperHexagon`

## Capture Context Frames

Pretend we want to capture frames for *main_menu*.

1. Manually bring the game to the area that represents the *main_menu* context.
2. Start a context frame capture: `serpent capture context SuperHexagon 0.5 main_menu`
3. While the capture is running, interact with the game to generate as much visual variety as possible without leaving what you would consider the current context.
4. Wait until you have at least 200 frames captured (more is better!) and click the terminal and press Ctrl-C.
5. If you browse to *datasets/collect_frames_for_context/main_menu* you should see the captured frames.
6. Repeat until you have all other contexts captured.

## Train the Context Classifier

Next up is the easiest machine learning training operation you will ever perform in your life:

`serpent train context 15`

This will split your collected frames for every context in training and validation sets and train a model over 15 epochs. The training operation will take a few minutes if you are using the GPU-accelerated version of Tensorflow. Otherwise, be prepared to wait a long time.

When the training is over, you will have to analyze the terminal output. First, look at the accuracy scores of every epoch, we are looking for a few epochs above 0.9. If all your values are under 0.9, you likely need to collect more frames for every context. Otherwise, you have successfully trained a model! No early celebrating though... The reality is that you probably overtrained your model. That's fine. Let's take another look at the terminal output. We are on the lookout for a number of epochs sweet spot: Great accuracy score & next loss value barely diminished (or goes up!). Remember that sweet spot. We are going to train again but with that number of epochs. Say that number is 8:

`serpent train context 8`

Verify that the results are satisfactory. You model file is available at *datasets/context_classifier.model*

## Bundle the Context Classifier in your Game Agent

Copy the model to *plugins/SerpentSuperHexagonGameAgentPlugin/files/ml_models/*.

## Load the Context Classifier in your Game Agent code

Import the following class if not already present:

```python
from serpent.machine_learning.context_classification.context_classifiers.cnn_inception_v3_context_classifier import CNNInceptionV3ContextClassifier
```

Add the following to your game agent's constructor or frame handler setup function:

```python
plugin_path = offshoot.config["file_paths"]["plugins"]

context_classifier_path = f"{plugin_path}/SerpentSuperHexagonGameAgentPlugin/files/ml_models/context_classifier.model"

context_classifier = CNNInceptionV3ContextClassifier(input_shape=(384, 512, 3))  # Replace with the shape (rows, cols, channels) of your captured context frames
context_classifier.prepare_generators()
context_classifier.load_classifier(context_classifier_path)

self.machine_learning_models["context_classifier"] = context_classifier
```

## Use the Context Classifier at Runtime

```python
context = self.machine_learning_models["context_classifier"].predict(game_frame.frame) 
```
