---
layout: post
title: Breaking up Fat Models With Delegation
date: '2012-12-26 19:03:00'
---

<p>Hope everyone had a relaxing Christmas with loads of great food, booze and gossip. I know I did!</p>

<p>So not being a classically trained CS guy, I have no idea what the name for this pattern is, but I find myself using it more and more to break up fat models. It basically involves a lot of explicit delegation in the class, but no decorator classes. I&rsquo;m still wondering whether this pattern is a &ldquo;good thing&rdquo; or not, so I&rsquo;d appreciate comments.</p>

<p>Use case - your model has been collecting a lot of crufty methods that don&rsquo;t have anything, really, to do with the logic proper of the model. Classic examples are methods like <code>full_name</code>, which basically only deal with presentation or some orthagonal logic like currency conversion and so on. There have been several approaches to fixing the presentation issue, the most notable of which has been <a href="https://github.com/jcasimir/draper" target="_blank">Draper</a>, but I didn&rsquo;t enjoy using Draper. For one, I don&rsquo;t want to call <code>UserDecorator.decorate(User.get(params[:id]))</code>. Why? I just don&rsquo;t ok?! Just kidding - actually in my experience, decorating large arrays (think CSV dump for the last months data) takes f.o.r.e.v.e.r and damned if I didn&rsquo;t get some subtle bugs with DataMapper associations on the decorated class. I didn&rsquo;t dig too deep, being a shallow and easily influenced guy, and instead started looking for other solutions. I present mine below</p>

<script src="https://gist.github.com/4382175.js"></script><p>So there are several funky things about this approach.</p>

<ul><li>First of all, it is totally explicit. There is absolutely zero magic going on here.</li>
<li>Secondly, because of the injected dependency, we can choose the class we would like to use to present our object at runtime.</li>
<li>Thirdly, it saves us from having to explicitly decorate our objects.</li>
<li>Fourthly, the ViewDelegate object gets access to all the original objects methods using <code>method_missing</code>. This means that the implicit <code>self</code> in the ViewDelegate class is the original object for all practical purposes. Thus, any object that responds to the required methods can use this Delegate, not just a User.</li>
<li>Fifthly, the delegate objects are not instantiated until one of the delegated methods are called. This might have performance implications. Of course, the ViewDelegate can be memoized as well.</li>
<li>Lastly, this can be used for any type of delegation, not just for presentation. For example, one might choose to delegate a <code>height</code> method to a <code>MetricUnitDelegate</code> or to an <code>ImperialUnitDelegate</code>, depending on the context.</li>
</ul><p>I have no idea if this is a good or even an original approach. Would love to hear from you in the comments.</p>