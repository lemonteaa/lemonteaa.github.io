---
layout: post
title:  "How to get into the brave new world of Neural Network and Reinforcement Learning"
date:   2018-02-08 22:00:00 +0800
categories: misc
comments: true
---
*(Note: This is a short preparatory post for the real one, where I plan to share my experience dabbling with the rusty but beautiful [keras-rl](https://github.com/matthiasplappert/keras-rl) library. I will release that after I finally fulfill my promise and deliver the PPO+A2C integration that has been dragged longer than I am comfortable with :stuck_out_tongue:)*

*(Hint: some of the not-explicitly-referenced links are just Wiki, but many others are hidden treasure troves. Thanks for all these authors for their amazing works! :blush:)*

Modern Artificial Intelligence is a paradoxical case of an unstoppable force meeting an unmovable object. On one hand, this field has been so hyped in the last few years that some people worry about a bubble and potential backlash (probably bad memories from the last [AI winter](https://en.wikipedia.org/wiki/AI_winter) ?); on the other hand its progress is undeniable even to hardcore skeptics - even if we assume that there will be no more new breakthrough (which is quite unlikely), fully exploiting/applying the technology available right now to its potential will still take a long long time (if nothing else, because the list of new applications opening up from this round of new methods is huuuge :wink: ). This is an interesting time indeed.

So, if you're here, you probably wants to get into the field, but just like any bandwagon, the signal-to-noise ratio isn't exactly nice. How do I gain a foothold and stay?
<!--more-->

I personally believe that principles matter. To borrow some armchair buddhism, things in this world are impermanent, always in flux - whatever libraries, frameworks, or even language you hold dear has no guarantee to stay alive, much less the same, as the industry moves forward (or in circle?) But ideas live on, and may even reincarnate when the time is right.

Which is why I'm now outlining a list of the enduring "basics", to answer this question.

To make things simple, it boils down to these few things:

# Fundamental

* Learn programming, preferably Python (because of its strong ecosystem, mainstream status, and beginner-friendliness)
* Learn some scientific computing (the usual barrage of [panda](https://pandas.pydata.org/), [matplotlib](https://matplotlib.org/), [numpy](http://www.numpy.org/) etc for Python), specifically functions/syntax for working with vector/matrix/tensor [^1] - i.e. [vectorization](https://stackoverflow.com/questions/1422149/what-is-vectorization) (See [this](https://www.mathworks.com/help/matlab/matlab_prog/vectorization.html?s_tid=gn_loc_drop) for the guide in matlab), and also try out some [symbolic computing](https://www.wolframalpha.com/input/?i=derivative+of+x%5E4+sin+x&lk=3) (The Wolfram code for that is `f[x_] := x^4 * Sin[x]` then `f'[x]`, see the [doc](http://reference.wolfram.com/language/ref/Derivative.html))
* Learn math (good if undergrad major level in both pure and applied maths, best if PhD in selected areas :smirk: )
  - Linear algebra (the neural interconnections are expressed by matrix, and [convolution](http://colah.github.io/posts/2014-07-Understanding-Convolutions/) is an important example of a [linear operator](https://en.wikipedia.org/wiki/Bounded_operator) [^2])
  - Calculus (to help do manual computation related to the quantified performance of a network)
  - Probability theory/Statistic/[Stochastic process](https://en.wikipedia.org/wiki/Stochastic_process) (The main trend of modern AI is statistical in nature[^3] - Instead of analyzing any particular training/testing data, we treat the whole ensemble/possible inputs seem in real life statistically, which is much more tractable. Stochastic process, which is roughly speaking the more eloquent brother of probability theory, is useful for more complicated setup like in reinforcement learning, where the basic framework is already [Markov Decision Process](https://leonardoaraujosantos.gitbooks.io/artificial-inteligence/content/markov_decision_process.html))
  - Optimization algorithm/theory (The whole Neural Network thing is just optimizing a function's fit to data by tuning parameters to minimize error while constraining the complexity of our function, to be deliberately provocative. Anyway, the first algorithm you will see is the [Stochastic Gradient Descent](http://ruder.io/optimizing-gradient-descent/), which comes from the simple and naive [gradient descent](https://distill.pub/2017/momentum/), but have some not-so-obvious new behavior.)

# Concepts in Machine Learning

Once you're done with that, or at least have a good enough understanding, go and learn basic concepts in machine learning:

* The data-centric, or statistical paradigm, 
* supervised vs (un/semi)-supervised learning, 
* the basic workflow, 
* feature engineering, 
* choosing algorithm/methods, 
* overfitting/generalization problem, 
* pitfalls in [testing](https://xkcd.com/882/) [^4] /cross validation

To list a few.

After that, it is really no different from any other learning:

Search, read, think/tinker, repeat

The end. (See? It's not that hard afterall :laughing:)

Actually, not yet... to help you along (and to thank you for bearing with me for a supposedly "short note"), here is a filtered list of links/resources I find helpful:

# References

For a comprehensive introduction to all these diverse ideas, the [e-book written by Leonardo Araujo dos Santos](https://www.gitbook.com/book/leonardoaraujosantos/artificial-inteligence/details) seems to do the tricks.

For understanding neural network, check out this [book](http://neuralnetworksanddeeplearning.com/). It's a bit long, but very worth it as a long term investment. If you're short on time/prefer a more hands-on approach, then see [Stanford's basic tutorial](http://ufldl.stanford.edu/tutorial/).

We will be skipping all those baroque network architecture as that's not our focus today.

For reinforcement learning, the resources are scattered:

* [Andrej Karpathy's blog post](http://karpathy.github.io/2016/05/31/rl/) is a popular introductory post that's nonetheless deep in some aspects.

* [Ruben Fiszel's blog post](https://rubenfiszel.github.io/posts/rl4j/2016-08-24-Reinforcement-Learning-and-DQN.html) - He is an intern who learnt massively in that short timespan and as a result has a trove of treasure/insights/experience to share

More formally, there are some tutorial series that gradually ease in people to successively more complex concepts: [this](https://medium.com/emergent-future/simple-reinforcement-learning-with-tensorflow-part-0-q-learning-with-tables-and-neural-networks-d195264329d0), and [this](https://medium.com/machine-learning-for-humans/reinforcement-learning-6eacf258b265).

In short, learn about the setup of reinforcement learning, then learn about the theory of MDP, then learn about Q-function/value-based approach, then policy-based method, then the actor-critic architecture.

Congratulations, you're now ready! :clap:

----

[^1]: Please note that while for computational purpose a vector can be treated as a 1 by n (or n by 1) matrix and a matrix can be treated as a second rank tensor, in math/physics they are not exactly the same thing. See [this](https://physics.stackexchange.com/questions/20437/are-matrices-and-second-rank-tensors-the-same-thing) for a physics point of view and [this](https://math.stackexchange.com/questions/412423/differences-between-a-matrix-and-a-tensor) for a math point of view.

[^2]: Well I cheated a bit, since there is both a discrete and continuous version of convolution, and for the continuous version you've got [analytic](https://en.wikipedia.org/wiki/Mathematical_analysis) issues such as [convergence](https://math.stackexchange.com/questions/172504/why-do-we-say-the-harmonic-series-is-divergent) or boundedness, etc. I'm not sure whether the [application of functional analysis in machine learning](https://en.wikipedia.org/wiki/Reproducing_kernel_Hilbert_space) is relevant at an introductory level though.

[^3]: Two remarks here. First is that if you want to go *really* hardcore, see the topic called "Statistical Learning Theory". It isn't exactly necessary if your goal is to merely understanding enough theory to be able to work effectively with those tools in practise though. Second point is that in the last ~~hype cycle~~ generation, the main trend of AI is to use [symbolic method](https://en.wikipedia.org/wiki/Symbolic_artificial_intelligence). Given the massive success story of AI recently, this predictably spaked a debate on the future of AI, especially since [Singularitarians](https://en.wikipedia.org/wiki/Technological_singularity) such as those in the [MIRI](https://intelligence.org/) or [Less Wrong](http://lesswrong.com/) believes that strong/general AI is not only possible, but could arrive pretty soon (and once you have that, it can then bootstrap/improve itself to become super-intelligent in short order). In particular, is statistical method really superior to symbolic method? See [this](https://www.tor.com/2011/06/21/norvig-vs-chomsky-and-the-fight-for-the-future-of-ai/) for an example debate.

[^4]: Jokes and obligatory xkcd reference aside, data dredging/[spurious correlation](https://www.wired.com/2013/02/big-data-means-big-errors-people/) is indeed a serious problem even in traditional statistics. Cross validation is one standard technique to avoid such trap. Here, the new problem is to test using the same data set you trained the program/model with. (Which is the generalization problem in the last bullet point I think?)


