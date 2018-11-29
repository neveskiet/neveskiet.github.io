---
layout: post
title:  "Load solutions in Dynamics 365 Customer Engagement with Powershell"
---

Let's say your in a Dynamics 365 Customer Engagement (CRM Online) project and you're responsible for deploying customizations. Let's say you're also having a proper DTAP (**D**evelopment, **T**est, **A**cceptance and **P**roduction) environment setup and when new solutions are delivered you're supposed to deploy them accross the application landscape. Let's say you're having 5 custom solutions and you need to at least deploy them in test, acceptance and production. That's 15 imports total of clickery. Yey!

Enough chat-chat allready, lets automate this process.

## 1. Importing the "Microsoft.Xrm.Data.PowerShell" Powershell module

First we need to import the proper Powershell module:

```
if (!(Get-Module "Microsoft.Xrm.Data.PowerShell")) {
    Install-Module Microsoft.Xrm.Data.PowerShell -Scope CurrentUser
}
```
The `-Scope CurrentUser` can be added if you're not running Powershell in elevated mode/as administrator.

If you're experiencing any trouble importing the module (or anything else), I suggest you'd take a visit to the module's author github page: https://github.com/seanmcne/Microsoft.Xrm.Data.PowerShell

## 2. Connect to CRM organization

Connect to the CRM organization using the following command:
```
$connection = Connect-CrmOnlineDiscovery -InteractiveMode
```

We're using the interactive logon mode here. Automated logons are also possible: https://github.com/seanmcne/Microsoft.Xrm.Data.PowerShell

## 3. Import the modules

Ok let's Import the modules. Sometimes it's needed that modules need to be loaded in a specific order. Now you can either do this:
- on file system level: rename the files in alphabetical order
- in the Powershell script: Sort the array that holds the modules

Personally i'd opt-in for the first option as I would like as minimal future changes to my script as possible.

```
$pathToModules = "c:\temp\path\to\modules\"
$modules = Get-ChildItem -path $pathToModules | Sort-Object Name #Sort the modules in the array. Just to be sure
```

