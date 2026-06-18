---
layout: default
title: Dynamic Runtime Project
---

# Dynamic Runtime Project

*Mar 13 2019*

For those who have read prior posts in this pseudo-blog, you will see an emphasis on using data to drive application design.
And when I say data, I do not mean data stored in database tables, but data created using code or stored in files in
source code. Recently, I started trying to look for other developers that may have come to similar conclusions to my
own, but I have yet to find a kindred soul. As I explained in the prior blog, this is due to the ambiguity in terms like
*schema*, *runtime*, and *dynamic* making it impossible to create a good google search to find people with similar ideas
to my own.

I got so frustrated, that I asked, has anybody registered a domain name using a combination of my search
terms? For example, picking two of the most generic of my terms, surely somebody has created a website with a domain
name that joins the words *dynamic* and *runtime*. So I looked to see if there was a website with domain name
*dynamicruntime.com* or *dynamicruntime.org* and I was shocked to discover that nobody had yet thought to register this
domain name. So, in a challenge to the world at large, I have registered both **dynamicruntime.org** and
**dynamicruntime.com** under the ownership of *Gyassa Software* and I have set up a website at
https://dynamicruntime.org.

Fortunately, during the last few months, I have had the opportunity to focus exclusively on my own projects. During
these months I decided to investigate AWS and all the amazing things it allows software developers do for free. As part
of that effort, I created the github account https://github.com/sampwhite/dynamicruntime where I have put my recent
labors. It is all under the MIT license, so feel free to steal or copy any of it. This repository is the code base for
the website that is now deployed at *dynamicruntime.org*.

The thing I like most about this project is the speed with which the server starts up. By eschewing the standard large
frameworks (such as Spring) and writing endpoint implementations from scratch, I am able to keep the start up time under
five seconds. And this is for an application that automatically creates database tables and implements a full login
implementation that supports two step authentication using email. The code base has other nice characteristics that come
from avoiding the usage of third party frameworks. For example the method call stacks are fairly shallow and IntelliJ
just loves the code.
