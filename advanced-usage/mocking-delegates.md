---
title: Delegates
page_title: Mocking Delegates | JustMock Documentation
description: Mocking Delegates
previous_url: /advanced-usage-mocking-delegates.html
slug: justmock/advanced-usage/mocking-delegates
tags: mocking,delegates
published: True
position: 9
---

# Mocking Delegates

With __Telerik® JustMock__ you can mock delegates and use all mock capabilities on them. For example, you can assert against their invocation, arrange certain expectations and then pass them to the system under test. This topic explains how you can use that functionality.

## Sample Setup

For the examples in this topic, let's assume we have the following `Foo` class:

#### Sample setup
```C#
public class Foo
{
    public Func<int, int> FuncDelegate { get; set; }
            
    public int GetInteger(int toThisInt)
    {
        return FuncDelegate(toThisInt);
    }
}
```
```VB
Public Class Foo
    Public Property FuncDelegate() As Func(Of Integer, Integer)
        Get
            Return m_FuncDelegate
        End Get
        Set(value As Func(Of Integer, Integer))
            m_FuncDelegate = Value
        End Set
    End Property
    Private m_FuncDelegate As Func(Of Integer, Integer)
    
    Public Function GetInteger(toThisInt As Integer) As Integer
        Return FuncDelegate(toThisInt)
    End Function
End Class
```

## Asserting Delegate Occurrences

You can use JustMock to verify that a delegate is actually called, as demonstrated in **Example 1**.

#### Example 1: Test that a delegate inside a method is actually called
```C#
[TestMethod]
public void ShouldArrangeOccurrenceExpectation()
{
    // Arrange
    var delegateMock = Mock.Create<Func<int, int>>();
    
    Mock.Arrange(() => delegateMock(Arg.AnyInt)).MustBeCalled();
    
    // Act
    var foo = new Foo();
    foo.FuncDelegate = delegateMock;
    var actual = foo.GetInteger(123);
    
    // Assert
    Mock.Assert(delegateMock);
}
```
```VB
<TestMethod>
Public Sub ShouldArrangeOccurrenceExpectation()
    ' Arrange
    Dim delegateMock = Mock.Create(Of Func(Of Integer, Integer))()
    
    Mock.Arrange(Function() delegateMock(Arg.AnyInt)).MustBeCalled()
    
    ' Act
    Dim foo = New Foo()
    foo.FuncDelegate = delegateMock
    Dim actual = foo.GetInteger(123)
    
    ' Assert
    Mock.Assert(delegateMock)
End Sub
```

The test from **Example 1** asserts that, during the execution of the test method, `Func<int, int>` is called at least once no matter the integer argument.

>note You can also verify the order and the number of invocations. For more details about that functionality, check the [Asserting Occurrence]({%slug justmock/basic-usage/asserting-occurrence%}) topic.

## Arranging Return Expectation

To test the `GetInteger` method in different scenarios, we may need to arrange a specific behavior to the `FuncDelegate`. This can be done like this:

#### Example 2: Change the return value of a delegate
```C#
[TestMethod]
public void ShouldArrangeReturnExpectation()
{
    // Arrange
    var delegateMock = Mock.Create<Func<int, int>>();
    
    // Arrange the `Func<int, int>` delegate to return 20 whenever it is called with 10 as an argument.
    Mock.Arrange(() => delegateMock(10)).Returns(20);
    
    // Act
    var foo = new Foo();
    foo.FuncDelegate = delegateMock;
    var actual = foo.GetInteger(10);
    
    // Assert
    Assert.AreEqual(20, actual);
}
```
```VB
<TestMethod>
Public Sub ShouldArrangeReturnExpectation()
    ' Arrange
    Dim delegateMock = Mock.Create(Of Func(Of Integer, Integer))()
    
    ' Arrange the `Func<int, int>` delegate to return 20 whenever it is called with 10 as an argument.
    Mock.Arrange(Function() delegateMock(10)).Returns(20)
    
    ' Act
    Dim foo = New Foo()
    foo.FuncDelegate = delegateMock
    Dim actual = foo.GetInteger(10)
    
    ' Assert
    Assert.AreEqual(20, actual)
End Sub
```


## Passing Delegate Mocks in the System Under Test

For the examples in this section, the following `DataRepository` class is used:

#### Sample setup
```C#
public class DataRepository
{
    public string GetCurrentUserId(Func<string> callback)
    {
        return callback();
    }
    
    public void ApproveCredentials(Action<int> callback)
    {
        // Some logic here...
    
        callback(1);
    }
}
```
```VB
Public Class DataRepository
    Public Function GetCurrentUserId(callback As Func(Of String)) As String
        Return callback()
    End Function
    
    Public Sub ApproveCredentials(callback As Action(Of Integer))
        ' Some logic here...
    
        callback(1)
    End Sub
End Class
```

The first test method will explain how to pass a prearranged delegate mock in the above system under test:

#### Example 3: Passing a delegate mock as a method argument
```C#
[TestMethod]
public void ShouldPassPrearrangedDelegateMockAsArgument()
{
    // Arrange
    var delegateMock = Mock.Create<Func<string>>();
    
    Mock.Arrange(() => delegateMock()).Returns("Success");
    
    // Act
    var testInstance = new DataRepository();
    var actual = testInstance.GetCurrentUserId(delegateMock);
    
    // Assert
    Assert.AreEqual("Success", actual);
}
```
```VB
<TestMethod>
Public Sub ShouldPassPrearrangedDelegateMockAsArgument()
    ' Arrange
    Dim delegateMock = Mock.Create(Of Func(Of String))()
    
    Mock.Arrange(Function() delegateMock()).Returns("Success")
    
    ' Act
    Dim testInstance = New DataRepository()
    Dim actual = testInstance.GetCurrentUserId(delegateMock)
    
    ' Assert
    Assert.AreEqual("Success", actual)
End Sub
```

The second test method demonstrates how to pass a delegate mock in the above system under test and assert its occurrence instead of invoking its original logic during the test execution.

#### Example 4: Passing a delegate mock as a method argument and asserting its occurrence
```C#
[TestMethod]
public void ShouldPassDelegateMockAsArgumentAndAssertItsOccurrence()
{
    bool isCalled = false;
    
    // Arrange
    var delegateMock = Mock.Create<Action<int>>();
    
    // Arrange that instead of invoking the original implementation of the delegate, 
    // it should only set isCalled to true when invoked.
    Mock.Arrange(() => delegateMock(Arg.AnyInt)).DoInstead(() => isCalled = true);
    
    // Act
    var testInstance = new DataRepository();
    testInstance.ApproveCredentials(delegateMock);
    
    // Assert
    Assert.IsTrue(isCalled);
}
```
```VB
<TestMethod>
Public Sub ShouldPassDelegateMockAsArgumentAndAssertItsOccurrence()
    Dim isCalled As Boolean = False
    
    ' Arrange
    Dim delegateMock = Mock.Create(Of Action(Of Integer))()

    ' Arrange that instead of invoking the original implementation of the delegate, 
    ' it should only set isCalled to true when invoked.    
    Mock.Arrange(Sub() delegateMock(Arg.AnyInt)).DoInstead(Sub() isCalled = True)
    
    ' Act
    Dim testInstance = New DataRepository()
    testInstance.ApproveCredentials(delegateMock)
    
    ' Assert
    Assert.IsTrue(isCalled)
End Sub
```


## See Also

* [Asserting Occurrence]({%slug justmock/basic-usage/asserting-occurrence%})

* [DoInstead]({%slug justmock/basic-usage/mock/do-instead%})

* [Returns]({%slug justmock/basic-usage/mock/returns%})
