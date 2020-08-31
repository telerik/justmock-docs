---
title: Add Telerik JustMock to Your Test Project
page_title: Add Telerik JustMock in Your Test Project| JustMock Documentation
description: Create a JustMock Test Project or add JustMock to your project and start using it.
previous_url: /getting-started-using-telerik-justmock-in-your-test-project.html
slug: justmock/getting-started/using-telerik-justmock-in-your-test-project
tags: using,telerik,justmock,in,your,test,project
published: True
position: 2
---

# Add Telerik JustMock to Your Test Project

This topic demonstrates how to use __Telerik® JustMock__ in a new test project or integrate it in your existing project.

>Before proceeding further, make sure you have successfully completed [Installation and Setup]({%slug justmock/getting-started/installation-instructions%}).

    	
## Create New Project with JustMock Test Project Template

1. Start by adding a new test project to your Solution. Right click on the Solution, select Add, then New Project.... 

	**Figure 1: Add new project**  
	![Add New Project to VS Solution](images/AddNewProject.png)

1. Select the Test Project template. Enter a Name and click OK.

	**Figure 2: Choose the JustMock Test Project template**  
	![JustMock Test Project Template](images/ProjectTemplate.png)

1. Using the default *JustMock Test Project* template, you can start writing JustMock tests right away. 
	
	>tip See an example how to use JustMock in [JustMock API Basics]({%slug justmock/basic-usage/mock%})

## Add JustMock to a Test Project

If you want to apply JustMock to an existing unit test project, you need to add a reference to Telerik.JustMock.Dll. 

1. Right click on your test project and select Add Reference.

	**Figure 3: Add reference to a project**  
	![JustMock Test Project Template](images/AddReference.png)

1. Go to the Browse tab and navigate to the Libraries folder in the JustMock installation directory (by default C:\Program Files (x86)\Progress\Telerik JustMock\Libraries). Select __Telerik.JustMock.dll__ and click OK.

	**Figure 4: Select Telerik.JustMock.dll reference**  
	![JustMock Test Project Template](images/SelectReference.png)

1. Further, you will need to include the *Telerik.JustMock* namespace into your test project.

	**Figure 5: Include TelerikJustMockNamespace namespace**  
	![JustMock Test Project Template](images/Namespace.png)

1. Finally, you are ready to create your first test with JustMock.

	**Figure 6: Write your first test**  
	![JustMock Test Project Template](images/FirstTest.png)
	
## Next Steps

* [JustMock API Basics]({%slug justmock/basic-usage/mock%})
* [Test Your Application with JustMock]({%slug justmock/getting-started/quick-start%})

## See Also

 * [JustMock API Basics]({%slug justmock/basic-usage/mock%})

 * [Commercial vs Free Version]({%slug justmock/getting-started/commercial-vs-free-version%})
