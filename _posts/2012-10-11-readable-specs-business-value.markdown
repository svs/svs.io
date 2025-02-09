---
layout: post
title: Readable Specs == Business Value
date: '2012-10-11 06:53:00'
---

Consider the situation of a product manager or software architect overseeing the work of several dozen if not hundreds of programmers. If you have been in this situation you know what the problems are

<ul><li>Ensuring delivery quality is a nightmare - you are spending as much time verifying the work of the junior programmers as you are solving problems.</li>
<li>It is very difficult to maintain a clear separation between the architect and the implementation team. Architects are expensive and should not be concerning themselves with low level implementation details. Also, architects need to think at a different level of abstraction than the implementers. However, without this clear separation, you find that a lot of the senior programmers time is wasted on low level issues</li>
<li>Levels of supervision being high, code reviews become time consuming and expensive. Bugs slip through regardless and such bugs become expensive in terms of client satisfaction.</li>
<li>Programmers leave frequently and new programmers need to come up the curve quickly in order to avoid interruptions to service delivery.</li>
<li>Good programmers are hard to find. One has to make do with locally available talent which most likely cannot deliver working code without supervision. Because good programmers are hard to find, they are expensive. Also, they bring about &ldquo;key-man&rdquo; risk in your organisation.</li>
</ul>If your organisation is not serious about writing tests for your code, it is obvious that the product delivery manager is going to have a hard time supervising the output. It is very time consuming to read through source code and one has to verify the correctness of the behaviour by hand. This is clearly not scalable.

If you are writing specs, you&rsquo;re in a much better position. Reading the specs is much easier than reading code and passing specs mean that the code is performing as desired. However, it is possible to turn the humble spec into a powerful force multiplier and generate huge amounts of business value through the simple expedient of making the spec readable.

Consider the following three examples. Which one would you rather work with?

First, the actual implementation itself.

<script src="https://gist.github.com/3870621.js"> </script>This is very cumbersome to work with. The reader has to try and follow along the logic in their mind, an impossible task. Failing that, they must test the functionality by hand which makes the whole process very slow and time-consuming. Clearly not a scalable process. This is why the software industry developed automated testing.

Then, a naively written spec of questionable readability

<script src="https://gist.github.com/3870616.js"> </script>
This is much better. It tests the behaviour of the program automatically and so this means that simply verifying that the spec is properly written means that passing tests give you a lot of confidence in the behaviour of the software. However, the spec is very long and verbose and it is possible that the spec itself has bugs in it. Secondly, lazy programmers can slip in some tests that do not actually test anything.

And lastly, a readable loan spec

<script src="https://gist.github.com/3870632.js"> </script>Ah! So much easier to work with. A casual glance at the spec reveals the intention of the spec and passing specs give confidence as to behaviour. A delivery manager working with the latter is going to be orders of magnitude more productive than with the first. i.e. if you are not writing specs, you are living in the dark ages. If you are, then making them more readable is perhaps the single most effective thing you could do to improve productivity.

Last weekend we were doing a TDD course with a client and here&rsquo;s the spec file that emerged from it. I consider this to be a very readable spec file and my thesis is that were I to be the delivery manager on this project, I have achieved complete decoupling between the architecture and the implementation of the product. Here&rsquo;s the spec for a simple tic-tac-toe game.

<script src="https://gist.github.com/3870588.js"> </script>Readable specs enable the product manager or architect to simply forget about implementation issues and concern himself only with the API and behaviour issues. Specs like these will enable you to manage many more projects than you currently do. With specs like these, you can add more people to the team seamlessly since these specs act as a very readable form of documentation. Readable specs completely decouple your architecture from it implementation. This is a huge win considering the manpower churn that goes on in the industry.

The entire RSpec team must be saluted for creating this amazing piece of software. Whenever I look at a Ruby codebase these days and I don&rsquo;t see a <code>spec/</code> directory, I do a little head-shake inside. There is just so much value hidden inside this gem! <a href="https://twitter.com/dchelimsky" target="_blank">David Chelimsky</a> and team, do take a bow!

If you&rsquo;d like your team to write readable specs, <a href="mailto:svs@svs.io" target="_blank">give me a shout</a>.