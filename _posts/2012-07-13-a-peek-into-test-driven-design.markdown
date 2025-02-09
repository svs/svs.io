---
layout: post
title: A peek into Test Driven Design
date: '2012-07-13 14:45:00'
---

<p>As the software that we write grows in complexity, there&rsquo;s been an increasing trend amongst practitioners to guarantee or prove that their programs operate as desired. No serious programmer will deny that this ability is one of the most important ones in running complex software systems. Without such guarantees, it becomes impossible to state anything meaningful about the system and refactoring the system or optimising the performance of it becomes fraught with risk.</p>

<p>There are several approaches to getting these guarantees from the system and unit testing is a component of most of these approaches that I am aware of. Test Driven Development is a well known term and especially for dynamic languages like Ruby, it is a phenomenal methodology for building systems that work as advertised. However, the correctness of the program is just one of the good things that happens when testing drives development. Testing also has the ability to drive the design of your software. I recently had one such a-ha moment related to test driven design that I&rsquo;d like to share with you.</p>

<p>So I was looking at some test code that I wrote some time ago. Let&rsquo;s look at the code in all its undecipherable complexity. This is some test code to test a state machine implemented with the wonderful <a href="https://github.com/geekq/workflow" target="_blank"><code>workflow</code> gem</a></p>

<script src="https://gist.github.com/3105262.js"> </script><p>wow! horrible, right? I mean, you can kind of tell whats going on, but there&rsquo;s way too much text over there. Another smell - comments in your test. @ponappa and @aninda42 did an awesome session on code smells in tests at RubyConfIndia 2012, and they are 100% right - your test code should not need comments. And yes, multiple assertions in one test case. Another code smell. So, what&rsquo;s the fix?</p>

<p>Here&rsquo;s where unit testing can really help to drive the design of your code. So we&rsquo;re looking at these horrible tests and thinkng - why am I testing the workflow gem? Given that the gem itself is extensively unit tested, I should not really have to worry whether a given transaction succeeds or not. Instead, I should concentrate on checking whether my workflow is correctly defined, rather than whether the definition of the workflow leads to correct actions. The latter part is the responsibility of unit tests on the workflow gem.</p>

<p>So, with this is mind, we start to rewrite our tests without the code smell. Now, they look like this.</p>

<script src="https://gist.github.com/3104578.js"> </script><p>Oh, much better. Specs are totally readable. It&rsquo;s apparent at a glance what is going on there and we&rsquo;re not busy messing with testing Workflow functionality, we&rsquo;re only testing our workflow definition. This is akin to saying <code>it { should have_property(:foo)}</code> as we do for DataMapper models.</p>

<p>Now here&rsquo;s the rub. The workflow gem is not able to answer the question <code>has_events?</code> properly. Sometimes a state will have a particular event associated with it, but the transition is not possible due to some guards around the transition. We need to update the <code>workflow</code> gem and add a method called <code>has_events?</code>, which correctly filters out events which exist but which are not possible.</p>

<p>This is where the design is being driven by the tests. We update our workflow DSL to accept an <code>:if</code> clause, write some tests to make sure the workflow is respecting the clause and voila, we&rsquo;ve contributed to open source, made our tests much cleaner and more readable and ended up with a nicer workflow DSL in the bargain.</p>

<p>You can find my changes in the workflow gem here: <a href="https://github.com/svs/workflow/tree/update" target="_blank">https://github.com/svs/workflow/tree/update</a></p>

<p>This is just a small example of how you can use test code smells to drive better software design.</p>