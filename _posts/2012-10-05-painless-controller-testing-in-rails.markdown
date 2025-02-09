---
layout: post
title: Painless Controller Testing In Rails
date: '2012-10-05 06:21:00'
---

There seems to be reasonable support for the opinion that controllers do not require testing and that integration tests or acceptance tests are sufficient testing for controllers. I think in large part this opinion arises because controllers are hard to test. In this post, I&rsquo;d like to share a technique I use for painless controller testing, but before that we can <strong>try and answer the question whether acceptance/integration tests are sufficient tests for the controllers as well</strong>.

In my opinion, <strong>what an application does and the visual representation of those actions are two completely different things</strong>. To give a trivial example, when testing the <code>create</code> method using capybara one might say <code>page.should have_text("Item was saved")</code>. Is this a sufficient test? Could there be a situation where the controller thinks an item has been saved but it hasn&rsquo;t actually? I think so. A much more comprehensive test would be to specify <code>expect { post :create, {:item =&gt; {...}}}.to change(Item, :count).by(1)</code> in a controller test. Secondly, if tomorrow you get an edgy designer who says hang on, this message is really boring. We want the flash message to say &ldquo;Awesomesauce! You&rsquo;re bloody brilliant you are!&rdquo; then you have no way of knowing that your controller is ok but your views are out of date. <strong>Coupling the UI with the functionality is unnecessary complexity</strong>, which brings us neatly to our next point.

<strong>Controllers are the API of your application</strong>. They are how your application talks to the outside world and is a major piece of your architecture. This code does some very specific things and it must be under test coverage. Additionally, <strong>testing is less about catching bugs than it is about coaxing a solid architecture out of your various constraints and requirements</strong>. Test Driven Design really works and the payoffs are so fundamental that I for one am never going back to the old way of doing things. I&rsquo;m not saying TDD is the silver bullet of software development, just that it is probably the easiest way to come up the curve when it comes to thinking architecturally. My previous articles on Test Driven Design and State Machines all came out of rigorously applying TDD and really taking a step back to address the issues that were making my tests painful. The payoff is huge - code that is very modular and easy to maintain.

So I took the same approach to controller testing. The first thing to do is to take a naive approach to controller testing. If you look at the controller spec file that RSpec generates, it does a pretty good job of enumerating the responsibilites of a controller. Let&rsquo;s quickly go over these

<ul><li>variable assignment - does the controller gather the correct data from various places?</li>
<li>message expectations - does the controller call the correct methods with the correct parameters on the correct object?</li>
<li>response handling - does the controller render the correct template or redirect to the correct URL?</li>
</ul>Here&rsquo;s the rspec output

<script src="https://gist.github.com/3838232.js"> </script>These are the classic responsibilites of the controller. Technically, it is not the responsibility of the controller to worry about the side-effects of the method calls it makes. However, for some issues there is no better place to put this spec and I personally prefer this functionality to be tested in the controller. RSpec agrees and does the same -

<ul><li>side effects - does the controller add/delete items as requested? Specifically you will often see a <code>expect { ... }.to change(Foo, :count).by(1)</code>.</li>
</ul>In a simpler world, this would be enough to decide if the controller is working as specified. For better or for worse, our world is not so simple and we have additional responsibilites in our controller, specifically authentication and authorization. Deciding whether to allow a particular action to a particular user is responsibility of the controller and this adds a lot of pain to our tests. Now for each user role, we need to have one set of tests. Already we start to feel the pain but we want to delineate the pain more precisely so we plough through it, ending up with an RSpec output that looks like this.

<script src="https://gist.github.com/3838391.js"> </script>OK, this is just for two user roles and we already have a spec file of 400 lines. <em>Clearly, an unsustainable situation!</em>. A spec should be easy to read and comprehensible at a glance. We need to refactor. Ploughing through the pain of our repetitive tests we learned a few things about our controller. The first thing we learned is that the only method that behaves differently from the others is the <code>index</code> method. The reason for this is that when a user is not authorised to edit or update some object, a CanCan::Unauthorized is raised. It is only for the index action that we might want to return different objects based on access control. What this means is that we only need to login with credentials to test correct assignment of data for the index method. And we do so with a small shared example group like so

<script src="https://gist.github.com/3838340.js"> </script>Voila, we have a one-liner to check data assignment for any given user role.

Now, we move on to testing the various other actions. Since the remaining actions now only need to know whether the user is authorised or not, we don&rsquo;t need to actually login. This is a good place to use a stub because the stubbed functionality is unlikely to change. Here&rsquo;s a look at the little DSL we might implement to help us keep our tests DRY.

<script src="https://gist.github.com/3838370.js"> </script>One nice thing about this approach is that it preserves the informative failure messages from RSpec.

<script src="https://gist.github.com/3838383.js"> </script>The whole example including testing authentication, authorisation, message expectations, variable assignment, response handling and side effects is about 80 lines of RSpec with about 40 lines of shared examples.

The whole example is available here: <a href="https://github.com/svs/painless_controller_tests/blob/master/spec/controllers/items_controller_spec.rb" target="_blank">https://github.com/svs/painless_controller_tests/blob/master/spec/controllers/items_controller_spec.rb</a>