---
title: "Unit Testing 101"
date: 2008-02-13T00:00:00-06:00
slug: "unit-testing-101"
url: "/blog/2008/02/13/unit-testing-101/"
---

In response to [Chad Myers'](http://www.lostechies.com/blogs/chad_myers/archive/2008/02/10/you-need-to-blog-now.aspx) admonition to blog now, I'm going to try my hand at writing about a few of the topics he suggested, starting with "Unit Testing 101." I think these entries will mostly be for me, to solidify my own understanding of these topics. But maybe I can say something differently than the way it's been said before and help someone out a bit.

**DISCLAIMER:** I do not claim to be an expert on this or any further topics suggested by Chad. Feel free to correct me when I post something that's incorrect or misleading, but let's keep it friendly. I'm a newbie here, okay? 😉 Examples will be in C#.

First off, why would we need unit tests? We can already build applications that work correctly without writing a single unit test. Why go to the extra effort (and write the extra code) when we can get along just fine without them?

The purpose of unit tests is to verify that each particular atomic piece (usually a method on a class) works correctly, independent of the code around it. By testing a method in isolation, you can have greater confidence that your method works the way you intended it to. Once you have a reasonable level of confidence that methods work the way they are supposed to, you can narrow down the source of a bug by eliminating possibilities of where that bug might exist. Unit tests also help when changes need to be made to a system. After you've changed the way a method works, your unit tests can verify that the method still works correctly.

To illustrate, let's look at a very simple method called GCD. As you might expect, this method will take two integers as parameters, and return the greatest common divisor of those two integers. The method might look something like this:

```csharp
static int GCD(int a, int b)
{
    int Remainder;

    while( b != 0 )
    {
        Remainder = a % b;
        a = b;
        b = Remainder;
    }

    return a;
}
```

Pretty simple. So now, let's write a function to make sure this method returns the correct value for a given set of inputs.

```csharp
static void GCDOf10And25ShouldBe5()
{
    Console.Write("Test – GCDOf10And25ShouldBe5: ");
    int result = GCD(10, 25);
    if(result == 5)
    {
        Console.WriteLine("Passed");
    }
    else
    {
        Console.WriteLine("Failed");
    }
}
```

We can write other tests like this for our other methods, and call them all to produce a kind of report that will tell us which tests passed and which failed. I know what you're thinking. "Brian, that sure is a lot of code." Yep. And repetitive, too. Lots of calls to `Console.WriteLine`. And what happens when the number of tests you have starts to grow? You'll have a hard time picking out that one line out of twenty or thirty that says "Failed."

Well, that's because I just showed you the old-fashioned way of writing unit tests, using only what's available by default in the programming environment. If you're going to use unit tests, you need to use a unit testing framework. There are lots of them out there, for just about every language you can think of (and quite a few that you didn't know existed, most likely). A lot of them are in the "xUnit" family, which means that they have their roots in a framework called SUnit originally developed for Smalltalk by a guy named Kent Beck. I'm going to use one of those frameworks, MbUnit, in my examples. When you download MbUnit or other similar framework, you get a set of libraries to use in your tests, plus a test runner console application (and sometimes a GUI version) that will run all of the tests in a class or group of classes.

So let's rewrite that test we just made using MbUnit:

```csharp
[Test]
public void GCDOf10And25ShouldBe5()
{
    Assert.AreEqual(5, GCD(10, 25));
}
```

Well, that was a bit shorter at least. Let's take a look at what's going on here. That `[Test]` attribute up top is just to let MbUnit know that this method needs to be called when we use the test runner. The only method we're calling, `AreEqual`, is the backbone of an xUnit type testing framework. When the two parameters passed in to the method aren't equal, an `AssertionException` is thrown. It is caught by the test runner and turned into a readable message. The test runner also keeps track of how many tests passed and how many failed, and displays a summary once all the tests have been run.

There are tons of other features of this and other frameworks, way too many to go into here. If I've piqued your interest at all, check out the [Wikipedia article](http://en.wikipedia.org/wiki/List_of_unit_testing_frameworks) on testing frameworks and pick one for your environment.
