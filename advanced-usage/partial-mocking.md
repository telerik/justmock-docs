---
title: Partial Mocking
page_title: Partial Mocking | JustMock Documentation
description: Partial Mocking
previous_url: /advanced-usage-partial-mocking.html
slug: justmock/advanced-usage/partial-mocking
tags: partial,mocking,method,property
published: True
position: 5
---

# Partial Mocking

Partial mocks allow you to mock some of the methods of a class while keeping the rest intact. Thus, you keep your original object, not a mock object, and you are still able to write your test methods in isolation. 

>note Partial mocking can be performed on both static and instance calls.

> This is an elevated feature. Refer to [this]({%slug justmock/licensing/license-agreement%}#commercial-vs-free-version) topic to learn more about the differences between both the commercial and free versions of Telerik JustMock.

## Prerequisites

In the further examples, we will use the following sample class to test:

#### __[C#] Sample setup__

{{region PartialMocks#Foo}}

    public class Foo
    {
        public static int FooStaticProp { get; set; }

        public int Echo(int arg1)
        {
            return default(int);
        }
    }
{{endregion}}

#### __[VB] Sample setup__

{{region PartialMocks#Foo}}

    Public Class Foo
        Public Shared Property FooStaticProp As Integer
    
        Public Function Echo(arg1 As Integer) As Integer
            Return Nothing
        End Function
    End Class
{{endregion}}


> **Important**
>
> To use partial mocking you first need to go to elevated mode by enabling Telerik JustMock from the menu. Learn how to do that in the [How to Enable/Disable Telerik JustMock](./advanced-usage#how-to-enabledisable-telerik-justmock) topic.

## Partially Mock Instance Calls

**Example 1** shows how you can use a non mocked instance and still arrange the behavior of one of its methods.


#### __[C#] Example 1: Partial mocking of a method__

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

#### __[VB] Example 1: Partial mocking of a method__

{{region PartialMocks#MockInstanceCallPartially}}

    <TestMethod>
    Public Sub ShouldMockInstanceCallPartially()
        ' Arrange
        Dim foo As New Foo()
        Mock.Arrange(Function() foo.Echo(Arg.IsAny(Of Integer)())).Returns(Function(arg As Integer) arg)

        ' Act
        Dim actual As Integer = foo.Echo(10)

        ' Assert
        Assert.AreEqual(10, actual)
    End Sub
{{endregion}}

With partial mocks, we are still able to arrange a method call even when we don't use a mock object. Running the above test would pass.

## Assert Partial Calls

While not using a mock object in partial mocking, you can still assert that a specific call has been made during the execution of a test. To do that, you should pass the lambda of the method that you are expecting.

#### __[C#] Example 2: Assert the behavior of partial mock__

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

#### __[VB] Example 2: Assert the behavior of partial mock__

{{region PartialMocks#AssertInvokedPartialCalls}}

    <TestMethod>
    Public Sub ShouldAssertCallsPartially()
        ' Arrange
        Dim foo As New Foo()

        Mock.Arrange(Function() foo.Echo(Arg.IsAny(Of Integer)())).Returns(Function(arg As Integer) arg)

        ' Act
        foo.Echo(10)
        foo.Echo(10)

        ' Assert
        Mock.Assert(Function() foo.Echo(10), Occurs.Exactly(2))
    End Sub
{{endregion}}

In the Assert section of **Example 2**, we make sure that the `foo.Echo` method has been called exactly two times by passing `10` as an argument. It is not required to enter a specific value for the argument - you can use a [matcher]({%slug justmock/basic-usage/matchers%}) instead.

## Arrange Static Calls

Another common usage of partial mocks is to arrange a call to a ***static method/property*** of a class. **Example 3** demonstrates how you can arrange the return value of a static property.

#### __[C#] Example 3: Partial mocking of static property__ 

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

#### __[VB] Example 3: Partial mocking of static property__

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


## See Also

 * [Matchers]({%slug justmock/basic-usage/matchers%})
 * [Mock Static Classes]({%slug justmock/advanced-usage/static-mocking%})
