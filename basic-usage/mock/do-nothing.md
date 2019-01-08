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

The `DoNothing` method is used to arrange that a call to a method or property should be ignored.
We will use the following interface for the examples in this article:

  #### __[C#]__

  {{region DoNothing#IFooSUT}}
    public interface IFoo
    {
        int Bar { get; set; }
        void VoidCall();
    }
  {{endregion}}

  #### __[VB]__

  {{region DoNothing#IFooSUT}}
    Public Interface IFoo
        Property Bar() As Integer
        Function VoidCall()
    End Interface
  {{endregion}}


## Assert Void Call
Before describing the usage of the `DoNothing` method, let's first see an example to illustrate it.

  #### __[C#]__

  {{region DoNothing#AssertDoNothingOnVoidCall}}
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
  {{endregion}}

  #### __[VB]__

  {{region DoNothing#AssertDoNothingOnVoidCall}}
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
  {{endregion}}

In the example, we mark `foo.VoidCall()` with `DoNothing` and `MustBeCalled`. In this way we indicate that a call to `foo.VoidCall` must be ignored, but still the method should be called during the execution of the test. We can achieve the same behavior without marking the call with `DoNothing`. Marking it explicitly improves the code readability. The `DoNothing` method makes no functional difference in the test execution, just a good practice to improve your code. The following two lines are functionally equivalent.

  #### __[C#]__

  {{region DoNothing#Example}}
    [TestMethod]
        public void ShouldShowHowDoNothingIsUsed()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            Mock.Arrange(() => foo.VoidCall()).MustBeCalled();
            Mock.Arrange(() => foo.VoidCall()).DoNothing().MustBeCalled();
        }
  {{endregion}}

  #### __[VB]__

  {{region DoNothing#Example}}
    <TestMethod()>
        Public Sub ShouldShowHowDoNothingIsUsed()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Function() foo.VoidCall()).MustBeCalled()
            Mock.Arrange(Function() foo.VoidCall()).DoNothing().MustBeCalled()
        End Sub
  {{endregion}}

You can use `DoNothing` with non-void calls as well. For the example we will be using the following `Foo` class:

  #### __[C#]__

  {{region DoNothing#FooSUT}}
    public class Foo
    {
        public int Echo(int num)
        {
            return num;
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region DoNothing#FooSUT}}
    Public Class Foo
        Function Echo(num As Integer) As Integer
            Throw New NotImplementedException
        End Function
    End Class
  {{endregion}}


  #### __[C#]__

  {{region DoNothing#ExampleNonVoid}}
    [TestMethod]
        public void DoNothingWithNonPublicCalls()
        {
            // Arrange
            var foo = Mock.Create<Foo>();

            Mock.Arrange(() => foo.Echo(Arg.AnyInt)).DoNothing();

            // Act
            foo.Echo(10);
        }
  {{endregion}}

  #### __[VB]__

  {{region DoNothing#ExampleNonVoid}}
    <TestMethod()>
        Public Sub DoNothingWithNonPublicCalls()
            ' Arrange
            Dim foo = Mock.Create(Of Foo)()
            Mock.Arrange(Function() foo.Echo(Arg.AnyInt)).DoNothing()

            ' Act
            foo.Echo(10)
        End Sub
  {{endregion}}


## Assert DoNothing on property set
`DoNothing` can be also used on a call to a property set.

  #### __[C#]__

  {{region DoNothing#AssertDoNothingOnPropertySet}}
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
  {{endregion}}

  #### __[VB]__

  {{region DoNothing#AssertDoNothingOnPropertySet}}
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
  {{endregion}}


# See Also


 * [Call Original]({%slug justmock/basic-usage/mock/call-original%})

 * [Do Instead]({%slug justmock/basic-usage/mock/do-instead%})[](b9461116-b200-4739-aff1-af8458c7095e)

 * [Must Be Called]({%slug justmock/basic-usage/mock/must-be-called%})

 * [Raise]({%slug justmock/basic-usage/mock/raise%})

 * [Raises]({%slug justmock/basic-usage/mock/raises%})

 * [Returns]({%slug justmock/basic-usage/mock/returns%})

 * [Throws]({%slug justmock/basic-usage/mock/throws%})
