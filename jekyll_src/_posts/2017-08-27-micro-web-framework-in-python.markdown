---
layout: post
title:  "Micro Web Framework in Python"
date:   2017-08-27 20:26:00 +0800
categories: exercises
comments: true
---
Web Framework is arguably the most often used kind of library in real world, as well as being something that keeps being reinvented for every programming language and every generation of programmer. Given such ubiquity, it is strange that it is not included in the canon of the undergrad Computer Science cirriculum alongside Operating System, Database, and Compiler. This post seeks to bridge this gap by presenting a series of short exercise that culiminates in building your own micro web framework. Have fun!

*Important Note:* This exercise is inspired by [Ruslan's "Let's Build A Web Server"][Ruslan-Web] series as a kind of follow up. As we will be using online resources to help, please read the reference section at the end first before beginning the exercise.
<!--more-->


## 0. Preparation
*(Difficulty: Easy to Medium+ depending on how serious you get)*

First we want to refine some aspect of the codes in the blog series to make further development on top of them more pleasant to work with.

a) You may come across problem when trying to test part 1 using telnet in window. It turns out that telnet may decide to send packets as it receives characters you type, rather than sending them all at once. Since the function `client_connection.recv(1024)` means trying to get **something** with an upper bound of fetching 1024 bytes at most, you may get cut off after entering a single character. What we really need is some sort of protocol coupled with a stream interface - we should be persistantly reading until we know enough about the beginning part of the content sent to know the actual length of the whole payload - in this case it is the `Content-Length` http header field that will tell us.

Extract suitable function from part 2's code to help with parsing the packet's content, and then write a code snippet to properly read the packet and insert it into the code in part 1. (Hint: use the readline and read function)

b) Add logging to the code in part 1 and part 2 so that we can debug more easily. After importing logging, add the line
{% highlight python %}
logging.basicConfig(stream=sys.stdout, level=logging.DEBUG, format='%(asctime)s [%(levelname)s] %(message)s')
{% endhighlight %}
for a minimal setup, then add log in places you think are important.

c) The code in part 2 skipped some details of the WSGI requirement to simplify things, and we need to add some of that back in.
i. The `PATH_INFO` parameter is currently the whole path. Write functions to extract query string into `QUERY_STRING` and correct the line for `PATH_INFO` correspondingly.
ii. Write functions to extract all http headers and add them to the environ dict in one stroke. (Be careful about special case of `CONTENT_LENGTH`)

(Optional) d) Study using multiprocessing to make a httpd daemon so that starting/stopping server is less tedious.


## 1. Elements of a Framework
A Web Framework provides its own model of how things work, as well as a more convenient and/or more powerful interface to the user than the actual interface to the web server (which in this case is the WSGI). They achieve this by layering on top of the basic interface, doing some of the tasks that can be generalized. We will build a heavily simplified micro web framework incrementally, starting with miscellaneous functions.

*(Difficulty: Easy)*

a) The first issue is that the data structure in WSGI is too low-level with everything packed in one single env dict - we want to present the data in a cleaner way that is also more meaningful from a user's point of view.

Let's create a class named `HttpReq` (Shortform for HTTP Request) that is just data structure:

{% highlight python %}
class HttpReq:
	pass
{% endhighlight %}

It should have the following attributes:

| Name       | Description |
|------------|-------------|
| url        | The URL being requested. |
| method     | `GET` or `POST` |
| headers    | a python dict whose keys are the header names and the values are the corresponding header values. |
| getparams  | if request method is `GET`, this should be a python dict of the request parameters whose keys are parameter names and the values are the corresponding parameter values. The value should be a python array if there is a repeat of parameter name. |
| postparams | if request method is `POST`, this should be a python dict with the request parameters in the same schema as getparams. |

i. Write a helper function `extractHeaders(env)` to extract headers from the env dict according to the requirement for the class `HttpReq`.

ii. Write a function `get_req(env)` to construct and return a `HttpReq` object from the env dict, conforming to the above specification.

*Note:* You are allowed to use the module `urlparse` (`urllib.parse` if using Python 3) in standard library for this question.

*(Difficulty: Hard)*

b) The second and third (and fourth) issues are that it is still too cumbersome to work directly with HTTP request and response (too much boilerplate code to extract data from request object manually as well as assembly/building response object), and for larger projects with many 'pages' located at various URL, doing everything in one function is confusing and difficult to manage (try searching through/jumping around the code!). Different frameworks may use different mechanisms, but some common ideas are:
- Use handler that focus on dealing with requests on one particular (or one family of) resource.
- Inject various kinds of parameters into the handler, and only require the handler to return the core content of the response (or return an abstracted response object)
- Some frameworks will also allow automatic configuration of the web application itselfs to varying degree.

In this exercise, we will implement a 'router' in the framework (explained in question 2 below), use dependency injection-like method to provide the parameters, and use decorator for auto-config (a similar thing is called annotation-driven config in Java).

Implement a decorator called `route` that is applied to methods (which is a kind of callable in python) acting as handler. The decorator should have the following arguments:

| Name   | Required?   | Description |
|--------|-------------|-------------|
| url    | *mandatory* | for which URL will this handler be able to process the request? |
| method | *optional*  | any restriction on request type? `GET` if handler only deals with GET request, `POST` for POST request only, and `ANY` for no restriction. Defaults to `ANY`. |

It should wrap the method and apply dependency injection by inspecting the method argument names and providing the corresponding request parameters (using `None` as value if it doesn't exist).

As an example:

{% highlight python %}
class TodoApp:
	@route('/todo/create', method='POST')
	def addTodoListItem(self, task, pos):
		# ...snip...
{% endhighlight %}

Here the route's URL is '/todo/create', the method is `POST`, and the decorator should extract request parameters `task` and `pos` if they exists.


## 2. Piecing Stuff Together
A router is something that route a request to a suitable handler for further processing. The handlers are matched/tested against the request by examining its HTTP method and URL, and in more advanced cases patterns/regex can be used when matching URL (which we won't do here). Usually a map/registry of handlers are internally recorded and used for matching.

*(Difficulty: Easy+)*

a) In older generations of web framework the user is responsible for providing/configuring the handler map manually/explicitly, for example by calling a function with the full handler map as argument, or by calling a register function for each handler/route. Newer generation can be auto-config as mentioned in Q1 above.

i. Write a function `buildHandlerMap(clz)`, that accept a class meta-object as argument, extract all handlers inside that class (i.e. methods in the class decorated with `@route`), and return the handler map. The map should be a list of 4-tuple with format `(<method name>, <URL in route decorator>, <method in route decorator>, <the handler callable>)`.

(Hint: modify your code in Q1 b to add suitable attributes to the callable, so that the meta-data stored in the decorator itself can be inspected)

ii. Write a function `dispatchReq(handlerMap, req)`, that when given the handler map built from Q2 a(i), and a `HttpReq` object, will perform the routing as described above. It should call the first handler in the list that matches successfully with req supplied as argument, and then return the result of that call, or raise an Exception with message `No route found` otherwise.

*(Difficulty: Medium)*

b) We are now finally ready to write the 'main function' of our framework. Finish the following code snippet:

{% highlight python %}
class microFrame:
	def __init__(self, clz):
		self.handlerMap = buildHandlerMap(clz)
	
	def app(self, env, start_response):
		# TODO
{% endhighlight %}

*(Difficulty: Easy+)*

c) i. Now test the framework you've just written by writting an example web application. Your test should include at least one route each for:

- Returning a html form to submit
- A page accepting query parameter called through GET
- A page that accept form submit through POST

You may optionally test for default argument in function in Python.

ii. Test drive your web application. Open an interactive Python session, and type suitable command to package a web application, "deploy" it to the custom web server (after enhancements in question 0), and start the server. Open your browser and tests that it behaves as expected.


## (Optional) 3. Extension
In this question we try to extend our web framework to support more functionality. We will provide only the goal - you are free to come up with your own idea! (See reference section for explanation, supporting materials etc if you get stuck)

a) Add support for manipulating Cookie.

b) i. Based on part a, now implement Session.

ii. Can you make your implementation of a and b(i) be thread-safe?


## Reference

### Q.0
The original tutorial/DIY series is at [here][Ruslan-Web] (Part 1, follow links at bottom to get to other parts), and we assume that the reader has already read (but not neccessarily worked out) both part 1 and part 2 throughout this exercise.

We will be working and perhaps tinklering with both HTTP's protocol detail and drill down on WSGI's interface a bit. For http, the authoritative reference is the [original spec][HTTP-Spec].

Python's standard socket library provide a function `socket.makefile` to allow accessing socket through a file-like api. However there are some subtlety involved and it have (previously?) some limitations. Nonetheless it is good enough for our exercise. See [this][StackOverflow-1] for hints on how to use it.

One of Python's strength has been in its battery included philosophy while providing an easy, accessible interface and supporting a wide variety of system integration tasks. Logging is taken care of in a similar spirit and it is in fact a built in library. Read [Python's doc][Logging-Intro] for an introduction.

It turns out that you cannot test the simple server in part 2 with chrome because of speculative connection that send nothing, see [this][StackOverflow-2] for explanation.

WSGI is an old school interface that still works - similar to the CGI interface at the dawn of web 2.0 one or two decades before. Refer to [the WSGI Community Website][WSGI-Community] for a brief description of the environ keys, and read [Clodoaldo Neto's WSGI Tutorial][WSGI-Tutorial-1] to quickily learn how to work with it practically.

For a concise but still in-depth explanation of the multiprocessing module, see [the Python Module Of the Week's entry][PyMOTW-Multiprocessing]. This website is also useful in general for getting a tour of various Python modules.

Aside from specific pages mentioned above, Python has a good amount of online materials ranging from beginner-level-tutorial to more advanced articles. One example is [The Hitchhiker's Guide to Python by Kenneth Reitz][Hitchhiker-Guide]. Meanwhile, do not forget that Python's [official documentation][Python2-OfficialDoc] has reasonable average quality too, so check it out before looking for supplementary materials.

### Q.1
Web framework is a foundational, classical piece of software and as such there has been numerous specific frameworks, in a wide variety of context (e.g. different language) throughout the years. In spite of "recent" innovations such as Ruby On Rails and Express in Node.js (and many new generation, lightweight frameworks emphasizing interactive development), their basic principles remain the same (and so in my opinion belongs to the realm of Computer Science). See [the MDN Web Docs' entry on Server-side Web Framework][MDN-WebFramework] for an introduction to what they do. (*Sidenote:* The MDN is also in general an authoritative reference especially for front-end developer)

Decorator is a somewhat tricky feature in Python because it uses higher order function in an essential way, and also because there are different use cases with subtly different syntax. Two blog posts that explain these points are [[1]][Decorator-1] at a beginner level, and [[2]][Decorator-2] that also covers the more advanced cases of using a class as decorator.

### Q.3
Cookie and web sessions are closely related concepts. Cookie solves the problem of maintaining state across HTTP requests (the HTTP protocol itself is stateless) by changing the protocol specification to require browser cooperation. Server can set a designated header in its HTTP response, which the browser should honor by saving the data in its own memory and repeating the data in all subsequent requests (until expiry or reset) in another designated header. See [Wikipedia's Article][Cookie-Wiki] for details and [RFC6265][Cookie-RFC] for the specification itself.

Web Session solve a similar class of problem as Cookie, but with the state stored on server side. The usual way to do it is to build on top of Cookie: the server generate a session ID and set it as the Cookie value. Any state that need to be saved is stored on server side by associating that data with the session ID, which can later be retrieved by lookup. Authentication can be achieved by choosing a suitable scheme for generating the session ID with various cryptographic/security properties (Full security requires many more considerations though). In this exercise you can ignore security issues and just use the `uuid` module in Python. See [[3]][Session-1] for an illustrated example. Also see [[4]][Session-2] for a condensed summary of the discussions above.

Concurrency is an intrinsically difficult problem that we sometimes cannot avoid (espcially more so in this age when the free lunch given by Mooer's law is basically over). Python offer a number of facilities for doing multithreading - see for example [[5]][Concurrency-1] for a catalogue of options.

Unfortunately it also turns out that concurrency is one of the weak spot in Python due to the infamous Global Interpreter Lock (GIL). In short, even if multi-threading is done correctly you may not get the desired performance boost. See [Jeff Knupp's blog post "Python's Hardest Problem"][Jeff-Hardest-1] for an account of the problem and its [follow up article][Jeff-Hardest-2] for a list of possible remedies. The standard advise given when asking this problem on forums such as stackoverflow is "Use multiprocessing instead of threading". Read [this][StackOverflow-3] for a summarized comparison.

*(Wow, you really read to the end! As a reward for that, [here](https://github.com/lemonteaa/python-exercise/tree/master/micro_web_framework) is my own work-out of these exercises - exluding bonus sections)*

[Ruslan-Web]: https://ruslanspivak.com/lsbaws-part1/
[HTTP-Spec]: https://www.w3.org/Protocols/rfc2616/rfc2616-sec4.html#sec4
[Logging-Intro]: https://docs.python.org/2/howto/logging.html#logging-basic-tutorial
[WSGI-Community]: https://wsgi.readthedocs.io/en/latest/definitions.html
[WSGI-Tutorial-1]: http://wsgi.tutorial.codepoint.net/
[PyMOTW-Multiprocessing]: https://pymotw.com/2/multiprocessing/
[Hitchhiker-Guide]: http://docs.python-guide.org/en/latest/
[Python2-OfficialDoc]: https://docs.python.org/2/index.html

[MDN-WebFramework]: https://developer.mozilla.org/en-US/docs/Learn/Server-side/First_steps/Web_frameworks

[Cookie-Wiki]: https://en.wikipedia.org/wiki/HTTP_cookie
[Cookie-RFC]: https://tools.ietf.org/html/rfc6265

[StackOverflow-1]: http://stackoverflow.com/questions/12203800/should-i-close-makefileed-socket-and-it-is-original-socket-independently
[StackOverflow-2]: http://stackoverflow.com/questions/4761913/server-socket-receives-2-http-requests-when-i-send-from-chrome-and-receives-one
[StackOverflow-3]: https://stackoverflow.com/questions/3044580/multiprocessing-vs-threading-python

[Decorator-1]: https://www.thecodeship.com/patterns/guide-to-python-function-decorators/
[Decorator-2]: http://scottlobdell.me/2015/04/decorators-arguments-python/

[Session-1]: http://machinesaredigging.com/2013/10/29/how-does-a-web-session-work/
[Session-2]: https://web.stanford.edu/~ouster/cgi-bin/cs142-fall10/lecture.php?topic=cookie

[Concurrency-1]: http://www.laurentluce.com/posts/python-threads-synchronization-locks-rlocks-semaphores-conditions-events-and-queues/comment-page-1/

[Jeff-Hardest-1]: https://jeffknupp.com/blog/2012/03/31/pythons-hardest-problem/
[Jeff-Hardest-2]: https://jeffknupp.com/blog/2013/06/30/pythons-hardest-problem-revisited/