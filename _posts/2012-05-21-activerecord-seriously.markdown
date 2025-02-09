---
layout: post
title: ActiveRecord - seriously?
date: '2012-05-21 06:47:16'
---

There are some pieces of technology that are so horrible that they quickly become ubiquitous. why is that? PHP, Java, Javascript and MongoDB come readily to mind. Another member of this list is ActiveRecord. That it was developed is not really surprising - that it has gained such widespread popularity is. How come no one has tried to rip apart the beast that is Rails and rip out its putrid heart that is ActiveRecord? Oh, sorry - <a href="http://www.merbivore.com/why_merb.html" target="_blank">they did</a>!

A confession - I had never used ActiveRecord until recently. My Ruby life started with Merb, which ships <a href="http://datamapper.org" target="_blank">DataMapper</a> as default, and DataMapper is beautiful. some of things that you never need to deal with in DataMapper

<ul><li>Your model properties in some other file. Retardedness like &lsquo;schema.rb is the canonical list of columns&rsquo;&hellip;.and what if you are using Postgres HStore? you cannot use schema.rb anymore and you set the dump format to sql? Now do I need to wade through raw SQL? Or open an irb prompt and say Model.attributes?Seriously?</li>
<li>No Enums? Seriously? Go hunting for any of a hundred enumerated_attribute gems. Most of them will store your Enum as a String in the database? Seriously? Why wouldn&rsquo;t I just use String then?</li>
<li>Validating with custom validators is like - seriously?

<pre><code>validates_each :user do |record, attr, value|
 record.errors.add attr, " has to be a customer but is #{value.role}" if value and not    ["customer","admin"].include?(value.role)
end
</code></pre></li>
<li>Setting default proprty values merits <a href="http://stackoverflow.com/questions/328525/what-is-the-best-way-to-set-default-values-in-activerecord" target="_blank">a StackOverflow post</a></li>
<li>Migrations. Holy Mother of God - what a pain in arse. I understand there are some 0.01% corner cases where you want total control over how the database is migrated but come on - your framework should be smart enough to figure out what the database table should look like. Besides, ActiveRecord migrations are so half-assed, they&rsquo;re almost a joke - You can say t.references :other_models and it doesn&rsquo;t even add a foreign key, and to add salt to DRY wounds, you have to go back and specify the relationship in the model?</li>
</ul>Why are so many people building complex applications using ActiveRecord? How are they doing it? I could not abide it for more than a week. I really tried. I&rsquo;ve been lured by the awesome support from the community such as <a href="https://github.com/softa/activerecord-postgres-hstore" target="_blank">ActiveRecord Postgres HStore support</a> and the super gorgeous <a href="http://activeadmin.info/" target="_blank">ActiveAdmin</a> but seriously - it doesn&rsquo;t seem worth it.

The big surprise for me was the response to the question - Who has used DataMapper? - at RubyConf India. Only the questioner and myself had (and on the same project no less). 
Rails people - try DataMapper. It will change your life. And DM2 is going to be even more awesome.

Here&rsquo;s some links to whet your appetite:

<ul><li><a href="http://solnic.eu/2012/01/10/ruby-datamapper-status.html" target="_blank">http://solnic.eu/2012/01/10/ruby-datamapper-status.html</a></li>
<li><a href="http://solnic.eu/2012/03/13/datamapper-2-presentation-from-wroc-love-rb-conference.html" target="_blank">http://solnic.eu/2012/03/13/datamapper-2-presentation-from-wroc-love-rb-conference.html</a></li>
</ul>