---
title: "PowerShell Scripts for Mercurial + IIS Development Workflow"
date: 2010-05-01T00:00:00-06:00
slug: "powershell-iis-mercurial"
url: "/blog/2010/05/powershell-iis-mercurial/"
---

I'm currently using Mercurial for source control at work, and I absolutely love it. I love the cheap branching, fast operations, and merging that actually works. One of the side effects of using a branch-per-feature workflow in Mercurial is that you're constantly creating new copies of your project structure in the file system. Unlike Git, where the guidance is to create branches within one working copy of the repository and switch between them, the Mercurial community recommends creating full clones instead.

Even when doing development work, I like to use IIS for serving my web applications rather than the Visual Studio web server (Cassini), so my development environment is as close to production as possible. I've gotten bitten a couple of times when transitioning from Cassini during development to IIS in production, so I decided to just use IIS from the start.

Using these technologies in combination, I started to run into a problem. Every time I created a clone of my web app's repository, I had to set up the directory as an IIS application, plus add the permissions required for IIS to read static files (and in my case, write to a temp images directory, since I'm using the Microsoft charting tool). To make this process easier, I whipped up a couple of PowerShell scripts to take care of all those tasks in one fell swoop.

```powershell
# New-App.ps1
# usage: New-App "VirtualDirectoryName"
param([string]$appName = "appName")
$path = $pwd.Path
$fullAppName = 'IIS:SitesDefault Web Site' + $appName
pushd
cd iis:
ni $fullAppName -physicalPath $path -type Application
cd c:
popd
$acl = Get-Acl $pwd.Path
$inherit = [system.security.accesscontrol.InheritanceFlags]"ContainerInherit, ObjectInherit"
$propagation = [system.security.accesscontrol.PropagationFlags]"None"
$arIUSR = New-Object system.security.AccessControl.FileSystemAccessRule("IUSR", "FullControl", $inherit, $propagation, "Allow")
$arIISIUSRS = New-Object system.security.AccessControl.FileSystemAccessRule("IIS_IUSRS", "FullControl", $inherit, $propagation, "Allow")
$acl.SetAccessRule($arIUSR)
$acl.SetAccessRule($arIISIUSRS)
Set-Acl $pwd.Path $acl
```

A couple of things about this one. A few things are hard-coded, like the site name (this will probably be "Default Web Site" on your machine, too unless you're running a server OS) and the "FullControl" access, which can be changed to whatever minimum level of access you need the IIS accounts to have, like "Read" or "ReadAndExecute".

I wish there was an easier way to set the permissions on the directory, but the System.Security .NET API was the only way that I found. I've always felt that calling .NET code from PowerShell was a little bit kludgey, but I'm glad it's at least possible to fill in the gaps in functionality.

In order to not leave an orphaned IIS virtual directory when I'm done with a branch, I use this script, which will search for the app by the physical path. This one could be fleshed out a little more. It assumes that the virtual directory exists at the root of the default web site, and that you're executing the script from the directory itself.

```powershell
#Remove-App.ps1
$path = $pwd.Path
pushd
cd iis:
cd 'IIS:sitesDefault Web Site'
$site = ls | Where-Object {$_.PhysicalPath -eq $path}
ri $site.Name
cd c:
popd
```

One more note: you'll need to import the "WebAdministration" PowerShell module to get this to work. If you're on Windows 7 and you've got PowerShell docked on your task bar, you can just right click and choose "Import System Modules", the web admin module (along with a few others) will be imported into your PS session. Otherwise, you can execute "Import-Module WebAdministration" at the PowerShell prompt or in your profile script.

Hope this helps somebody!
