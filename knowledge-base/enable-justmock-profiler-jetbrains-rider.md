---
title: Enabling JustMock Profiler in JetBrains Rider
description: Learn how to activate the JustMock Profiler in JetBrains Rider including steps for .NET Framework and .NET Core projects.
type: how-to
page_title: How to Activate the JustMock Profiler for JetBrains Rider IDE
slug: enable-justmock-profiler-jetbrains-rider
tags: justmock, jetbrains rider, profiling, mocking, static functions
res_type: kb
ticketid: 1675616
---

## Environment

| Product | Version |
| --- | --- |
| Progress® Telerik® JustMock | 2023.3.1122.188 |

## Description
When working with JetBrains Rider, you might encounter difficulties in enabling the JustMock Profiler. Unlike Visual Studio, Rider does not have an extension menu for enabling/disabling the JustMock profiler. This article outlines how to activate advanced JustMock features in the JetBrains Rider IDE.

This knowledge base article also answers the following questions:
- What steps are needed to configure the JustMock profiler in Rider?
- How can I ensure JustMock is properly set up for my .NET projects in Rider?

## Solution
To enable the JustMock Profiler in JetBrains Rider, you must manually configure the Test Runner Environment Variables. This process involves adding specific environment variables to your Rider IDE settings.

### For .NET Framework Projects
1. Open the **Test Runner Environment Variables** settings in JetBrains Rider. For more information on accessing this setting, refer to the [JetBrains documentation](https://www.jetbrains.com/help/rider/Reference__Options__Tools__Unit_Testing__Test_Runner.html#environment-variables).
2. Add the following environment variables:
   - `JUSTMOCK_INSTANCE` with value `1`
   - `COR_ENABLE_PROFILING` with value `1`
   - `COR_PROFILER` with value `{B7ABE522-A68F-44F2-925B-81E7488E9EC0}`
   - `COR_PROFILER_PATH_32` with value `<JustMock Install Root>\Progress\Telerik JustMock\Libraries\CodeWeaver\32\Telerik.CodeWeaver.Profiler.dll`
   - `COR_PROFILER_PATH_64` with value `<JustMock Install Root>\Progress\Telerik JustMock\Libraries\CodeWeaver\64\Telerik.CodeWeaver.Profiler.dll`

Make sure to replace `<JustMock Install Root>` with the actual installation path of JustMock on your machine.

### For .NET Core Projects
For projects targeting .NET Core, use the corresponding `CORECLR_` prefixed variables instead of `COR_`. The variable names and their purposes remain the same, ensuring CLR correctly loads the JustMock Profiler.

### Using .runsettings File
Alternatively, integrate JustMock with Rider through a `.runsettings` file. This method involves specifying test runner settings and environment variables within a `.runsettings` file placed in your solution folder. Then, configure your Rider IDE to use this settings file during test execution.

1. Create `.runsettings` file containing the necessary JustMock environment variables.
2. Place the `.runsettings` file in your solution directory.
3. In Rider, specify the path to the `.runsettings` file within the Test Runner settings.

This approach allows for a more centralized and version-controlled setup of your testing environment.

## See Also
- [JustMock Documentation](https://docs.telerik.com/devtools/justmock/)
- [Configuring Test Runner Environment Variables in JetBrains Rider](https://www.jetbrains.com/help/rider/Reference__Options__Tools__Unit_Testing__Test_Runner.html#environment-variables)

