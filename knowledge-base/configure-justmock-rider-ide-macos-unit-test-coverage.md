---
title: Configuring JustMock with Rider IDE on macOS for Unit Test Coverage
description: Learn how to configure Telerik JustMock for seamless unit test coverage with Rider IDE on macOS, including setup for Apple Silicon.
type: how-to
page_title: How to Configure JustMock with Rider IDE on macOS for Unit Test Coverage
slug: configure-justmock-rider-ide-macos-unit-test-coverage
tags: progress telerik justmock, unit testing, rider ide, macos, apple silicon, code coverage, dotcover
res_type: kb
ticketid: 1652919
---

## Environment

| Product | Progress® Telerik® JustMock |
| --- | --- |
| Version | 2024.3.805.336 |

## Description
While attempting to run unit test coverage in JetBrains Rider IDE on macOS, an error occurs indicating that the Telerik.JustMock profiler must be enabled. The error message suggests that a third-party profiler is active and advises disabling it or configuring JustMock to link with the third-party profiler. This process involves ensuring the JustMock profiler can run alongside another profiler, like dotCover, which is embedded in Rider IDE.

This KB article also answers the following questions:
- How do I enable JustMock coverage in Rider IDE on macOS?
- How do I resolve the `ElevatedMockingException` when running tests with coverage in Rider?
- What are the steps to link JustMock with another profiler on macOS for unit test coverage?

## Solution

To configure Telerik JustMock for unit test coverage with Rider IDE on macOS, including support for Apple Silicon, follow these steps:

1. **Ensure Correct Profiler Path**: Make sure that the `CORECLR_PROFILER_PATH` environment variable in your runsettings file contains an existing path to the specific JustMock profiler (libTelerik.CodeWeaver.Profiler.dylib) for macOS (Intel or Apple Silicon).

2. **Enable transparent Integration with dotCover**: Ensure [transparent integration](https://www.jetbrains.com/help/dotcover/Profiling_Guidelines__Transparent_Integration.html) with the dotCover profiler embedded in Rider IDE. This requires preparing and copying the `.jetbrains-profiler-transparent-integration-config` file into your home directory.

3. **Configure Rider IDE**:
    - Remove any `.runsettings` files and environment variables related to other profilers, such as Coverlet.
    - Configure the Rider test runner to use the specific `runsettings` file containing the necessary environment setup.

4. **Verify Configuration**: Open your solution with Rider IDE, and run unit tests with coverage to verify the configuration is correct. The JustMock profiler should now work in harmony with Rider's dotCover, allowing for successful test coverage analysis.

## Notes

- Ensure you are using the correct version of JustMock that supports transparent integration with dotCover for macOS machines.
- The `CORECLR_PROFILER_PATH` and other environment variables in your runsettings file must reflect your specific environment setup and JustMock version.

## See Also

- [Telerik JustMock Documentation](https://docs.telerik.com/devtools/justmock/introduction)
- [Transparent Integration with dotCover](https://www.jetbrains.com/help/dotcover/Profiling_Guidelines__Transparent_Integration.html)

