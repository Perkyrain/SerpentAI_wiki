_Note: Before proceeding with this guide, make sure you have followed the installation guide for your OS, including running 'serpent setup'_

In this section, we will create our first, minimal SerpentAI project: A Hello World of sorts. The examples will use the Steam version of [Super Hexagon](http://store.steampowered.com/app/221640/Super_Hexagon/) as the target game, but you can swap it with another Steam game if you don't happen to own it.

## Game Preparation

Whenever you start working with a new game, there will be few preparation steps to go through:

1. Install the game
2. Run the game
3. Tweak the video settings:
    * switch to Windowed mode
    * select a Resolution (if applicable)
4. Write down the title of the game window

## Creating a Game Plugin

In Serpent.AI, to add support for a new game, you must create a plugin for it. The _serpent_ executable provides a code generator for that exact task, so creating one is trivial.

Run `serpent generate game`

You will be greeted by a prompt that looks like this:

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/hello1.png)

Provide the following answers:

* SuperHexagon
* steam

You will see some plugin installation output that should end with: _SerpentSuperHexagonGamePlugin was installed successfully!_

If you go and take a look at your plugins directory, you will see that files were generated for your Super Hexagon game plugin.

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/hello2.png)

## Tweaking the Game Plugin

To get Super Hexagon to launch through Serpent.AI, you will need to make a few edits in the game plugin.

Open _plugins/SerpentSuperHexagonGamePlugin/files/serpent_SuperHexagon_game.py_. The constructor should look similar to this:

```python
    def __init__(self, **kwargs):
        kwargs["platform"] = "steam"

        kwargs["window_name"] = "WINDOW_NAME"

        kwargs["app_id"] = "APP_ID"
        kwargs["app_args"] = None
        

        super().__init__(**kwargs)

        self.api_class = SuperHexagonAPI
        self.api_instance = None
```

* Replace _WINDOW\_NAME_ with the game window title you wrote down earlier (_Super Hexagon_ for Super Hexagon).
* Replace _APP\_ID_ with the Steam APPID of the game (_221640_ for Super Hexagon)

Pro-tip: You can find Steam APPIDs quickly by searching for game titles on [SteamDB](https://steamdb.info/search/?a=app)

## Launching the Game

Run `serpent launch SuperHexagon`

The game will proceed to launch. You will know that Serpent.AI can find the game window if it repositions it to the top left corner.

Leave the game running.

## Creating a Game Agent Plugin

In Serpent.AI, to receive frames from the game and interface with it, you must create a game agent plugin. There is also a code generator for this task to accelerate development.

Run `serpent generate game_agent`

You will be greeted by a prompt that looks like this:

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/hello3.png)

Provide the following answers:

* SuperHexagon

You will see some plugin installation output that should end with: _SerpentSuperHexagonGameAgentPlugin was installed successfully!_

If you go and take a look at your plugins directory, you will see that files were generated for your Super Hexagon game agent plugin.

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/hello4.png)

## Hello World

Open _plugins/SerpentSuperHexagonGameAgentPlugin/files/serpent_SuperHexagon_game_agent.py_.

Change _handle\_play_ so it looks like this:

```python
    def handle_play(self, game_frame):
        print("Hello World!")
```

Run `serpent play SuperHexagon SerpentSuperHexagonGameAgent`

If all goes well, you see a bunch of "Hello World!" start streaming in your terminal.

## Something a Little More Visual

Every time you saw a new "Hello World!" appear in the previous example, your game agent was fed the latest frame from the game. You might have noticed they were not streaming extremely fast. That's because there is a limiter that throttles the maximum amount of frames an agent can see and react to per second. The threshold is configurable (see _config/config.plugins.yml_) but there is a reason for the default of 2 FPS: Reacting to 2 frames per second is still 120 APM (actions per minute). This is probably faster then you personally react to most games. Unless you are dealing with a game that purely deals with analog controls (racing games, for example), playing at extreme APMs will make the gameplay look _unnatural_ and cause your agents to overreact in most cases.

But enough of this. Let's say that on top of printing "Hello World!" you would like to visualize the frames your game agent saw. Serpent.AI ships with an application called the Visual Debugger. The Visual Debugger allows you to examine image data while your agent is running.

Open another terminal and run `serpent visual_debugger`

You should see something like this appear:

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/hello5.png)

Resize it if needed and position it under the game.

Now change _handle\_play_ to this:

```python
    def handle_play(self, game_frame):
        print("Hello World!")

        for i, game_frame in enumerate(self.game_frame_buffer.frames):
            self.visual_debugger.store_image_data(
                game_frame.frame,
                game_frame.frame.shape,
                str(i)
            )
```

Go to the level select of Super Hexagon.

Run `serpent play SuperHexagon SerpentSuperHexagonGameAgent`

You should see the frames start cycling in the visual debugger, loosely matching the rate at which the "Hello World!" strings are printed.

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/hello6.png)

A game agent automatically maintains a small buffer of the frames it has received which make it possible to send them to the Visual Debugger in this example.

## Something a Little More Interactive

As a last step in this Serpent.AI Hello World, let's add in some key presses.

Change _handle\_play_ to this:

```python
    def handle_play(self, game_frame):
        print("Hello World!")

        for i, game_frame in enumerate(self.game_frame_buffer.frames):
            self.visual_debugger.store_image_data(
                game_frame.frame,
                game_frame.frame.shape,
                str(i)
            )

        self.input_controller.tap_key("right")
```

Can you guess what is going to happen?

Try it out! Run `serpent play SuperHexagon SerpentSuperHexagonGameAgent`

![](https://s3.ca-central-1.amazonaws.com/serpent-ai-assets/wiki/hello7.gif)

#### This concludes the Serpent.AI Hello World. It was a speck of sand on a beach in terms of what can be accomplished but hopefully it got you interested and you are curious to know more!