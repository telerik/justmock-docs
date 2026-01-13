---
title: Mocking Chained Calls
page_title: Mocking Chained Calls | JustMock Documentation
description: Mocking Chained Calls
previous_url: /basic-usage-recursive-mocking.html
slug: justmock/basic-usage/recursive-mocking
tags: recursive,mocking, mock, chain, nested, method, property
published: True
position: 11
---

# Mocking Chained Calls

This feature enables you to mock members that are obtained as a result of "chained" calls on a mock. For example, mocking chained calls is useful in the cases when you test code like this: `foo.Bar.Baz.Do("x")`.

In the next examples, we will use the following sample code to test:

#### Sample setup
```C#
public interface IFoo
{
    IBar Bar { get; set; }
    string Do(string command);
}

public interface IBar
{
    int Value { get; set; }
    string Do(string command);
    IBaz Baz { get; set; }
}

public interface IBaz
{
    string Do(string command);
}
```
 ```VB 
Public Interface IFoo
    Property Bar() As IBar
    Function [Do](command As String) As String
End Interface

Public Interface IBar
    Property Value() As Integer
    Function [Do](command As String) As String
    Property Baz() As IBaz
End Interface

Public Interface IBaz
    Function [Do](command As String) As String
End Interface
```

## How to Mock Chain of Method Calls

Consider the above code. Let's arrange that a call to `IFoo.Do` method returns a particular string. `IFoo` contains an `IBar` which, in turn, also contains a `Do` method. `IBar.Do` method can be accessed via a nested call to `IFoo.Bar`. To arrange the call directly from `IFoo`, we just need to chain it considering the fact that JustMock will **automatically** create the necessary mock for `IBar`.

#### Example 1: Mocking chain of method calls example
```C#
[TestMethod]
public void ShouldAssertNestedVeriables()
{
    // Arrange
    var foo = Mock.Create<IFoo>();
    
    string ping = "ping";
    
    Mock.Arrange(() => foo.Do(ping)).Returns("ack");
    Mock.Arrange(() => foo.Bar.Do(ping)).Returns("ack2");
    
    // Act
    var actualFooDo = foo.Do(ping);
    var actualFooBarDo = foo.Bar.Do(ping);
    
    // Assert
    Assert.AreEqual("ack", actualFooDo);
    Assert.AreEqual("ack2", actualFooBarDo);
}
```
```VB
<TestMethod()>
Public Sub ShouldAssertNestedVeriables()
    ' Arrange
    Dim foo = Mock.Create(Of IFoo)()

    Dim ping As String = "ping"

    Mock.Arrange(Function() foo.Do(ping)).Returns("ack")
    Mock.Arrange(Function() foo.Bar.Do(ping)).Returns("ack2")

    ' Act
    Dim actualFooDo = foo.Do(ping)
    Dim actualFooBarDo = foo.Bar.Do(ping)

    ' Assert
    Assert.AreEqual("ack", actualFooDo)
    Assert.AreEqual("ack2", actualFooBarDo)
End Sub
```

Note: If `foo.Bar` is not arranged, it will still be instantiated. **Example 2** shows how you can verify that.

#### Example 2: Verifying object instantiation__

```C#
[TestMethod]
public void ShouldNotInstantiateFooBar()
{
    // Arrange
    var foo = Mock.Create<IFoo>();

    // Assert
    Assert.IsNotNull(foo.Bar);
}
```
```VB
<TestMethod()>
Public Sub ShouldNotInstantiateFooBar()
    ' Arrange
    Dim foo = Mock.Create(Of IFoo)()

    ' Assert
    Assert.IsNotNull(foo.Bar)
End Sub
```


## Nested Property Calls

This section shows how to arrange the getter and setter of a nested property. All you need to do is to specify the property as you would do in real cases. JustMock will automatically setup all the requirements for the member like, for example, instantiating other objects in the chain.

#### Example 3: Arrange the getter of a nested property__
```C#
[TestMethod]
public void ShouldAssertNestedPropertyGet()
{
    // Arrange
    var foo = Mock.Create<IFoo>();
    Mock.Arrange(() => foo.Bar.Value).Returns(10);

    // Act
    var actual = foo.Bar.Value;

    // Assert
    Assert.AreEqual(10, actual);
}
```
```VB
<TestMethod()>
Public Sub ShouldAssertNestedPropertyGet()
    ' Arrange
    Dim foo = Mock.Create(Of IFoo)()

    Mock.Arrange(Function() foo.Bar.Value).Returns(10)

    ' Act
    Dim actual = foo.Bar.Value

    ' Assert
    Assert.AreEqual(10, actual)
End Sub
```

Here we arrange `foo.Bar.Value` to return `10`.

You can also arrange the setter of a nested property. In **Example 4**, you will see how to arrange a nested property setter in scenario with **Strict** behavior. When using that behavior, you are allowed to call only arranged members and all other calls throw exception. If you need more details on this setup, check the [Strict behavior]({%slug justmock/basic-usage/mock-behaviors/strict%}) topic.


#### Example 4: Arrange the setter of a nested property__
```C#
[TestMethod]
[ExpectedException(typeof(StrictMockException))]
public void ShouldAssertNestedPropertySet()
{
    // Arrange
    var foo = Mock.Create<IFoo>(Behavior.Strict);

    Mock.ArrangeSet(() => { foo.Bar.Value = 5; }).DoNothing();

    // Act
    foo.Bar.Value = 5;
    foo.Bar.Value = 10; // This line will throw MockException. The mocking behavior is Strict and only foo.Bar.Value = 5 is arranged and allowed.
}
```
```VB
<TestMethod()>
<ExpectedException(GetType(StrictMockException))>
Public Sub ShouldAssertNestedPropertySet()
    ' Arrange
    Dim foo = Mock.Create(Of IFoo)(Behavior.Strict)

    Mock.ArrangeSet(Function() foo.Bar.Value = 5).DoNothing()

    ' Act
    foo.Bar.Value = 5
    foo.Bar.Value = 10 ' This line will throw MockException. The mocking behavior is Strict and only foo.Bar.Value = 5 is arranged and allowed.
End Sub
```

We use `Bahavior.Strict` to enable only arranged calls and to reject any other calls. Only setting `foo.Bar.Value` to `5` is allowed and as we set it to `10`, an exception will be thrown. 

## Nested Method Calls

Similarly to the nested properties, you can also arrange nested methods.

#### Example 5: Arrange the behavior of a nested method__
```C#
[TestMethod]
public void NestedPropertyAndMethodCalls()
{
    // Arrange
    var foo = Mock.Create<IFoo>();
    
    Mock.Arrange(() => foo.Bar.Do("x")).Returns("xit");
    Mock.Arrange(() => foo.Bar.Baz.Do("y")).Returns("yit");
    
    // Act
    var actualFooBarDo = foo.Bar.Do("x");
    var actualFooBarBazDo = foo.Bar.Baz.Do("y");
    
    // Assert
    Assert.AreEqual("xit", actualFooBarDo);
    Assert.AreEqual("yit", actualFooBarBazDo);
    }
```
```VB
<TestMethod()>
Public Sub ShouldAssertNestedPropertyHavingMethods()
    ' Arrange
    Dim foo = Mock.Create(Of IFoo)()

    Mock.Arrange(Function() foo.Bar.Do("x")).Returns("xit")
    Mock.Arrange(Function() foo.Bar.Baz.Do("y")).Returns("yit")

    ' Act
    Dim actualFooBarDo = foo.Bar.Do("x")
    Dim actualFooBarBazDo = foo.Bar.Baz.Do("y")

    ' Assert
    Assert.AreEqual("xit", actualFooBarDo)
    Assert.AreEqual("yit", actualFooBarBazDo)
End Sub
```

In this arrangement we specify that `foo.Bar.Do` should return `"xit"` when invoked and `foo.Bar.Baz.Do` will always return `"yit"`.

