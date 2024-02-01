---
title: The type initializer for 'Telerik.JustMock.SecuredReflection' threw an exception
description: How to solve 'Telerik.JustMock.SecuredReflection' exception
type: troubleshooting
page_title: How to solve 'Telerik.JustMock.SecuredReflection' exception
slug: type-initializer-for-secured-reflection
tags: justmock, tests, exception
res_type: kb
---

# Description
When JustMock is referenced from the installation folder, it is possible to get the following exception during test run: 

System.TypeInitializationException : The type initializer for 'Telerik.JustMock.SecuredReflection' threw an exception.
---- System.IO.FileNotFoundException : Could not load file or assembly 'System.Security.Permissions, Version=4.0.1.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'. The system cannot find the file specified.

# Solution
Add reference to System.Security.Permissions' nuget package. Target versions greater than 4.0.1.0.
