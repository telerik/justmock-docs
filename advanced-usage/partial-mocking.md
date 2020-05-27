---
title: Partial Mocking
page_title: Partial Mocking | JustMock Documentation
description: Partial Mocking
previous_url: /advanced-usage-partial-mocking.html
slug: justmock/advanced-usage/partial-mocking
tags: partial,mocking
published: True
position: 5
---

# Partial Mocking

Partial mocks allow you to mock some of the methods of a class while keeping the rest intact. Thus, you keep your original object, not a mock object, and you are still able to write your test methods in isolation. Partial mocking can be performed on both *static* and *instance* calls.

> This is an ElevatedFeature. Refer to [this]({%slug justmock/getting-started/commercial-vs-free-version%}) topic to learn more about the differences between both the commercial and free versions of Telerik JustMock.


In the further examples we will use the following sample class to test:

  #### __[C#]__

  {{region PartialMocks#Foo}}
    public class Foo
    {
        public static int FooStaticProp { get; set; }

        public int Echo(int arg1)
        {
            return default(int);
        }

        public void Execute()
        {
            throw new NotImplementedException();
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region PartialMocks#Foo}}
    Public Class Foo
        Public Shared Property FooStaticProp() As Integer
            Get
                Return m_FooStaticProp
            End Get
            Set(value As Integer)
                m_FooStaticProp = value
            End Set
        End Property
        Private Shared m_FooStaticProp As Integer

        Public Function Echo(arg1 As Integer) As Integer
            Return 0
        End Function

        Public Sub Execute()
            Throw New NotImplementedException()
        End Sub
    End Class
  {{endregion}}


> **Important**
>
> To use partial mocking you first need to go to elevated mode by enabling TelerikJustMock from the menu. [How to Enable/Disable](./advanced-usage#how-to-enabledisable-telerikjustmock)

## Partially Mock Instance Calls
In the following example we use a non mocked instance, but we call a method which is arranged.

  #### __[C#]__

  {{region PartialMocks#MockInstanceCallPartially}}
    [TestMethod]
    public void ShouldMockInstanceCallPartially()
    {
        // Arrange
        Foo foo = new Foo();
        Mock.Arrange(() => foo.Echo(Arg.IsAny<int>())).Returns((int arg) => arg);

        // Act
        int actual = foo.Echo(10);

        // Assert
        Assert.AreEqual(10, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region PartialMocks#MockInstanceCallPartially}}
    <TestMethod>
    Public Sub ShouldMockInstanceCallPartially()
        ' Arrange
        Dim foo As New Foo()
        Mock.Arrange(Function() foo.Echo(Arg.IsAny(Of Integer)())).Returns(Function(arg__1 As Integer) arg__1)

        ' Act
        Dim actual As Integer = foo.Echo(10)

        ' Assert
        Assert.AreEqual(10, actual)
    End Sub
  {{endregion}}

With partial mocks we are still able to arrange a method call even when we don't use a mock object. Running the above test would pass.

## Assert Partial Calls
While not using a mock object in partial mocking you can still assert that a specific call has been made during the executing of a test. In order to do that we need to pass the lambda of the method that we are expecting.

  #### __[C#]__

  {{region PartialMocks#AssertInvokedPartialCalls}}
    [TestMethod]
    public void ShouldAssertCallsPartially()
    {
        // Arrange
        Foo foo = new Foo();

        Mock.Arrange(() => foo.Echo(Arg.IsAny<int>())).Returns((int arg) => arg);

        // Act
        foo.Echo(10);
        foo.Echo(10);

        // Assert
        Mock.Assert(() => foo.Echo(10), Occurs.Exactly(2));
    }
  {{endregion}}

  #### __[VB]__

  {{region PartialMocks#AssertInvokedPartialCalls}}
    <TestMethod>
    Public Sub ShouldAssertCallsPartially()
        ' Arrange
        Dim foo As New Foo()

        Mock.Arrange(Function() foo.Echo(Arg.IsAny(Of Integer)())).Returns(Function(arg__1 As Integer) arg__1)

        ' Act
        foo.Echo(10)
        foo.Echo(10)

        ' Assert
        Mock.Assert(Function() foo.Echo(10), Occurs.Exactly(2))
    End Sub
  {{endregion}}

In the Assert section we make sure that the `foo.Echo` method has been called exactly two times by passing `10` as an argument. It is not required to enter a specific argument - you can use a [matcher]({%slug justmock/basic-usage/matchers%}) instead.

## Arrange Static Calls
Another common usage of partial mocks is to arrange a call to a *static method/property* of a class.

  #### __[C#]__

  {{region PartialMocks#ArrangeStaticCallsDirectly}}
    [TestMethod]
    public void ShouldArrangeStaticCallPartially()
    {
        // Arrange
        Mock.Arrange(() => Foo.FooStaticProp).Returns(10);

        // Act
        int actual = Foo.FooStaticProp;

        // Assert
        Assert.AreEqual(10, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region PartialMocks#ArrangeStaticCallsDirectly}}
    <TestMethod>
    Public Sub ShouldArrangeStaticCallPartially()
        ' Arrange
        Mock.Arrange(Function() Foo.FooStaticProp).Returns(10)

        ' Act
        Dim actual As Integer = Foo.FooStaticProp

        ' Assert
        Assert.AreEqual(10, actual)
    End Sub
  {{endregion}}

Here we arrange that the static `Foo.FooStaticProp` property should return `10`.

## See Also

 * [Matcher]({%slug justmock/basic-usage/matchers%})
