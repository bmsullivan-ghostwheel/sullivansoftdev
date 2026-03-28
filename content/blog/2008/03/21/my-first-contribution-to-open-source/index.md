---
title: "My First Contribution to Open Source"
date: 2008-03-21T00:00:00-06:00
slug: "my-first-contribution-to-open-source"
url: "/blog/2008/03/21/my-first-contribution-to-open-source/"
---

As you probably know from previous posts, I am a recent convert to [Twitterism](http://www.twitter.com/). I totally dig the dirt simple minimalist web UI that the service provides, but I noticed that several of the people I was following were using some other kind of client to tweet with. So I checked out the one I saw [The Elder](http://keithelder.net/) using, called [Witty](http://code.google.com/p/wittytwitter/).

Well, it turns out that he helped write the thing, along with [Jon Galloway](http://weblogs.asp.net/jgalloway/default.aspx), project owner [Alan Le](http://blogs.vertigosoftware.com/alanl/default.aspx), and several others. It was a nice piece of WPF-y eye candy, and worked perfectly, too. At least, as long as you weren't behind a proxy. I've mentioned my utter loathing of my employer's proxy before (and in association with Twitter), and this just added insult to injury. So, since it was open source, I decided to make a patch to add proxy support for Witty.

It took me most of an afternoon, but I'm sure others could have done it much more quickly. The real meat of the changes were pretty darn simple, since the .NET API for performing HTTP requests allows a proxy configuration as an optional parameter. But I'd never used [TortoiseSVN](http://tortoisesvn.tigris.org/) before (a popular application used to interact with a [Subversion](http://subversion.tigris.org/) source control repository), I'd never written an angle bracket of XAML, and I was looking at the codebase for the first time, so I cut myself some slack. 😉

So, by that afternoon, I had the changes working. A few days later, after getting in contact with Alan and him patiently explaining the proper way to post changes (there's a "Create Patch" option in TortoiseSVN), I submitted my changes for review. Frankly, having never contributed to an open source project before, and coming from a development shop that is really just getting started in .NET (and thusly hasn't provided a whole lot of guidance on good design), I was afraid my code wasn't going to be up to snuff. However, Alan contacted me later that day to tell me he thought my changes looked solid and that he had merged them with lots of other improvements contributors had made to the project. That evening, he posted Witty version 0.1.7 Beta 1 to it's Google Code page. Code I wrote was officially available to the whole world!

It was a lot of fun working on the enhancement, and rewarding to think that I may have helped a person or two out. There's a lot still to work on, though. For example, even though you can receive tweets through an authenticated proxy on Witty now, the avatar images don't show up if you've got that same proxy configuration on IE. That's because the XAML elements used for the avatar images use the real twitter URLs as their source property, so the application uses IE's configuration when going to get those images. Since there's no way to authenticate when the app makes those requests, you never get the images back.

Anyway, such is the life of an OS app. Always more to do. I highly encourage you to find an open source project you like and take a crack at adding a feature or fixing a bug. I had a great time doing it, and I plan to continue. Not only is it rewarding to help someone out, but it also exposes you to others' code, which can only help you improve your own style and technical chops.

P.S. – Don't forget to check out [Witty](http://code.google.com/p/wittytwitter)!

P.P.S. – Note to other people at the company I work for: this doesn't mean Witty works for us now. The net nanny still blocks any web requests to a Twitter URL, so we're pretty much out of luck. :-/
