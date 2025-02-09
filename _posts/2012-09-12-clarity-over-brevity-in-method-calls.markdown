---
layout: post
title: Clarity over Brevity in Method Calls
date: '2012-09-12 07:45:00'
---

<p>A method name is actually just one part of a method invocation and since methods take arguments, one can use the arguments to provide a lot more clarity of what the method is doing than simply restricting oneslef to the method name. Infact, methods with names like <code>make_person_an_outside_subscriber_if_all_accesses_revoked</code> are just begging for a plethora of methods like <code>make_person_an_inside_subscriber_if_all_accesses_not_revoked</code>, <code>make_person_an_outside_observer_if_some_accesses_revoked</code> and so on. I am not sure I would like to work with such an API, let alone maintain it.</p>

<p>Instead, my suggestion is to for <strong>clear method invocations</strong>. Clear method invocations are a much better indicator of a well thought out API, where reading and writing the code starts to seem very natural. For example, the above method call would seem just as natural if called as</p>

<p><code>@person.set_role(:to =&gt; :outside_observer, :if =&gt; :all_accesses_revoked?)</code></p>

<p>Just as readable and without the deleterious effects on your API.</p>

<p>Interestingly, the method definition of <code>set_role</code> suffers a bit, and here&rsquo;s where we have an aesthetic tradeoff. 
<script src="https://gist.github.com/3704962.js"> </script></p>

<p>However, I would much rather write some <a href="http://tomdoc.org/" target="_blank">Tomdoc</a> on the method than pollute my API. Clearly this is a matter of choice. The ideal solution of course would be named arguments, which will come in Ruby 2.0.</p>