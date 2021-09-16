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

The `CallOriginal` method marks a mocked method/property call that should execute the original method/property implementation. 



## CallOriginal with Void Calls

You can use `CallOriginal` on methods and properties. This also includes methods that doesn't return a value. Consider the following class:

#### __[C#] Sample setup__

{{region CallOriginal#LogClass}}

    public class Log
    {
        public virtual void Info(string message)
        {
            throw new Exception(message);
        }
    }
{{endregion}}

#### __[VB] Sample setup__

{{region CallOriginal#LogClass}}

    Public Class Log
        Public Overridable Function Info(ByVal message As String) As String
            Throw New Exception(message)
        End Function
    End Class
{{endregion}}

Let's arrange its `Info` method to be called with its original implementation and verify the call.

#### __[C#] Example 1: Calling the original implementation__

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

#### __[VB] Example 1: Calling the original implementation__

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

The call of the `Info` method throws an exception. To verify this behavior, we use the [ExpectedException attribute](https://docs.microsoft.com/en-us/dotnet/api/microsoft.visualstudio.testtools.unittesting.expectedexceptionattribute?view=visualstudiosdk-2019) provided from the *Microsoft.VisualStudio.TestTools.UnitTesting* namespace (found in *Microsoft.VisualStudio.QualityTools.UnitTestFramework.dll*).

## CallOriginal Depending on Parameter Value

This section shows how you can set up that a call to a method should call the original implementation for one argument and fake for another argument.

For this example, we will use the following sample class:

#### __[C#] Sample setup__

{{region CallOriginal#FooBase}}

    public class FooBase
    {
        public string GetString(string str)
        {
            return str;
        }
    }
{{endregion}}

#### __[VB] Sample setup__

{{region CallOriginal#FooBase}}
    Public Class FooBase
        Public Function GetString(ByVal str As String) As String
            Return str
        End Function
    End Class
{{endregion}}

**Example 2** shows how to implement the different behavior of the mocked object depending on the parameter value the method is invoked with.

#### __[C#] Example 2: Calling original implementation vs customizing method behavior depending on the parameter value__

{{region CallOriginal#AssertCallOriginal}}

    [TestMethod]
    public void ShouldCallOriginalForSpecificArgs()
    {
        // Arrange
        // Create a mock of FooBase.
        var foo = Mock.Create<FooBase>();

        // Set up that the original method implementation should be called when the method is called with argument "x".
        Mock.Arrange(() => foo.GetString("x")).CallOriginal();
        // Set up that once the method is called with argument "y", it should return "z".
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

#### __[VB] Example 2: Calling original implementation vs customizing method behavior depending on the parameter value__

{{region CallOriginal#AssertCallOriginal}}

    <TestMethod()>
    Public Sub ShouldCallOriginalForSpecificArgs()
        ' Arrange
        ' Create a mock of FooBase.
        Dim foo = Mock.Create(Of FooBase)()

        ' Set up that the original method implementation should be called when the method is called with argument "x".
        Mock.Arrange(Function() foo.GetString("x")).CallOriginal()
        ' Set up that once the method is called with argument "y", it should return "z".
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


## See Also

 * [Do Instead]({%slug justmock/basic-usage/mock/do-instead%})
 * [Do Nothing]({%slug justmock/basic-usage/mock/do-nothing%})
 * [Must Be Called]({%slug justmock/basic-usage/mock/must-be-called%})
 * [Raise]({%slug justmock/basic-usage/mock/raise%})
 * [Raises]({%slug justmock/basic-usage/mock/raises%})
 * [Returns]({%slug justmock/basic-usage/mock/returns%})
 * [Throws]({%slug justmock/basic-usage/mock/throws%})
