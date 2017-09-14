---
layout: post
title:  "A Quick Note on the Clojure (Backend) Web Space"
date:   2017-09-14 20:16:00 +0800
categories: misc
comments: true
---
*(Note to Experts: if you find any factual error in this note, please tell me by leaving a comment in the comment section below. Many thanks!)*

## Frameworks
Clojure as a community has emphasized two points (among others):
1. No complicated, Rube-Goldberg-Machine framework - prefer simple, focused, composable libraries for specific features. Pick your own choice for each feature and combine them in your application.
2. Data is a first-class-citizen. Instead of coupling it with function, let it stand alone and have functions operating on/transforming them.

That being said it is tedious and intimidating for a rookie to have to make informed choice on every step. So there is still some "frameworks":
<!--more-->

* [Luminus](http://www.luminusweb.net/) is a funky thing: while it is called a "micro framework" on its webpage, if you look closer you will find nothing beyond the series of guide webpages. It is not even a library! Instead it is just a lein template you can call to scaffold a web project with sensible default choices of libraries (per point 1 above) and all the tedious configuration done for you.
* [Arachne](http://arachne-framework.org/) (still in alpha) is more opinionated and co-opt point 2 above: the app is data-driven and the kind of messy configurations so common in other web framework (in other languages) are "real data" and given first-class, systematic/unified treatment. (Similar project and/or successor on this line is the [Duct](https://github.com/duct-framework/duct) framework)
* [Liberator](http://clojure-liberator.github.io/liberator/) is more a library targeted to those who want to write a RESTful web service backend. It has put heavy emphasis on fully embracing the doctrine of REST - even those that are less adhered to in practise ([HATEOAS][HATEOAS-Explain] comes to mind). It seems that its more pragmatic, _asynchronous_ counterpart - [Pedestal](http://pedestal.io/) - has more adoption in the real world.

## Build Tools
Just in case you don't know (yet), there are two:

* [Leiningen](https://leiningen.org/) (lein in short) is the standard one and is a task runner, declarative project configuration, and dependency management combined (sort of like Maven if you're in Java, npm if you're from Javascript, rake if you're in Ruby I think(?), XX if you're from language YY - really most modern languages nowadays have this particular combination as the default)
* [Boot](http://boot-clj.com/) is (ironically) the new kid on the block and pursue a procedural style that allows more flexibility/programmability. (E.g. Just Ant for Java, or Gulp over Grunt for Javascript guy - I know, that two are ancient in 2017, but still)

## Servers

_(I assume you have the Java background to understand, well, the Java part. If not, well that's too bad - wait until my planned next post where I will briefly talk about that in a tangential subsection :P )_

Of course two "universal" deployment options are:
* Build an uberjar that is completely standalone (web application is packaged with an embedded web server) and then just execute it on the JVM
* Build an (Servlet-compliant) uberwar and deploy it into any Java web server / web container

Unfortunately thing starts to get unruly at this point, so tighten your seat belt...

While Servlets are a good thing _in spite of_ how it might be [badly designed][Servlet-Critique], by abstracting away differences between web/application servers and presenting a uniform interface to application developer, maintaining this kind of standard (especially in Java) pretty much ensure that it will be slow to pick up on innovation in the web server space. But the rise of asynchronous web servers such as Node.js and other emerging forms of interaction (such as websockets) put on pressure for adoption. When the standard fails to adapt *fast enough* (Newer version of the Servlet spec has support for them), people will begin to work around (and hence without) it. So instead of a unified interface, expects custom API specific to each web server.

Another trend that may be unfamiliar to people used to working in an enterprise context is the rise of embedded server. [^1] It is interesting to note a kind of *Inversion of Control* here: instead of deploying an application to the container/server and letting the server's execution manage starting/stopping the app, we now have explicit control: the application contains the server and triggers the start/stop of the server through application logic. [^2] One advantage of this approach is that the *implicit* logic of a server managing a deployment is now [*explicit*](https://www.python.org/dev/peps/pep-0020/) so that when problem occurs, things are more traceable.

So, with these understanding, what clojure-specific web servers do we have?

First, [ring](https://github.com/ring-clojure/ring) is the Clojure analogue of the Servlet spec. It is _a HTTP server abstraction/interface plus a number of small libraries_.

* [http-kit](http://www.http-kit.org/) is a ring-compatible web server written in Java and Clojure.
* Jetty and Netty are classical Java web servers. Jetty is a standard, embedded Java server, while Netty is closer to Node.js with emphasis on the Transport Layer by providing access to TCP/UDP/socket server as well as supporting asynchronity.
  - Ring itself has an adaptor for Jetty, and that's an (easy) default choice in some contexts.
  - [Aleph](http://aleph.io/) is a server that's a thin wrapper over Netty. It can also acts as drop-in replacement for ring.
* [Immutant](http://immutant.org/) is a more heavyweight library/server that includes other services needed for more sophisticated system (messaging queue for instance). It can be deployed to either Java's WildFly or since 2.x the [Undertow](http://undertow.io/) web server.

## Libraries
There are just too many of them to discuss here. Instead I just highlight some of the foundational ones:

* For something similar to dependency injection in Java, we've got application lifecycle/state management libraries: [Component](https://github.com/stuartsierra/component) vs [Mount](https://github.com/tolitius/mount).
* For the routing part of your typical/classical MVC framework, we've got [Compojure](https://github.com/stuartsierra/component) vs just plain [ring](https://github.com/ring-clojure) (Remember ring is an API + a collection of libraries) vs [yada](https://juxt.pro/yada/index.html).

## And One More Thing...
I haven't mentioned database in this post so far. Well of course you can use any as long as JDBC supports it... database is not usually tied to a particular programming language. But since we're talking about clojure, it is criminal not to mention [Datomic](http://www.datomic.com/), a [super-innovative][Datomic-Innovative] "database" made by [Cognitect](https://cognitect.com/) (if it can be called that, per the talks by Rick on the [Database-as-a-value][Database-AsValue]). Unfortunately, it is one of the rare infrastructural piece of software in our ecosystem that is not Open Source nor free (they have a free version, but it has [restrictions](https://my.datomic.com/downloads/free)).

## References
Thanks Google!

* [Announcing Aleph](https://groups.google.com/forum/#!topic/clojure/9nsVazn44u0)
  - [Discussion on HN](https://news.ycombinator.com/item?id=1498198)
* [Advancing Duct](https://www.booleanknot.com/blog/2017/05/09/advancing-duct.html)
* [My first time using boot over leiningen](https://adambard.com/blog/i-finally-get-boot/)
* [Differences between Jetty vs Netty](https://stackoverflow.com/questions/5385407/whats-the-difference-between-jetty-and-netty)

----

[^1]: My own pet theory: Ever heard of DevOps? Instead of externalising config and dependent systems with a dedicated, separate team to manage them, why not put everything back to the hands of developer with the same powerful set of tool - general purpose programming language - to manage config? Really nice for lone wolf developer... the operation department, not so much.
[^2]: I will confess to having personal prejudice that embedded server are somehow slower: no they are not *a priori* slow. How could it be? Afterall web/application server are just codes, and where exactly are those code executed does not affect its performance.

[HATEOAS-Explain]: http://timelessrepo.com/haters-gonna-hateoas
[Servlet-Critique]: http://misko.hevery.com/2009/04/08/how-to-do-everything-wrong-with-servlets/
[Datomic-Innovative]: http://augustl.com/blog/2016/datomic_the_most_innovative_db_youve_never_heard_of/
[Database-AsValue]: https://www.infoq.com/presentations/Datomic-Database-Value
