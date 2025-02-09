---
layout: post
title: Multiple Mailer Configurations with ActionMailer
date: '2012-04-26 17:28:12'
---

<p>So I just started using the wonderful service from <a href="http://postmarkapp.com" target="_blank">Postmark</a> which makes processing inbound emails absolutely a breeze, but I was a little concerned with how much it was costing me to send notification emails to the admins on the site, along with exception notifications. Even though Postmark is a reasonably priced service, there&rsquo;s no reason really to pay Rs. 0.075 per email to admins for exception notifications. So how do you set up multiple mailer configs in your Rails app? It&rsquo;s trivially simple actually - say you have two Mailer classes a User Mailer and an AdminMailer. Set up your normal Gmail smtp mailer as usual</p>

<pre><code>config.action_mailer.delivery_method = :smtp

config.action_mailer.smtp_settings = {
  :address              =&gt; "smtp.gmail.com",
  :port                 =&gt; 587,
  :domain               =&gt; 'svs.io',
  :user_name            =&gt; 'svs@svs.io',
  :password             =&gt; 'N0tMyPa55w0rd!',
  :authentication       =&gt; 'plain',
  :enable_starttls_auto =&gt; true  }
</code></pre>

<p>and then add the magic line which tells UserMailer to use postmark</p>

<pre><code>UserMailer.delivery_method = :postmark
config.action_mailer.postmark_settings = { :api_key =&gt; "n0tmY-ap1-kev-e1th3r" }
</code></pre>

<p>simple!</p>