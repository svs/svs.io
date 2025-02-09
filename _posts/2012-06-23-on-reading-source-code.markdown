---
layout: post
title: On Reading Source Code
date: '2012-06-23 13:30:00'
---

<p>There are some things that people you look up to as masters of the hacker craft do on a regular basis - they contribute to open source projects, they have commits all over the place in software that you know and use every day, they build and maintain gems that thousands if not millions of people download and use every day. If you&rsquo;re already sending pull requests and contributing to open source, or you have a few gems of your own on rubygems, then none of what I say will be news to you. But if like me, you&rsquo;ve been wondering who those people are who are not afraid to write code that&rsquo;s going to be used by the general public, I&rsquo;ve found that they are usually people who read other people&rsquo;s source code.</p>

<h1>Code is meant to be read</h1>

<p>Once you start reading other people&rsquo;s code, you will realise that your heroes write code that they intend to be read many times over, and they&rsquo;ve done everything that they can to make it easier to do so. It is easy to see why this is important - readable code means you can scale your team while keeping your training overhead in check. Readable code means bugs have fewer places to hide. Readable code means that developers have worked on the problem to the point where they can communicate their intent clearly and concisely. Readable code implies clarity of thought. But readable code is hard - just like with natural languages, one needs a firm grasp of grammar and idiom to write well and this takes time and effort. Luckily, the easiest way to acquire a feel for the grammar and the idiom is to read and read some more, until every clever turn of phrase and elegantly beautiful data structure is in your vocabulary somewhere, to be used to communicate your intent to others who would read your code.</p>

<h1>RVM - a real blessing</h1>

<p>One of the perhaps unintended consequences of using something like RVM is that earlier, your gems were this kind of sacred thing, locked away behind a sudo in a read-only directory that you could reach, sure, but the unstated assumption always was that you should not be poking around in those deep dark corners. With RVM, gemsets and bundler, suddenly, the inner sanctum has been thrown open to the unwashed masses, and we are all much the better for it. It is now trivial to read the source code to all our favourtie gems. A simple <code>cd $GEM_HOME</code> puts us in the gem directory. Since its in our home directory,we can spew debugger statements at will. We can change the source code as we like, knowing a simple <code>bundle install</code> will bring us back to a pristine condition. The change is nothing short of revolutionary and one should definitely take advantage of this to read, tweak and step through as much of other people&rsquo;s code as possible.</p>

<h1>Scary</h1>

<p>But there&rsquo;s so much of other people&rsquo;s code! Where to start? Start right at the first opportunity - scanty documentation usually is a huge opportunity to go trawling through source code. Or finding a gem somewhere that does 95% of what you want, it&rsquo;s almost an invitation to figure out a way to improve it. And like anything, reading code just gets easier and easier the more you do it.</p>

<p>The specs are usually a very good place to start reading source code. A well written project like CanCan will have plenty of specs that show very clearly the intent of the developer. Browsing through the lib/ directory of your favourite gems, you will soon get a feel for how these projects are laid out. Once you start, you will in no time start realising that in the end, gems are quite logically laid out, and that its quite simple to make a new one.</p>

<h1>Consequences</h1>

<p>Funny things start happening as you read other people&rsquo;s code - it starts to change the way you write your own code. A simple trick here, a beautiful idiom there, they seep into your own programming style till your own code starts to look like the code of the people you respect. After a while, you start to develop an instinct for what good and bad code looks like.</p>

<p>Then, one day, you see a very obvious way to add a new feature and you do, and just like that, you&rsquo;ve sent your first pull request. Don&rsquo;t worry if it doesn&rsquo;t get merged. That&rsquo;s just feedback, just like everything else, and it should encourage you to try harder, to look harder at your code and see where it falls short of the standards set by your community.</p>

<p>Reading other people&rsquo;s code is by far the most direct route to writing code that will be read by many others. Once you start, you will wonder how you ever got by without it.</p>

<h1>Autonomy</h1>

<p>Jeff Atwood has <a href="http://www.codinghorror.com/blog/2012/04/learn-to-read-the-source-luke.html" target="_blank">a lovely post on this topic</a>. It&rsquo;s very cool of him to say this because the fact is, if people started reading source code, we wouldn&rsquo;t need a StackOverflow. Reading source code is the most empowering thing you can do as a programmer. Don&rsquo;t wait to start.</p>