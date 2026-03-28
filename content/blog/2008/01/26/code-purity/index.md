---
title: "Code Purity"
date: 2008-01-26T00:00:00-06:00
slug: "code-purity"
url: "/blog/2008/01/26/code-purity/"
---

The other day, I was listening to the [latest .NET Rocks episode](http://www.dotnetrocks.com/default.aspx?showNum=310) with [Michael Palin](http://www.imdb.com/name/nm0001589/)… I mean [Simon Peyton Jones…](http://research.microsoft.com/~simonpj/) on functional programming and the [Haskell](http://en.wikipedia.org/wiki/Haskell_%28programming_language%29) language. One of the things he talked about really struck a chord with me, namely function "purity" in Haskell.

In modern programming languages, when we call a function, we know what kind of things it needs to do its job (parameter types) as well as what kind of thing it will give back to us as an answer (return type). For example, a function in C# may look like this:

```csharp
int DoSomething(int foo){ … }
```

We know that means it takes an integer and gives us back an integer. But what it doesn't tell us is how it affects the state of the rest of the system, like writing to a file or a database, printing something to the screen, modifying variables outside its local scope, etc. In other words, it may have side effects. Sometimes, we call functions precisely for their side effects (`void Foo(){…}`).

In Haskell, though, by default functions cannot have side effects. These are "pure" functions. If you want a function to be able to, say, write to a file, you must explicitly declare it that way. It actually has a different signature from a function that can't do IO.

This got me thinking about a language I use practically every day at work, COBOL. That's right, boys and girls, it's still out there, alive and (perhaps) well. Where I work, practically all of our business logic is still in COBOL. We're making the transition to the .NET platform, but it's a painfully slow process. Anyway, as I was listening, it occurred to me that every single function ever written in COBOL is, for lack of a better term, "impure." It exists solely to change the state of the system. When you call it, you must know not only what it does, but precisely how it does it.

I'm not trying to just bash COBOL here (as much as it puts a bad taste in my mouth sometimes), but to say that I think this can influence my design of programs in other languages. It's my hypothesis that if more of the functions that I write were "pure" in the Haskell sense, the entire system would be easier to test, debug, and understand. If anyone else has thoughts on this, I'd love to hear them.
