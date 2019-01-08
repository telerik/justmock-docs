---
title: Call Original
page_title: Call Original | JustMock Documentation
description: Call Original
previous_url: /basic-usage-mock-call-original.html
slug: justmock/basic-usage/mock/call-original
tags: call,original
published: True
position: 1
---

# Call Original

The `CallOriginal` method marks a mocked method/property call that should execute the original method/property implementation. This topic goes through a number of scenarios where the `CallOriginal` method is usefull.

## Assert CallOriginal
For this example, we will use the following class:

  #### __[C#]__

  {{region CallOriginal#FooBase}}
    public class FooBase
    {
        public string GetString(string str)
        {
            return str;
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region CallOriginal#FooBase}}
    Public Class FooBase
    Public Function GetString(ByVal str As String) As String
        Return str
    End Function
End Class
  {{endregion}}

Now, we will set up that a call to a method should call the original implementation for one argument and fake for another argument.

  #### __[C#]__

  {{region CallOriginal#AssertCallOriginal}}
    [TestMethod]
        public void ShouldCallOriginalForSpecificArgs()
        {
            // Arrange
            var foo = Mock.Create<FooBase>();

            Mock.Arrange(() => foo.GetString("x")).CallOriginal();
            Mock.Arrange(() => foo.GetString("y")).Returns("z");

            // Act
            var actualX = string.Empty;
            var actualY = string.Empty;
            actualX = foo.GetString("x");
            actualY = foo.GetString("y");

            var expectedX = "x";
            var expectedY = "z";

            // Assert
            Assert.AreEqual(expectedX, actualX);
            Assert.AreEqual(expectedY, actualY);
        }
  {{endregion}}

  #### __[VB]__

  {{region CallOriginal#AssertCallOriginal}}
    <TestMethod()>
    Public Sub ShouldCallOriginalForSpecificArgs()
        ' Arrange
        Dim foo = Mock.Create(Of FooBase)()

        Mock.Arrange(Function() foo.GetString("x")).CallOriginal()
        Mock.Arrange(Function() foo.GetString("y")).Returns("z")

        ' Act
        Dim actualX = String.Empty
        Dim actualY = String.Empty
        actualX = foo.GetString("x")
        actualY = foo.GetString("y")

        Dim expectedX = "x"
        Dim expectedY = "z"

        ' Assert
        Assert.AreEqual(expectedX, actualX)
        Assert.AreEqual(expectedY, actualY)
    End Sub
  {{endregion}}

With the first arrange we set up that the original method implementation should be called when the method is called with argument "x", while with the second arrange we use the `Returns` behavior to specify that once the method is called with argument "y" it should return "z". Then we act and finally assert the execution.

## CallOriginal with void calls
`CallOriginal` can be used also with methods returning void. Consider the following class:

  #### __[C#]__

  {{region CallOriginal#LogClass}}
    public class Log
    {
        public virtual void Info(string message)
        {
            throw new Exception(message);
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region CallOriginal#LogClass}}
    Public Class Log
    Public Overridable Function Info(ByVal message As String) As String
        Throw New Exception(message)
    End Function
End Class
  {{endregion}}

Let's arrange its `Info` method to be called with its original implementation and verify the call.

  #### __[C#]__

  {{region CallOriginal#AssertCallOriginalForVoid}}
    [TestMethod]
        [ExpectedException(typeof(Exception))]
        public void MockAssertOnCallOriginal()
        {
            // Arrange
            var log = Mock.Create<Log>();
            Mock.Arrange(() => log.Info(Arg.IsAny<string>())).CallOriginal();

            // Act
            log.Info("test");
        }
  {{endregion}}

  #### __[VB]__

  {{region CallOriginal#AssertCallOriginalForVoid}}
    <TestMethod()>
    <ExpectedException(GetType(Exception))>
    Public Sub MockAssertOnCallOriginal()
        ' Arrange
        Dim log = Mock.Create(Of Log)()
        Mock.Arrange(Function() log.Info(Arg.AnyString)).CallOriginal()

        ' Act
        log.Info("test")
    End Sub
  {{endregion}}

The call of the `Info` method throws an exception. To verify this behavior we use the `ExpectedException` attribute provided from Microsoft.VisualStudio.TestTools.UnitTesting namespace (found in Microsoft.VisualStudio.QualityTools.UnitTestFramework assembly).

# See Also


 * [Do Instead]({%slug justmock/basic-usage/mock/do-instead%})

 * [Do Nothing]({%slug justmock/basic-usage/mock/do-nothing%})[](b9461116-b200-4739-aff1-af8458c7095e)

 * [Must Be Called]({%slug justmock/basic-usage/mock/must-be-called%})

 * [Raise]({%slug justmock/basic-usage/mock/raise%})

 * [Raises]({%slug justmock/basic-usage/mock/raises%})

 * [Returns]({%slug justmock/basic-usage/mock/returns%})

 * [Throws]({%slug justmock/basic-usage/mock/throws%})
