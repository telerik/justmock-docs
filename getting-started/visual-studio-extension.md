---
title: Visual Studio Extension
page_title: Visual Studio Extension | JustMock Documentation
description: Telerik JustMock Visual Studio Extension and the options it provides
previous_url: /getting-started-visual-studio-extension.html
slug: justmock/getting-started/visual-studio-extension
tags: visual,studio,extension
published: True
position: 4
---

# Visual Studio Extension

When you install __Telerik® JustMock__, you also get a JustMock Visual Studio extension installed by default. It deploys a _JustMock_ menu inside Visual Studio.

#### Figure 1: JustMock Menu in Visual Studio
![JustMock Visual Studio Extension](images/VSExtension.png)

This article will walk you through the different settings the Visual Studio extension provides. 

## Enable/Disable Profiler

Enables or disables the JustMock profiler. The profiler is only needed when when you want to use the [advanced features]({%slug justmock/advanced-usage%}) of JustMock. 

You can enable the profiler using the shortcuts `Ctrl+Shift+[` and `Ctrl+Shift+]`.

## Options...

Opens the Telerik JustMock Configuration window, which is used to link JustMock with 3rd party profilers. For more information, navigate to [this]({%slug justmock/integration/codecoverage-tools%}) article.

## Documentation

Opens the online Telerik JustMock documentation. With your JustMock installation you also have access to the documentation in CHM format, the file is located in the installation folder (by default C:\Program Files (x86)\Progress\Telerik JustMock\Documentation).

## Suggest a Feature

Opens the [JustMock Ideas and Feedback Portal](https://feedback.telerik.com/Project/105) where you can submit ideas and feature requests or vote for features that are already in the backlog.

## Check for Updates

Opens the Telerik JustMock Updater window.  Gives you the options to *Include internal builds when checking for updates* and *Check for JustMock updates when Visual Studio starts*.

#### Figure 2: JustMock Updater
![Updater Window](images/UpdaterWindow.png)

## Customer Experience and Improvement Program 

Opens the Telerik JustMock customer experience and improvement program window, where you can enable or disable the anonymous reporting of the product usage.

#### Figure 3: JustMock Customer Experience and Improvement Program 
![Analytics Window](images/AnalyticsWindow.png)


## Update References 

Opens the Telerik JustMock Update References window. Provides the ability to update all JustMock references (*Telerik.JustMock.dll* and *Telerik.JustMock.Container.dll*) in the solution. Further, you can choose to not show this window again for that particular solution.

This window prompts automatically when a solution which contains JustMock references different from the currently installed JustMock version is loaded.

#### Figure 4: Update JustMock References window ####

![Update Reference Window](images/UpdateReferenceWindow.png)
 
The functionality to update references can also be used by right-clicking on the references field for a certain project. This way, you will be able to update only the JustMock references for that particular project: 

#### Figure 5: Update JustMock References from the context menu

![Update References From The Context Menu](images/UpdateReferencesFromTheContextMenu.png)

## About JustMock 

Opens the Telerik JustMock about window.

## Troubleshooting

*Problem:* **Missing Telerik menu in Visual Studio**

*Reason:* Telerik Visual Studio Extensions are disabled.

*Suggested solution:*

* Open Visual Studio;

* Go to menu Tools - > Extensions and Updates...(for Visual Studio 2019 Extensions - > Manage Extensions)

* Open the Installed tab on the left​

* Search for Telerik JustMock extension and make sure it is Enabled

![vsextensions-disabled](images/vsextensions-disabled.png)

>important If the article does not help solving your problem, please follow these steps to generate Visual Studio [ActivityLog](https://docs.microsoft.com/en-us/visualstudio/ide/reference/log-devenv-exe?view=vs-2019) file before contacting our support:
>* Open [Developer Command prompt](https://docs.microsoft.com/en-us/dotnet/framework/tools/developer-command-prompt-for-vs) for Visual Studio 20xx under **Administrative rights**.
>* Execute the command - devenv /log %userprofile%\desktop\ActivityLog.xml . This will start Visual Studio and create logs on your Desktop.
>* Reproduce the problem
>* Attach the **Activitylog** files when you contact our support.

# See Also

 * [Installation and Setup]({%slug justmock/getting-started/installation-instructions%})

 * [Adding Telerik JustMock to Your Test Project]({%slug justmock/getting-started/using-telerik-justmock-in-your-test-project%})

