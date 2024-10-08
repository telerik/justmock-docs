---
title: Resolving Profiler Interceptor Initialization Exception in JustMock
description: How to fix the "The type initializer for 'Telerik.JustMock.Core.ProfilerInterceptor' threw an exception" error due to version incompatibility in JustMock.
type: troubleshooting
page_title: Fix Profiler Interceptor Initialization Exception in JustMock Due to Version Mismatch
slug: fix-profiler-interceptor-initialization-exception-justmock
tags: [profiler interceptor, version mismatch, justmock, initialization exception]
res_type: kb
ticketid: 1639876
---

## Environment

| Product | Version |
| --- | --- |
| Progress® Telerik® JustMock | 2024.1.124.247 |

## Description
While using [JustMock](https://docs.telerik.com/devtools/justmock/introduction), an initialization exception is thrown with the message: "The type initializer for 'Telerik.JustMock.Core.ProfilerInterceptor' threw an exception." This issue indicates a version mismatch between the referenced `Telerik.JustMock.dll` and the `Telerik.CodeWeaver.Profiler.dll`.

## Cause
The error occurs due to different versions of the JustMock package referenced in the project and the locally installed JustMock product. Specifically, the exception message highlights an incompatible profiler version, which prevents proper initialization.

## Solution
To resolve the issue, consider the following approaches:

### Align the Versions
Ensure that the version of the JustMock package referenced in your project matches the version of the locally installed JustMock product. This alignment will eliminate the version incompatibility and allow the profiler to initialize correctly.

### Use runsettings to Prepare the Profiler Environment
If aligning the versions is not feasible or you prefer a different approach, you can disable the JustMock Visual Studio Extension and manually configure the profiler environment using a `.runsettings` file. This method requires specifying JustMock settings within the `.runsettings` file to correctly prepare the environment for JustMock's profiler.

Follow these steps to configure the `.runsettings` file:

1. Create a `.runsettings` file in your solution.
2. Include the necessary JustMock configuration settings in the `.runsettings` file. Refer to the official Microsoft documentation on [configuring unit tests using a `.runsettings` file](https://learn.microsoft.com/en-us/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file?view=vs-2022) for the exact syntax and options.
3. Specify the path to the `.runsettings` file in your test runner configuration.

For detailed instructions on using the NuGet package in `.runsettings`, consult the [JustMock documentation](https://docs.telerik.com/devtools/justmock/integration/general#using-the-nuget-package-in-runsettings).

## See Also

- [JustMock Introduction](https://docs.telerik.com/devtools/justmock/introduction)
- [Configure Unit Tests by Using a `.runsettings` File](https://learn.microsoft.com/en-us/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file?view=vs-2022)
- [Using the NuGet Package in `.runsettings`](https://docs.telerik.com/devtools/justmock/integration/general#using-the-nuget-package-in-runsettings)
