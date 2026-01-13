---
title: Do Nothing
page_title: Do Nothing | JustMock Documentation
description: Do Nothing
previous_url: /basic-usage-mock-do-nothing.html
slug: justmock/basic-usage/mock/do-nothing
tags: do,nothing
published: True
position: 2
---

# Do Nothing

The `DoNothing` method is used to arrange that a call to a method or property should be ignored. When using `DoNothing`, all the logic inside the arranged method or property body is skipped and nothing happens when you call it.

We will use the following interface for the examples in this article:

#### Sample setup
```C# 
public interface IFoo
{
    int Bar { get; set; }
    void VoidCall();
}
```
```VB
Public Interface IFoo
    Property Bar() As Integer
    Function VoidCall()
End Interface
```


## Skip the Logic of a Method

Before describing the usage of the `DoNothing` method, let's first see an example to illustrate it.

#### Example 1: Sample usage of DoNothing on a void method
```C#
[TestMethod]
public void SimpleExampleWithDoNothing()
{
    // Arrange
    var foo = Mock.Create<IFoo>();

    Mock.Arrange(() => foo.VoidCall()).DoNothing().MustBeCalled();

    // Act
    foo.VoidCall();

    // Assert
    Mock.Assert(foo);
}
```
```VB
<TestMethod()>
Public Sub SimpleExampleWithDoNothing()
    ' Arrange
    Dim foo = Mock.Create(Of IFoo)()

    Mock.Arrange(Function() foo.VoidCall()).DoNothing().MustBeCalled()

    ' Act
    foo.VoidCall()

    ' Assert
    Mock.Assert(foo)
End Sub
```

In the example, we mark `foo.VoidCall()` with `DoNothing` and `MustBeCalled`. In this way, we indicate that a call to `foo.VoidCall` must be ignored and nothing should happen, but still the method should be called during the execution of the test.

You can use `DoNothing` with **non-void calls** as well. For the example we will be using the following `Foo` class:

#### Sample setup__
```C# 
public class Foo
{
    public int Echo(int num)
    {
        return num;
    }
}
```
```VB

Public Class Foo
    Function Echo(num As Integer) As Integer
        Throw New NotImplementedException
    End Function
End Class
```

#### Example 2: Sample usage of DoNothing with non-void method
```C# 
[TestMethod]
public void DoNothingWithNonPublicCalls()
{
    // Arrange
    var foo = Mock.Create<Foo>();

    Mock.Arrange(() => foo.Echo(Arg.AnyInt)).DoNothing();

    // Act
    foo.Echo(10);
}
```
```VB  
<TestMethod()>
Public Sub DoNothingWithNonPublicCalls()
    ' Arrange
    Dim foo = Mock.Create(Of Foo)()
    Mock.Arrange(Function() foo.Echo(Arg.AnyInt)).DoNothing()

    ' Act
    foo.Echo(10)
End Sub
```


## Skip the Logic of a Property Setter

`DoNothing` can be also used on a call to a property set.
#### Example 4: Usage of DoNothing on property setter
```C# 
[TestMethod]
public void AssertDoNothingOnPropertySet()
{
    // Arrange
    var foo = Mock.Create<IFoo>();

    Mock.ArrangeSet(() => foo.Bar = 1).DoNothing().MustBeCalled();

    // Act
    foo.Bar = 1;

    // Assert
    Mock.Assert(foo);
}
```
```VB
<TestMethod()>
Public Sub DAssertDoNothingOnPropertySet()
    'Arrange
    Dim foo = Mock.Create(Of IFoo)()
    Mock.ArrangeSet(Sub() foo.Bar = 0).DoNothing().MustBeCalled()

    'Act
    foo.Bar = 0

    'Assert
    Mock.Assert(foo)
End Sub
```


# See Also

* [Call Original]({%slug justmock/basic-usage/mock/call-original%})
* [Do Instead]({%slug justmock/basic-usage/mock/do-instead%})
* [Must Be Called]({%slug justmock/basic-usage/mock/must-be-called%})
* [Raise]({%slug justmock/basic-usage/mock/raise%})
* [Raises]({%slug justmock/basic-usage/mock/raises%})
* [Returns]({%slug justmock/basic-usage/mock/returns%})
* [Throws]({%slug justmock/basic-usage/mock/throws%})
