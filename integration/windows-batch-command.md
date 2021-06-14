---
title: Windows Batch Command
page_title: Windows Batch Command | JustMock Documentation
description: In this topic, you will learn how to execute Telerik® JustMock test DLLs with cmd.exe.
slug: justmock/integration/windows-batch-command
tags: windows,batch,command
published: True
position: 15
previous_url: /scenarios/running-justmock-profiler-outside-visual-studio, /scenarios/running-justmock-profiler-outside-visual-studio.html, /integration-windows-batch-command.html, /integration-windows-batch-command
---

# Windows Batch Command

You can execute test DLLs via the __Command Prompt(cmd.exe)__. This topic demonstrates how to execute __Telerik® JustMock__ test DLLs with __cmd.exe__.

##  How to Run Profiler-Enabled JustMock Tests in Command Prompt

The procedure of executing test DLLs in __Command Prompt__ is almost identical regardless of the different unit testing frameworks that you may use.

To prepare your environment, you should set the __JustMock environment variables__. That can be done using one of the following approaches:

### With **JustMock Console**
    
The **JustMock Console** automatically prepares the environment so you can directly execute your tests and it is **the suggested approach** for running tests in console. For detailed instructions on how to use it, check the [JustMock Console]({%slug justmock/integration/justmock-console%}) topic.

### Manually

If for any reason using __JustMock Console__ does not apply to your case, it is possible to set the __JustMock environment variables__ manually before running the tests. The next list shows the required variables you should set:

* `SET JUSTMOCK_INSTANCE=1`
* `SET COR_ENABLE_PROFILING=1`
* `SET COR_PROFILER={B7ABE522-A68F-44F2-925B-81E7488E9EC0}`

Once the environment variables are set, you will be able to run elevated JustMock tests through the __Command Prompt__, simply by following the standard steps for the chosen unit testing framework. The next example shows setting up the environment and running tests using VS 2019.

![Windows Batch Command 2](images/WindowsBatchCommand2.png)

Execution command for VS 2019: 

`"C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\TestWindow\vstest.console.exe" "C:\Program Files (x86)\Progress\Telerik JustMock\Examples\CSExamples\JustMock.ElevatedExamples\bin\Debug\JustMock.ElevatedExamples.dll"`

### With **JustMock Runner**

>important**JustMock Runner** is outdated and the section is for backward compatibility only.

__JustMock Runner__ will automatically setup an appropriate environment for the product so you don't need to do any additional settings. 

__JustMockRunner.exe__ can be found in the JustMock installation directory under the Libraries folder (by default *C:\Program Files (x86)\Progress\Telerik JustMock\Libraries*). 

 Below you will find an example batch file, that will use __MSTest__ to run all the tests in JustMock.ElevatedExamples.dll (found in the *Examples\CSExamples* folder inside Telerik JustMock root folder). 

![Windows Batch Command 1](images/WindowsBatchCommand1.png)

 Execution command for VS 2019: 

`"C:\Program Files (x86)\Progress\Telerik JustMock\Libraries\JustMockRunner.exe" "C:\Program Files (x86)\Microsoft Visual Studio\2019\Preview\Common7\IDE\MSTest.exe" /testcontainer:"C:\Program Files (x86)\Progress\Telerik JustMock\Examples\CSExamples\JustMock.ElevatedExamples\bin\Debug\JustMock.ElevatedExamples.dll"`
