---
title: Controlling Mock Dependencies in JustMock for Specific Assertion Behavior
description: This article explains how to manage mock dependencies in Telerik JustMock to achieve accurate assertion behaviors in unit tests.
type: how-to
page_title: Managing Mock Dependencies in JustMock for Accurate Assertions
slug: controlling-mock-dependencies-justmock
tags: [mock, assert, dependencies, unit testing]
res_type: kb
ticketid: 1521224
---

## Description
While using Progress® Telerik® JustMock, you might encounter an unexpected behavior where an assertion on a mock object incorrectly includes assertions for another mock object that is used as a return value in one of its arrangements. This can lead to confusion when assertions fail for the wrong reasons. 

This knowledge base article also answers the following questions:
- How can I isolate mock assertions in JustMock?
- What is the correct way to set up mock dependencies in JustMock?
- How do I ensure accurate assertion behavior for mock objects in JustMock?

## Environment

| Product | Version |
| --- | --- |
| Progress® Telerik® JustMock | 2021.2.511.1 |

## Solution
The behavior observed is due to JustMock's feature that automatically creates dependencies between mocks when one mock is returned as a result of another mock's arrangement. This can inadvertently cause assertions to validate all related mocks, not just the one explicitly specified. 

To isolate assertions to the intended mock object without affecting its dependencies, utilize the **Returns** overload with a **Func\<TResult\>** parameter type. This approach prevents JustMock from creating automatic dependencies between the mocks.

### Example

Consider the scenario where you have a mock `SqlConnection` object and a mock `SqlTransaction` object:

```csharp
// Arrange
SqlConnection sqlConnection = Mock.Create<SqlConnection>();
SqlTransaction sqlTransaction = Mock.Create<SqlTransaction>();

Mock.Arrange(() => new SqlConnection()).Returns(sqlConnection);
Mock.Arrange(() => sqlConnection.BeginTransaction()).Returns(sqlTransaction);

// Assert
Mock.Assert(sqlConnection, "Problem in Connection"); 
```

In the sample code when asserting `sqlConnection` will automatically assert `sqlTransaction`. By changing the arrangement to use a **Returns** overload with **Func\<TResult\>** the assertion against `sqlConnection` will not inadvertently include `sqlTransaction`. Here is the code:

```csharp
Mock.Arrange(() => sqlConnection.BeginTransaction()).Returns(() => sqlTransaction);
```

## See Also
- [JustMock Documentation: Arrangement Fluent API](https://docs.telerik.com/devtools/justmock/api/Telerik.JustMock.Expectations.FuncExpectation-1.html#Telerik_JustMock_Expectations_FuncExpectation_1_Returns__0_)
- [JustMock Documentation: Asserting Mocks](https://docs.telerik.com/devtools/justmock/basic-usage/asserting)
