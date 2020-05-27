---
title: Must Be Called
page_title: Must Be Called | JustMock Documentation
description: Must Be Called
previous_url: /basic-usage-mock-must-be-called.html
slug: justmock/basic-usage/mock/must-be-called
tags: must,be,called
published: True
position: 4
---

# Must Be Called

The `MustBeCalled` method is used to assert that a call to a given method or property is made during the execution of a test.

In this article you will find various examples of the `MustBeCalled` usage, for which we will be using the following class:

  #### __[C#]__

  {{region MustBeCalled#FooSUT}}
    public class Foo
    {
        public void Execute()
        {
        }

        public int Execute(int str)
        {
            return str;
        }

        public int Echo(int a)
        {
            return 5;
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region MustBeCalled#FooSUT}}
    Public Class Foo
        Public Sub Execute()
        End Sub

        Public Function Execute(ByVal str As Integer)
            Return str
        End Function

        Public Shared Function Echo(num As Integer) As Integer
            Throw New NotImplementedException
        End Function
    End Class
  {{endregion}}


## Assert All Calls Marked as Assertable

Let's arrange that a call must be made and then assert that. Only calls marked as assertable, i.e. with `MustBeCalled`, are verified.

  #### __[C#]__

  {{region MustBeCalled#OnlyTheOnesMarkedAsAssertable}}
    [TestMethod]
    public void ShouldAssertAllCallsMarkedAsAssertable()
    {
        // Arrange
        var foo = Mock.Create<Foo>();

        Mock.Arrange(() => foo.Echo(1)).Returns(1).MustBeCalled();
        Mock.Arrange(() => foo.Echo(2)).Returns(2);

        // Act
        var actual1 = 0;
        var actual2 = 0;
        actual1 = foo.Echo(1);
        actual2 = foo.Echo(2);

        // Assert
        Assert.AreEqual(1, actual1);
        Assert.AreEqual(2, actual2);
        Mock.Assert(foo);
    }
  {{endregion}}

  #### __[VB]__

  {{region MustBeCalled#OnlyTheOnesMarkedAsAssertable}}
    <TestMethod()>
    Public Sub ShouldAssertAllCallsMarkedAsAssertable()
        ' Arrange
        Dim foo = Mock.Create(Of Foo)()

        Mock.Arrange(Function() foo.Echo(1)).Returns(1).MustBeCalled()
        Mock.Arrange(Function() foo.Echo(2)).Returns(2)

        ' Act
        Dim actual1 = 0
        Dim actual2 = 0
        actual1 = foo.Echo(1)
        actual2 = foo.Echo(2)

        ' Assert
        Assert.AreEqual(1, actual1)
        Assert.AreEqual(2, actual2)
        Mock.Assert(foo)
    End Sub
  {{endregion}}

Here we ensure that `foo.Echo` is called with argument 1, however, the call with argument 2 is not verified.

## Throwing Exception When MustBeCalled Setup Is Never Invoked

When `MustBeCalled` setup for some method is never invoked an exception is thrown. In the following example with `foo.Execute()).MustBeCalled();` we specify that `foo.Execute` is required to be called but this is never done.

> In the next example is used NUnit Testing Framework.


  #### __[C#]__

  {{region MustBeCalled#FailsOnNotCalling}}
    [TestMethod]
    public void ShouldThrowExceptionWhenMustBeCalledSetupIsNeverInvoked()
    {
        // Arrange
        var foo = new Foo();

        Mock.Arrange(() => foo.Execute()).MustBeCalled();

        // Assert
        Assert.Throws<AssertFailedException>(() => Mock.Assert(foo));
    }
  {{endregion}}

  #### __[VB]__

  {{region MustBeCalled#FailsOnNotCalling}}
    <TestMethod()>
    Public Sub ShouldThrowExceptionWhenMustBeCalledSetupIsNeverInvoked()
        ' Arrange
        Dim foo = New Foo()

        Mock.Arrange(Sub() foo.Execute()).MustBeCalled()

        ' Assert
        Assert.Throws(Of AssertFailedException)(Sub() Mock.Assert(foo))
    End Sub
  {{endregion}}

As a result, when verifying the `foo` object, `MockAssertion` exception is thrown as `foo.Execute` is never actually called.

## Using MustBeCalled for Property Set

Let`s assume we have the following interface:

  #### __[C#]__

  {{region MustBeCalled#IFooSUT}}
    public interface IFoo
    {
        int Value { get; set; }
    }
  {{endregion}}

  #### __[VB]__

  {{region MustBeCalled#IFooSUT}}
    Public Interface IFoo
        Property Value() As Integer
    End Interface
  {{endregion}}

You can use `MustBeCalled` when arranging property set. Here is an example:

  #### __[C#]__

  {{region MustBeCalled#OnPropertySet}}
    [TestMethod]
    public void ShouldArrangeMustBeCalledForPropertySet()
    {
        // Arrange
        var foo = Mock.Create<IFoo>();
        Mock.ArrangeSet(() => { foo.Value = 1; })
            .DoNothing()
            .MustBeCalled();

        // Assert
        Assert.Throws<AssertFailedException>(() => Mock.Assert(foo));

        // Act
        foo.Value = 1;

        // Assert
        Mock.Assert(foo);
    }
  {{endregion}}

  #### __[VB]__

  {{region MustBeCalled#OnPropertySet}}
    <TestMethod()>
    Public Sub ShouldArrangeMustBeCalledForPropertySet()
        ' Arrange
        Dim foo = Mock.Create(Of IFoo)()

        Mock.ArrangeSet(Sub() foo.Value = 1).
                    DoNothing().
                    MustBeCalled()

        ' Assert
        Assert.Throws(Of AssertFailedException)(Sub() Mock.Assert(foo))

        ' Act
        foo.Value = 1

        ' Assert
        Mock.Assert(foo)
    End Sub
  {{endregion}}

We use `DoNothing` to ignore the actual implementation when `foo.Value` is set to `1` and specify that it must be set to exactly `1` in our test. Before acting with `foo.Value = 1;` an exception of type `MockAssertion` would be thrown when asserting `foo`. After acting we verify that `foo.Value` was previously set to `1`.

## Using MustBeCalled When Ignoring Arguments

You can use `MustBeCalled` in conjunction with `IgnoreArguments`. Here is an example:

  #### __[C#]__

  {{region MustBeCalled#IgnoreArguments}}
    [TestMethod]
    public void ShouldCombineMustBeCalledWithIgnoreArguments()
    {
        // Arrange
        var foo = Mock.Create<Foo>();

        Mock.Arrange(() => foo.Execute(0)).IgnoreArguments().MustBeCalled();

        // Act
        foo.Execute(10);

        // Assert
        Mock.Assert(foo);
    }
  {{endregion}}

  #### __[VB]__

  {{region MustBeCalled#IgnoreArguments}}
    <TestMethod()>
    Public Sub ShouldCombineMustBeCalledWithIgnoreArguments()
        ' Arrange
        Dim foo = Mock.Create(Of Foo)()

        Mock.Arrange(Function() foo.Execute(0)).IgnoreArguments().MustBeCalled()

        ' Act
        foo.Execute(10)

        ' Assert
        Mock.Assert(foo)
    End Sub
  {{endregion}}

We use `IgnoreArguments()` to ignore the arguments passed to `foo.Execute` method and specify that it must be called. Our acting is by `foo.Execute(10);`. Finally, we verify that the method is actually called with some or other argument.

## See Also


 * [Call Original]({%slug justmock/basic-usage/mock/call-original%})

 * [Do Instead]({%slug justmock/basic-usage/mock/do-instead%})

 * [Do Nothing]({%slug justmock/basic-usage/mock/do-nothing%})[](b9461116-b200-4739-aff1-af8458c7095e)

 * [Raise]({%slug justmock/basic-usage/mock/raise%})

 * [Raises]({%slug justmock/basic-usage/mock/raises%})

 * [Returns]({%slug justmock/basic-usage/mock/returns%})

 * [Throws]({%slug justmock/basic-usage/mock/throws%})
