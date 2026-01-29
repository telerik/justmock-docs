---
title: Mocking Inside WinRT
page_title: Mocking Inside WinRT | JustMock Documentation
description: Mocking Inside WinRT
previous_url: /advanced-usage-mocking-inside-winrt.html
slug: justmock/advanced-usage/mocking-inside-winrt
tags: mocking,inside,winrt
published: True
position: 11
---

# Mocking Inside WinRT

You can use __Telerik® JustMock__ in Windows Runtime to perform elevated or non-elevated testing.

Having an Unit Test Library (Windows Store apps) project, you need to refer the Telerik.JustMock assembly(ies). Then you will be able to mock everything as in standard .NET project.

## Non-Elevated Examples

The next examples can be applied with both JustMock Lite and JustMock commercial ([Commercial vs Free Version]({%slug justmock/licensing/commercial-vs-free-version%}). For them, we will use the following system under test:

```C#
public interface IFoo
{
    void DoSomething();

    string ReturnMyString(string arg);
}
```
```VB
Public Interface IFoo
    Sub DoSomething()

    Function ReturnMyString(arg As String) As String
End Interface
```

### Mocking Void Method from an Interface

To mock an interface under Windows Runtime, we use `Mock.Create<T>()`:           
          
```C#
[TestMethod]
public void DoSomething_MustBeCalled()
{
    // Arrange
    var myMock = Mock.Create<IFoo>();

    Mock.Arrange(() => myMock.DoSomething()).DoNothing().MustBeCalled();

    // Act
    myMock.DoSomething();

    // Assert
    Mock.Assert(myMock);
}
```
```VB
<TestMethod> _
Public Sub DoSomething_MustBeCalled()
    ' Arrange
    Dim myMock = Mock.Create(Of IFoo)()

    Mock.Arrange(Sub() myMock.DoSomething()).DoNothing().MustBeCalled()

    ' Act
    myMock.DoSomething()

    ' Assert
    Mock.Assert(myMock)
End Sub
```


          
In the above test, we create a mocked instance of the *IFoo* interface. Then, we arrange that, *DoSomething()* must be called during the test execution and it should do nothing. After acting on the system under test, we assert the expected behavior.

### Mocking Functions from an Interface

Here we perform a mocking of an interface `string` method:

```C#
[TestMethod]
public void ReturnMyString_ShouldReturnExpectedAndMustBeCalled()
{
    // Arrange
    var myMock = Mock.Create<IFoo>();

    Mock.Arrange(() => myMock.ReturnMyString(Arg.AnyString)).Returns("Test").MustBeCalled();

    // Act
    var actual = myMock.ReturnMyString("Telerik");

    // Assert
    Assert.AreEqual("Test", actual);
}
```
```VB
<TestMethod>
Public Sub ReturnMyString_ShouldReturnExpectedAndMustBeCalled()
    ' Arrange
    Dim myMock = Mock.Create(Of IFoo)()

    Mock.Arrange(Function() myMock.ReturnMyString(Arg.AnyString)).Returns("Test").MustBeCalled()

    ' Act
    Dim actual = myMock.ReturnMyString("Telerik")

    ' Assert
    Assert.AreEqual("Test", actual)
End Sub
```

Again, we create a mocked instance of the *IFoo* interface. Then, we arrange that *ReturnMyString()* must be called during the test execution and it should return "Test". Note that we ignore the arguments by expecting any string to be passed. After acting on the system under test, we assert that the actual and the expected return value are the same.
        
## Elevated Examples

> This feature is available only in the commercial version of Telerik JustMock. Refer to [this]({%slug justmock/licensing/commercial-vs-free-version%}) topic to learn more about the differences between both the commercial and free versions of Telerik JustMock.

For the examples, we will use the following system under test:

```C#
static class FooStatic
{
    public static string StatProp { get; set; }

    public static void DoSomething()
    {
        throw new NotImplementedException();
    }
}
```
```VB
NotInheritable Class FooStatic
    Private Sub New()
    End Sub
    Public Shared Property StatProp() As String
        Get
            Return m_StatProp
        End Get
        Set(value As String)
            m_StatProp = Value
        End Set
    End Property
    Private Shared m_StatProp As String

    Public Shared Sub DoSomething()
        Throw New NotImplementedException()
    End Sub
End Class
```


> **Important**
>
> You first need to go to elevated mode by enabling JustMock from the menu. Learn how to do that in the [How to Enable/Disable Telerik JustMock]({%slug justmock/advanced-usage%}#how-to-enable-disable-telerik-justmock) topic.

### Mocking Static Property

To mock a static property inside Windows Runtime you can refer to the following:           
          
```C#
[TestMethod]
public void StatProp_ShouldReturnExpectedAndMustBeCalled()
{
    // Arrange
    Mock.Arrange(() => FooStatic.StatProp).Returns("Test").MustBeCalled();

    // Act
    var actual = FooStatic.StatProp;

    // Assert
    Assert.AreEqual("Test", actual);
}
```
```VB
<TestMethod>
Public Sub StatProp_ShouldReturnExpectedAndMustBeCalled()
    ' Arrange
    Mock.Arrange(Function() FooStatic.StatProp).Returns("Test").MustBeCalled()

    ' Act
    Dim actual = FooStatic.StatProp

    ' Assert
    Assert.AreEqual("Test", actual)
End Sub
```

We directly arrange that, *StatProp* must be called during the test execution and it should return "Test". Then, the only thing needed is to act on the system under test and assert the results.

### Mocking Static Method

To mock a static method you use similar syntax as when mocking properties. However, as we are mocking a void method we arrange it to do nothing this time:
          
```C#
[TestMethod]
public void DoSomething_ShouldDoNothingAndMustBeCalled()
{
    // Arrange
    Mock.Arrange(() => FooStatic.DoSomething()).DoNothing().MustBeCalled();

    // Act
    FooStatic.DoSomething();

    // Assert
    Mock.Assert(() => FooStatic.DoSomething());
}
```
```VB
<TestMethod>
Public Sub DoSomething_ShouldDoNothingAndMustBeCalled()
    ' Arrange
    Mock.Arrange(Sub() FooStatic.DoSomething()).DoNothing().MustBeCalled()

    ' Act
    FooStatic.DoSomething()

    ' Assert
    Mock.Assert(Sub() FooStatic.DoSomething())
End Sub
```


          
Further, you can also specify expected method arguments, as described in the [Matchers article]({%slug justmock/basic-usage/matchers%}).
