---
layout: post
title: Extreme Decoupling FTW
date: '2013-01-14 19:49:00'
---

After spending years writing fat, ugly classes, I suppose it is inevitable that the pendulum swings the other way and we head towards &lsquo;wat? you got a class for that..?!?!?!?&rsquo; territory. For the moment though, the wins are huge and keep coming.

Dependency Injection is just a big word for explicitly passing in stuff that your object is going to depend on. Recently, I&rsquo;ve been working on an app that automatically goes and make reservations using certain APIs. Testing it is always problematic because one has to constantly stub out the actual API call with something that doesn&rsquo;t contractually oblige you to several thousand rupees of payments :-) Also, the frontend is being worked on by a separate team and I don&rsquo;t want the dev/staging server to make actual bookings during development. With DI, dealing with situations like this is easy.

While building the app, I decided that nothing was going to talk to anything without talking to something else first. Here&rsquo;s a rough sketch of the architecture.

<div class="gist"><a href="https://gist.github.com/svs/4532644" target="_blank">https://gist.github.com/svs/4532644</a></div>

<script src="https://gist.github.com/4532644.js" type="text/javascript"></script>The <code>TripBooker</code> first calls out to a <code>VendorSelector</code> service which provides a list of vendors based on any filtering rules that might apply. Then, each vendor is passed into a <code>VendorBooker</code> service that does the booking with that vendor. It also calls out to the <code>CredentialSelector</code> service to choose an appropriate set of credentials for the calls. What does the <code>VendorBooker</code> look like?

<script src="https://gist.github.com/4532659.js" type="text/javascript"></script>Oooh&hellip;.more indirection. The <code>VendorBooker</code> creates objects of class <code>FooCustomer</code> and <code>FooBooking</code> and uses the class <code>FooBookingResponse</code> to parse the results from the booking. Internally, <code>FooBooking</code> calls the wrapper class to the API with the appropriate parameters. <code>FooBooking</code> is the translator class that translates from a generic <code>Booking</code> object into one that fits with <code>Vendors::Foo</code>&rsquo;s idea of what a booking is. The API wrapper class <code>Vendors::Foo</code> has no idea about anything that just happened above. It merely accepts some arguments to the constructor and makes appropriate calls to the foo.com API.

So, what does four levels of indirection get you? Easy pluggability. As I mentioned, I&rsquo;d had to stub out the calls to the foo API during testing and I was about to deploy the app on to a staging server so our frontend team could write code against it, but I didn&rsquo;t want to actually make bookings. So, I decided that I would have a different setup for development, staging and testing. Basically, during testing, I make <code>VendorSelector</code> return <code>[:dummy]</code> as the vendor. Then the class <code>DummyBooking</code> and <code>DummyBookingResponse</code> returns a dummy booking without making any API calls.

By being explicit about the dependencies at every step of the way and making sure each class only does one thing, we&rsquo;ve made life super easy when we need to introduce new behaviour in particular situations.

This is just one of the ways in which DI helps massively. Here&rsquo;s a flowcharty diagram

<a href="http://imgur.com/1u0W0" target="_blank"><img src="http://i.imgur.com/1u0W0.png" title="Hosted by imgur.com" alt=""/></a>