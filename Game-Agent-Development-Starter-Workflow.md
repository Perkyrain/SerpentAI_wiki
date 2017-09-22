## Prepare the Game

* Install the game
* Set the game to use windowed mode
* Select a suitable resolution for the work you are looking to accomplish
* Take note the window name of the game

## Generate a Game Plugin

* Generate a game plugin using `serpent generate game`
* Replace the placeholders inside the generated Game class, including the window name
* Try launching the game with `serpent launch <game_name>`
* Leave the game up and running

## Register Screen Regions

## Extract and Bundle Sprites

## Train a Context Classifier

## Develop Your Game API

## Generate a Game Agent Plugin

* Generate a game agent plugin using `serpent generate game_agent`
* Register your Context Classifier in `self.machine_learning_models`
* In your frame handler, predict context and implement logic for everyone of them

# The rest is up to you! Have fun!