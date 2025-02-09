---
layout: post
title: Project Carpool Update
date: '2007-10-11 10:00:00'
---

<div align="left">After several months of intensive coding, an upgrade to Project Carpool is now available. The url is <a href="http://patang.org/projects/carpool/" target="_blank">http://patang.org/projects/carpool/</a>

Many new features have been made available. Some of them are

<ul><li><div align="left">Wikimapia search interface - search for any place on Planet Earth using text.</div></li><li><div align="left">PhpBB Integration - Users are signed up and for the accompanying phpBB forums automatically. Login is automatic. phpBB provides a private messaging mechanism between users so that your email address remains secure. In addition, it provides a platform for community discussion.</div></li><li><div align="left">Groups - Start a group and share trips only with your colleagues and friends. Protect your privacy. </div></li><li><div align="left">Pleasing additions to the user-interface&hellip;.</div></li></ul><div align="left">Some things yet to be implemented properly include better trip management so users can easily accept ridesharers, delete trips etc. A low-bandwidth and mobile version also need to be implemented. Also, a version that looks as pretty in IE as it does in Firefox. Currently, IE does not seem to be happy with the rounded corners and hover effects that show up just fine in Firefox, but I think that&rsquo;s a discussion best left to developers.

Here are some screenshots
</div><a href="http://www.patang.org/blog/uploaded_images/carpool_1-788043.png" target="_blank"><img style="margin: 0px auto 10px; display: block; text-align: center;" alt="" src="http://www.patang.org/blog/uploaded_images/carpool_1-788030.png" border="0"/></a><p align="center">Logged in


<a href="http://www.patang.org/blog/uploaded_images/carpool_2-722873.png" target="_blank"><img style="margin: 0px auto 10px; display: block; text-align: center;" alt="" src="http://www.patang.org/blog/uploaded_images/carpool_2-722862.png" border="0"/></a><p align="center">Searching for location by name

<a href="http://www.patang.org/blog/uploaded_images/carpool_3-738543.png" target="_blank"><img style="margin: 0px auto 10px; display: block; text-align: center;" alt="" src="http://www.patang.org/blog/uploaded_images/carpool_3-738532.png" border="0"/></a><p align="center">Searching for trips starting in a 2km radius from Mani Bhavan<p align="left">Sometimes it seems like this application is never going to get out of beta&hellip;.though that&rsquo;s not such a bad club to be in per se ;-) I am looking for developers to help me out so please leave a comment if you wish to contribute code, css, icons etc.<p align="left"><strong>Some Programming Notes</strong><p align="left">The application is written in PHP with a MySQL database and javascript for the frontend work. I have attempted to preserve clean separation of data and presentation. The backend talks only XML and XHTML, so it can be used as a webservice with the results being plugged into your client of choice. A very quick and dirty templating engine makes sure that there is no confusion between the core classes and the XML generating backend. A particular query can quite easily be switched between returning an XML document or an HTML files.


Open Source - I do believe that this effort represents the worlds first open source carpooling engine (hear hear!). It&rsquo;s on sourceforge. To check out the code do: <ul><li><div align="left">cvs -d:pserver:anonymous@patangcarpool.cvs.sourceforge.net:/cvsroot/patangcarpool login </div></li><li><div align="left">cvs -z3 -d:pserver:anonymous@patangcarpool.cvs.sourceforge.net:/cvsroot/patangcarpool co -P carpool</div></li></ul><p align="left">or browse the code at <a href="https://sourceforge.net/projects/patangcarpool/" target="_blank">https://sourceforge.net/projects/patangcarpool/</a></div>