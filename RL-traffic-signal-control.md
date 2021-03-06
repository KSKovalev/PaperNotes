## RL-based multi-agent system for network traffic signal control

1. Paper link: https://ieeexplore.ieee.org/document/5478405/
2. Paper PDF: http://web.eecs.utk.edu/~itamar/Papers/IET_ITS_2010.pdf

-----

## Abstract

This paper introduces a novel use of a multi-agent system and RL framework to obtain an efficient traffic signal control policy. The latter is aimed at minimising the average delay, congestion and likelihood of intersection cross-blocking. 

A five-intersection traffic network has been studied in which each intersection is governed by an autonomous intelligent agent. Two types of agents, a central agent and an outbound agent, were employed. 

The outbound agents schedule traffic signals by following the longest-queue-first (LQF) algorithm, which has been proved to guarantee stability and fairness, and collaborate with the central agent by providing it local traffic statistics. 

The central agent learns a value function driven by its local and neighbours’ traffic conditions. 

The novel methodology proposed here utilises the Q-Learning algorithm with a feedforward neural network for value function approximation. 

Experimental results clearly demonstrate the advantages of multi-agent RL-based control over LQF governed isolated single-intersection control, thus paving the way for efficient distributed traffic signal control in complex settings.


## Model
Four outbound intersections operate based on local information - longest-queue-first (LQF) algorithm.

The central intersection hosts an RL-based agent that controls traffic signaling.

Each intersection is labelled following the National Electrical Manufacturers Association convention, applying a [two-ring structure.](https://www.webpages.uidaho.edu/TrafficSignalSystems/traffic/instructor/ch3a.pdf)

<img src="https://i.paste.pics/56AVN.png">

* during each simulation time step, new vehicles are generated by a Poisson process at the end of the queue outside each outbound intersection
* vehicles at the even lanes can either go straight or turn right
* vehicles at the odd lanes should turn left to their designated queues
* all lanes can queue at most 40 vehicles

## Definition of the RL elements
**State:** an eight-dimensional feature vector with each element representing the *relative traffic flow* at one of the lanes. 
*Relative traffic flow* - the total delay of vehicles in a lane divided by the average delay at all lanes in the intersection

**Actions:** {(1,5), (1,6), (2,5), (2,6), (3,7), (3,8), (4,7), (4,8)}

**Reward:** 
<a href="https://www.codecogs.com/eqnedit.php?latex=D_{last},&space;D_{current}&space;\;&space;-" target="_blank"><img src="https://latex.codecogs.com/gif.latex?D_{last},&space;D_{current}&space;\;&space;-" title="D_{last}, D_{current} \; -" /></a>
the previous and the last intersection total delays accordingly

<a href="https://www.codecogs.com/eqnedit.php?latex=r&space;=&space;\frac{D_{last}&space;-&space;D_{current}}{max[D_{last},&space;D_{current}]}\in&space;[-1,1]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?r&space;=&space;\frac{D_{last}&space;-&space;D_{current}}{max[D_{last},&space;D_{current}]}\in&space;[-1,1]" title="r = \frac{D_{last} - D_{current}}{max[D_{last}, D_{current}]}\in [-1,1]" /></a>




## Experiments

#### Simulation setup and parameters
* one time unit = one time step = one discrete second
* intersection agents took actions once every 20 time units
* no more than one vehicle was allowed to traverse the intersection at each time unit
* if a light signal transitions from red to green, there is a 2 unit delay for red-clear
* traffic arrivals followed the Poisson distribution with average arrival rate ranging from 0.1 to 1.0
* to evaluate the performance, only statistics of vehicles which passed the central intersection was used
* the duration of each simulation run was 20 000 time units
* the weight decay factor was 0.05
* the Q Learning discount factor was chosen to be 0.95
* prior to measuring and evaluating the performance RL-based agent took 10 000 steps to learn the environment with a decreasing exploration rate.

#### Figures
<img src="https://i.paste.pics/56B3H.png" width="1000" height="398" alt="Screenshot">

## What things could be improved

In future work, the authors intend to extend the initial results obtained to include additional performance metrics,
such as probability of stopping and vehicle velocity jitter. 

Moreover, the basic five-intersection network considered here might be expanded to include larger traffic networks and more extensive collaboration among agents.
