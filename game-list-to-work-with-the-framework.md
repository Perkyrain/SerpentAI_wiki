## Intro
Greetings, you want to start experimenting with Serpent.AI framework but dont know with which game you can start? Dont worry this page is here to help you finding your first (or maybe new) game to work with.
So what will you find here?  
On this page you will find a section with the games that were run on the [serpent\_ai\_labs](https://www.twitch.tv/serpent_ai_labs) or the [serpent\_ai](https://www.twitch.tv/serpent_ai) main channel with a few explanations on the process.  
then you will find the game list that you might use for an experiment, as long with an other list for the games tested by the comunity.
and last but not least a few tips on how you could find the reward function.

1. [The games already tested on lab](#The-games-already-tested-on-the-lab)
2. [the big list of games](#The-big-list-of-games-that-might-be-a-good-choice-to-work-with)
3. [games tested by comunity](#games-tested-by-the-comunity)
4. [tips to design the reward](#\[BONUS\]-tips-to-design-the-reward)


## The games already tested on the lab
Some games have already been tested here on the [Serpent\_AI\_labs](https://www.twitch.tv/serpent_ai_labs), here is the list with a bunch of infos on them and the links to the game plugin and game agent when they are availables.

### The binding of Isaac Rebirth
* available on: Windows, MacOS, Linux
* download here: <https://store.steampowered.com/app/250900/The_Binding_of_Isaac_Rebirth/>  
_note: you need the afterbirth+ dlc to use the in game debug console to teleport in monstro room_
* game agent here: 
    _the one listed here might not be the latest version running on the lab_
    * Game Plugin: <https://github.com/SerpentAI/SerpentAIsaacGamePlugin>
    * Game Agent Plugin: <https://github.com/SerpentAI/SerpentAIsaacGameAgentPlugin>

This one might be the experiment with which you came across the framework, you can write your own game agent or start playing around with the one already made by Serpent.  
you can teleport in monstro room by using the following command:  
usualy on qwerty keyboard console command interface is spawned by using '~', however it can differ regarding you keyboard layout (on azerty one, you will have to use the key above the tabulation one)  
syntax is: goto s.boss.<boss\_room\_id>  
```
goto s.boss.1010
```
if you want you can give him items with the following:  
syntax is giveitem <itemid>  
the following will give him soy milk and libra
```
giveitem c330
giveitem c304
```
here is some doc about the isaac console: <https://bindingofisaacrebirth.gamepedia.com/Debug_Console>  
here are some cool clips of the agent working:  
* <https://clips.twitch.tv/FunnyBlazingCoyotePartyTime>
* <https://clips.twitch.tv/WonderfulMuddyOtterKippa>

oh and btw it did it, was the 14/07/2018 around 6:20 AM GMT, clip here <https://clips.twitch.tv/DreamyUglyPicklesCurseLit>  
it did it again <https://clips.twitch.tv/SuspiciousModernHabaneroWOOP> 14/07/2018 around 10:30 AM GMT


## Horizon chase turbo
* available on: Windows, MacOS, Linux
* download here: <https://store.steampowered.com/app/389140/Horizon_Chase_Turbo/>
* game agent here: 
    * Game Plugin:  
    * Game Agent Plugin:   

One of the most successful experiment to this day, the game agent manage to rank 3rd after less that one day of training on one track.  
clip of the ai ranking 3rd in the race: <https://clips.twitch.tv/TriumphantInspiringShrimpTheRinger>  
Later the game agent has been extended to allow multiple track training.


### Super Flight
* available on: Windows  
* download here: <https://store.steampowered.com/app/732430/Superflight/>  
* game agent here: 
    * Game Plugin: <https://github.com/SerpentAI/SerpentSuperflightGamePlugin>
    * Game Agent Plugin: <https://github.com/SerpentAI/SerpentSuperflightGameAgentPlugin> 

<https://clips.twitch.tv/DependableBovineZucchiniCoolStoryBro>

### Super Hexagon
* available on: Windows, MacOS, Linux
* download here: <https://store.steampowered.com/app/221640/Super_Hexagon/>
* game agent here: 
    * Game Plugin:  
    * Game Agent Plugin:  

Actually this is the tutorial game for the framework
find the tutorial here [The Serpent.AI Hello World.md](The-Serpent.AI-Hello-World.md)  
you have also the "code along with serpent" vods on the the main channel, it will help you set up the game agent with it.

### The Elder Scrolls V Skyrim
_special edition or not it doesnt matter_  
* available on: Windows
* download here: 
    - <https://store.steampowered.com/app/489830/The_Elder_Scrolls_V_Skyrim_Special_Edition/>
    - <https://store.steampowered.com/app/72850/The_Elder_Scrolls_V_Skyrim/>
* game agent here:  
    * Game Plugin:  
    * Game Agent Plugin:  

**note** this is not a full game agent, it's only foccusing on the Lockpick "minigame".
to do this experiment a mod with a single small room with a locked chest has been made.
making one shouldn't be hard.



## The big list of games that might be a good choice to work with
click on the game name, to go to the store page,  
the list might move in a dedicated file as it grows  
currently we list only the PC games.  
later a section with older games (NES/SNES/GameBoy/MegaDrive ...) that can be emulated will be added, but any arcade game is likely going to work.  
note that non linux games might work in wine (but you will likely have to use a non steam version: CD, gog or humblebundle).

**be aware**: not all types of games are going to work  
a game like a open rpg (Zelda, Pokemon, ...) is not possible, or you'll have to focus on a specific part of the game (like the Skyrim lock-pick agent) ;  but a arcade game will most likely work as long as a reward can be defined.

   
|Game Name       | Windows | MacOs | Linux | Notes                       |
|:--------------:|:-------:|:-----:|:-----:|-----------------------------|
|[Boson X](https://store.steampowered.com/app/302610/Boson_X/)       | yes     |  yes  | yes   |                             |
|[Peggle](https://store.steampowered.com/app/3480/Peggle_Deluxe/)        | yes     |  yes  | no    |                             |
|[Peggle Nights](https://store.steampowered.com/app/3540/Peggle_Nights/) | yes     |  yes  | no    |                             |
|[Timberman](https://store.steampowered.com/app/398710/Timberman/)     | yes     |  yes  | yes   | very easy (avoid multiplayer)    |
|[Goat Simulator](https://store.steampowered.com/app/265930/Goat_Simulator/)| yes     |  yes  | yes   | easy                        |
|[Street Fighter 4](https://store.steampowered.com/app/21660/Street_Fighter_IV/)| yes     |  no   | no    | dont do online multiplayer  |
|[Street Fighter 5](https://store.steampowered.com/app/310950/Street_Fighter_V/)| yes     |  no   | no    | dont do online multiplayer  |
|[Cuphead](https://store.steampowered.com/app/268910/Cuphead/)       | yes     |  no   |  no   | (Bosses only planed on the lab) |
|[dinosaur game in Chrome](https://www.google.fr/chrome/index.html)|  yes  |   yes   |  yes  | free                     |
|[FF15 fishing mini game](https://store.steampowered.com/app/637650/FINAL_FANTASY_XV_WINDOWS_EDITION/)|  yes    |  no   |   no  | **might not work** due to qte needed to be quick and not displayer in a way very understandable by AI |
|[Snake game]()    |    ?    |   ?   |   ?   | you might find an implementation on you OS default games if not you can easily find one for free (as in freeware) on internet |
|[Defend the planet](https://store.steampowered.com/app/780330/Defend_the_planet/)| yes |   no  |  no   |                             |
|[Space blaster 8bits](https://store.steampowered.com/app/857110/SPACE_BLASTER_8_BIT/)|   yes   |  no   |  no   |                             |
|[QWOP](http://www.foddy.net/Athletics.html)           |   yes   |  yes  |  yes  |  Free  web browser game         |
|[Amputea](https://store.steampowered.com/app/289090/AmpuTea/)      |  yes    |  no   |  no   |                             |
|[Surgeron simulator](https://store.steampowered.com/app/233720/Surgeon_Simulator/)|  yes    |  yes  |  yes  |                             |
|[Happy Wheels](http://www.totaljerkface.com/happy_wheels.tjf) |    yes  |  yes  | yes   | Free web browser game, work as long as you have flash  |
|[SuperCratebox](https://store.steampowered.com/app/212800/Super_Crate_Box/)|  yes    | yes   |  yes  |     free                    |
|[The wizzard who fell in a hole](https://store.steampowered.com/app/562680/The_Wizards_Who_Fell_In_A_Hole/)|  yes    |  yes  |   yes |                             |
|[Multiball](https://store.steampowered.com/app/877060/MultiBall_BLADOSHARIK/)   |  yes    |  no   |   no  | not all levels might work   |
|[clone hero](http://clone-hero.wikia.com/wiki/Clone_Hero_Wiki)|  yes    |  yes  |  yes  |                             |
|[octo gravity](https://store.steampowered.com/app/877060/MultiBall_BLADOSHARIK/)|  yes    |  yes  |  yes  |                            |
|[burnout paradise](https://store.steampowered.com/app/24740/Burnout_Paradise_The_Ultimate_Box/) | yes |no | no | it could be a fun experiment but i dont know how we could design a reward (no speedometer), maybe using some kind of mod can help |
|[arcade pac man](https://store.steampowered.com/app/394160/ARCADE_GAME_SERIES_PACMAN/) | yes | no | no |  |
| [Atari vault](https://store.steampowered.com/app/400020/Atari_Vault/) | yes | yes | yes | this one has  atari games in it, use it if you dont want emulators |
|[Wormis](https://store.steampowered.com/app/466910/Wormis_The_Game/) | yes | yes | no | F2P |



## games tested by the community
here we list the games tested by the Framework users
with sucessful issue or not  
you can also found the list in the serpentHabitat repository

|Game Name       | Windows | MacOs | Linux |Notes and is it working or not|
|----------------|---------|-------|-------|------------------------------|
|&nbsp;|&nbsp;|&nbsp;|&nbsp;|&nbsp;|


## \[BONUS\] tips to design the reward
**note:** no code here but more tips on how you can use the UI to build your reward function.

lets take for example The binding of isaac experiment,  
to use in the reward function the game plugin monitors the evolution of the health bars for monstro and isaac, then regarding how thoses are changing we apply a certain reward.  
if isaac health bar is reduced, the AI gets a -1 reward (same when his hps are 0), and it wont be able to get positive reward for a few seconds ( due to invulnerability frames).  
On the other hand if monstro Hp bar is reduced it gets positive reward each time it lands a sucessful shot.

For the horizon chase turbo, the reward was defined by the current speed on the step, the higher the speed was the bigger was the reward.

So the general tips that we can learn from that is:
* find UI elements that display easy to use info such as health bars, numbers.
* try to find a relation between the goal of the game and thoses informations.
* try to keep the reward function simple.