---
layout: post
title: "It's because of you, motherfucker, that we are not all using Lisp."
date: 2013-04-09 00:37
comments: true
categories: [fun, node.js, apache]
---

<iframe width="560" height="315" src="http://www.youtube.com/embed/bzkRVzciAZg" frameborder="0" allowfullscreen></iframe>

Transcript
----------

And in conclusion, we have found Apache to be excellent server for our web applications. Any questions?

_Yes. I have a question. Why didn't you use Node.js. Node.js is an event driven non-blocking IO server, that can be used to build high performance web applications._

That is an excellent question. We evaluated several alternative web servers, and concluded that, while options like Node.js are very interesting, Apache meets our needs and has a solid track record.

_But it doesn't have performance. Everybody knows that Apache applications are slow, because they use blocking IO and have context switches._

That's a commonly held belief, that threaded web servers are somehow less performant or scalable than event based servers. If fact, if you measure carefully, you'll find that both models have similar performance charasteristics.

_Threads don't scale, simple as that._

That may have been true 10 years ago, it's not true today.

_Node.js will run circles around Apache, because Apache was build before async was discovered._

This is where I'd typically stab myself repeatedly in the ears with a fork until I stop hearing you.

I've looked at Node. It uses a single threaded loop that dispatches events to handlers. It's a proven model that solves some concurrency problems, but at the cost of code complexity.

_It gives techies like me the control, to wring every last CPU cycle out of our servers._

it must feel empowering to be totally responsible to the performance of your application. To be always on the lookout for blocking operations that should be split into little pieces each perfectly tailored to optimize concurrent throughput, all the complexities of assembler with the efficiencies of JavaScript

_I'm a total speed junky._

Do you know what this reminds me?

_The invention of the transistor._

It reminds me the invention of threads. Threading libraries do exactly what you are doing manually. They break up pieces of code to be executed intermittently, switching from instruction that are waiting on IO, to instructions that are ready to run. But your sequential code stays intact. You may recall sequential code. That's the code you can read.

_But it's slow as a dog._

Your async program is like something from the 19th century Gothic horror story, drunk with your own sense of power, you reassemble pieces of code that were once coherent, stitching them together with event loops and call-back functions, until your monster, grotesque and menacing, is ready to be brought to life in a JavaScript VM. You throw the switch and the hideous creatures awakes, rises, and lurches forward, you simultaneously elated and terrified that something so unnatural could work at all. When you realized what you've unleashed, the pure immorality of it, your creation reaches out with its bloodied mangled arms and strangles you.

_But it's fast as hell._

If you are willing to suffer complex code for performance, why not write an Nginx module in C?

_Node.js is the most bad ass rock star tech to come out, since Ruby on Rails._

As much as I want to be optimistic and look forward to human progress, people like you stop me dead in my tracks. You are fanatic in the church of technology fashion. I could present you with fact after fact after fact that your thinking about asynchronous programing is completely wrong, but why bother when you equate technology to rock bands, fancy yourself a software hipster by wearing your Node T-shirt and celebrate horrible code. Maybe you're cool. Hell, maybe you have groupies, but when it comes to knowing what the fuck you are talking about, you have all facilities of a parrot that says non-blocking, over and over again.

_Non-blocking, is the secret in the async sauce. With it, you go fast, without it, you go slow._

Do you know when human discovered that the world is round?

_1492, Columbus sailed the ocean blue._

It was 240 BC, by a Greek mathematician named Eratosthenes. He conducted a simple experiment using the length of shadows and the distance between cities, to not only prove the earth is round, but also calculated its circumference.

_Sounds low-tech._

It was utterly brilliant. But somehow, for one and half millennia, it was common knowledge that the world was flat, hundreds and hundreds and hundreds years of raft ignorance. How in motherfucking hell does this happen?

It was you. You, who made claims about thread performance without measuring. You, who claimed hacking your code with a machete, turning it inside out into a twisted unrecognizable mass will somehow make it go faster. You, who spend your time alternating between problems that are already solved, and problems that don't actually exist.

You are the reason that science was set back a thousand years. The reason we've not cured cancer. The reason we've not solved world hunger.

It's because of you, motherfucker, that we are not all using Lisp.

_I'm sorry, what was the last part again?_

Nevermind.

_Did you just say Lisp?_

You misheard me.

_I could've sworn you just said Lisp._

If there are no other questions, this concludes my presentation.





