---
title: "So There :P"
date: 2008-03-04T00:00:00-06:00
slug: "so-there-p"
url: "/blog/2008/03/04/so-there-p/"
---

What do you do when the Twitter site gets blocked? Host a Twitter client yourself, of course! Since Twitter exposes a public API, it's very easy to set something up that gets your personal timeline and lets you update your own status. [Twitteroo](http://rareedge.com/twitteroo/blog/) makes it even easier to do from a .NET environment; many thanks to the guys at RareEdge for providing a library that abstracts away all of the WebRequest stuff.

What I have is pretty ugly-looking right now, since I wasn't very worried about how it looked, just that it worked. I'm not displaying people's pictures, since the images tags would result in requests to the Twitter server from the end-user's browser and would get blocked. I'll make it work eventually, but it'll require some gymnastics. Something like having the aspx page do a web request for the image, save the bytes off in some temp directory, then change the tag to point to the temp directory. That, plus a little bit of styling, and it should be passable.

Feel free to use it as-is. You'll just have to trust that I'm not stealing your username and password and using it to post embarrassing messages to your account. ;-P Also, be forewarned, it's not the most secure thing in the world. Just to get it to work quickly, I save off the password to a session variable, since I have to have it to pass to the Twitter API.

I'm becoming a big fan of these open web APIs and services. I'm thinking of using the [Virtual Earth API](http://dev.live.com/virtualearth/sdk/) in the site that I'm currently developing. It fits the problem well, and it's just so darn easy to use. Conveniently, there's also a [DNRTV](http://www.dnrtv.com/default.aspx?showNum=104) episode this week about using it in ASP.NET that I'm looking forward to watching tomorrow.

Hooray for a web-enabled world!
