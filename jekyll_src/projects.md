---
layout: page
title: Projects
permalink: /projects/
comments: true
---
An overarching theme of the Open Source Projects I choose to engage in/create is that they are infrastructural, innovative (hopefully), fun to me, and pushes my own skill and knowledge of both programming and computer science.

## Phase 1 (WIP)
[Clojure](https://clojure.org/) and [Python](https://www.python.org/) are chosen because they are both high level enough that I can do something substantial with them without requiring long blocks of uninterrupted time (i.e. [Flow](https://en.wikipedia.org/wiki/Flow_(psychology))). An additional consideration is that I really love the Lisp family of language, and Clojure is one of the most promising dialect with a modernized, pragmatic, innovative ethos, while managing to have a good community. And because it is still niche relatively speaking there is more possibility to make fundamental contribution to the ecosystem.

Projects started:
- [relabel](https://github.com/lemonteaa/relabel): An (almost) trivial declarative domain converter. Also a small exercise to get me started on Open Source Development.
- [test-runner](https://github.com/lemonteaa/test-runner)/[tester-nrepl](https://github.com/lemonteaa/tester-nrepl): I have been solely using [LightTable](http://lighttable.com/) as the IDE for Clojure, I loved how it takes the interactivity of REPL to a whole new level by integrating it right next to the text fragment of the editor. That said it is a pity Chris couldn't/didn't continue development on it and it is left in some kind of fuzzy limbo - LightTable in its current state is pretty far away from the full potential promised by the demos [[1]] [[2]] (which doesn't contradict the fact that it is still useful right now with only a fraction of it implemented). As a small step for pushing this vision forward, I'm trying to port the standard Unit Testing plugin you find in Eclipse to here. Although the console and the autotest utilities offered by existing libraries may be good enough for hackers, integrating it into the IDE allow some nice stuffs such as a tree view of all your test cases, and unified presentation of the diff of expected versus actual value (when they are complex data).

Projects contributing to:
- [Carmine](https://github.com/ptaoussanis/carmine): a clojure redis client. I just find it a pity that cluster support is 80% done by the last volunteer and then abandoned due to unforeseen circumstances. I've always thought that cluster is one of the critical feature of any [NoSQL](https://en.wikipedia.org/wiki/NoSQL) offers, so supporting it is absolutely essential if one's goal is to convince enterprise to adopt it. This is a lots of fun - figuring out the design and implementation feels like doing actual Computer Science ;)
- [Keras-RL](https://github.com/matthiasplappert/keras-rl): Keras is a wonderful Python library that aims to [democratize][Keras-Democrat] modern Artificial Intelligence by providing an easy to use, uniform interface to deep learning. Keras-RL is an extension of that project that does the same for Reinforcement Learning. As this is a fast moving field, and I love math, I feel I could potentially make a genuine contribution by helping with implementating some of the latest algorithm published.

[1]: http://lighttable.com/2012/04/12/light-table-a-new-ide-concept/
[2]: http://lighttable.com/2012/05/21/the-future-is-specific/

[Keras-Democrat]: https://blog.keras.io/on-the-importance-of-democratizing-artificial-intelligence.html
