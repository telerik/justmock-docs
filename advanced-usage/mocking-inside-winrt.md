---
title: Inside WinRT
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

The next examples can be applied with both JustMock Lite and JustMock commercial ([Commercial vs Free Version]({%slug justmock/getting-started/commercial-vs-free-version%})). For them, we will use the following system under test:

  #### __[C#]__

  {{region MockingInWinRT#IFoo}}
    public interface IFoo
    {
        void DoSomething();

        string ReturnMyString(string arg);
    }
  {{endregion}}

  #### __[VB]__

  {{region MockingInWinRT#IFoo}}
    Public Interface IFoo
        Sub DoSomething()

        Function ReturnMyString(arg As String) As String
    End Interface
  {{endregion}}

### Mocking Void Method from an Interface

To mock an interface under Windows Runtime, we use `Mock.Create<T>()`:           
          
  #### __[C#]__

  {{region MockingInWinRT#NonElevatedTest1}}
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
  {{endregion}}

  #### __[VB]__

  {{region MockingInWinRT#NonElevatedTest1}}
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
  {{endregion}}


          
In the above test, we create a mocked instance of the *IFoo* interface. Then, we arrange that, *DoSomething()* must be called during the test execution and it should do nothing. After acting on the system under test, we assert the expected behavior.

### Mocking Functions from an Interface

Here we perform a mocking of an interface `string` method:

  #### __[C#]__

  {{region MockingInWinRT#NonElevatedTest2}}
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
  {{endregion}}

  #### __[VB]__

  {{region MockingInWinRT#NonElevatedTest2}}
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
  {{endregion}}

Again, we create a mocked instance of the *IFoo* interface. Then, we arrange that *ReturnMyString()* must be called during the test execution and it should return "Test". Note that we ignore the arguments by expecting any string to be passed. After acting on the system under test, we assert that the actual and the expected return value are the same.
        
## Elevated Examples

> This feature is available only in the commercial version of Telerik JustMock. Refer to [this]({%slug justmock/getting-started/commercial-vs-free-version%}) topic to learn more about the differences between both the commercial and free versions of Telerik JustMock.

For the examples, we will use the following system under test:

  #### __[C#]__

  {{region MockingInWinRT#FooStatic}}
    static class FooStatic
    {
        public static string StatProp { get; set; }

        public static void DoSomething()
        {
            throw new NotImplementedException();
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region MockingInWinRT#FooStatic}}
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
  {{endregion}}


> **Important**
>
> You first need to go to elevated mode by enabling JustMock from the menu. Learn how to do that in the [How to Enable/Disable Telerik JustMock](./advanced-usage#how-to-enabledisable-telerik-justmock) topic.

### Mocking Static Property

To mock a static property inside Windows Runtime you can refer to the following:           
          
  #### __[C#]__

  {{region MockingInWinRT#ElevatedTest1}}
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
  {{endregion}}

  #### __[VB]__

  {{region MockingInWinRT#ElevatedTest1}}
    <TestMethod>
    Public Sub StatProp_ShouldReturnExpectedAndMustBeCalled()
        ' Arrange
        Mock.Arrange(Function() FooStatic.StatProp).Returns("Test").MustBeCalled()

        ' Act
        Dim actual = FooStatic.StatProp

        ' Assert
        Assert.AreEqual("Test", actual)
    End Sub
  {{endregion}}

We directly arrange that, *StatProp* must be called during the test execution and it should return "Test". Then, the only thing needed is to act on the system under test and assert the results.

### Mocking Static Method

To mock a static method you use similar syntax as when mocking properties. However, as we are mocking a void method we arrange it to do nothing this time:
          
  #### __[C#]__

  {{region MockingInWinRT#ElevatedTest2}}
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
  {{endregion}}

  #### __[VB]__

  {{region MockingInWinRT#ElevatedTest2}}
    <TestMethod>
    Public Sub DoSomething_ShouldDoNothingAndMustBeCalled()
        ' Arrange
        Mock.Arrange(Sub() FooStatic.DoSomething()).DoNothing().MustBeCalled()

        ' Act
        FooStatic.DoSomething()

        ' Assert
        Mock.Assert(Sub() FooStatic.DoSomething())
    End Sub
  {{endregion}}


          
Further, you can also specify expected method arguments, as described in the [Matchers article]({%slug justmock/basic-usage/matchers%}).
