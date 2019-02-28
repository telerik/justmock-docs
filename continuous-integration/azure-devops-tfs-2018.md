---
title: Azure Devops and TFS 2018
page_title: Azure Devops | JustMock Documentation
description: Integrating Telerik JustMock with Azure Devops and TFS 2018
previous_url: azure-devops, azure-devops.html
slug: justmock/integration/azure-devops
tags: azure,tfs,2018
published: True
position: 0
---

# Azure Pipelines

The [Telerik JustMock Tests v.2](https://marketplace.visualstudio.com/items?itemName=vs-publisher-443.jm-vstest-2) extension for Azure Piplines and TFS 2018 is designed to deploy your JustMock test projects to Azure Pipelines or TFS 2018 with minimal manual configurations for setting up your build environment.

It breaks you free from tedious configuration of environment variables and provides you simple options that can get you on the move in no time. In this topic we will focus on what are the required configuration steps when working with JustMock Tests v.2 extension.

## Steps for installing the JustMock Tests v.2 extension and the task

1. To install the extension go to the Azure DevOps marketplace and search for JustMock or click on this [link](https://marketplace.visualstudio.com/items?itemName=vs-publisher-443.jm-vstest-2).

2. Click on the "Get it free" button. The marketplace will require you to login if you haven't done that already.

![JustMock extension Get it free](images/MarketPlace_JustMock.png)

3. Click on the install button and follow the procedure.

![Install JustMock extension](images/Install_JustMock_Extension.png)

## Adding the Telerik JustMock VSTest v.2 to your build

Go to your build pipepile and click on the "Add task" button and find the "Telerik JustMock VSTest v.2" task located under the "Test" tab and click on the "Add" button of the task. As an alternative you could drag the task to the desired location in your build pipeline.

![Add Telerik JustMock VSTest v.2 task to the build](images/Add_JustMock_Task.png)

## Confuguring the Telerik JustMock VSTest v.2 task

The task is in essence a wrapper of the VSTest task and the configuration is the same. The only additional configuration is related to whether the build agent is at the could or at on-premisses server.

### Using build agent at the Azure DevOps cloud

For this scenario you have to configure the JustMock options in the "Telerik JustMock VSTest v.2 task". The options allow you to specify a relative path to the 32 and 64 bit JustMock Profiler. In this scenario the JustMock Profiler in his 32 and 64 bit variant should be included in your repository. The different variations are used in regards of whether the test execution process is 32 or 64 bit. The bitness of this process is different for .Net Framework and .Net Core. This is why we recommend specifying both paths.

![Configure JustMock Options](images/JustMock_Options.png)

### Using build agent at on-premises server

For this scenario you have two possible variants to choose from.

1. The first variant is to configure the JustMock options in the "Telerik JustMock VSTest v.2 task" which is explained above. 

2. The second variant is to simply install JustMock at the on-premises server without the need of additional configuration.

## See Also

* [Cruise Control .NET]({%slug justmock/integration/cruise-control-.net%})

* [Jenkins CI]({%slug justmock/integration/jenkins-ci%})

* [TeamCity]({%slug justmock/integration/teamcity%})