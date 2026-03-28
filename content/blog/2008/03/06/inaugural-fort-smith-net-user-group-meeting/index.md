---
title: "Inaugural Fort Smith .NET User Group Meeting"
date: 2008-03-06T00:00:00-06:00
slug: "inaugural-fort-smith-net-user-group-meeting"
url: "/blog/2008/03/06/inaugural-fort-smith-net-user-group-meeting/"
---

Thanks to the hard work of several [people](http://www.fsdnug.org/officers.html) in the Fort Smith area, our first DNUG meeting was a great success. [Raymond Lewallen](http://codebetter.com/blogs/raymond.lewallen/default.aspx) spoke on Behavior Driven Development, a subject I had been curious about. I'd heard that it was the right way to do Test Driven Development, but little other than that. Ray did a great job explaining the gist of it. Since [Mo](http://www.mohundro.com/blog/2008/03/04/FSDNUGMeetingWithRaymondLewallenOnBehaviorDrivenDesign.aspx) has already listed some great resources on learning about BDD, I'm just going to highlight some of the things that were interesting to me.

First of all, the way Ray named his test fixtures and tests jumped out at me. Normally in TDD (in my limited experience), you'd have a fixture named AccountTests to test an Account object, and tests named something like `New_balance_should_reflect_old_balance_plus_deposit_amount`. Rather than doing that, Ray's fixture would be named `When_depositing_money_into_an_account` and the test would be named `New_balance_should_reflect_old_balance_plus_deposit_amount`. The difference there is that the context of the tests is defined by the fixture name, and the test describes the event and the expectation of the results of that event. I think that's a much better way to name your fixtures, and better leads you to what tests you need to write. Ray said that that was one of the main goals of BDD, not just writing tests, but writing the right tests.

Also, he demonstrated a project he worked on called [Spec Unit](http://code.google.com/p/specunit-net/), which makes using assertions in [NUnit](http://www.nunit.org/) easier. Normally, you'd write an assertion in NUnit like this:

```csharp
Assert.AreEqual(250.00, account.Balance);
```

By using extension methods, Spec Unit allows you to write this instead:

```csharp
account.Balance.ShouldEqual(250.00);
```

That reads a whole lot more like English, and thus makes it easier to understand what the test is doing (which is another goal of BDD). It also includes some stuff to let you run some reports on your tests that help you track your development. Definitely worth checking out if you're writing tests in NUnit.

I'm afraid a subject this in-depth may have scared off some of the people attending the meeting who are fairly new to .NET, and may have not ever seen unit testing frameworks before. I think the meeting on March 31st may be a little more accessible. [Chris Koenig](http://blogs.msdn.com/chkoenig/) will be coming to speak on Silverlight, which ought to be cool. I'm hoping that he shows us some Silverlight 2.0 code, since that's what's really interesting to me. If you're in the area, please come to the meeting! And stay tuned to the user group's [site](http://www.fsdnug.org/) for information on future events.
