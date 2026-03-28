---
title: "ASP.NET MVC on IIS: Fixing the Blank Page Problem"
date: 2010-10-01T00:00:00-06:00
slug: "mvc-iis-blank-page-fix"
url: "/blog/2010/10/mvc-iis-blank-page-fix/"
---

Since I recently purchased a new laptop for my personal dev machine, I've been working with a pretty fresh install of Windows 7. As I was prepping a presentation for Tyson Dev Con, I ran into an odd and frustrating problem. My presentation involves an ASP.NET MVC application, specifically served from IIS rather than the ASP.NET development server, so I enabled IIS and ASP.NET in the "Add/Remove Windows Features" dialog.

I successfully deployed the app to IIS, but when I hit the site, all I got was a blank page. What was odd was that when I deployed a Web Forms app to IIS, it worked just fine. This led me to think it was a problem with my MVC installation, but that wasn't actually the case.

When you install IIS with the default features enabled, one of the things that is off by default is HTTP Redirection. The first thing the default MVC template application does when you request the root of the app is redirect to Home/Index. Enabling this IIS feature fixed my problem immediately.

Hopefully this will help someone who runs into the same problem!
