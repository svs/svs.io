---
layout: post
title: Simple Activity Logging in Ruby
date: '2013-01-04 10:45:00'
---

Activity Logs are one of the most requested (and consequently, most under-used) features in an application. Managers love the idea of having a full history of everyone&rsquo;s activity, even though their startup will probably pivot before they have a chance to look at these logs. Jokes apart though, the data in the activity logs are tremendous sources of business value which can help you iterate ever closer to your users requirements. So, it kind of helps to have handy a nice pattern to simplify the creation of activity logs.

The whole problem with activity logging is that it straddles the Controller and Model layers. The activity is being performed on a model, but the model has no knowledge of who is performing the activity. This knowledge is the preserve of the Controller. There have been many approaches to solve this problem. Some of them include setting global environment variables (ouch! not thread safe), using <code>cattr_accessor</code> and hacking around with threads and so on, or perhaps this approach recommended in <a href="http://rails-bestpractices.com/posts/47-fetch-current-user-in-models" target="_blank">http://rails-bestpractices.com/posts/47-fetch-current-user-in-models</a> which includes thread local variables.

The other layer of complexity of course arises from the question of how to actually do the logging. Who should do it? When should it be done? Traditionally, approaches have centered around before/after callbacks in the model i.e Doing a <code>before_save :get_attributes</code> to get the original state of the object and then a <code>after_save :log</code> to write the activity log. We all know what the problems are with before and after hooks - they add behaviour to the model which is orthogonal to the models main concern which is persistence. Also, such hooks need to stubbed out while testing, etc. Also, adding hooks means we do not have any control over when stuff gets logged and when it doesn&rsquo;t. If tomorrow we want to create a secret backdoor for the NSA to silently change data behind people&rsquo;s backs, we can&rsquo;t do that without changing the model, which means that a concern with logging is causing the class that deals with persistence to change - Holy SRP Violation, Batman!

If you&rsquo;ve been in this situation, then I can say, fear no more - there&rsquo;s a very simple way of solving this problem. Here goes - consider the following situation

<script src="https://gist.github.com/4450794.js"></script>We have Users, who can have Foos and also there are Staffs who do data entry as well and can modify Foos that belong to other users. In our <code>ActivityLog</code>, we want to persist the following stuff
<script src="https://gist.github.com/4450809.js"></script>

Now, here&rsquo;s the absurdly simple solution. Are you ready? Ok. In your controller, instead of calling <code>@foo.save</code>, say <code>@foo.save_with_log(current_user)</code>. Got it? Ok, I&rsquo;ll say it again.

<script src="https://gist.github.com/4450828.js"></script>Simple? We need the model to know about a user? Pass it in as a parameter! <d-oh></d-oh>

Now, what about the model side? As much as we hate mixins as a form of inheritance/delegation, logging is one of those cases where mixins are a good approach (I&rsquo;m willing to be convinced to the contrary though). This is behaviour that is shared across all classes that need logging and it actually has nothing to do with the behaviour of the class itself, so we&rsquo;ll make a nice little module called ActivityLogger which looks like this. Please note, this is pseudo code (it hasn&rsquo;t been tested)

<script src="https://gist.github.com/4450851.js"></script>and we update our model thus

<script src="https://gist.github.com/4450866.js"></script>The <code>log_with :foo_activity_log</code> is important. What we&rsquo;ve done in the <code>save_with_log</code> and <code>update_with_log</code> methods that we included from <code>ActivityLogger</code>, is to inject a dependency on the appropriate class that will do the actual logging for us, in this case <code>FooActivityLog</code>. In order to save (or update) and log, we need the objects attributes, a handle to the user/staff who is performing the action and alongside these, we can pass any other data in a hash which the actual logger can use as it deems fit. To achieve all this, what the <code>ActivityLog</code> module does is to translate a call to <code>@foo.save_with_log(staff)</code> into a call to <code>FooActivityLog.save_with_log(@foo, staff, options)</code>.  So, what does the actual logger look like? Something like this

<script src="https://gist.github.com/4451580.js"></script>Several advantages to this approach which accrue from favouring the explicit over the implicit.

<ul><li>We have two implicit methods now, <code>save_with_log</code> and <code>update_with_log</code>, so we don&rsquo;t need to mess with before and after callbacks.</li>
<li>The actual activity log model inherits from <code>ActivityLog</code>, so we can override behaviour as we wish. For example, while saving nested models we might want to call save_with_log on the nested model as well. For such situations, we can easily implement methods as we deem fit in the appropriate child class of ActivityLog</li>
<li>We are injecting controller variables explicitly, such as the current user or staff member performing the action. This saves us from having to mess with thread local variables. It also makes it super easy to test this functionality.</li>
</ul>So, in short, being explicit about all our dependencies gives us a tonne of advantages over other approaches that depend on magic or callbacks.

What do you think?