---
layout: post
title: A Generic Loan Management System (was Microfinance MIS)
date: '2012-03-30 10:35:00'
---

<p>Hello and welcome to the next episode of the thread that started as &ldquo;Towards an Architecture for Microfinance MIS&rdquo;. One of the main learnings from our time spent building Mostfit is that MIS is but a special case of what can be termed as Lending and so our goal actually should be to build a Loan Management System that can support the Microfinance operations models (like JLG, SHG, etc) but not be tightly coupled with them, hence the title of this second part.</p>

<h1>Stack</h1>

<p>Before we can proceed with laying out such an architecture, we must first try and decide what we will be implementing this software in. As an open source product, issues of community and adoption must also be considered along with the pure technical merit of the proposed solution. I suspect this is going to be a long and interesting debate but there are many things that are a must for such an undertaking. Broadly they are</p>

<ul><li><p>A mature MVC web framework.
This means that the web framework takes care of all the little things like</p>

<ul><li>Easy to deploy</li>
<li>Authentication</li>
<li>Routing</li>
<li>Access Control</li>
<li>ORM if using an Object Oriented Approach. The ORM must be atleast as awesome as DataMapper.</li>
<li>API (!) Extremely important.</li>
<li>Testing infrastructure for all the above with Unit, Integration and Functional tests</li>
</ul></li>
<li>Ample libraries

<ul><li>pdf, xml, json, etc</li>
<li>message queues, background jobs, scheduled tasks</li>
<li>recurring events</li>
<li>date functions of the level that can be used in Financial calculations</li>
</ul></li>
<li>Other Language Considerations

<ul><li>Large community</li>
<li>Proven to work at &ldquo;web scale&rdquo;</li>
<li>An awesome toolset</li>
</ul></li>
<li><p>An Awesome database
Our experience with MySQL has been that MySQL is OK. Unfortunately, OK won&rsquo;t do. I  haven&rsquo;t dived deep into it yet but PostgreSQL is looking VERY exciting indeed because of the following reasons</p>

<ul><li><a href="https://postgres.heroku.com/blog/past/2012/3/14/introducing_keyvalue_data_storage_in_heroku_postgres/" target="_blank">Built-in, indexable key-value store</a> This is absolutely essential to cater to lender&rsquo;s varying ideas of what a borrower is (Individual, Business Owner, Corporation, etc.) and what data needs to be captured about him or any other entity in the database that can mean different things to different people</li>
<li>Built in support for MONEY. There are so many corner cases when dealing with money and rounding that it is nice when the database does them for you.</li>
<li>Built in support for tree structures and most organisations are nothing if not tree structures.</li>
<li>Built in Array data types. This is awesome for storing history for example, which branches a center has belonged to in the past, etc.</li>
</ul><p>It definitely looks like PostgreSQL will be the database engine of choice for this project. If it is good enough for Heroku, it&rsquo;s good enough for me ;-)</p></li>
</ul><p>So, while we decide on the language and the framework we will use, we can still lay out a general architecture for such a system. We will use Ruby as a reference language.</p>

<h1>Mistakes and Learnings</h1>

<p>Over the past week, while thinking about this post, I came to the conclusion that rather than go over the architecture of Mostfit, we would instead lay out what we believe would be a good architecture for a system designed to run micro-finance operations at scale. Mostfit architecture evolved over the course of two and a half years and reflects in many ways on the learnings we had along the way. In other words, it is an unholy mess! Mostfit has very poor separation of concerns. For example, we don&rsquo;t have a Cashflow class which can calculate IRRs. Our amortization code is jammed into some modules which the loan extends itself with to calculate its cashflow - and there are numerous other examples of poor decision making. Halfway through the project, under client and investor pressure, we totally lost our testing hygiene and yes the code began to smell. All this is true. Now what can we do to learn from it?</p>

<p>As has been so correctly noted in the Mythical Man Month, the first version of the software will be thrown away and is basically there to map out the complexity of the domain you are targeting. Mostfit is no exception and it can be counted as a successful software because not only did it validate the value of the proposition, it also brings with it tremendous learnings, and this blog is an attempt to distill that.</p>

<p>Joel Spolsky wrote very eloquently on the dangers of rewriting working code, but the truth also is that were I to rewrite Mostfit today, I would definitely not choose Merb as the framework. In fact it is debatable whether Mostfit should even be done in Ruby. A number of new languages and platforms, specifically Clojure and Haskell, present them as exciting new candidates for the language of choice. For now, our mind is not yet made up which one of the three languages we will use to build the next version of Mostfit, so stay tuned for some exciting language wars. In the meantime, we will continue to talk about the architecture in terms of Ruby, as there exists a whole lot of code for example purposes.</p>

<p>The first mistake to make of course, was to tightly couple all the code to the dominant business model at the time - the Grameen Methodology. What we should have done instead was to build a system where the Loan classes know nothing about the topology nor the operational details of the organisation. In order to do this though, we have to accurately delineate the touch-points between the Loan class and the organisation&rsquo;s heirarchy. Of course it is interesting to know where in the organisation the Loan sits as when we do aggregation and so on these issues become relevant. However, we need to be able to express this information in a way that when clients move from one branch to another and from one center to another, that this information propagates through the reporting mechanism.</p>

<p>The second mistake was to incorrectly anticipate the complexity required in the system. To be fair, microfinance used to be a very simple business and just about the time when Mostfit was becoming ready, all hell broke loose and what used to be a simple business with very basic loan products morphed overnight into banking proper. Naturally, catching up had all sorts of bad consequences on code smell. Luckily Ruby is a very nice language and MVC is a very nice methodology and we could, using refactoring, keep up with many of the changes that were thrown at us. A quick glance at how the Loan model file has evolved over the years will give an indication to the interested reader.</p>

<p>In the next episode, we will look at a generic Loan model and how it should be architected to maintain clean separation of concerns.</p>