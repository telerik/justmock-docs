---
title: General Setup Guidelines
page_title: General Setup Guidelines | JustMock Documentation
description: Learn how to setup Telerik JustMock and integrate it with CI.
slug: justmock/integration/setup-guidelines
tags: ci, integration, setup, install, continuous
published: True
position: 0
---

# General Setup Guidelines

You can use JustMock in your CI/CD environment. How would you set it up depends on the specific environment and the permissions you have to modify it. JustMock can be used with or without installation.

## With Installation

It is recommended to install **JustMock** whenever this is possible. The automatic process sets up the environment so that you can directly run the profiler and start executing your tests. Further information about the installation is available in the [Installation and Setup]({%slug justmock/getting-started/installation-instructions%}) article.

## Installation-Free

When installing software is not allowed in the used environment, you can still set up **JustMock** to run so that you can execute your tests. This is achieved by setting **environment variables**. For more information about the variables you should set, refer to the [Installation-Free Elevated Mocking]({%slug justmock/integration/installation-free-elevated-mocking%}) topic.