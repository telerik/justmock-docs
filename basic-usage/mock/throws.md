---
title: Throws
page_title: Throws | JustMock Documentation
description: Throws
previous_url: /basic-usage-mock-throws.html
slug: justmock/basic-usage/mock/throws
tags: throws
published: True
position: 8
---

# Throws

The `Throws` method is used to throw an exception when a given call is made. This topic goes through a number of scenarios where the `Throws` method is useful. 

Here is the system under test for these examples:

  #### __[C#]__

  {{region Throws#IFooSUT}}
    public interface IFoo
    {
        string Execute(string myStr);
    }
  {{endregion}}

  #### __[VB]__

  {{region Throws#IFooSUT}}
    Public Interface IFoo
        Function Execute(ByVal str As String)
    End Interface
  {{endregion}}


## Throw Exception on Method Call

Change a method behavior to throw an exception once it is called.

  #### __[C#]__

  {{region Throws#ThrowExceptionOnMethodCall}}
    [TestMethod]
    [ExpectedException(typeof(ArgumentException))]
    public void ShouldThrowExceptionOnMethodCall()
    {
        // Arrange
        var foo = Mock.Create<IFoo>();

        Mock.Arrange(() => foo.Execute(string.Empty)).Throws<ArgumentException>();

        // Act
        foo.Execute(string.Empty);
    }
  {{endregion}}

  #### __[VB]__

  {{region Throws#ThrowExceptionOnMethodCall}}
    <TestMethod()>
    <ExpectedException(GetType(ArgumentException))>
    Public Sub ShouldThrowExceptionOnMethodCall()
        ' Arrange
        Dim foo = Mock.Create(Of IFoo)()

        Mock.Arrange(Function() foo.Execute(String.Empty)).Throws(Of ArgumentException)()

        ' Act
        foo.Execute(String.Empty)
    End Sub
  {{endregion}}

The assert step is done via the `ExpectedException` attribute, where we explicitly specify that an exception of type `ArgumentException` must be thrown during the execution of the test.

## Throw Exception with Arguments on Method Call

Change a method behavior to throw an exception once it is called and pass arguments to the exception.

  #### __[C#]__

  {{region Throws#ExceptionWithArgumentsOnMethodCall}}
    [TestMethod]
    [ExpectedException(typeof(ArgumentException))]
    public void ShouldThrowExceptionWithArgumentsOnMethodCall()
    {
        // Arrange
        var foo = Mock.Create<IFoo>();

        Mock.Arrange(() => foo.Execute(string.Empty)).Throws<ArgumentException>("Argument shouldn't be empty.");

        // Act
        foo.Execute(string.Empty);
    }
  {{endregion}}

  #### __[VB]__

  {{region Throws#ExceptionWithArgumentsOnMethodCall}}
    <TestMethod()>
    <ExpectedException(GetType(ArgumentException))>
    Public Sub ShouldThrowExceptionWithArgumentsOnMethodCall()
        ' Arrange
        Dim foo = Mock.Create(Of IFoo)()

        Mock.Arrange(Function() foo.Execute(String.Empty)).Throws(Of ArgumentException)("Argument shouldn't be empty.")

        ' Act
        foo.Execute(String.Empty)
    End Sub
  {{endregion}}

The assert step is done via the `ExpectedException` attribute, where we explicitly specify that an exception of type `ArgumentException` must be thrown during the execution of the test. Calling `foo.Execute` with empty string will result in throwing an exception and passing "Argument shouldn't be empty." to it.

## See Also

 * [Call Original]({%slug justmock/basic-usage/mock/call-original%})

 * [Do Instead]({%slug justmock/basic-usage/mock/do-instead%})

 * [Do Nothing]({%slug justmock/basic-usage/mock/do-nothing%})[](b9461116-b200-4739-aff1-af8458c7095e)

 * [Must Be Called]({%slug justmock/basic-usage/mock/must-be-called%})

 * [Raise]({%slug justmock/basic-usage/mock/raise%})

 * [Raises]({%slug justmock/basic-usage/mock/raises%})

 * [Returns]({%slug justmock/basic-usage/mock/returns%})
