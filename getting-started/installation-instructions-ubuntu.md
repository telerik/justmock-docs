---
title: Ubuntu Installation and Setup 
page_title: Ubuntu Installation and Setup | JustMock Documentation
description: Learn how you can install the different versions of the Telerik JustMock framework.
slug: justmock/getting-started/installation-instructions-ubuntu
tags: installation,instructions,ubuntu,linux
published: True
position: 8
---

# Installation and Setup for Ubuntu

This topic outlines how to configure [Telerik JustMock](https://www.telerik.com/products/mocking.aspx) to run already built tests on a Ubuntu machine.


### Download JustMock tar.gz Archive

1. Download the JustMock tar.gz archive from www.telerik.com:
	* If this is your first time here and you want to try JustMock, download the trial installer file from here: [Download JustMock](https://www.telerik.com/account/downloads/product-download?product=JUSTMOCK). Keep in mind that this will require you to either log in or create a new Telerik account.
	* Note that there are separate installers for x64 and arm64 platforms. Choose the one corresponding to your device system architecture.

1. Unzip the archive to local folder. Lets assume the forlder is named **<JM_FOLDER>**.

1. Open a terminal and execute following commands:

    export JUSTMOCK_INSTANCE=1
    export CORECLR_ENABLE_PROFILING=1
    export CORECLR_PROFILER={B7ABE522-A68F-44F2-925B-81E7488E9EC0}
    export CORECLR_PROFILER_PATH="**<JM_FOLDER>**/CodeWeaver/64/libTelerik.CodeWeaver.Profiler.so"

1. Execute **dotnet test** command against the dll that contains your tests. If we assume the dll is named **Tests.dll** and you want to export the results to .trx file, then the command should look like:

    dotnet test Tests.dll --logger trx

## Resources and Documentation

- **Offline Documentation**

    The documentation is also available in PDF format which you can download from your [Telerik account](https://www.telerik.com/account/my-downloads).

- **Additional Assistance**

    If you need additional assistance, take a look at our [online JustMock forums](https://www.telerik.com/forums/justmock) or [contact support](https://www.telerik.com/account/support-tickets?pid=743).

- **Suggestions and Reports**

    If you want to suggest a new feature or vote for a popular one, please visit [JustMock Feedback Portal](https://feedback.telerik.com/justmock).

## Next Steps

* [JustMock API Basics]({%slug justmock/getting-started/basics/basics%})
