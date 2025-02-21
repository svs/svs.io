---
layout: post
title: Your Tools Matter
date: 2025-02-20 13:02 +0530
---
Your tools matter. The tools you use say a lot about you as a developer. Specifically they tell me a lot about how much you have been exposed to which programming culture.


![Alt text](/assets/tools-matter-1.png)


There used to be an OG programming culture. The lineage that we count Kernighan and Ritchie and Linus and Stallman in is where all of programming came from. And we have an incredible opportunity to step into their minds by using the tools they (or their contemporaries) designed and built. These guys started out being able to edit only one line at a time. They built `ed` which is a line editor. Yes, that's right, one could only edit one line at a time.

As computers improved, text editing of entire documents became possible. At this time, the only people using computers were high powered nerds, so they built power user systems. And this is the killer factor in any tool that comes from that age. They were moulded by the day-to-day use of these power users and as such give us a unique insight into the minds of their creators. And when you start to approach power use of these tools, you start to completely change your approach to computation.

Especially in text editing, the goal of the early editor developers was always to play text like music, where the keyboard is the piano. Text, especially code, is ideas manifest. And to combine and separate those ideas, to manipulate them like some raw material, has always been the core philosophy of these older tools. Take even the UNIX way - lots of small tools working together, coordinating through pipes. Simple philosophy, emergent behaviours. See something like xargs, which can pipe the output from one command as an input to another. Excellent tooling for working with, for example, all files in a directory and so on.

And when you use these tools you find that their driving motivation was this - this thing I do 100 times a day, this needs to be easier to do. And to do this in such a generic place - text editing - and to cater to the editing needs of the whole world, from code, to papers, to love letters - this software has to be incredibly well designed. All your 'system design' and 'architecture' problems are present here. Take some time to see how this was solved. It will change everything about how you program.

See also the modern equivalents. I cannot use VSCode because there is no way to create a file without going through the Mac file dialog. That seems so absurd to me. Why would you allow your users to go through this pain? You can see right at the start the lack of a coherent philosophy behind editing text. The point is not to put characters into a file. The point is to allow you to mould those characters like clay - characters, lines, words, paragraphs and even s-expressions....

One of my favourite stories is of the time we hired a junior developers - we were working from my house and I had an old PC which he was to use. I said what do you use to do your work and he said Arch Linux. I said ok - if you can install Arch on this computer then you have the job. Two hours later he was done with networking, X, Gnome and everything. This was a guy who knew his tools. And it came as no surprise to anyone that he turned out to be a cracked programmer!

I've seen people write 300 lines of python where a few well connected shell commands would do. And the attitude that causes this is the exact opposite of the attitude that formed the early culture of programming. Y'all take pain too easily. I look at some codebases and I can see that not a single person who worked on that codebase ever felt that life should be easier. Come to work, spend 12 hours, go home, progress be damned - this is the state of the normal programmer. And they accept this because they haven't seen better. And they haven't seen better because they accept this.

Demand more from your tools. Ask why the stuff you do all day is so tedious and difficult. It does not have to be. There is always a better way, and when you ask the better way of your tools, you will come into a meeting of minds with the greatest system builders that ever lived, through the tools they left us.

