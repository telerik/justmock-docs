---
title: NuGet Troubleshooting with JustMock
description: Learn how to address various issues that may occur when you work with JustMock and the NuGet installation approach.
type: troubleshooting
page_title: Troubleshooting Telerik NuGet Feed Issues with JustMock
slug: justmock-nuget-troubleshooting
tags: justmock, nuget, troubleshooting
res_type: kb
---

## Telerik NuGet Feed Troubleshooting

This article summarizes the issues that may occur when you work with JustMock and the online [Telerik NuGet feed]({%slug justmock/getting-started/installation-instructions#installing-justmock-from-nuget-package%}), and their solutions.

Regardless of the cause for the issue, it is recommended that you start from the section on the commonly occurring issues.

* [Tips for handling common NuGet issues](#tips-for-handling-common-nuget-issues)
* [Removing stored credentials](#removing-saved-credentials)
* [Error `401 Unauthorized`](#error-401-unauthorized)
* [Error `503 Service Unavailable`](#error-503-service-unavailable)
* [Error `Unable to resolve ... . PackageSourceMapping is enabled`](#unable-to-resolve-package-due-to-packagesourcemapping)
* [Error `Failed to retrieve information about ... from remote source`](#failed-to-retrieve-information-from-remote-source)

## Telerik NuGet Feed Status

Visit [status.telerik.com](https://status.telerik.com) to check the status of the Telerik NuGet server. The top section shows manually logged incidents with possible updates or workaround suggestions. The [**System Metrics** section](https://status.telerik.com/#system-metrics) provides real-time automated diagnostics.

## Tips for Handling Common NuGet Issues

The most common reasons for issues with the private Telerik NuGet feed are related to:

* Authentication and credentials.
* Licensing. For example, requesting commercial packages with a trial license or vice-versa.
* Missing or wrong local configuration (`NuGet.Config`).
* Network connectivity issues, including **proxies** and **firewalls**.

Errors like `Unable to load the service index for source https://nuget.telerik.com/v3/index.json` don't indicate the exact cause of the problem. In such cases, check the additional error information which usually provides an error code.

To verify if you can access the Telerik NuGet server and the expected packages, open the https://nuget.telerik.com/v3/search?q=justmock&prerelease=true&skip=0&take=100&semVerLevel=2.0.0 URL directly in the web browser and enter your Telerik credentials in the prompt.

As a result, you will see a JSON output with the NuGet packages and versions that are available for you. You can search for `JustMock.Commercial` or `Telerik.JustMock.Console`.

If the above URL doesn't open, you have either come across a local networking issue or [the NuGet server is down](#error-503-service-unavailable).

If you can access the feed in the browser, but you do not see the packages in Visual Studio, most likely the problem is caused by entering wrong credentials or using a different Telerik account. Make sure your saved credentials are correct. Also, check to see if you have a `NuGet.Config` file in the project. That will add its package sources and package source credentials. If there is one, make sure it is using the correct values.

## Removing Saved Credentials

If you suspect that your saved credentials are wrong, use the following steps to remove them from Windows and, then, add the correct ones:

1. Remove the saved credentials in the [Windows Credential Manager](https://support.microsoft.com/en-us/windows/accessing-credential-manager-1b5c916a-6a16-889f-8581-fc16e8165ac0). These credentials will appear as `nuget.telerik.com` or `VSCredentials_nuget.telerik.com` entries.
2. Remove the Telerik NuGet package source from Visual Studio.
3. If you have added the Telerik  package source by NuGet CLI, try to remove it from the CLI by running the following commands:
    * [`dotnet nuget list source`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-nuget-list-source)
    * [`dotnet nuget remove source`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-nuget-remove-source)
4. Check if you have any credentials stored in `%AppData%\NuGet\Nuget.Config`. If so, remove them.
5. Try to reset the Visual Studio user data by [forcing NuGet to ask for authentication](https://stackoverflow.com/questions/43550797/how-to-force-nuget-to-ask-for-authentication-when-connecting-to-a-private-feed).
6. Restart Visual Studio.
7. Enter the Telerik NuGet package source again through Visual Studio or CLI. If you are using the feed in a .NET Core application, store your credentials as plain text.
    * Option 1 - When using CLI to add the package source, you need to use the `--store-password-in-clear-text` flag
    * Option 2 - When using a nuget.config file, the key should be `ClearTextPassword`:
    ```XML
    <packageSourceCredentials>
      <Telerik>
        <add key="Username" value="YOUR_TELERIK_USERNAME" />
        <add key="ClearTextPassword" value="YOUR_TELERIK_PASSWORD" />
      </Telerik>
    </packageSourceCredentials>
    ```

## Error 401 Unauthorized

If your credentials are correct and your license includes the requested product and version, then the password probably contains special characters. You need to escape these characters or the authentication can fail on the NuGet server. For example, a common character you must escape is the ampersand (`&`); however, the character causing the issue may be as unique as the section character (`§`).

To solve the issue:

1. Change the password so that it doesn't include characters you need to escape.
2. Escape the special characters before storing the credentials. For example, `my§uper&P@§§word` encodes to `my&sect;uper&amp;P@&sect;&sect;word`.

Avoid using an online encoder utility for a password. Instead, use a Powershell command:

```
Add-Type -AssemblyName System.Web
[System.Web.HttpUtility]::HtmlEncode('my§uper&P@§§word')
```

## Error 503 Service Unavailable

If you get a message like `Unable to load the service index for source` and error `503 (Service unavailable)`, check the Telerik NuGet server health at the [Telerik live services status page](https://status.telerik.com/).

In urgent cases, download the NuGet packages from your [Telerik Account **Downloads** page](https://www.telerik.com/account/downloads/). Then, [set up a local NuGet feed](https://learn.microsoft.com/en-us/nuget/hosting-packages/local-feeds).

## Unable to Resolve Package due to PackageSourceMapping

Incorrect package source mapping can result in errors similar to:

`NU1100 Unable to resolve 'Telerik... (>= ...)' for 'net...'. PackageSourceMapping is enabled, the following source(s) were not considered: ...`

One solution is to check the **Package Source Mapping** settings in Visual Studio, or [review all applicable `NuGet.Config` files](https://learn.microsoft.com/en-us/nuget/consume-packages/configuring-nuget-behavior#config-file-locations-and-uses) on the machine. Then, adjust the package source mapping configuration.  You can refine your mapping to account for JustMock and other Telerik products like [the following example](https://github.com/LanceMcCarthy/DevOpsExamples/blob/95d980fbea68a64c602f4ed0f5cfbd7d62a7fddd/src/NuGet.Config#L31-L48).

## Failed to Retrieve Information from Remote Source

An attempt to use the [obsolete Telerik NuGet v2 feed](https://www.telerik.com/blogs/sunsetting-nuget-v2-server) after November 2024 will result in an error:

`Failed to retrieve information about 'JustMock.Commercial' from remote source 'https://nuget.telerik.com/nuget/FindPackagesById()?id='JustMock.Commercial'&semVerLevel=2.0.0'.`

The solution is to [use the Telerik NuGet v3 feed]({%slug justmock/getting-started/installation-instructions#installing-justmock-from-nuget-package%}).

Another possible reason for the same error is an incorrect NuGet feed URL.
