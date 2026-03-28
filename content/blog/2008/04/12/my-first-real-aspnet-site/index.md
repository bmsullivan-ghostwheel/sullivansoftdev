---
title: "My First Real ASP.NET Site"
date: 2008-04-12T00:00:00-06:00
slug: "my-first-real-aspnet-site"
url: "/blog/2008/04/12/my-first-real-aspnet-site/"
---

I'm fortunate enough to be one of the first developers at my current employer to create a site using ASP.NET. Currently, almost all our internet and intranet applications are built with classic ASP. Yeah, I know. Early adopters we ain't. As cool as it is that I'm getting to blaze this trail, it also means that there's little guidance to be had, and no internal examples to look at. Even the guys I usually go to for advice at work (Kerry Jenkins and David Mohundro) haven't used ASP.NET that much.

Right now, I'm still struggling to come up with an architecture that I think will be clean, but will also be understandable by people that come along behind me. I know this problem has been solved a bazillion times before, but I've found it a bit hard to find some examples that are close enough to real-world complexity. I took a look at Code Camp Server, but it uses NHibernate, which I don't think is an option for us. Using open-source software to build applications is viewed with more than a little apprehension by management. Their reasoning is that if something goes wrong with the software, no one has a responsibility to offer us support. I think that hinders us in a lot of ways, but that's a topic for another post. 😉

So, lacking any good examples, I came up with something myself. To illustrate, I'll use an example scenario: creating a new user account. I'm going to simplify it and just talk about the architecturally relevant parts.

The solution has four main projects:

- Web (Controls and aspx's)
- BusinessLogicLayer (so there's as little code in the aspx.vb's as possible)
- DataAccessLayer (DB2 Database stuff)
- ObjectModel (DTOs and stuff)

One thing that gave me a bit of trouble was how to get error messages back to the UI. I toyed with the idea of having something named Context that would contain both the object the UI needed, plus messages about any errors that happened while performing the request. I eventually rejected that in favor of having my pages implement an interface like this:

```csharp
public interface IView
{
    string ErrorMessage { set; }
}
```

So my business logic services can take an IView in their constructors, then set the ErrorMessage property when it encounters an error. So I end up with something like this:

```csharp
public partial class _Default : System.Web.UI.Page, IView
{
    WebUserService _userSvc;

    protected void Page_Load(object sender, EventArgs e)
    {
        _userSvc = new WebUserService(this);
    }

    protected void submitButton_Click(object sender, EventArgs e)
    {
        WebUser user = _userSvc.CreateNewWebUser(userNameTextBox.Text, passwordTextBox.Text);

        if (user != null)
        {
            FormsAuthentication.SetAuthCookie(user.UserName, false);
            Response.Redirect("Home.aspx");
        }
    }

    #region IView Members

    public string ErrorMessage
    {
        set
        {
            serverValidator.ErrorMessage = value;
            serverValidator.IsValid = false;
        }
    }

    #endregion
}

public class WebUserService
{
    private IView _view;

    public WebUserService(IView view)
    {
        _view = view;
    }

    public WebUser CreateNewWebUser(string userName, string password)
    {
        WebUserRepository rep = new WebUserRepository();
        WebUser user = null;

        if (rep.GetWebUserByUserName(userName) == null)
        {
            user = rep.CreateNewWebUser(userName, password);
        }
        else
        {
            _view.ErrorMessage = "That username is not available. Please choose a different one.";
        }

        return user;
    }
}

public class WebUser
{
    public string UserName { get; set; }
}
```

Another thing I had a bit of trouble with was deciding where to check for things like duplicates in the database. I thought that if I did most of that in the data access layer, I'd have the same problem communicating error messages back to the business logic layer that I did to the UI. So, I decided to try to make the data access layer methods as granular as possible so I could do all that kind of validation in the business logic layer. It's more chatty than I'd like, but I think it will work for me, since we really don't have any logic in the database. (We don't use stored procs or anything, because higher-ups don't want any logic in the DB.)

I think this setup ought to work for most cases, but I don't really have enough experience to know if I'm on the right track here. If someone with more ASP.NET experience than me has any suggestions, I'm wide open. I'll post more of my experiences as I go along.
