---
layout: post
title: Decouple your APIs from their implementation
date: '2012-10-14 15:14:00'
---

<p>A common pattern seen across Rails applications is the following <code>update</code> action in controllers</p>

<script src="https://gist.github.com/3888882.js"> </script><p>Convention over configuration is a very powerful way of getting developers up the curve quickly. However, it can lead to a certain amount of automatic programming which obscures the possibility of creating beautiful APIs with our standard RESTful actions. In the above case, years go by before Rails programmers even begin to notice that the tight coupling between the route, the controller action and the model is just an illusion. Infact, <strong>resource != controller + model</strong>!</p>

<h3>Be RESTful</h3>

<p>A resource is a completely different layer of abstraction than the controller or the model. Controllers and models are elements of an MVC framework. Resources are the nouns in the language of the web. We use Models and Controllers to implement resources in our web application, but breaking the coupling between routes, models and controllers is one step in the direction to Rails nirvana.</p>

<h3>Why is this important?</h3>

<p>I&rsquo;ve found it very helpful to adhere strictly to a RESTful architecture. This means thinking of everything as a resource that responds to the six default RESTful actions. This helps to keep your interface really clean, your controllers really lean and simple to test and pushes your application logic into the model layer where it rightfully belongs.</p>

<h3>A &ldquo;for example&rdquo;</h3>

<p>Let&rsquo;s take a look at a simple case like a board game. Once a game is set up, the following things can be done to it</p>

<ul><li>a player can join</li>
<li>a player can leave</li>
<li>a player can make a move</li>
<li>a player can resign</li>
</ul><p>All of these constitute an &ldquo;update&rdquo; to the game resource. It might be tempting to start adding controller actions like <code>join_game</code>, <code>leave_game</code>, <code>move</code> and <code>resign</code>. Let&rsquo;s see what happens if we do.</p>

<script src="https://gist.github.com/3888175.js"> </script><p>Interesting. Our controller is exploding and is not very DRY at all. Is there some way we can be more RESTful about this? Here&rsquo;s technique number 1</p>

<h3>Decouple your Controllers and your Models</h3>

<p>Rails nested resources to the rescue! We declare Players and Moves as nested resource of Game</p>

<script src="https://gist.github.com/3888291.js"> </script><p>Please note, there is no model called Player. A Player is nothing but a User. Secondly <code>MovesController#create</code> doesn&rsquo;t call <code>@move.save</code>. It calls <code>@game.add_move</code> and all the corresponding logic that truly belongs in game is being called from the Moves controller. Thus, we&rsquo;ve created a resource called Player out of the model user, and our Moves resource uses the Game model API to add moves to the game. There is no spoon!</p>

<h3>Decouple your API from your actions</h3>

<p>So let&rsquo;s say even after thinking really hard you can&rsquo;t find a resource that can give you the API you want. An example? Oh, let&rsquo;s say -  an article going through the various states of &ldquo;moderated&rdquo;, &ldquo;published&rdquo;, &ldquo;unpublished&rdquo; etc. Instead of adding various methods like <code>approve</code>, <code>publish</code>, <code>unpublish</code>, we can decouple our API from our actions and drive all these state changes through the <code>update</code> method. i.e.</p>

<script src="https://gist.github.com/3888854.js"> </script><p>Note, we&rsquo;ve replaced the <code>update_attributes</code> method with <code>Article#update</code> which can contain all the convoluted logic to deal with the parameters sent. As you can see, we&rsquo;ve decoupled our API from our controller and our model. The shape of the API as exposed to the outside world has very little to do with the internal implementation in terms of models.</p>

<p>Your API is the little jewel of your app. Users of your app will judge you based on the intuitiveness and consistency of your API. Therefore, it is not a good idea to shoehorn your API to fit current Rails conventions. Rather, you can and should try as far as possible to decouple your API from its implementation.</p>

<p>Another advantage - if your app is to work at any kind of serious scale, at some time you are going to have to consider a polyglot implementation. Maybe you hand over to a service written in Go or call an external web-service. At this point, a clean separation between your app&rsquo;s API and its implementation will really stand you in good stead.</p>