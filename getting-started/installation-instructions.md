---
title: Step 1 - Installation and Setup
page_title: Installation and Setup | JustMock Documentation
description: Installation Instructions and Setup for Telerik JustMock
previous_url: /getting-started-installation-instructions.html
slug: justmock/getting-started/installation-instructions
tags: installation,instructions
published: True
position: 0
---

# Installation and Setup

This topic outlines the steps required to install __Telerik® JustMock__.

## System Requirements
JustMock supports the following operating systems:

* Windows 10
* Windows 8.1
* Windows 8
* Windows 7
* Windows Vista
* Windows Server 2008
* Windows Server 2003
* Windows XP

## Development Environment

In order to use JustMock the following additional development tools must be installed beforehand:

* __.NET Framework 4.0, .NET 4.5, .NET 4.5.1, .NET 4.5.2 or .NET 4.6__
* __Microsoft Visual Studio 2010/2012/2013/2015/2017__ - download from [here](https://www.microsoft.com/visualstudio/eng/downloads).

## Installing JustMock

1. Download the JustMock installer from www.telerik.com:
	* If this is your first time here and you want to try JustMock, download the trial installer file from [here](https://www.telerik.com/download-trial-file/v2-b/justmock-b).
	* If you are a licensed JustMock user, log in your Telerik account and navigate to the [Downloads section](https://www.telerik.com/account/my-downloads).

1. Run the installer and follow the steps. Configure the default installation folder.
![Installer](images/Installer.png)
1. You are all set.

>If you encounter issues during the installation process, submit a support ticket in our [support ticketing system](https://www.telerik.com/account/support-tickets) with as much details as possible and we will assist you. 


## Using JustMock

To start using JustMock in your test projects, add a reference to the __Telerik.JustMock.dll__ (or Telerik.JustMock.Silverlight.dll if you use it in a Silverlight project). The assembly is located in the installation directory under the *Libraries* folder (by default C:\Program Files (x86)\Progress\Telerik JustMock\Libraries). Alternatively, use the [Visual Studio Extension]({%slug justmock/getting-started/visual-studio-extension%}).


## Integrating JustMock in TFS Code Activity Workflow

Please follow these articles for detailed steps, describing the JustMock integration with [TFS 2010]({%slug justmock/integration/tfs-2010%}), [TFS 2012]({%slug justmock/integration/tfs-2012%}) or [TFS 2013]({%slug justmock/integration/tfs-2013%}).

In case your test runner is being executed under one of these accounts:

* NT AUTHORITY\SYSTEM
* NT AUTHORITY\LOCAL SERVICE
* NT AUTHORITY\NETWORK SERVICE

To mock non-virtual/static methods on TFS you might have to perform manual configuration by setting up the following system environment variables:

* `COR_ENABLE_PROFILING=1`
* `COR_PROFILER={B7ABE522-A68F-44F2-925B-81E7488E9EC0}`
* `JUSTMOCK_INSTANCE=1`

This will enable JustMock profiler for all managed processes on your build/test server.

> Enabling JustMock profiler on global scale could slow down the execution of all managed processes.
>
> __We strongly recommend configuring your test runner to execute under a dedicated account and setup the environment variables for that account only. With such configuration other managed applications won’t be affected by JustMock profiler.__

## Resources and Documentation

After installing JustMock, you can find a Getting Started Guide and example projects in the installation directory (by default C:\Program Files (x86)\Progress\Telerik JustMock). 

The example projects provide a hands-on approach, unit testing JustMock itself. The documentation is also available in Help3 format shipped separately in a ZIP file which you can download from your [Telerik account](https://www.telerik.com/account/my-downloads).

If you need additional assistance, take a look at our [online JustMock forums](https://www.telerik.com/forums/justmock) or [contact support](https://www.telerik.com/account/support-tickets).

## See Also

 * __[Step 2: Using Telerik JustMock in Your Test Project]({%slug justmock/getting-started/using-telerik-justmock-in-your-test-project%})__

 * [Visual Studio Extension]({%slug justmock/getting-started/visual-studio-extension%})

 * [Commercial vs Free Version]({%slug justmock/getting-started/commercial-vs-free-version%})
