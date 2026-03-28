---
title: "More Adventures with Witty"
date: 2008-04-04T00:00:00-06:00
slug: "more-adventures-with-witty"
url: "/blog/2008/04/04/more-adventures-with-witty/"
---

As you may have noticed, I haven't been keeping up with my desired posting rate lately. That's mostly because I've been spending most of my outside-of-work "geek time" working on [Witty](http://code.google.com/p/wittytwitter/), an open source Twitter client written in C# using WPF. After my positive experience building in proxy support, I decided to implement another feature that I particularly wanted to see, namely "toast" popups. (You know, those little mini windows that show up at the bottom right of your screen when you get a message in some IM clients.)

This enhancement was already posted as an issue on Witty's issue board, so I knew I wasn't off base thinking it was missing. The person who originally asked for the enhancement suggested that we send messages using [Snarl](http://www.fullphat.net/), an application aimed at recreating Growl on the Mac, to minimize the additional code needed to achieve the toast functionality. So I visited the site, looked at the API, and set about trying to send Snarl messages from a C# program.

Even though the site listed C# as one of the supported environments, it turned out that the only semi-useful library available was unmanaged C++. "No problem," I thought. All Snarl requires that you do is send a Windows message to the Snarl application, so I figured that I could translate the needed Win32 calls by looking at [pinvoke.net](http://pinvoke.net/) (a wonderful resource for Win32 interop programming, btw).

I was half right, I suppose. The SendMessage call was easy enough to duplicate, but the kicker was the data structure I needed to pass to the Snarl application. As I would soon discover, even though the type names in C++ and C# are very similar, they mean different things behind the scenes.

For example, a "long" that was part of the data structure in C++ is actually only 32 bits in size, whereas a "long" in C# is 64 bits. Those were easy enough to fix, since C#'s "int" type is the appropriate length. The character arrays were a bit trickier. First of all, a "char" in C++ is one byte, while in C# it is 2 bytes. You can get around that by changing the char arrays to byte arrays. But the thing is, arrays in .NET are a totally different animal than they are in C++. In order to use fixed-length arrays like Snarl wanted I had to use the "fixed" keyword, like this:

```csharp
public fixed byte text[1024];
```

The problem with that is that in order to get it to compile, you have to mark the struct using it with the keyword "unsafe." Yuck, that doesn't sound good, does it? And it's not. When you do things like that, you open yourself up to all kinds of mess that you thought you left behind when you started writing .NET code. Not fun.

But, hey, it got messages to from Witty to Snarl, so I submitted a patch. The guys were happy to see progress made, but the unsafe marker made everybody nervous. Plus, it meant relying on an outside application for notifications. So, I got to work doing what I probably should have in the first place: creating integrated popups in WPF.

Meanwhile, [Michael Letterle](http://blog.prokrams.com/) got wind of the Snarl integration that we were trying to do, and fixed those notifications faster that should be humanly possible. The trick was a couple of attributes I'd never heard of before: `[StructLayout]` and `[MarshalAs]`. The structure ended up looking like this:

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
struct SNARLSTRUCT
{
    public int cmd;
    public int id;
    public int timeout;
    public int lngData2;
    [MarshalAs(UnmanagedType.ByValArray, SizeConst = SnarlInterface.SNARL_STRING_LENGTH)]
    public char[] title;
    [MarshalAs(UnmanagedType.ByValArray, SizeConst = SnarlInterface.SNARL_STRING_LENGTH)]
    public char[] text;
    [MarshalAs(UnmanagedType.ByValArray, SizeConst = SnarlInterface.SNARL_STRING_LENGTH)]
    public char[] icon;
};
```

Normally, the CLR will store a structure's component parts wherever it feels like, so the parts of the struct could be separated. In a managed world, this doesn't bother us, we can always get back to the data if we need it. But if we need to pass this struct to unmanaged code, we have to tell the CLR specifically to keep all the parts in one place, which we can do by specifying the `[StructLayout]` attribute. I'm a little fuzzier about what the `[MarshalAs]` attribute does, but it's pretty obvious that it's telling the CLR how this managed data needs to be translated when passed to unmanaged code, and it lets us specify a length for the arrays.

Back in WPF land, I got to play around with a bunch of neat stuff. Even just copying the look and feel of the login window, I started to get a feel for XAML, at least in some of its simpler forms. I thought that the way it defines animations was particularly interesting. To do a fade-out for the popups, I used a XAML animation sequence declaratively. I just think it's cool that you can declaratively define an animation sequence. It just seems a lot more self-contained and comprehendible when it's laid out like this, as opposed to a chunk of imperative code. It's hard to define why it feels better, but it just does. I think that's what they call getting the zen of a particular technology, and I think I may have gotten a whiff of the zen of WPF here.

I'm hopeful that we'll be able to start using WPF in my workplace within the next couple of years. We've still got some Win2k machines out in the field, so they will have to be phased out before that can happen. That, and they'll have to spring for VS 2008 for the developers ;-).

I've had a lot of fun working on this, but after this feature is declared finished, I think I ought to work on a bug fix next. One of the things I've heard about OS projects is that most people just want to work on new features, and it's hard to get people to fix bugs. I want to be more responsible than that, so I think I'll take one (or a few) for the team and do something that may not be quite as fun next time.

Keep checking for new releases, Witty is getting better all the time!
