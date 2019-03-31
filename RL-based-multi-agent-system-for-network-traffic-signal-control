## EX2: Exploration with Exemplar Models for Deep Reinforcement Learning

1. Link: https://arxiv.org/abs/1703.01260
2. Website: https://sites.google.com/view/ex2exploration

-----

### Problem

Modern deep RL methods do not by themselves solve the exploration problem when the rewards are rare and sparse
(e-greedy, gaussian noise etc. don't explicitly look for interesting states; 
[VIME](https://arxiv.org/abs/1605.09674) requires a model of the environment).


### Solution

The key idea of determining novelty of a state is how easy it is for a discriminator to distinguish a given state from other
states seen previously. 

Authors propose to train *exemplar models* for each state that distinguish it from other states.

More formally, assume we have a dataset `X = {x_1, x_2, ..., x_n}`. An exemplar model consists of a set of `n` classifiers
(discriminators) `{D_{x_1}, D_{x_2}, ..., D_{x_n}}`. For each `x_i` we want to train corresponding discriminator.

Let <img src="https://image.prntscr.com/image/kOafQLh2Tua0Qmy5GePgdA.png" height="20px"> denote the discriminator associated
with exemplar `x^*`. We will train it by maximum likelihood to produce 1 on `x=x^*` and 0 on `x`-s which are from background
data distribution <img src="https://image.prntscr.com/image/Gp3Ur4aRQiGXUNsZvvwHaA.png" height="20px">. Note that optimal
classifier doesn't produce 1 on the exemplar `x^*` because the exemplar may also come from background distribution as negative.

If we've trained such a classifier, it comes out that we could estimate density of the point `x^*`! The equation for it will be

<img src="https://image.prntscr.com/image/xiMGlKaJSv6rD26gh5DYxg.png">

So, if we trained the exemplar model on past states of our RL agent, we could estimate the density on new states. Then,
we could add an exploration bonus to visiting states which are novel (have low density or, equivalently, high discriminator
score). We do this by augmenting the reward:

<img src="https://image.prntscr.com/image/BdRhvmMlRxOGzgCmJbblCA.png">

Function `f` is a model choice and depends on task. The authors use `-log(p(s))` in their experiments. One also could use
ideas from pseudo-counts exploration: `f = 1/sqrt(N(s))`, where `N(s) = n*p(s)`.

### Details

#### How authors overcome intractable problem of fitting a discriminators for each point?

They use *amortized exemplar model*. Actually, they train only one discriminator for all exemplar states.
Here is an architecture:

<img src="https://image.prntscr.com/image/QKbgHwrhShaKaMFMJTxEHQ.png">

The parameters of both encoders are shared. They also use KL divergence between encoded representation of a state and `N(0, 1)`
distribution. So encoder actually produces a mean and logsigma of a gaussian distribution. Final density is produced by sampling
gaussian noise many times and averaging discriminator scores on these noises. This helps because sampling around latent code of
a state corresponds to taking *similar* states.


#### The algorithm pseudo-code

<img src="https://image.prntscr.com/image/gn_gc3ZRSLaSFKVC58KTgg.png">

### Experiments

* Three experiments with low-dimensional state space -- to assess whether EX2 can perform density estimation and compute
exploration bonuses
* Four high-dimensional image-based tasks of varying difficulty intended to evaluate whether density estimation provides
improvement in domains where generative modeling is difficult.

* EX2 significantly outperform other methods on Doom task and quite competitive on other tasks.

### What things could be improved

* Highly tuned EX2 specific parameters for each task.
