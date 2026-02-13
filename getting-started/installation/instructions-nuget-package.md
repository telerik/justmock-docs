---
title: Using NuGet package
page_title: Installation using NuGet package | JustMock Documentation
description: Learn how you can install the Telerik JustMock framework using NuGet package.
previous_url: /getting-started-installation-instructions-nuget-package.html
slug: justmock/getting-started/installation/instructions-nuget-package
tags: installation,instructions
published: True
position: 4
---

# Installation using NuGet package

This topic outlines the steps required to install [Telerik JustMock](https://www.telerik.com/products/mocking.aspx) and [Telerik JustMock Lite](https://www.telerik.com/justmock/free-mocking) using the NuGet package inside Visual Studio or on the command line.

## Generate an API Key

As the Telerik NuGet server requires authentication, the first step is to obtain an API key that you will use instead of a password. Using an API key instead of a password is a more secure approach, especially when working with `.NET CLI` or the `NuGet.Config` file.

1. Go to the [API Keys](https://www.telerik.com/account/downloads/api-keys) page in your Telerik account.
2. Click **Generate New Key +**.

   ![Manage API Keys](images/account-generate-api-key.png)

3. In the **Key Note** field, add a note that describes the API key.
4. Click **Generate Key**.
5. Select **Copy and Close**. Once you close the window, you can no longer copy the generated key. For security reasons, the **API Keys** page displays only a portion of the key.
6. Store the generated NuGet API key as you will need it in the next steps. Whenever you need to authenticate your system with the Telerik NuGet server, use `api-key` as the username and your generated API key as the password.

> API keys expire after two years. Telerik will send you an email when a key is about to expire, but we recommend that you set your own calendar reminder with information about where you used that key: file paths, project links, AzDO and GitHub Action variable names, and so on.

## Installing JustMock

#### Visual Studio

To install the JustMock NuGet package, first add the Telerik NuGet feed to the package sources using NuGet Package Manager:

1. Open Visual Studio and go to **Tools** > **NuGet Package Manager** > **Package Manager Settings**.

1. Select **Package Sources**, and then select the **+** button.

1. In the **Name** field, enter `Telerik.com`.

1. In the **Source** field, enter `https://nuget.telerik.com/v3/index.json`, and then select **OK**.

    > The Telerik NuGet feed is available for authorized access at https://nuget.telerik.com/v3/index.json. Note that the previous v2 server, accessible at https://nuget.telerik.com/nuget, is deprecated and is no longer in use.

    ![Add NuGet Source](images/NugetPackageManagerSources.png)

Once you have configured Visual Studio to access the Telerik NuGet server, add the JustMock NuGet package to the project:

1. In Visual Studio, open the solution in which you will use mocking.

1. Go to **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution...**.

1. From the **Package source** drop-down, select `Telerik.com`.

1. On the **Browse** tab, search for `JustMock`.

1. Select the `JustMock.Commercial` package, select the desired project, and then select **Install**.

    ![Install JustMock NuGet Package](images/ManagePackagesForSolution.png)

#### Command Line

- On the command line you can use the following commands:

```bat
dotnet nuget add source "https://nuget.telerik.com/v3/index.json" --name "Telerik.com" --username "api-key" --password <TELERIK_NUGET_API_KEY>
dotnet add package JustMock.Commercial --version 2024.4.1203.350
```

> To learn more about using the Telerik NuGet feed with NuGet API keys, check this [article](https://docs.telerik.com/kendo-ui/intro/installation/nuget-keys).

## Installing JustMock Lite

#### Visual Studio

- JustMock Lite NuGet package is available on [nuget.org](https://www.nuget.org/). You can follow similar steps from the second part of the procedure above to add the JustMock Lite package to your test project in Visual Studio. Simply select the `nuget.org` source and browse for the JustMock Lite package:

    ![Install JustMock Lite NuGet Package](images/ManagePackagesForSolutionLite.png)

#### Command Line

- On the command line run the following command:

    ```bat
    dotnet add package JustMock --version 2024.4.1203.350
    ```

## Resources and Documentation

- **Offline Documentation**

    The documentation is also available in PDF format which you can download from your [Telerik account](https://www.telerik.com/account/my-downloads).

- **Additional Assistance**

    If you need additional assistance, take a look at our [online JustMock forums](https://www.telerik.com/forums/justmock) or [contact support](https://www.telerik.com/account/support-tickets?pid=743).

- **Suggestions and Reports**

    If you want to suggest a new feature or vote for a popular one, please visit [JustMock Feedback Portal](https://feedback.telerik.com/justmock).

## Next Steps

* [Add Telerik JustMock to Your Test Project]({%slug justmock/getting-started/configuration/using-telerik-justmock-in-your-test-project%})
* [JustMock API Basics]({%slug justmock/getting-started/basics/basics%})
