---
title: Mock Behaviors
page_title: Mock Behaviors | JustMock Documentation
description: Mock Behaviors
previous_url: /basic-usage-mock-behaviors.html
slug: justmock/basic-usage/mock-behaviors
tags: mock,behaviors
published: True
position: 0
---

# Mock Behaviors

This article provides a quick overview of the four mock behaviors in __Telerik® JustMock__. For more details about each mock behavior, follow the links below.

__RecursiveLoose Behavior__

This is the default mock behavior. It takes care of members at all times by recursively creating empty stubs for them. Mocks with RecursiveLoose behavior will never return `null` on any of the mocks in the dependency chain even if none of them are arranged.

[Read more...]({%slug justmock/basic-usage/mock-behaviors/recursiveloose%})

__Loose Behavior__

Unless otherwise arranged, the members of loose mocks act like stubs - they return the default value of their return type - null for reference types and the default value for value types.

[Read more...]({%slug justmock/basic-usage/mock-behaviors/loose%})

__CallOriginal Behavior__

A mock with behavior CallOriginal will follow the original implementation of the mocked instance.

[Read more...]({%slug justmock/basic-usage/mock-behaviors/calloriginal%})

__Strict Behavior__

When a strict mock is created, you are only allowed to call arranged methods. Every other call will result in `MockException`.

[Read more...]({%slug justmock/basic-usage/mock-behaviors/strict%})

## See Also


 * [Arrange Act Assert]({%slug justmock/basic-usage/arrange-act-assert%})

 * [JustMock API Basics]({%slug justmock/basic-usage/mock%})
