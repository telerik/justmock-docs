---
title: Sealed Mocking
page_title: Sealed Mocking | JustMock Documentation
description: Sealed Mocking
previous_url: /advanced-usage-sealed-mocking.html
slug: justmock/advanced-usage/sealed-mocking
tags: sealed,mocking, mock, class
published: True
position: 3
---

# Mock Sealed Classes

This functionality allows you to fake sealed classes and calls to their members, set expectations and verify results using the [AAA]({%slug justmock/basic-usage/arrange-act-assert%}) principle. Mocking sealed classes and calls to their methods/properties doesn't affect the way you write your tests, i.e. the same syntax is used for mocking non-sealed classes.

>important **Important**
>
> To use sealed mocking, you first need to **go to elevated mode** by enabling JustMock from the menu. Learn how to do that in the [How to Enable/Disable Telerik JustMock]({%slug justmock/advanced-usage%}#how-to-enable-disable-telerik-justmock) topic.
>
>This feature is available only in the **commercial** version of **Telerik JustMock**. Refer to [this]({%slug justmock/licensing/commercial-vs-free-version%}) topic to learn more about the differences between the commercial and free versions.

## Arrange Method Call on a Sealed Class

To demonstrate how you can mock the behavior of a method from a sealed class, let's take the following class as an example:

#### Sample setup
```C#
public sealed class FooSealed
{
    public int Echo(int arg1)
    {
        return arg1;
    }
}
```
```VB
Public NotInheritable Class FooSealed
    Public Function Echo(arg1 As Integer) As Integer
        Return arg1
    End Function
End Class
```

**Example 1** shows how you can arrange a non-virtual method of a sealed class. More specifically, how to setup that a call to the `foo.Echo` method with any `int` argument should return `10`.

#### Example 1: Arrange method
```C#
[TestMethod]
public void ShouldAssertFinalMethodCallOnASealedClass()
{
    // Arrange
    var foo = Mock.Create<FooSealed>();

    Mock.Arrange(() => foo.Echo(Arg.IsAny<int>())).Returns(10);

    // Act
    var actual = foo.Echo(1);

    // Assert
    Assert.AreEqual(10, actual);    
}
```
```VB
<TestMethod()>
Public Sub ShouldAssertFinalMethodCallOnASealedClass()
    ' Arrange
    Dim foo = Mock.Create(Of FooSealed)()

    Mock.Arrange(Function() foo.Echo(Arg.IsAny(Of Integer)())).Returns(10)

    ' Act
    Dim actual = foo.Echo(1)

    ' Assert
    Assert.AreEqual(10, actual)
End Sub
```



## Create Mock for Sealed Class with Internal Constructor

Even the `sealed` class has only an `internal` constructor, you can still create a mock, call its members and verify the results.

Following is the sample class that will be used to demonstrate that:

#### Sample setup
```C#
public sealed class FooSealedInternal
{
    internal FooSealedInternal()
    {
    }

    public int Echo(int arg1)
    {
        return arg1;
    }
}
```
```VB
Public NotInheritable Class FooSealedInternal

    Friend Sub New()
    End Sub

    Public Function Echo(arg1 As Integer) As Integer
        Return arg1
    End Function
End Class
```

**Example 2** sets up that a call to the `foo.Echo` method with any `int` argument should return 10. After we ensure that the object is actually created, we assert for the return value of the `foo.Echo` method.

#### Example 2: Create mock of sealed class with internal constructor
```C#
[TestMethod]
public void ShouldCreateMockForASealedClassWithInternalConstructor()
{
    // Arrange
    var foo = Mock.Create<FooSealedInternal>();

    Mock.Arrange(() => foo.Echo(Arg.IsAny<int>())).Returns(10);

    // Assert
    Assert.IsNotNull(foo);

    // Act
    var actual = foo.Echo(1);

    // Assert
    Assert.AreEqual(10, actual);
}
```
```VB
<TestMethod()>
Public Sub ShouldCreateMockForASealedClassWithInternalConstructor()
    ' Arrange
    Dim foo = Mock.Create(Of FooSealedInternal)()

    Mock.Arrange(Function() foo.Echo(Arg.IsAny(Of Integer)())).Returns(10)

    ' Assert
    Assert.IsNotNull(foo)

    ' Act
    Dim actual = foo.Echo(1)

    ' Assert
    Assert.AreEqual(10, actual)
End Sub
```

## Create Mock for Sealed Class with Interface

You can also mock sealed classes that implement an interface. 

#### Sample setup
```C#
public interface IFoo
{
    void Execute();
    void Execute(int arg1);
}

public sealed class Foo : IFoo
{
    public void Execute()
    {
        throw new NotImplementedException();
    }

    void IFoo.Execute(int arg1)
    {
        throw new NotImplementedException();
    }
}
```
```VB
Public Interface IFoo
    Sub Execute()
    Sub Execute(arg1 As Integer)
End Interface

Public NotInheritable Class Foo
Implements IFoo
    Public Sub Execute() Implements IFoo.Execute
        Throw New NotImplementedException()
    End Sub

    Private Sub Execute(arg1 As Integer) Implements IFoo.Execute
        Throw New NotImplementedException()
    End Sub
End Class
```

When a mock is created using `Mock.Create`, all of its dependencies are also implemented behind the scenes. In **Example 3**, we mock the `Foo` class, which implements the `IFoo` interface.

#### Example 3: Create mock from sealed class implementing an interface and control its method behavior
```C#
[TestMethod]
public void ShouldAssertCallOnVoid()
{
    // Arrange
    var foo = Mock.Create<Foo>();

    bool called = false;
    // When foo.Execute() is invoked, skip its implementation and set called to true instead.
    Mock.Arrange(() => foo.Execute()).DoInstead(() => called = true);

    // Act
    foo.Execute();

    // Assert
    Assert.IsTrue(called);
}
```
```VB
<TestMethod()>
Public Sub ShouldAssertCallOnVoid()
    ' Arrange
    Dim foo = Mock.Create(Of Foo)()

    Dim called As Boolean = False
    ' When foo.Execute() is invoked, skip its implementation and set called to true instead.
    Mock.Arrange(Sub() foo.Execute()).DoInstead(Sub() called = True)

    ' Act
    foo.Execute()

    ' Assert
    Assert.IsTrue(called)
End Sub
```

Furthermore, if you are interested in `IFoo` interface implementation, you can use the `as` operator and call the interface members as shown below.

#### Example 4: Create mock from sealed class implementing an interface and verify the behavior of the interface's method
```C#
[TestMethod]
public void ShouldAssertCallOnVoidThroughAnInterface()
{
    // Arrange
    var foo = Mock.Create<Foo>();

    bool called = false;
    // When foo.Execute() is invoked, skip its implementation and set called to true instead.
    Mock.Arrange(() => foo.Execute()).DoInstead(() => called = true);

    // Act
    IFoo iFoo = foo;
    iFoo.Execute();

    // Assert
    Assert.IsTrue(called);
}
```
```VB
<TestMethod()>
Public Sub ShouldAssertCallOnVoidThroughAnInterface()
    ' Arrange
    Dim foo = Mock.Create(Of Foo)()

    Dim called As Boolean = False
    ' When foo.Execute() is invoked, skip its implementation and set called to true instead.
    Mock.Arrange(Sub() foo.Execute()).DoInstead(Sub() called = True)

    ' Act
    Dim iFoo As IFoo = foo
    iFoo.Execute()

    ' Assert
    Assert.IsTrue(called)
End Sub
```

## See Also

* [Advanced Usage]({%slug justmock/advanced-usage%})
* [Mock Non-Abstract and Non-Virtual Classes or Members]({%slug justmock/advanced-usage/concrete-mocking%})
* [Mock Static Classes and Members]({%slug justmock/advanced-usage/static-mocking%})