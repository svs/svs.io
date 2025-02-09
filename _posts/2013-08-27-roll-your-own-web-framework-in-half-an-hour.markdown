---
layout: post
title: Roll your own web framework in half an hour
date: '2013-08-27 15:36:00'
---

<h3>i.e. Fun With ActionDispatch</h3>

<p>So, a long time ago, in a land far far away there once lived a powerful programmer and this programmer had created a web framework called Rails. Rails was a massive and monolithic framework and about as hard to tame as a dragon and so the Ruby community went to work on solving that particular problem. Armed with hexes, swords and gems, the community surrounded the dragon and cut him up into tiny pieces with the result that what we today know as Rails is mostly a curated (or omakase, iyw) collection of modular functionality that actually works independently of the framework. The dragon got turned into a bunch of tiny and not too ugly dragons that play well with others.</p>

<p>So it has come to pass that making your own web framework in Ruby has now become a trivial undertaking, one that with practice can be completed in the first ad break of the Saturday night Bollywood blockbuster on Sony TV.</p>

<p>So, what&rsquo;s in a framework then? In order to complete our framework before Katrina Kaif finishes eating her chocolate, we&rsquo;re going to cheat a bit and change the definition of web framework. Well, we&rsquo;re going to change it from Rails&rsquo; definition to something more in tune with this idea from Bob Martin from his eye-opening talk called Architecture The Lost Years <a href="http://www.youtube.com/watch?v=WpkDN78P884" target="_blank">http://www.youtube.com/watch?v=WpkDN78P884</a> (Highly reccommended if you haven&rsquo;t seen it already) -  A web framework is something that makes it possible to serve your application on the web.</p>

<p>By this definition, the web framework only does all http-y things basically translating http requests into method calls to the domain logic of your application and returning nice status codes and so on to complete the response. It also handles sessions, cookies and user authentication. All the domain logic including persisting data is handled by your application which sits inside its own gem.</p>

<p>So before we go any further, let&rsquo;s answer the question that is burning in everyone&rsquo;s mind - <a href="http://en.wikipedia.org/wiki/The_Killing_(Danish_TV_series)" target="_blank">Why did Sarah Lund do it</a>? Err&hellip;.sorry&hellip;wrong audience. The question we&rsquo;re going to answer is - For helvede dude! Why make your own framework?</p>

<h3>Seriously?! &lsquo;Because we can&rsquo; doesn&rsquo;t answer anything!</h3>

<p>Let me say this upfront. The chance that your new framework gets used by anyone apart from you answers true to #nil? However, there&rsquo;s no doubt that you&rsquo;re going to have a better understanding of many of the components that make up your day-to-day experience as a Ruby developer.</p>

<p>Also, Rails controllers are - how do I put this gently - pretty weird! There you are reading about this wonderful thing called Single Responsibility Principle and then you look at your Rails controller blithely ignoring object oriented purity and yeah, it makes you think. Don&rsquo;t take my word for it - <a href="http://www.youtube.com/watch?v=iUe6tacW3JE&amp;feature=player_detailpage#t=381" target="_blank">Gary Bernhardt nails it right here </a></p>

<p>Maybe Rails is just a little too omakase for you. Maybe you don&rsquo;t entirely trust the LiveController. Maybe you just want to know what makes up a web framework. Whatever your reason for rolling your own framework, Rack and ActionDispatch have your back.</p>

<p>In the following example, we&rsquo;re going to build a little chess playing API. Very little. Just enough to prove that we have a real web application. The source code for all this is here: <a href="https://github.com/svs/ryowf" target="_blank">https://github.com/svs/ryowf</a></p>

<h3>Rack</h3>

<p>Everyone knows what Rack is, right? It sits behind your web server, turns your web requests into nice Ruby hashes and provide a uniform API so you and your pair don&rsquo;t have to spend hours arguing over names. A rack app is anything that responds to #call(env) with something like [200, {}, &ldquo;hello world&rdquo;]. Simple. Our little Rack app looks like this.</p>

<script src="https://gist.github.com/svs/6354679.js"></script><p>We have a method called #call which receives the env (which in turn contains the request and associated data) and responds with something Rack can send back to the web server. The first thing we need to do is to figure out which functionality was requested, for which we need a router. Routers are built with ActionDispatch.</p>

<h3>ActionDispatch</h3>

<p>ActionDispatch is a lovely little gem which lets you do this -</p>

<script src="https://gist.github.com/svs/6354584.js"></script><p>It&rsquo;s basically the Rails router and the backbone of our web framework. Our router looks basically like this</p>

<script src="https://gist.github.com/svs/6354722.js"></script><p>The router in turn responds to call by calling the &ldquo;handler&rdquo;.</p>

<p>A small word about the design of our framework here. Since we don&rsquo;t like Controllers that do 10 things, our controller only does two or three. Still a violation of SRP but hey - a tremendous improvement. So what we&rsquo;re aiming for is for every controller action to be its own class. for example, instead of saying 
<script src="https://gist.github.com/svs/6354772.js"></script></p>

<p>we want to say something like this</p>

<script src="https://gist.github.com/svs/6354889.js"></script><p>This has a number of advantages. Each class is now back to doing way fewer things. We cannot accidentally expose an action to GET requests because the methods are named according to the request method.</p>

<p>The controller action is basically now calling out to the domain logic and returning a response.  If you want more control over how the json is formatted, hand off to a formatter of your choice, use ActiveRecordSerializers or rabl or roar or any of a number of lovely presenters.</p>

<p>With this in mind, we can write our little chess playing API like so</p>

<script src="https://gist.github.com/svs/6354982.js"></script><h2>Conclusion</h2>

<p>Router.rb + ControllerAction.rb are together 43 LOC. That&rsquo;s all you need to get a decent router and decent controller DSL to put your app on the web. Need authentication - use warden. Need authorization - use any of the authorization gems. Need caching &mdash;- you get the idea. Nothing in this approach precludes you from using the gems you love.</p>

<p>Everything else that Rails provides is basically either a massive convenience or insupportable bloat, depending on how you look at it. Rails does make the life of developers very easy by providing helpful rake tasks, caching, turbolinks, helpers, multiple environment support, code reloading, asset pipeline,  a freaking ORM INSIDE THE WEB FRAMEWORK!! It saves hundreds of man weeks arguing about where to put stuff (simple, put anything anywhere). Some of these things are required if you&rsquo;re generating HTML on the server but if like the rest of people who like to enjoy programming you are also mostly writing APIs and microservices, half of Rails is already not required. To do the job of exposing your business logic to the web, Rails is more and more seeming like overkill.</p>

<p>One of the goals behind the beautiful and very successful modularisation of Rails 3 was indeed to let a thousand web frameworks bloom and I would say Rails core team has done an admirable job. Whereas previously Rails was an all or nothing proposition, we now have the freedom to choose our individual tools, whether they be ORMs, templating engines or what have you. These same freedoms now allow us to step out into the world with really lean, focussed code in case we want to esches the excessive ceremony of Rails.</p>

<p>This is just one of the many reasons I love Ruby and the Ruby community so much!</p>