---
layout: post
title: Towards an Architecture for Microfinance MIS Systems
date: '2012-03-08 10:30:00'
---

<h1>Introduction</h1>

<p>Since January of 2009, I and various colleagues, most notably Piyush Ranjan and Cies Breijs, have been working on an open source MIS for Microfinance, specifically JLG or Grameen type MFIs. The system is called Mostfit and is available under the Gnu Affero Public Licence (AGPL) and can be downloaded from its <a href="http://github.com/svs/mostfit" target="_blank">github repository</a>.
What follows is a series of blog posts detailing all the things that I have learned about building MISs that are cheap, robust, easy to use and operate at the massive scale that MFIs are wont to do while still retaining the flexibility to enable radical changes in business processes and products.</p>

<p>The last three years have been a kind of deep meditiation focussed on the above problems and I am happy to state that we have made remarkable progress towards achieving the above stated goals. Anyone sufficiently technically inclined is welcome to check out the source code at the above address and follow along as we dive deep into the architecture that allows us to achieve stunning speed and robustness and technically support MFIs at pretty much any scale.</p>

<p>I will be talking about the following salient points</p>

<ul><li>Background to the problem - what is the problem we are trying to solve.</li>
<li>Core Issues</li>
<li>Loans and Loan Products</li>
<li>Repayments</li>
<li>Meeting Schedules and Holidays</li>
<li>Access Control</li>
<li>Data Entry</li>
<li>Reporting</li>
<li>Business Rules</li>
<li>Third party integration / APIs</li>
<li>Data Migration</li>
<li>Accounting</li>
</ul><p>In each of these issues, there are a million learnings that we have acquired, mostly the hard way, over the course of 3 years and about 200,000 loans. This series is basically to try and preserve these learnings in the public domain so they can be useful to a larger audience.</p>

<p>We will be going into some technical depth with lots of code to illustrate our points. However, less technically inclined readers should still be able to follow the concepts presented here. Mostfit itself is written in Ruby using the awesome if slightly dated Merb framework. I still feel Ruby is the most suitable language for such a system given the tremendous dynamisn and meta-programmability of the language, but the concepts presented here are not tied to any one particular language. We also feel that the MVC pattern as implemented by the Merb framework (or Rails even) is also very appropriate choice for developing such an application.</p>

<p>Please note, this is meant to be the starting point for this discussion, and the content presented here is no way meant to be &ldquo;authoritative&rdquo; in any way, hence the title of the series - &ldquo;<em>Towards</em> an Architecture&hellip;.&rdquo;</p>

<h1>Background</h1>

<p>In 2008, when I started on an early draft of Mostfit (at the time called at MIS-Fit), Microfinance was very different from what it is today. There was no regulator, the crisis had not yet set in and Yunus&rsquo;s Nobel Prize was still shiny and brand new. Most MFIs ran on pen and paper and got by with offering one or two simple loan products. Credit Bureaus were not even being discussed and neither were interest rate caps and so on. In short, Life was good. At the time it seemed that &ldquo;fixing&rdquo; the microfinance technology problem was not going to be very hard. Simple flat repayments, weekly, no decimals points anywhere - what could go wrong? In a word - everything! Once the regulators got involved, more and more :standard&quot; core banking idioms started coming into play - reducing balances, IRRs and so on. In this regard I must say that we were really blessed. Had I known the complexity that was going to eventually be involved, I would probably never have started to write Mostfit.</p>

<p>We have also been very lucky in many other regards. Writing software during these volatile times means that there is now a deep understanding, even an intuition I would say, about how to develop flexible software that maintains its reliability even while being transformed. It has convinced us that openness, both in terms of licensing and APIs will be the determining factor of success for MFI systems.</p>

<h1>Problem Definition</h1>

<h2>Efficiency</h2>

<p>Microfinance Institutions are in the business of making small loans to large numbers of people. The problems associated with this business model all basically stem from the fact that small ticket sizes means lower profitability, but behind this simple statement lie a myriad of problems. Increasing profitability means increasing operational efficiency. Increasing operational efficiency means reducing the redundancy in the system, thereby making it more fragile. Implementing technology to solve this problem usually means massive expense, thus pushing the breakeven point even further. Further, &ldquo;increasing operational efficiency&rdquo; can mean different things in different places and so your system needds to be flexible enough to allow your organisation to implement operational efficiency in various cultural contexts.</p>

<h2>Flexibility</h2>

<p>Implementing operational efficiency naturally means having excellent insight into your operations and when generating large amounts of data, this becomes difficult if you have not implemented the correct model of reality in your reporting systems. How do we give micro-financiers the ability the peer into their data in any way that they choose? If we make things very generic, then speed and scalability suffer. If we make them too rigid we can optimise very well for speed but give up on flexibility. Where then is the correct place to draw the line?</p>

<h2>Adoption</h2>

<p>Large scale and small ticket sizes means that without scale, there is no point in doing the business. The whole MFI game is a game of scale and so every Rupee you have must go to increasing your customer base. In this scenario, technology becomes one of those things to &ldquo;implement later when we need it&rdquo;.  Our intention with Mostfit has always been to take away this conflict. Have a dead simple and super cheap system that can scale to any limit. The endeavour has always been to convert the upfront cost of technology as much as possible into a &ldquo;running&rdquo; cost, to make adoption as frictionless as possible and to make the interface as intuitive as possible to reduce training costs and mitigate the effects of employee churn. Naturally advances in &ldquo;cloud&rdquo; technology have put these goals within the reach of any well engineered system.</p>

<h2>Interoperability</h2>

<p>I am quite convinced that the future belongs to systems that can interoperate. We have often had requests to add a payroll system or a HR system on to Mostfit and we have always refused for two reasons
* There are already many systems that do payroll and HR, etc very well at a reasonable price, since the development cost of these systems has in many cases already been amortised over large number of installations and across many years
* Any hour that we spend building features on problems that have already been solved is one hour less that we have to work on unsolved problems, and the problem of providing gret MFI systems is as yet unsolved.
* Monolithic systems eventually become rigid and sclerotic. To maintain flexibility, it is important to make systems focussed but at the same time interoperable.</p>

<p>However, we know that Mostfit must be able to exchange data with other such systems and so we have built it with an emphasis on interoperability. In this sense, Mostfit adheres as far as possible to the Unix philosophy of do one thing, do it really well and interoperate with others. We will be seeing more examples of this philosophy as we go through the system in more detail.</p>

<p>This is, in very short, the statement of the problem - build system that support microlending, supporting all kinds of loans and operational models and give managers insight into the data thus generated, while integrating well with the outside world.</p>

<p>In the next part, I will be talking about how we implemented a Loan class to handle any possible loan repayment style and talk some more about Loan Products, Fees and so on. Stay tuned by subscribing to this blog&rsquo;s RSS feed or follow me on twitter</p>