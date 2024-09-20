---
title: Configuring JustMock Profiler with Relative Paths in .NET Projects
description: Learn how to set up Telerik JustMock profiler using relative paths in runsettings for .NET projects to ensure compatibility across different environments.
type: how-to
page_title: How to Use JustMock Profiler with Relative Paths for .NET Testing
slug: justmock-profiler-relative-paths-dotnet
tags: justmock, .net, dotnet test, runsettings, relative path, profiler
res_type: kb
ticketid: 1661375
---

## Environment

| Product | Version |
| --- | --- |
| Progress® Telerik® JustMock | 2024.2.514.325 |

## Description

When running `dotnet test` with runsettings in a .NET project, setting the profiler's path as a relative path can ensure that tests run correctly across different systems. However, problems arise when the relative path doesn't correctly point to the JustMock profiler DLLs (`Telerik.CodeWeaver.Profiler.dll`), especially when the test runner's working directory is not as expected.

This KB article also answers the following questions:
- How to configure JustMock profiler for .NET testing with relative paths?
- What considerations should be made when setting up JustMock profiler paths in runsettings files?
- How to ensure the JustMock profiler is correctly located by the test runner in a .NET project?

## Solution

To configure the JustMock profiler with a relative path in your .NET project, follow these guidelines:

1. **Understand the Test Runner's Working Directory**: The path used in the runsettings should be relative to the working directory of the test runner, which is usually the build output for the project (e.g., `C:\MySolution\MyLibrary.Test\bin\$(Configuration)\$(TargetFramework)\`).

2. **Configure the Runsettings File**: Adjust the `CORECLR_PROFILER_PATH_32` and `CORECLR_PROFILER_PATH_64` settings in your runsettings file to point to the 32-bit and 64-bit versions of the JustMock profiler DLLs using a relative path from the test project's output folder. For example:

    ```xml
    <RunSettings>
        <RunConfiguration>
          <EnvironmentVariables>
            <JUSTMOCK_INSTANCE>1</JUSTMOCK_INSTANCE>
            <CORECLR_ENABLE_PROFILING>1</CORECLR_ENABLE_PROFILING>
            <CORECLR_PROFILER>{B7ABE522-A68F-44F2-925B-81E7488E9EC0}</CORECLR_PROFILER>
            <CORECLR_PROFILER_PATH_32>..\..\..\..\..\..\..\JustMock\Libraries\32\Telerik.CodeWeaver.Profiler.dll</CORECLR_PROFILER_PATH_32>
            <CORECLR_PROFILER_PATH_64>..\..\..\..\..\..\..\JustMock\Libraries\64\Telerik.CodeWeaver.Profiler.dll</CORECLR_PROFILER_PATH_64>
          </EnvironmentVariables>
        </RunConfiguration>
    </RunSettings>
    ```

3. **Consider the Project's Target Framework**: If your project targets the .NET Framework, note that there are limitations preventing the usage of relative paths and might require using absolute paths as a workaround.

4. **Sample Command**: Use a command similar to the following to run your tests with the configured runsettings file:
    ```shell
    dotnet test YourSolution.sln --configuration Release --no-build --no-restore --settings YourRunsettings.runsettings --verbosity normal --logger "trx;LogFileName=reports\YourReport.trx"
    ```

Remember to adjust the relative paths according to the structure of your solution and the location of the JustMock profiler DLLs within your repository.

## Notes

- Ensure all projects within your solution follow a consistent directory pattern to facilitate the use of a single runsettings file.
- If facing issues with profiler not being enabled, verify the calculated relative path by manually navigating from the test output directory to the profiler DLLs to ensure correctness.

## See Also

- [JustMock Documentation](https://docs.telerik.com/devtools/justmock/)
- [Configuring JustMock Profiler](https://docs.telerik.com/devtools/justmock/getting-started/justmock-configuration)
- [.NET Core Documentation](https://docs.microsoft.com/en-us/dotnet/core/)
