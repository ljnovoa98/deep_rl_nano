[//]: # (Image References)

[image1]: https://user-images.githubusercontent.com/10624937/43851024-320ba930-9aff-11e8-8493-ee547c6af349.gif "Trained Agent"

# Project 2: Continuous Control Report

![image2][image1]

### Methodology

The Deep Deterministic Policy Gradient (DDPG) algorithm utilized in the bipedal example was modified in order to solve the reacher Unity environment. The solution is valid for both the single and multi reacher environments. This report will include the results for the multi-reacher environment that includes 20 manipulators but equivalent plots will be located in the repository for the single reacher environment. This environment is unique when compared to the previous project as it utilize an action space that is continuous.

DDPG is an Actor-Critic reinforcement learning policy. It utilizes two neural networks: one for the actor and one for the critic. The actor develops a policy for mapping observations to actions for the given environment. The critic evaluates the actions of the actor by estimating the expected return.

The actor network consists of two layers of 256 neurons. The two layers are fully connected and map the input observation features to the action space. The first layer utilizes a ReLU activation function; while the second layer utilizes a Tanh activation function in order to bound the output between [-1, 1].

The critic network consists of 4 full connected layers. The first and second layer have 256 neurons and the third has 128. The first layer utilizes a leaky ReLU activation function. The output of this is concatenated with the action featurs and then passed through the next two layers utilizng leaky ReLU activation functions. The final layer outputs the single value representing the estimated Q value.

Experience replay buffer was used in order to randomly sample past experiences to influence policy updates. This can prevent the agent from learning a local maxima/minima that prevents it from improving with additional training.

### Parameters

These are the agent specific parameters used for this project:

```
BUFFER_SIZE = int(1e5)  # replay buffer size
BATCH_SIZE = 128        # minibatch size
GAMMA = 0.99            # discount factor
TAU = 1e-3              # for soft update of target parameters
LR_ACTOR = 1e-4         # learning rate of the actor 
LR_CRITIC = 3e-4        # learning rate of the critic
WEIGHT_DECAY = 0.0      # L2 weight decay
```

The `BUFFER_SIZE` defines the max length of the replay buffer.

The `BATCH_SIZE` defines how many experiences from the replay buffer to learn from.

The `GAMMA` value defines the discount factor applied to weight actions taken early into training more when updating the policy.

The `TAU` value defines the rate of mixing the local 
parameters into the target parameters for soft updating.

The `LR_ACTOR` value defines the step size for the solver utilized for the actor.

The `LR_CRITIC` value defines the step size for the solver utilized for the critic.

The `WEIGHT_DECAY` helps prevent overfitting by penalizing large weights.

These are the specific parameters use for training the agent:
```
n_episodes = 500   # maximum number of training episodes
max_t = 1000       # maximum number of timesteps per episode
```

The `n_episodes` value dictates the maximum number of training episodes if a solution does not meet the pass criteria.

The `max_t` value dictates how long to run the agent in the environment for each episode if done criteria not met.

### Results

The agent was able to exceed the desired average reward of 30 in 113 episodes. The plot below shows the average reward of the 20 manipulators for each episode of training.

![Image2](multi_reacher_training.png)

The testing resulted in the agent achieving an average score of 37.94 over the 100 testing episodes. Throughout working on the project, these parameter settings have solved the environment in a range of as little as 107 episodes and as much as 135. However, it has consistently completed training well under the objective 500 episodes (including the single reacher environment). All in all, the DDPG algorithm was very successful at solving for a policy capable of consistently performing well in the multi-reacher Unity Environment. The tresting scores observed during testing the agent from the above training is included below.

![Image2](multi_reacher_testing.png)

### Future Improvements

The first change I would make to attempt to improve learning and solve in less episodes would be to modify the training hyperperameters, such as the weight decay, and the agent parameters, such as the buffer size. These parameters were mostly modified to find something viable to solve the environment but are far from optimized values and, in the case of the weight decay, remove functionality by setting the value to zero. Performing a trade study varying a few of these parameters independently and coupled could provide valuable insight into how their values impact performance.

Another possible improvement would be to implement Proximal Policy Optimization (PPO). PPO might take a bit of work to get training effectively but could result in quicker training to surpass the desired score when fine tuned.