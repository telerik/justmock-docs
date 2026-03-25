---
title: Mock Classes and Interfaces
page_title: Mock Classes and Interfaces | JustMock Documentation
description: Mock Classes and Interfaces
previous_url: /basic-usage-mock-classes-and-interfaces.html
slug: justmock/basic-usage/mock-classes-and-interfaces
tags: mock,classes,and,interfaces
published: False
position: 5
---

# Mock Classes and Interfaces

Use `Mock.Create<T>()` to create a mock of any interface or class. The mock intercepts calls to its members so you can arrange return values, throw exceptions, verify call counts, and control behavior — without modifying production code.

This article covers:
- Mocking interfaces and classes with `Mock.Create<T>()`
- Differences between mocking interfaces and classes
- Choosing a mock behavior
- Verifying that calls occurred

## Mocking an Interface

When you mock an interface, JustMock creates an object that implements all interface members. By default, unarranged methods return the default value for their return type (`0`, `null`, `false`, etc.).

```C#
public interface IWarehouse
{
    bool HasInventory(string productName, int quantity);
}

[TestMethod]
public void ShouldMockInterface()
{
    // Arrange
    var warehouse = Mock.Create<IWarehouse>();

    Mock.Arrange(() => warehouse.HasInventory("Camera", 2)).Returns(true);

    // Act
    bool result = warehouse.HasInventory("Camera", 2);

    // Assert
    Assert.IsTrue(result);
}
```
```VB
Public Interface IWarehouse
    Function HasInventory(productName As String, quantity As Integer) As Boolean
End Interface

<TestMethod()>
Public Sub ShouldMockInterface()
    ' Arrange
    Dim warehouse = Mock.Create(Of IWarehouse)()

    Mock.Arrange(Function() warehouse.HasInventory("Camera", 2)).Returns(True)

    ' Act
    Dim result As Boolean = warehouse.HasInventory("Camera", 2)

    ' Assert
    Assert.IsTrue(result)
End Sub
```

## Mocking a Class

You can mock a concrete class the same way. JustMock intercepts **virtual** and **abstract** members on classes.

> **Note:** Mocking **non-virtual** class members, **static** methods, and **sealed** classes requires JustMock Full with the JustMock profiler enabled.

```C#
public class Logger
{
    public virtual string GetMessage() => "real message";
}

[TestMethod]
public void ShouldMockClass()
{
    // Arrange
    var logger = Mock.Create<Logger>();

    Mock.Arrange(() => logger.GetMessage()).Returns("mocked message");

    // Act
    string result = logger.GetMessage();

    // Assert
    Assert.AreEqual("mocked message", result);
}
```
```VB
Public Class Logger
    Public Overridable Function GetMessage() As String
        Return "real message"
    End Function
End Class

<TestMethod()>
Public Sub ShouldMockClass()
    ' Arrange
    Dim logger = Mock.Create(Of Logger)()

    Mock.Arrange(Function() logger.GetMessage()).Returns("mocked message")

    ' Act
    Dim result As String = logger.GetMessage()

    ' Assert
    Assert.AreEqual("mocked message", result)
End Sub
```

### Interface vs. Class Mocking

| | Interface | Class |
|---|---|---|
| Virtual members | All members are virtual by default | Only members marked `virtual` or `abstract` |
| Non-virtual members | N/A | Requires JustMock Full + profiler |
| Static members | N/A | Requires JustMock Full + profiler |
| Sealed classes | N/A | Requires JustMock Full + profiler |

## Choosing a Behavior

Pass a `Behavior` value to `Mock.Create<T>()` to control how the mock responds to unarranged calls.

| Behavior | Description | When to use |
|---|---|---|
| `Behavior.Loose` (default) | Unarranged calls return default values and do not throw. | Use when you only care about specific interactions. |
| `Behavior.Strict` | Unarranged calls throw `StrictMockException`. | Use when you want to catch any unexpected call. |
| `Behavior.RecursiveLoose` | Unarranged calls on reference-type members return new mocks automatically. | Use when the code under test traverses object graphs. |
| `Behavior.CallOriginal` | Unarranged calls invoke the real implementation. | Use when you want to test partial behavior (spy pattern). |

```C#
[TestMethod]
[ExpectedException(typeof(StrictMockException))]
public void ShouldThrowOnUnarrangedCall()
{
    // Arrange - Strict behavior: only arranged calls are allowed
    var warehouse = Mock.Create<IWarehouse>(Behavior.Strict);

    // Act - HasInventory is not arranged, so this throws StrictMockException
    warehouse.HasInventory("Camera", 2);
}
```
```VB
<TestMethod()>
<ExpectedException(GetType(StrictMockException))>
Public Sub ShouldThrowOnUnarrangedCall()
    ' Arrange - Strict behavior: only arranged calls are allowed
    Dim warehouse = Mock.Create(Of IWarehouse)(Behavior.Strict)

    ' Act - HasInventory is not arranged, so this throws StrictMockException
    warehouse.HasInventory("Camera", 2)
End Sub
```

## Verifying Calls

After the act phase, use `Mock.Assert(mock)` to verify all expectations set with `MustBeCalled()`, `OccursOnce()`, or other occurrence constraints.

```C#
[TestMethod]
public void ShouldVerifyCallOccurred()
{
    // Arrange
    var warehouse = Mock.Create<IWarehouse>();

    Mock.Arrange(() => warehouse.HasInventory(Arg.AnyString, Arg.AnyInt))
        .Returns(true)
        .MustBeCalled();

    // Act
    warehouse.HasInventory("Camera", 2);

    // Assert
    Mock.Assert(warehouse);
}
```
```VB
<TestMethod()>
Public Sub ShouldVerifyCallOccurred()
    ' Arrange
    Dim warehouse = Mock.Create(Of IWarehouse)()

    Mock.Arrange(Function() warehouse.HasInventory(Arg.AnyString, Arg.AnyInt)) _
        .Returns(True) _
        .MustBeCalled()

    ' Act
    warehouse.HasInventory("Camera", 2)

    ' Assert
    Mock.Assert(warehouse)
End Sub
```

## See Also

 * [Arrange Act Assert]({%slug justmock/basic-usage/arrange-act-assert%})
 * [Mock Properties]({%slug justmock/basic-usage/mock-properties%})
 * [Mock Behaviors - Loose]({%slug justmock/basic-usage/mock-behaviors/loose%})
 * [Mock Behaviors - Strict]({%slug justmock/basic-usage/mock-behaviors/strict%})
 * [Asserting Occurrence]({%slug justmock/basic-usage/asserting-occurrence%})
 * [Returns]({%slug justmock/basic-usage/mock/returns%})
 * [Matchers]({%slug justmock/basic-usage/matchers%})