---
title: Cruise Control .NET
page_title: Cruise Control .NET | JustMock Documentation
description: Integrate Telerik JustMock and Cruise Control .NET
slug: justmock/integration/cruise-control-.net
tags: cruise,control,.net
hidden: True
position: 2
previous_url: integration-cruisecontrol.net.html, integration-cruisecontrol.net
---

# Cruise Control .NET

__CruiseControl.NET__ is a free and open source software tool for automating software build processes. It targets the .NET environment. __CruiseControl.NET__ (also known as "ccnet") uses pure XML in its config files where you can assemble tasks in order to build your project.

This article explains how the __JustMock Runner__ is integrated inside the ccnet configuration file, so that it can execute the tests inside your solution.

You can learn more about __CruiseControl.NET__ at their [official website](https://www.cruisecontrolnet.org/) or [sourceforge](https://ccnet.sourceforge.net/CCNET/Documentation.html).

## CruiseControl.NET and JustMock Runner

We will be using the Telerik.JustMock.CSExamples.VS2010.sln (found in the JustMock root folder under Examples) to show the process of integrating JustMock with __Cruise Control__.

To build the solution with ccnet we first have to add the appropriate entries inside the ccnet.config file, which can be found in the CruiseControl.Net root folder (ex: C:\Program Files (x86)\CruiseControl.NET\server).

Start by adding a new project, like the example below:

```xml
    <cruisecontrol xmlns:cb="urn:ccnet.config.builder">
          <!-- This is your CruiseControl.NET Server Configuration file. Add your projects below! -->

          <project>
          <name>MyProjectWithJustMock</name>
          <workingDirectory>C:\Program Files (x86)\Telerik\JustMock\ExamplesNew</workingDirectory>
          <webURL>http://localhost/ccnet/server/local/project/MyProjectWithJustMock/ViewProjectReport.aspx</webURL>
          <modificationDelaySeconds>2</modificationDelaySeconds>

          <state type="state" directory="C:\Program Files (x86)\CruiseControl.NET\server\State" />
```

To check if the tests in your project are passing or failing you will need to add a publisher like this:

```xml
    <publishers>
          <merge>
          <files>
          <file>C:\Program Files (x86)\CruiseControl.NET\server\Log\ccnet.xml</file>
          </files>
          </merge>
          <xmllogger />
          </publishers>
```

After this we will add the tasks:

```xml
    <tasks>

          <devenv>
          <solutionfile>Telerik.JustMock.CSExamples.VS2010.sln</solutionfile>
          <configuration>Debug</configuration>
          <executable>C:\Program Files (x86)\Microsoft Visual Studio 10.0\Common7\IDE\devenv.com</executable>
          <buildTimeoutSeconds>60</buildTimeoutSeconds>
          </devenv>

          <exec>
          <executable>C:\Program Files (x86)\Telerik\JustMock\Libraries\JustMockRunner.exe</executable>
          <baseDirectory>C:\Program Files (x86)\Telerik\JustMock\Examples\Telerik.JustMock.Tests\bin\Debug</baseDirectory>
          <buildArgs>"C:\Program Files (x86)\Microsoft Visual Studio 10.0\Common7\IDE\MSTest.exe" /testcontainer:Telerik.JustMock.Tests.dll</buildArgs>
          <buildTimeoutSeconds>120</buildTimeoutSeconds>
          </exec>

          </tasks>
```

The first task is setting the path to Visual Studio (`devenv.com`) and our solution file. The second is the task that actually executes the __JustMock Runner__.

Finally you will be able test if everything is working as expected.

1. Open any browser and go to the following URL: http://localhost/ccnet/server/local/project/MyProjectWithJustMock/ViewProjectReport.aspx

	![Cruise Control.NET 1](images/CruiseControl.NET1.png)

2. Build your project and check if the tests in it have passed or failed.
	
	![Cruise Control.NET 2](images/CruiseControl.NET2.png)

## See Also

 * [Jenkins CI]({%slug justmock/integration/jenkins-ci%})

 * [TeamCity]({%slug justmock/integration/teamcity%})
