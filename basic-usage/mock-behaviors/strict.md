---
title: Strict
page_title: Strict | JustMock Documentation
description: Strict
previous_url: /basic-usage-mock-behaviors-strict.html
slug: justmock/basic-usage/mock-behaviors/strict
tags: strict
published: True
position: 4
---

# Strict

By default __Telerik® JustMock__ uses loose mocks (`Behavior.RecursiveLoose`) and allows you to call any method on a given type. No matter whether the method call is arranged or not, you are able to call it. However, you may have a case where you want to enable only arranged calls and to reject non-arranged calls. In such cases you need to set the `Behavior` to `Strict` when creating the mock. Another case where you may need to use strict mocks is when arranging calls to property setters.

> **Caution**
> 
> If you try to call a method that is not arranged on a strict mock an exception will be thrown - `StrictMockException`.

## Syntax

`Mock.Create<T>(Behavior.Strict);`

## Examples

### Arbitrary Calls Generate Exception in Strict Mocks

The next example shows the creation of a strict mock, without any further arrangements. Calling a non previously arranged member will result in throwing of a `MockException`.
          
```C#
public interface IFoo
{
    void VoidCall();
}
```
```VB
Public Interface IFoo
    Sub VoidCall()
End Interface
```


```C#
[TestMethod]
[ExpectedException(typeof(StrictMockException))]
public void ArbitraryCallsShouldGenerateException()
{
    //Arrange
    var foo = Mock.Create<IFoo>(Behavior.Strict);

    //Act
    foo.VoidCall();
}
```
```VB
<TestMethod>
<ExpectedException(GetType(StrictMockException))> _
Public Sub ArbitraryCallsShouldGenerateException()
    'Arrange
    Dim foo = Mock.Create(Of IFoo)(Behavior.[Strict])

    'Act
    foo.VoidCall()
End Sub
```

### Testing If Functions Work in Isolation
          
Let assume we have the following class:
          
```C#
public class Foo
{
    public void VoidMethod()
    {
        // Some logic here...
    }
    public void ExecuteVoidMethod()
    {
        this.VoidMethod();
    }

    public string SomeStringFunctiond()
    {
        return default(string);
    }

    public int SomeIntFunctiond()
    {
        return default(int);
    }
}
```
```VB
Public Class Foo
    Public Sub VoidMethod()
        ' Some logic here...
    End Sub
    Public Sub ExecuteVoidMethod()
        Me.VoidMethod()
    End Sub

    Public Function SomeStringFunctiond() As String
        Return Nothing
    End Function

    Public Function SomeIntFunctiond() As Integer
        Return 0
    End Function
End Class
```

Now, we want to test the `ExecuteVoidMethod()` function. The next test should throw a `MockException` if non-arranged member is called. In our case such member is the `VoidMethod()` function:
          
```C#
[TestMethod]
[ExpectedException(typeof(StrictMockException))]
public void ShouldAssertIfThereAreCallsToNonArrangedMembers()
{
    // Arrange
    var foo = Mock.Create<Foo>(Behavior.Strict);

    Mock.Arrange(() => foo.ExecuteVoidMethod()).CallOriginal().OccursAtLeast(1);

    // Act
    foo.ExecuteVoidMethod();
}
```
```VB
<TestMethod>
<ExpectedException(GetType(StrictMockException))> _
Public Sub ShouldAssertIfThereAreCallsToNonArrangedMembers()
    ' Arrange
    Dim foo = Mock.Create(Of Foo)(Behavior.[Strict])

    Mock.Arrange(Sub() foo.ExecuteVoidMethod()).CallOriginal().OccursAtLeast(1)

    ' Act
    foo.ExecuteVoidMethod()
End Sub
```

The next example shows the basic usage of a strict mock. The test checks for the basic functions of the SUT in isolation: 

```C#
[TestMethod]
public void ShouldAssertIfThereAreNOArbitraryCalls()
{
    // Arrange
    var foo = Mock.Create<Foo>(Behavior.Strict);

    Mock.Arrange(() => foo.VoidMethod()).OccursAtLeast(1);
    Mock.Arrange(() => foo.ExecuteVoidMethod()).CallOriginal().OccursAtLeast(1);

    // Act
    foo.ExecuteVoidMethod();

    // Assert
    Mock.Assert(foo);
}
```
```VB
<TestMethod> _
Public Sub ShouldAssertIfThereAreNOArbitraryCalls()
    ' Arrange
    Dim foo = Mock.Create(Of Foo)(Behavior.[Strict])

    Mock.Arrange(Sub() foo.VoidMethod()).OccursAtLeast(1)
    Mock.Arrange(Sub() foo.ExecuteVoidMethod()).CallOriginal().OccursAtLeast(1)

    ' Act
    foo.ExecuteVoidMethod()

    ' Assert
    Mock.Assert(foo)
End Sub
```

First, we create the mock with `Behavior.Strict`. Then, we arrange the methods that should be invoked  during the test execution. Finally, we assert against them.
        
## See Also

 * [Mock Behaviors]({%slug justmock/basic-usage/mock-behaviors%})

 * [RecursiveLoose]({%slug justmock/basic-usage/mock-behaviors/recursiveloose%})

 * [CallOriginal]({%slug justmock/basic-usage/mock-behaviors/calloriginal%})

 * [JustMock API Basics]({%slug justmock/getting-started/basics/basics%})
