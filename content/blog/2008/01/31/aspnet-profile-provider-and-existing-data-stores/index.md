---
title: "ASP.NET Profile Provider and Existing Data Stores"
date: 2008-01-31T00:00:00-06:00
slug: "aspnet-profile-provider-and-existing-data-stores"
url: "/blog/2008/01/31/aspnet-profile-provider-and-existing-data-stores/"
---

I'm at the beginning of creating my first large-scale ASP.NET application. I've done plenty of WinForms stuff before, but not ASP.NET, and I didn't realize what a totally different animal it is until recently. Fortunately, [Dino Esposito's book](http://www.amazon.com/Programming-Microsoft-ASP-NET-Core-Reference/dp/0735621764/) on the subject is excellent and has helped me out a lot.

The site is essentially a rewrite of an existing classic ASP site with quite a bit of added functionality.

Right now I'm working on converting the home-grown membership system to use ASP.NET Membership, Roles, and Profiles. The tables to store all this information already exist, and thankfully, the provider model allows me to reuse them. The Membership stuff has gone just fine so far and maps really well to the way we were storing things anyway. When I got to the Profile stuff, though, I was presented with a bit of an icky situation.

To create custom profile properties, you add something like this to the section of web.config:

```xml
<properties>
  <add name="Salutation" type="String" />
  <add name="FirstName" type="String" />
  <add name="LastName" type="String" />
  …
</properties>
```

When your custom provider gets called to get the properties from your data store, you're presented with a collection keyed on the property name from web.config that contain the values of those properties set by the application. So now, when faced with the task of setting the resulting property values based on what you got from your database table, you must find a way to map those string property names to columns on your database. Like so:

```vb
sql = "select SALUTATION, FIRST_NAME, LAST_NAME from MYTABLE where …"
…
For Each item As SettingsProperty In collection
    prop = New SettingsPropertyValue(item)
    Select Case item.Name
        Case "Salutation"
            prop.PropertyValue = CStr(reader("SALUTATION")).Trim()
        Case "FirstName"
            prop.PropertyValue = CStr(reader("FIRST_NAME")).Trim()
        Case "LastName"
            prop.PropertyValue = CStr(reader("LAST_NAME")).Trim()
    …
```

See the ickyness? It really makes me uncomfortable to have so many strings flying around my application just waiting to cause a runtime error. I've been thinking about creating some kind of AddIn for Visual Studio to generate the web.config settings and database code for `GetPropertyValues()` and `SetPropertyValues()` in the Profile provider based on an existing database schema. Maybe a worthwhile foray into contributing something to the .NET community. That may be biting a bit more off than I can chew, though. Either way, I wish the process could be a bit more runtime-safe. The [WebProfile Generator](http://www.codeplex.com/WebProfile) on CodePlex would be a step in the right direction, but it only works for Web Application projects. Ah, well.
