---
layout: post
title: Param Objects in Rails
date: '2012-12-16 21:04:00'
---

In Ruby, everything is an object and this unembarrassed object-orientation gives Ruby much of its power and expressiveness. Being able to call methods on an Integer and override the behaviour of strings are just two of the awesome things that one can do due to this design decision in Ruby.

In Rails, however, sadly, there are large swathes which are not object oriented, and in my opinion, these areas tend to be the most painful parts of Rails. A case in point, and the one we discuss in this blog posts, is the <code>params</code> hash.

The <code>params</code> hash contains all the input sent in by the user to your application. It is, in effect, the way your user communicates with your API. It is the job of the controller to respond to these requests and for that it must do a number of things with the params. It must, first of all, check whether the user is allowed to send this request. For example, is the user trying to maliciously update some forbidden attribute on the object? Perhaps a <code>deleted_at</code> field or something similar? When it comes to functionality like search, we can also use Ruby&rsquo;s metaprogramming abilities to DRY up our views and controllers by allowing the user to send in a param like <code>scope=to_call</code> and dynamically call the scope on the model. But what if the user send <code>scope=delete</code>? We don&rsquo;t want to <code>Quotation.send("delete")</code> now, do we?

Also, many a times, providing a natural looking API to the user means we have to massage a lot of the parameters before we can send them on to our business logic. The API between the user and the controller may be very different than the API between the controller and the models and so a fair amount of massaging sometimes needs to be done. Currently, this is all being done in controllers. So, for example, one often sees things like <code>sanitize_params</code> or worse, code like this

<script src="https://gist.github.com/4313089.js"></script>Note the many conditionals as we try and protect ourselves from malicious code and try to massage the params into something that makes sense to our models.

Consider the following use case. You have a list of <code>Quotations</code>, they all have a particular status and in your model, you&rsquo;ve gone and defined some nice scopes. You want to make these scopes available to the user by passing in a <code>scope=foo</code> parameter. Additionally, you have several search terms you can pass in and there are certain fields that are forbidden to be set in the params. All of this becomes sooooo much easier if you just objectify the params hash. As a bonus, your controllers become even more thin and your param validations and what not become ridiculously easy to test.

I believe the proper term for this is Primitive Obsession, eloquently discussed <a href="http://solnic.eu/2012/06/25/get-rid-of-that-code-smell-primitive-obsession.html" target="_blank">here</a> by our very own Piotr Solnica. I agree with him, based largely on my own experience with this particular smell hanging around the <code>params</code> hash.

My solution is to turn the <code>params</code> hash into a class of its own. Infact, I make one class for every different controller action where I need it. For example, here&rsquo;s the <code>QuotationsIndexParams.rb</code> file

<script src="https://gist.github.com/4312838.js"></script>So easy, simple and clear. If you don&rsquo;t already know <a href="https://github.com/solnic/virtus" target="_blank">Virtus</a>, check it out. It&rsquo;s going to be in the new DataMapper2.

And now that we have full control over our params, our controller can becomes simple as well.
<script src="https://gist.github.com/4312999.js"></script>

How easy is that?

In case you want to be more permissive about the params you accept, you can use an OpenStruct instead of using Virtus. This way, you do not need to know beforehand the attributes you&rsquo;ll be setting.

<script src="https://gist.github.com/4354623.js"></script>In general, it seems to emerge that there is one layer missing in Rails. Something between the Controller and everything else. Between the Controller and the Models, we often have to put Service Objects to keep our controllers free of logic. Between the Controller and the Views, there have been several attempts at Presenters or Decorators like Draper and so on. Jim Gay in his book Clean Ruby has proposed using Contexts or UseCases to simplify our applications.

I am slowly beginning to think the perhaps MVC is only half the picture, and objectifying params is just one step in a multi-front war on spaghetti code in our Rails apps. While the concepts presented in this post are quite simple to implement, I&rsquo;m wondering if perhaps a nice params objectifying gem might be a good idea so we don&rsquo;t have to roll custom solutions each time. I have a great name for it too - <code>parampara</code>!