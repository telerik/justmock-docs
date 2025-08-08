---
title: Third-party Profilers Integration
page_title: Third-party Profilers Integration | JustMock Documentation
description: Integrate Telerik JustMock with third-party profilers
slug: justmock/integration/profiler-integration
tags: profiler
published: True
position: 4
previous_url: /integration-justmock-console-profiler-integration, /integration-justmock-console-profiler-integration.html
---

# Third-party Profilers Integration

This article explains how to integrate third-party profilers with the profiler used by JustMock. In most cases, integration is handled automatically through the [JustMock Configuration Tool]({%slug justmock/integration/codecoverage-tools%}), which allows users to enable supported profilers with a simple checkbox. However, when a profiler is not listed or auto-detection fails, JustMock provides a manual method for linking external profilers using the `--link-existing-profiler` (`-l`) option in the JustMock Console.

## Generic Command Structure

To link JustMock with an external profiler, users should invoke the external profiler’s console application and set the `-target` to Telerik.JustMock.Console.exe. 
The `-targetargs` parameter must include the JustMock execution command, starting with `runadvanced`, followed by `-c` to specify the test runner (e.g., vstest.console.exe), and `-a` to provide the full path to the test assembly (e.g., MyTests.dll).  
The profiler paths for 32-bit and 64-bit architectures must be provided using `--profiler-path-32` and `--profiler-path-64`, followed by `-l` to enable linking.

```bat
"full\path\to\ProfilerTool.Console.exe" -target:"%JUSTMOCK_BINARY_PATH%\Telerik.JustMock.Console.exe" -targetargs:"runadvanced -c \"full\path\to\vstest.console.exe\" -a \"full\path\to\MyTests.dll\" --profiler-path-32 \"%JUSTMOCK_PACKAGE_PATH%\CodeWeaver\32\Telerik.CodeWeaver.Profiler.dll\" --profiler-path-64 \"%JUSTMOCK_PACKAGE_PATH%\CodeWeaver\64\Telerik.CodeWeaver.Profiler.dll\" -l"
```

**Note:** Before executing the command, ensure that:
 - The profiling tool is downloaded and extracted.
 - JustMock is installed.
 - Your solution and test project are ready.

## OpenCover Example

For a clearer understanding of how to structure this integration, the following example demonstrates how JustMock can be linked with `OpenCover` using a fully composed command.

```bat
"C:\Program Files (x86)\OpenCover\OpenCover.Console.exe" -target:"%JUSTMOCK_BINARY_PATH%\Telerik.JustMock.Console.exe" -targetargs:"runadvanced -c \"C:\Program Files\Microsoft Visual Studio\2022\Professional\Common7\IDE\Extensions\TestPlatform\vstest.console.exe\" -a \"C:\Users\Administrator\source\repos\MyTests\MyTests\bin\Debug\net8.0\MyTests.dll\" --profiler-path-32 \"%JUSTMOCK_PACKAGE_PATH%\CodeWeaver\32\Telerik.CodeWeaver.Profiler.dll\" --profiler-path-64 \"%JUSTMOCK_PACKAGE_PATH%\CodeWeaver\64\Telerik.CodeWeaver.Profiler.dll\" -l"
```
