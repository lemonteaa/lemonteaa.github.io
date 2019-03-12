---
layout: page
title: Drafts
permalink: /drafts/
comments: true
---

*(Disclaimer: This area may contain anything - from outright rambling, to ambitious plan in anger (that may or may not pan out once I cool down and regain clarity), to serious plan (that may still be worse than that of a mice), to things that are sort of done already, to abandoned stuff (either because I have no time to get to it yet, or it is really a dead end). Have fun treasure hunting!)*

Listed below is a bunch of documents I have written/are writing. They are globally shared Google Docs:

### Programming Books Outline - [Link](https://docs.google.com/document/d/1YUZ68xV6VV7oyhcfDdsblRYMkje9vMH74-Uv0ZcuW-E/edit?usp=sharing)

I plan to write two books on programming. Both are a kind of distillation of things I (think) I already knew/learnt in the last coupon of years, and in this sense they are my "past".

One of them, **Lambda Calculus for the Working Programmer - A Gateway to Programming Language Theory**, is fairly standard, matter-of-facts knowledges and so should be relatively easy to write - my main motivation for this one is that I love lambda calculus - I think it's awesome - and more programmers should have an intimate knowledge of it. Unfortunately while most university programs in CS includes a course on Turing machine and related models, lambda calculus doesn't seem to be as universal. A second motivation is that while there are books on the topic, they are either limited in scope or too scholarly. What I want is beginner friendly material that also have broad coverage of its many facets, both theoretical and practical.

The other one, **Patterns of Functional Data Processing - Business Application and Big Data**, is (much) more ambitious. I target it to be more of an opinion piece. Despite the grand sounding title, its origin is modest: in my career as an enterprise/business programmer, I observed something sad. Many programmers cannot write "plain data processing" code. [^1] What I mean is that they are simply code that takes as input some in-memory data, do some computations that are algorithmic in nature (no external service call, no database interaction, no fancy stuff), and output some in-memory data. This is of course one of the perfect use case of functional programming. And mind you, I exclude computations that requires clever algorithms - they should all be relatively straight-forward conceptually. As examples of what I have in mind, consider the following problems:

1. Write the function `histogram`, that take as input a list, and output a histogram in the format of a list of tuple, where each tuple is of the form `(unique_item, count)`.
2. Write the function `sliding_window`, that take as input a list and an integer `k`, and output a list of the same items enriched with sliding windows. For instance, if the input is `[a_1, a_2, ...]` and `k` is 3, then the output is `[ (a_1, a_2, a_3), (a_2, a_3, a_4), ... ]`. You may use any reasonable scheme to handle the boundary case.

Again, to avoid misunderstanding, I am not considering problems of analysing their algorithmic complexity or using a fast algorithms. And many people just stares at them with a blank mind, totally lost at what to do/how to begin. In my opinion, this is more reflective of a weakness in our education system than the capability of said programmers (indeed, my belief is that with the right kind of mentoring, they can master them easily).

Now, given that this planned book is an opinion piece and not a beginner book, it aims at something that is somewhat different. The original Design Patterns are there to provide common solution to common problem, and perhaps more importantly, to build up a set of vocabulary and concepts that can be used to help us think more systematically and productively. It is the vision of this book to do the same for functional programming applied to the limited domain of plain data processing - as far as I know, this is a niche that is a gapping hole, perhaps considered too trivial by most experts.

*Logistics:* My plan is to push them to [Leanpub](https://leanpub.com/) once I've got at least 1/3 of each book written.

### Clojure Beginner Tutorial - [Link](https://docs.google.com/document/d/1FkxM1qkbCLvK4ZOMcUCG9eNcc2mpg-DzsNv9AC9kt-w/edit?usp=sharing)

Why another tutorial? I hesitated a bit at first but went ahead after I looked at the states of tutorials in other popular programming language. What I found out is that if you do a simple minded google search (which is what a beginner would likely do), those languages will turn up with a long list of tutorials by many different people. This redundancy (or not, because different learners may require materials with different approach/presentation/media etc to "work") is advantageous, as it gives the impression that the language is popular and well-supported by a large community.

Anyway. So I’ve broken it into two part: one is a "normal" tutorial at a beginning level (but I still assume some experience in other languages), and the second is a guided project that will lead the learner through creating a complete, non-trivial application.

Unfortunately, given the amount of other committed learning/writing works I have, this item is relatively low on the priority list. I might be able to put more time into it if there is interests expressed from the community.

*Logistics:* Different types of material have a natural fit with different media formats. Right now, it seems to me that [Clojurecademy](https://clojurecademy.com/) is a good fit for the usual tutorial (although I may need to do some fine-tuning to make the long expository part fits in without disrupting the "flow" of learning and growing by doing). Meanwhile for the (extended) projects, in-browser, self-hosting REPL using the *Klipsi plugin* appears to be more flexible. (See [this](https://blog.klipse.tech/clojure/2016/06/07/klipse-plugin-tuto.html) and [this](https://github.com/viebel/klipse))


### Haskell Survival Guide - [Link](https://docs.google.com/document/d/1fIKOO6gOVPQpDHpfwif3dKbmFL8WjrQJqyJ1abTUnP8/edit?usp=sharing)

Haskell is, hands down, one of the most elite language out there, right along with the old guard of C++ (On this point I would like to commend the Haskell community for their recent efforts of outreach and making the language more approachable to outsiders by a combination of tutorials, ecosystem work, and PR). As in real life, the full picture is fairly complicated - it is a convergence of many factors that make it so - being a semi-research language, being hardline on adopting pure functional style, being lazy, its very advanced type system, among other things.

While Haskell has many merits as far as learning functional programming properly is concerned, I also personally believe that there are other, less favourable factors at play, some being purely a historical accident and not really anybody's fault, that acts a kind of "accidental complexity" and become a harmful obstacle to Haskell's adoption uptake. They might be justified as being the only feasible way to maximise Haskell's power at the expert end of the scale, but they come at the price of making it harder for beginners to learn.

To this end, this guide tries to make explicit some of the implicit assumptions of the language - the "ground rule" if you will - that one have to know in order to minimise pain and wrong turns while learning. The latter half of the guide (still unwritten and just an area of copy-and-paste from the internet, pieces of info that I find useful) aims to give a relatively short summary of the core API of the language, both the normal data structures stuff, and the more advanced type classes, especially the cryptic shorthand operators (my ultimate goal is to have a clean cheat-list of the symbols organised in a way that highlight the conceptual hierarchy)

### Introduction to Software Developer - [Link](https://docs.google.com/document/d/1jtt9k9sOr1LK29Z8JyPBox9Hno1r4xrHDm6t_GgYcg4/edit?usp=sharing)

A pretty old document I wrote in response to friend's question on the technical details of a software developer. I've filtered away some personal/private stuff. Worth taking a look at the part where I talk about tooling/ecosystem - they don't exactly focus on that in the CS program, and no one holds your hand in that area in the real world either - you are just expected to figure out this mess on your own. I've compared the situation across a few technology stacks (for some definition of "few"), and get to the same conclusion as David Nolen in his [talk](https://youtu.be/1VLN57wJAcU?t=1387): despite superficial difference, we share many common practises in practise.

### Summary of CSS - [Link](https://docs.google.com/document/d/1aDDhG2fPVNqm6-4YdgF_m_O2uYZwnYeBwllRnb1kccM/edit?usp=sharing)

Oh ya, I planned to write a short book that summarise how to use CSS properly - because its conceptual model is actually pretty counter-intuitive due to its original design for plain, one dimensional document layout (think Microsoft Word instead of Magazine). I didn't have time for that yet (hope to eventually get to it). In the meantime this doc is the same thing in point form.

### Python Escape Plan - [Link](https://docs.google.com/document/d/1DcS-jIIDS2xaYLv30VY_D2ntK9PQdR3Ppqh3MzBJkHY/edit?usp=sharing)

Originally targeted to be a comprehensive plan to learn Python specifically for becoming a data scientist/Machine Learning Practitioner to someone who have no programming experience at all, and to catch up quickly (within 20 days!).

### Gist - How Computer Hardware Works (I) - [Link](https://docs.google.com/document/d/1IrP7DOmL50WIaUZFw8XNpxhJtGptCP8vIbtTec9jAp8/edit?usp=sharing)

A big WIP. Planned to write it in order to summarise what I've learned studying Computer Architecture (beyond the superficial programming model, where I really go read about Virtual memory/cache, bus controller, CPU design, etc). If and when I'm done I may put it on Github's gist I think?

### AI in 2019 - [Link](https://docs.google.com/document/d/1VZDHynqWYPhzNmVzaklR9GLv_emTRKxfw_xpMTLywQg/edit?usp=sharing)

And finally, on the far end of the scale, this is some of my personal thought on "strong AI" etc.

---

[^1]: Well, perhaps my observation isn't entirely new - there's always the [classical piece](https://blog.codinghorror.com/why-cant-programmers-program/) "Why can't programmer... programs?" Which claims that even senior programmers cannot write a basic FizzBuzz.
