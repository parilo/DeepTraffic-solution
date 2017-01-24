This is my top 1 solution to the [DeepTraffic](http://selfdrivingcars.mit.edu/deeptrafficjs) contest which allows me to reach 75 miles per hour.
solution.js contains solution and trained neural network. But you might not get 75 with this network because I get 75 in process of training with "Submit model to competition" button.
If you prefer video see video explanation at the end of description.

# Markov State

First, I used big field of view. LanesSide = 3, pacthesAhead = 50, patchesBehind = 10. And here comes first important thing as for my opinion. I set temporal window 0. Why? Sooo. We use deep q networks algorithm. It is reinforcement learning algorithm. It receives state vector and provides an action that we need to apply. Regarding state vector we need to keep it minimalistic and as much markov as possible.

So, what is markov state? Basically it is state that contains all information we need to predict later behaviour of our environment. If we use just one patch above the car we will not be able to predict anything. Then we use wider field of view. Patches of the field of view contains information about velocity of the object above the batch. So we receive information not only about positions but about velocities of other cars as well just in one time step. What about other time steps? I think it might be useful to determine other cars drive style or other sophisticated things about other cars behaviour. But, I believe that other cars have same and random behaviour, so it's useless to try to predict random. In another hand, positions and velocities are a lot of useful information. Also using other time steps will not slow computation for us.

I use 500 000 (five hundred thousand) train iterations. It's big but not huge)

# Neural Network architecture

As for neural network architecture. We have big freedom here. So even three layers of eight neurones shows 70 mph (seventy miles per hour). And I don't think that we need big amount of neurones here. I have a number of different architectures and stayed at 4 layers with 36, 24, 24, 24 neurones.
Second important thing I want to mention that we need tanh activation on last hidden layer. ReLU is quite good and let us to have deeper networks, but for me pure ReLU on all layers tend to diverge in reinforcement learning. Here I use tanh on all layers but relu is also possible if you have tanh on the last hidden layer.

# Gamma

I used 128 one hundred twenty eight batch size and default learning rate.
As for experience replay size. We need much bigger one then default. for example 100 000. In deep learning it's better to have big datasets.
Start learn threshold, 5000 (five thousand), to be able to accumulate different behaviour before learning.
Gamma is very important parameter for DQN. It defines how sensitive our car for future situations. gamma is between 0 and 1.
And if it is 0 it will not consider its future.
If it is 1 it will consider situations that may happen in the deepest future. You likely want to use 1. But with 1 the algorithm tend to diverge. So typical value is 0.9 (nine). I've used 0.98 (ninety eight).

# Results

With this parameters I get 75.04. But there is random in submission. So the same submission may vary about 0.5 mph. So try multiple times.

It drives pretty well but I spotted one bad situation when car is blocked by surrounding cars. It sometimes good to use break to live such conditions, change line and pass. But instead car waits. To handle this I decided to train second model and use first for pretraining. I edit downloaded file, increase batch size and decrease learning rate. It allows me to get 75.28. I had even bigger values on evaluation run.

So. There is veeeery simple explanation of using DQN and reinforcement learning. If you interested I advice to watch Google DeepMind David Silver [marvelous course](https://www.youtube.com/watch?v=2pWv7GOvuf0) on youtube. It is very clear and interesting.

[![DeepTraffic solution](http://img.youtube.com/vi/lQ-vjvyxD_w/0.jpg)](https://www.youtube.com/watch?v=lQ-vjvyxD_w)
