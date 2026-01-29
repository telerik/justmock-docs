---
title: Loose
page_title: Loose | JustMock Documentation
description: Loose
previous_url: /basic-usage-mock-behaviors-loose.html
slug: justmock/basic-usage/mock-behaviors/loose
tags: loose
published: True
position: 2
---

# Loose

__Loose__ mocks will create stubs for all void methods of the mocked type. For all value type members, the mock will return its default value and for all reference type members - `null`. For collection return types (e.g. array or `IEnumerable`) a non-null empty collection will be returned, instead of `null`.

To reach deeper-level functions, you will need to arrange them first. __Loose__ mocks are the predecessors of the [RecursiveLoose]({%slug justmock/basic-usage/mock-behaviors/recursiveloose%}) behavior, which makes them rarely used nowadays.

## Syntax

`Mock.Create<T>(Behavior.Loose);`

## Examples
To see the difference between `RecursiveLoose` and `Loose` `Behavior`, check [this example]({%slug justmock/basic-usage/mock-behaviors/recursiveloose%}#the-difference-between-recursiveloose-and-loose-mocks).

# See Also

 * [Mock Behaviors]({%slug justmock/basic-usage/mock-behaviors%})

 * [RecursiveLoose]({%slug justmock/basic-usage/mock-behaviors/recursiveloose%})

 * [CallOriginal]({%slug justmock/basic-usage/mock-behaviors/calloriginal%})

 * [Strict]({%slug justmock/basic-usage/mock-behaviors/strict%})
