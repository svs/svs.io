---
layout: post
title: Information Driven Architectures
date: '2014-10-06 20:12:00'
---

<p>Below are some initial thoughts on having a single Information API not just for applications, but for the entire host organisation. Throughout the below, I use the word &ldquo;information&rdquo; distinctly from &ldquo;data&rdquo;, with information being the transformation of data into something useful (for some value of useful).</p>

<p>The majority of applications being written today try to solve the various problems they are required to solve by modelling business concepts and their underlying data. This leads to having domain objects like Users, Posts, Comments, Payments and so on, and the corresponding tables in the datastore to store these data. What often gets overlooked in the whole process is the information contained in these basic objects and the relationships between them, and the whole cascade of effects that each new piece of data brings.</p>

<p>For example, there is no table in the datastore that says &ldquo;Signups on March 12 2014&rdquo; or &ldquo;Retention for Cohort 27&rdquo; or &ldquo;Friends of foo@bar.com&rdquo;. Of course there aren&rsquo;t such tables. We use relational databases precisely so we won&rsquo;t have to model any and every concept that can be thought of but we can, instead, generate them on demand from simpler data.</p>

<p>And it works. Worked. Up to a point. With the demands of Internet business what they are these days, business users - customer support and marketing for example - need this data to be correct and up to date and their use cases are often as important as those of paying customers. With increasing scale and complexity, generating these tables on demand is prohibitively expensive. In many cases, relational databases are not up to the task.</p>

<p>There have been two attacks on the problem, both of which have their own trade-offs. The first has been &ldquo;web-scale&rdquo; document databases. The second has been hosted peta-byte scale data warehouses such as Redshift and BigQuery.</p>

<p>Document stores try to take away the problem of generating information from data on demand. Instead, the information is stored in a ready-to-read format and reading it takes zero CPU cycles. This stuff scales, but as a downside, the data becomes unwieldy to use. For example, nested documents are expensive to access and all the lovely features of relational databases such as independent data access and ad-hoc joins are lost.</p>

<p>Large scale, parallel, hosted data warehouses are another attack on the problem. They are now cheap enough to be available to everyone but they often entail having expertise in data warehouse modelling (a black art, by all accounts), batched ETL processes which introduce another point of monitoring and failure and of course, these warehouses cannot be used to drive application behaviour in real time as they are not what one would call &ldquo;production&rdquo; infrastructure. Their use case is for internal customers and they are often out of sync with the production datastore by as much as a day.</p>

<p>So it seems an intractable problem - give up relational databases or embrace ETL and data warehouses? There must be a third way.</p>

<p>And there is. It&rsquo;s what I like to call Information Driven Architectures. This essentially entails that we stop looking at the user-facing application as a special case. Instead, we have to see it as one component of an information concentrating entity - the business - which has a whole continuum of information needs. Some of them move fast and in real-time, such as scores in a game that your users play, while others move slowly, like the monthly report that goes to investors or the annual tax filings. Seeing one of them as a first-class concern while ignoring the other is bound to lead to a lopsided emphasis on some data as opposed to others. Such lopsidedness is necessarily unhealthy to the prospects of the business.</p>

<p>On the other hand, seeing every piece of data in the organisation as part of a whole brings some interesting new perspectives into the mix.</p>

<p>First of all, we are now obliged to include all kinds of datastores into the mix. Postgres and Elasticsearch are obvious candidates, but your read-replicas, your monitoring APIs, Google Analytics, Google Drive Spreadsheets and Mailchimp stats are all parts of this same organism. In order to drive healthy feedback loops between users, marketers and product managers, all of them need to be looked at as first class citizens of the information landscape within the organisation.</p>

<p>So, for starters, we agree that monolithic data architectures are not possible any more. Data must live where it feels at home. Transactional data must live in relational databases. Realtime data must live in memory. Search indexes in Elasticsearch, user behaviour in Mixpanel, and stuff you don&rsquo;t mind losing in MongoDB. There live a heterogenous collection of &ldquo;Data APIs&rdquo;, which deal with low level issues of getting data into the system. Your ORMs and event tracking javascripts are part of this ecosystem.</p>

<p>This of course leads us to the second conclusion - Information, being available in a variety of data stores, needs a unifying API. This is a restatement of the principle which says that one should code to interfaces, not implementations. Today, you&rsquo;re more than happy to generate weekly user cohorts from your transactional datastore, but soon it is going to be time to have that done with Redshift. Can you make this change without breaking every single client that needs that report?</p>

<p>An Information Driven Architecture is not just for the app that users use. It&rsquo;s for the whole organisation. And for this, above the low level &ldquo;Data APIs&rdquo;, we need an &ldquo;Information API&rdquo; for the whole organisation. This is the API which does indeed have URLs which point to &ldquo;Signups on March 12 2014&rdquo; or &ldquo;Retention for Cohort 27&rdquo; or &ldquo;Friends of foo@bar.com&rdquo;. The various implementations of these concepts will change and evolve over time, which is why it becomes essential to have a unifying interface for every concept that makes sense for the organisation. It is this unifying API that will enable seamless data flow throughout the organisation.</p>

<p>A valuable corollary of this architecture is that information changes at various speeds, and this architecture enables us to control how many resources are devoted to keeping up with changes. A weekly report that is calculated each time it is requested is insanity. Such a report should be calculated on first read and cached for the rest of the week. Having a unified information architecture enables us to control how, when and where data are turned into information. This control enables us to economise on resources while still having available information that is relevant.</p>

<p>To sum up</p>

<ul><li>a viable Information API must be able to unify all underlying datastores of the organisation and present the data therein in desired formats.</li>
<li>Such an API must use the same vocabulary as the domain being modelled, and as such work at a higher level of abstraction than the &ldquo;implementation&rdquo; level which would speak SQL or some such dialect. As such it shields consumers from implementation details.</li>
<li>Such an API must also be able to specify when information is updated, when it is to be cached and when those caches are to be invalidated.</li>
<li>Information disseminating processes must only use the information API. Weekly reports mailed to users and realtime dashboards alike must run off the information API. There will be exceptions of course, but as organisations embrace service-oriented architectures and the Information service becomes a first-class citizen of the infrastructre, these exceptions will be fewer and further between, restricted increasingly to use cases where latency requirements are very aggressive.</li>
</ul><p>Once this is done, data can be written as per the usual process, but the conversion of that data into information is now mediated by an organisation-wide information API.</p>

<p>I admit these are initial and admittedly scattered thoughts here. It&rsquo;s what I like to call &ldquo;lean blog post&rdquo; :-) .I&rsquo;d love to hear feedback on the concept <a href="https://news.ycombinator.com/item?id=8418006." target="_blank">https://news.ycombinator.com/item?id=8418006.</a></p>