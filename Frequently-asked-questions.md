# Here we have the most commonly asked questions with some helpful answers to them

## Are you playing the game?

The software is a framework that enables agents, read AIs, to train and play games.
In the current incarnation the framework is build upon a Binding of Isaac: Rebirth agent.
So No, nobody but the machine is playing the game.

## The AI is terrible, will it get better?

The AI is training, and in a very early stage at it. Usually the random factor is quite high and it still needs to learn a lot before it gets better. But it, probably, will get better. Maybe even good enough to beat Monstro.

## What is displayed in the Stream?

If you don't see and hear a person on stream, the AI is in training mode. Then you will find:
* the gamewindow, in the top left corner
* the Agent's version right below it
* the VisualDebugger in the lower left corner
* AI Output in the top left of the terminal in the right window
* HTOP HW Stats in the top right of the terminal
* some more stats in the rest of the terminal
* Twitch-Chat-excerpt superimposed on the terminal on the lower right screen corner
* Local time of the stream in the lower right corner of the screen, may explain why there is no streamer visible

If there is somebody visible: say hello in chat, he usually is kind enough to answer questions, being a Canadians and all.

## Does the AI know where Isaac and Monstro are on the screen?

All that the AI receives are
* Isaac's health
* Monstro's health
* All frames of the game
It has no concept of Monstro or Isaac coded in, it doesn't "know" what it is doing or where. All it "knows" is: if Isaac's health drops it gets penalized and if Monstro's health drops it gets rewarded. The training is done so that it figures out how to achieve the latter while avoiding the first.

## Are these six frames in the VisualDebugger all the AI has to learn from?

No, these are just some randomly chosen frames to show that the AI is using framedata to learn.

## What are the stats that are displayed there?

Either some short status of the mini batch training progress or:

* Current Mode
  * Can be either TRAIN or RUN, while the first means it is gathering data the latter means it is applying whatever it learnt until now
* Current Step
* Current Epsilon
* Current random action probability
* Action
* Action Type
  * Can be either RANDOM or PREDICTED, so either the AI chose what to do and predicted what would be best or it is exploring by random action
* Reward
* Loss
* Current RUN
* Current HEALTH
* Current Boss HEALTH
* Duration of the last run
* Some estimations about achieving meaningful epsilons 

## What is this _epsilon_ you keep talking about?

In the Machine Learning algorithm this value is used to express the difference between exploration and exploitation of knowledge. It is a value between 0 and 1, 0 meaning no exploration at all and 1 representing purely random actions.
The lower the epsilon is, the less random the behaviour should be.

## What is meant by Reward?

The AI is using reinforcement learning, for that it needs to get a classification of its actions, was it good or bad? Did the action help reaching the goal or did it put it further out of our reach?
To achieve this the agent rewards some bonus points for hitting Monstro and penalizes losing some of Isaac's health.

## What is Loss?

The difference between the maximal possible reward in a situation and the actual received reward.
It could be understood as the distance between where we should be after the prediction and where we really are.

## I heard a GONG, what does it mean?

An AI only run is going to happen (every 20 runs).

## I heard a HORN, what does it mean?

Isaac did it, it killed Monstro! Or maybe it just glitched and didn't find the Bosses health anymore...

## Why is Isaac just standing around?

It is learning! After each run the AI enters a mini-batch-training mode, where it will reflect on the past and try to adjust it's neural network accordingly.

## Will it only use the last run to learn from?

No, all previous frames are used for learning, not just the last one.