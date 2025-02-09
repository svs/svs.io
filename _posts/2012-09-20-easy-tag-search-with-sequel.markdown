---
layout: post
title: Easy Tag Search With Sequel
date: '2012-09-20 21:18:00'
---

One of the most under-rated libraries in the Ruby world in my opinion is the brilliant SQL builder library <a href="http://sequel.rubyforge.org/" target="_blank">Sequel</a>. Sequel allows you to build SQL statements in Ruby and compose them using all kinds of nice logic and Turing-complete programming language goodness. So, why in the world of ActiveRecord and DataMapper would we need something that helps us write SQL?

Well, ActiveRecord and DataMapper are ORMs. They exist in order to persist your objects to the database and work fine as long as the particular object is the correct abstraction through which to approach your desired functionality. Sometimes however, you need more than an ORM. You need to be able to look at all the data in your database holistically. You need something where the level of abstraction is the Relational Algebra, so you can make broad, sweeping statements about your entire database. One typical use case for such a level of abstraction would be a search feature. When you search, you probably want to span a lot of tables in your database and ORMs handle such a use case clumsily - you don&rsquo;t really want to be firing off one query per table now do you?

In <a href="http://digidoc.co.in" target="_blank">digiDoc</a>, we offer powerful tag search as described in this video here:

<iframe src="http://www.screenr.com/embed/o438" width="650" height="396" frameborder="0"></iframe>

You can actually get this entire functionality including stuff not shown in the video in about a 75 lines of Ruby code (150 if you care about readability), and that is what judicious use of Sequel will get you.

Here&rsquo;s the code -

<script src="https://gist.github.com/3758355.js"> </script>As you can see, Sequel provides a beautiful abstraction of SQL and allows you to exploit the full power of your data store.

The tag filter bits where we compare for &ldquo;must have&rdquo; and &ldquo;must only have&rdquo; are based on a clever trick I read on StackOverflow (sorry can&rsquo;t find the link). Instead of comparing against a given set of tags, what we do is to aggregate the count of tags matching the given criterea per record and then check if the count is more than the number of tags we&rsquo;re looking for. This would have been extremely difficult without changing the level of abstraction. As Uncle Bob says - perspective is everything.

These days, I am using Sequel in all my projects. I cannot think of a better way to work with aggregation, reporting, search and other stuff that doesn&rsquo;t have to do with object persistence.

If you liked this article, and would love your web-apps to have similar cool features built with the same level of care and consideration, consider hiring our new Ruby consulting firm - Sealink Consulting. We&rsquo;re based in Mumbai but available to work remotely. Email svs at this domain to get in touch.