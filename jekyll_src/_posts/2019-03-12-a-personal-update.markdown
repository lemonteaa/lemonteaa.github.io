---
layout: post
title: "A Personal Update"
date: 2019-03-12 22:00:00 +0800
categories: meta
comments: true
---

Hello everyone! I haven't updated this blog for a whole year, and I think it's about time to give you all an update on what's happened to me and this blog.

Long story short, I am not in a day job now - instead I'm in a DIY style technology retreat (sorta like the [recurse center](https://www.recurse.com/)) where I focus on myself - to explore technologies and methods I'm curious of, to expand my horizon, to challenge and push my knowledge, capabilities, and limit, and to reflect on my career path, as well as the meaning of technologies and its relation to human progress.

The road has been rougher than I planned (it always [is](https://en.wikipedia.org/wiki/Hofstadter%27s_law), isn't it? :wink:). As a result, I haven't been able to spare any time for Open Source Work/Blogging, which used to be done on the side.

That being said, I will be doing what I can to compensate somewhat:
<!--more-->

* In the spirit of Open Source, I am opening up [a new section](/drafts/) on this blog - it contains a collection of writings/works that are not *production-ready* yet, but which nonetheless may be already useful, in raw form, to some people.
* I will be shifting my overall direction to *more execution* (over introspection/learning) in this phase of the retreat, given that I've done a round of catching up with basic knowledge already. This will hopefully make some of the planned blog posts ready faster. (I will explain this point below)

When I just began this blog, my vision is for it **to be a place where I promote/advocate for relatively advanced software development technologies (interpreted in a broad way) in a forward-thinking, but still pragmatic manner**. Because of this I cannot just write any posts here: if I pick an easy topic, it will not be aligned with this vision and risks diluting/detracting from the main goal. On the other hand, for those topics that are aligned, they are either so beyond my comfort zone that I cannot honestly find anything substantial and valuable to say; or they involve hands-on projects that I haven't myself completed yet. So I'm in a bind - in order to be able to write a post, I have to either 1) learn something that's pretty hard relative to what I already know (I'm gradually getting there), or 2) do some projects that're just beyond the extent of my ability. (I'm trying - just not done yet)

Now that the main business is dealt with, below are some relatively tangent topics. Read at your leisure :grimacing:

## Rationale

Why pursue *advanced* software development? This is not as trivial a question as may be perceived by some geeks. The context is a rather unspoken cultural divide (compare: [the two cultures](https://en.wikipedia.org/wiki/The_Two_Cultures) of science vs humanities) in the software industry:

* On one side is the business culture, where economy reigns supreme and both engineering and technologies are seen as just a tool (among many others) to some ends, being subservient to business.
* On the other side is the [hacker culture](https://en.wikipedia.org/wiki/Hacker_culture), where technologies are seen as a potent, powerful force, that when correctly used can further human potentials by opening up new possibilities, especially those that are forbidden explicitly or implicitly by authorities and society. [^1]

If you're on the Hacker's camp, using advanced technique to push the state of the art of software development forward would be a no-brainer. What they may have missed is that they may think everyone think this is a no-brainer, when in fact this is not true. From the eyes of someone who live and speak business, this is far from being *a priori* true - they are justified only to the extent that it increases profit, and after accounting for the costs and risks involved in this path, **including the opportunity costs of pursuing other paths instead**.

So, does that mean I'm biased? That I'm just a naive geek? (I hope not, but hey, it's not productive to live in constant self-doubt) I still believe that there is unused potential in advanced method.

Software Engineering lives in quiet desparation of *just not working*. [^2] Despite decades of effort on almost every angle of research people can come up with, our industry, as a whole, still cannot make software consistently and reliably. Schedule delay, costly bugs, over-budget, all these nightmares are a fact of life, and life in this state is akin to what it is like before modern medicine - we can try a cure, but there is no guarantee.

What can we do then? Can this situation be improved? Some people believe that ultimately it is the [human factor](https://www.amazon.com/Peopleware-Productive-Projects-Teams-3rd/dp/0321934113) that limits what we can solve and that there is [no universal formula](https://www.amazon.com/Mythical-Man-Month-Software-Engineering-Anniversary/dp/0201835959), no magic cure. While I respect the importance of these factors, I also believe that an attitude of disregarding, or dismissing, technical factor as trivial is counter-productive - most of us are not operating at a range where adding/improving software development technologies give diminishing marginal returns. **Unfortunately, large returns may require global optimization - fundamentally changing the way we do development - instead of local, incremental improvements**.

In short, we do not yet have a *science* of software engineering, and hence the appropiate action to take in the long run is to explore, formulate, and test hypothesis, and to be especially aware of any potential pre-conceptions that may be completely wrong.

## T-model of skills and my project list

The T-model of skills posits that a good engineer should have a broad understanding of various disciplines at a basic level (the horizontal bar), with one (or more) specialty that he drills down into and possess deep, expert skills (the vertical stroke). My own target list with respect to softwares is as follows:

* AI - Mainly neural network and reinforcement learning
* DevOps - Making operation less of a black art
* Software Architecture and Design - life after MVC
* Modern Frontend web development - Single Page Application (SPA) Frameworks, and recent features in browsers (also, HTML5)
* Computer Graphics

My background's mainly in backend, and that's where I'm going deep into. To that end I have planned a number of exercises for myself:

* Build a Big Data Pipeline using: Onyx, Kafka, some NoSQL DB (Cassandra?)
* Realtime live chat/videa streaming type app using: [sente](https://github.com/ptaoussanis/sente), WebRTC etc
* Complex Domain Modelling with Datomic
* Microservice/API using: Lacina (for GraphQL), [yada](https://github.com/juxt/yada)?


## Closing

Thanks for bearing with me. May the source be with you!

---

[^1]: This does *not* mean that hackers are anti-social. It simply means that they prefer to judge the morality of things more on whether they are actually of benefits, than by the dictate of social norms *in and of itself*. See also: [Social Constructionism](https://en.wikipedia.org/wiki/Social_constructionism), [Critical Theory](https://en.wikipedia.org/wiki/Critical_theory)
[^2]: This is an open secret, and a kind of "family shame".
