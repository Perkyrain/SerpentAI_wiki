# Ideas

**Fast and easy to implement:**
- [ ] Double DQN learning
- Basic idea: Implement 2 NNs where N1 estimates Q-Value, N2 is trained and N1 gets the weights of N2 each x steps --> Decorelate Q-Values (Updating Q-Value in step X-1 may lead to chose a similar state/action in Step X)
- https://arxiv.org/pdf/1509.06461.pdf easier explanation: https://jaromiru.com/2016/11/07/lets-make-a-dqn-double-learning-and-prioritized-experience-replay/
- [ ] SARSA 
- Basic idea: Do not calculate new Q-Value by using argmax(Q_t+1) but by using the real chose next action; see also "However, we found this to be much less stable than SARSA, with the Q-values rapidly diverging from reality, likely due to the iteration of the maximum operator during training." (https://arxiv.org/pdf/1702.06230.pdf) 
- https://studywolf.wordpress.com/2013/07/01/reinforcement-learning-sarsa-vs-q-learning/


**Somewhere in between:**
- [ ] Prioritizing experience 
- Basic idea: The key idea is that an RL agent can learn more effectively from some transitions than from others.  Transitions may be more or less surprising, redundant, or task-relevant. 
- https://arxiv.org/pdf/1511.05952.pdf easier explanation: https://jaromiru.com/2016/11/07/lets-make-a-dqn-double-learning-and-prioritized-experience-replay/
- [ ] Duelling DQNs 
- Basic idea: Using two networks combined in one, where Network1 estimates the quality of the state and Network2 estimates the quality of each possible action in this state 
- https://arxiv.org/pdf/1511.06581.pdf


**May take a while to implement:**
- [ ] A3C 
- Basic Idea: Use multiple Agents at once and combine their individual gradients to train --> Maybe not possible for Isaac 
- http://proceedings.mlr.press/v48/mniha16.pdf

**Side notes** extracted from a paper where they trained a DQN to play Super Smash bros (https://arxiv.org/pdf/1702.06230.pdf):

Epsilon value = 0.02

"While far from thorough, our attempts with different architectures did not yield improvements - some, such the 3 x 128 policy network, actually did worse.  On the other hand, the
number and sizes of the critic layers did not have much of an effect."

"Ideally, the learning rate would  be  as  large  as  possible,  while  still  ensuring  convergence. Often, some hand-tuning suffices; in our case, a learning rate of 1e-4 gave reasonable results."

"For each algorithm, we found little variance between experiments  with  different  initializations.   However,  the  two algorithms  found  qualitatively  different  policies  from  eachother.   Actor-Critics  pursued  a  standard  strategy  of  attacking and counter-attacking,  similar to the way humans play.
Q-learners on the other hand would consistently find the unintuitive strategy of tricking the in-game AI into killing itself. This multi-step tactic is fairly impressive; it involves moving to the edge of the stage and allowing the enemy to attempt a 2-attack string, the first of which hits (resulting in a small negative reward) while the second misses and causes the enemy to suicide (resulting in a large positive reward)."

**More side notes** extracted from https://github.com/dennybritz/reinforcement-learning/issues/30 :

Hi guys, I have spent a lot of time implementing my DQN code and I have solved many of the same challenges that you seem to be facing.
Right now I am crazy busy and I don't have the time to help you debug your code but I can give you some pointers of the things that are important and the things that aren't.
Also, I haven't read all of discussion or the code so I might be pointing out things that you already know. Sorry about that.

Important stuff:

    Normalise input [0,1]
    Clip rewards [0,1]
    don't tf.reduce_mean the losses in the batch. Use tf.reduce_max
    initialise properly the network with xavier init
    use the optimizer that the paper uses. It is not same RMSProp as in tf

Not really sure how important:

    They count steps differently. If action repeat is 4 then they count 4 steps for action. So divide all pertinent hyper-parameters by 4.

Little difference (at least in breakout):

    pass terminal flag when life is lost
    gym vs alewrap. Learning rate is different but If one works so will the other

I am probably missing a lot of other important titbits. If I remember anything else I will come back. But probably you will see big improvements if you implement all of this correctly.

If you have any doubts you can check out my code: https://github.com/cgel/DRL

Link Collection:
Overview over most of the mentioned methods: http://blog.bsu.me/294-per-doom/