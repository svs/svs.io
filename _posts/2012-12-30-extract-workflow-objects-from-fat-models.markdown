---
layout: post
title: Extract Workflow Objects From Fat Models
date: '2012-12-30 15:46:00'
---

If you&rsquo;re wondering why I&rsquo;ve got fat-model obsession, there&rsquo;s one simple reason - <a href="http://codeclimate.com" target="_blank">CodeClimate</a> is a highly addictive and enjoyable way to learn just how much your code sucks while telling you where exactly you should be focussing your improvements and once you get started, you just can&rsquo;t stop. Basically, refactoring extensively so that all your classes are tiny and focussed, and the accompanying feeling of freedom and fluidity that comes from this drives your code quality higher and higher.

People have talked at length about the benefits of SRP but all that reading hadn&rsquo;t prepared me for the actual experience of whittling classes down to doing just one thing.

<ul><li>Firstly, you can write really focussed tesets. Now more than half of the files in my models directory have nothing really to do with Rails. As POROs, testing them becomes super fast.</li>
<li>The elimination of churn from the classes is the biggest source of relief for me. Once the class is specced and written, it almost never changes and this gives me a lot more confidence that changes I make in one part of the code won&rsquo;t have unintended consequences elsewhere.</li>
</ul>The whole experience has been so fundamentally mind altering that I would recommend everyone try out CodeClimate.

During this process, naturally, I learned a lot about keeping models focussed on one thing. CodeClimate&rsquo;s biggest problem always has been with the  models that have an embedded workflow in them, and I figured it might be worthwhile to try and extract the workflow into an object all its own. Here&rsquo;s my story.

To start with, I have the following scenario. The model in question is the Quotation model, which handles the creation of quotations to be sent to clients. The quotations go through various stages, first being approved or rejected by an admin, then being sent to the client and finally being paid. The original model took care of defining and persisting the attributes. In addition it could answer various questions about what all could be done with the quotation in whatever state it happened to be in, as well as handling all state transitions (including sending email and so on).

Broadly, the class looked like this

<script src="https://gist.github.com/4413218.js" type="text/javascript"></script>The Quotation class is the God class of this application. It has a trip associated with it, a duration, is pooled with other Quotations in shared cabs, it has several fields which represent various orthagonal states (lead quality is one state, the workflow which leads to payment and closure is another), it shows up in the users account and so on. The whole class is very complex and was begging to be ripped apart, scoring as it did, a big fat F on CodeClimate.

So, among other things (such as creating separate scope and search classes), I started to extract a workflow class from this class using the dependency injection technique I talked about <a href="http://svs.io/post/38883550750/breaking-up-fat-models-with-delegation" target="_blank">in my previous post</a>. As the model currently is, there is a very tight coupling between the Quotation class and its workflow. It is not also possible to have more than one workflow field in the model. In addition, there is no way to have separate workflows based on say, the user associated with the quotation (i.e. different criteria and workflows for different classes of users like guest user, trial user, premium user, etc.) I am happy to report that ripping apart the class and using dependency injection has solved all these problems.

So, the first thing I did was to extract a QuotationStatusPolicy class which would handle all the questions about whether a given quotation was approvable, sendable and so on. i.e.
<script src="https://gist.github.com/4413279.js" type="text/javascript"></script>

Using this method, it becomes trivially simple to use a different status policy class for various classes of users.

Now, we need to rip out the heart of the beast, the actual workflow itself. This is done as follows
<script src="https://gist.github.com/4413305.js" type="text/javascript"></script>

The last thing thats left is to wire up these new classes into the Quotation class. i.e.
<script src="https://gist.github.com/4413397.js" type="text/javascript"></script>

All tests passing? We&rsquo;re good to go!