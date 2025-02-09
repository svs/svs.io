---
layout: post
title: Using State Machines to keep your Controllers Happy
date: '2012-08-28 09:59:00'
---

<p>I just wanted to share a technique I stumbled upon some time ago which helps to keep my controllers clean and easy to read and test.</p>

<p>So, use case - you have an object that goes through a workflow, inhabiting several different states and having different things that can be done to it by various types of users. Think of a simple Document processing system where documents come in, are scanned, then tagged, then checked, approved or rejected and so on. You could try a naive approach as shown below -</p>

<script src="https://gist.github.com/3496414.js"> </script><p>However, such an approach has a LOT of problems. Willem Van Bergen from Shopify has <a href="http://www.shopify.com/technology/3383012-why-developers-should-be-force-fed-state-machines" target="_blank">written a nice article</a> on why you should be using a state machine and when. In short, if you have a field or method called state and lots of date fields to keep track of the state transitions, using a state machine can significantly ease the cognitive burden of your code.</p>

<p>The above approach, first of all, deeply couples the workflow with the object. If tomorrow you want to add a different state with some different rules, this becomes very difficult. Also, it is totally impossible now to dynamically select the workflow based on some property of the object. One other very big drawback is that it makes your controllers very dirty. For example, you might now be tempted to have urls such as /documents/2/approve. This means adding an arbitrary (and ever expanding) number of routes and controller actions. Wouldn&rsquo;t it be great if we could encapsulate all this behaviour in one place so we could grok it in one screenful and test it easily? Well turns out you can.</p>

<p>There is, however, a secret sauce here - you have to eschew (or hack) the super-popular StateMachine gem and go down the list to the number three contender (actually, you have to go even further down and use my fork of it at <a href="https://github.com/svs/workflow" target="_blank">https://github.com/svs/workflow</a>. I can&rsquo;t remember why I didn&rsquo;t use StateMachine or AASm, but workflow is awesome. It&rsquo;s one file of super readable (and hackable) Ruby and it came with I think the best API, specifically the ability to pass arguments while changing the state.</p>

<p>Using this we can now define a workflow for the loan using the Workflow DSL.</p>

<script src="https://gist.github.com/3496546.js"> </script><p>The last bit of the workflow is where the secret sauce is. Simply add a guard around your state transition that makes a call to your ACL authorisations library (I am using CanCan) before transitioning. This piece of code also maintains the audit trail. Keeping a track of who changed what is quite a common feature, and sometimes you don&rsquo;t want to go the whole hog and store every version ever. In such instances, this code hook also provides a very clean way to record state transitions.</p>

<p>And now we come to the original promise, which was clean controllers. Because you&rsquo;re using a nice authorisation gem and all your state transitions are being properly authorised, we can call all the various state changes through a simple call to &ldquo;update&rdquo;. This keeps all the state transition logic out of the controller, to the extent that you do not need separate controller actions for each state transition. Like so</p>

<script src="https://gist.github.com/3496733.js"> </script><p>The controller is reduced to simply calling the requested state transition method. We completely give a miss to controller actions such as &ldquo;complete&rdquo; or &ldquo;approve&rdquo;, we keep our state transition logic and authorisation logic in neat little boxes, joined only through a slender thread of a line in the workflow. All logging and auditing logic also has a neat little home for itself and our tests become more readable.</p>

<p>TL;DR - State machines? Use them!</p>

<p>Do let me know what you think in the comments</p>