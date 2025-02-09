---
layout: post
title: Paperclip for PDFs
date: '2012-03-13 05:00:34'
---

<p>If you are using paperclip and find that thumbnails are not being generated for pdf files and you have checked that identify and convert are on your path, then perhaps the answer is to  specify a format in the styles hash. If this is not specified, pdfs remain as pds. so do something like</p>

<pre><code>has_attached_file :image,
  :styles =&gt; {
    :edit   =&gt; ['600x600', :png],
    :medium =&gt; ['400x400', :png],
    :thumb =&gt; ['100x100', :png]
  }
</code></pre>

<p>Hope this saves someone having to dive into paperclip code (as nice as it is)</p>