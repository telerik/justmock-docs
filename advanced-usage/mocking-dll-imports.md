---
title: Mocking DLL Imports
page_title: Mocking DLL Imports | JustMock Documentation
description: Mocking DLL Imports
previous_url: /advanced-usage-mocking-dll-imports.html
slug: justmock/advanced-usage/mocking-dll-imports
tags: mocking,dll,imports
published: True
position: 10
---

# Mocking DLL Imports

In elevated mode, you can use __Telerik® JustMock__ to mock imported functions (decorated with the `[DLLImport()]` attribute).

> This feature is available only in the commercial version of Telerik JustMock. Refer to [this]({%slug justmock/getting-started/commercial-vs-free-version%}) topic to learn more about the differences between both the commercial and free versions of Telerik JustMock.


In the next examples we will use the following sample class to test:

  #### __[C#]__

  {{region DLLImport#SUT}}
    public class Foo
    {
        [DllImport("Kernel32.dll")]
        public static extern int GetCurrentProcessId();

        [DllImport("user32.dll", CharSet = CharSet.Auto)]
        public static extern int MessageBox
           (IntPtr hWnd, String text, String caption, uint type);

        [DllImport("Kernel32.dll")]
        private static extern bool IsWow64Process();

        public string FormatCurrentProcessId()
        {
            var myPId = GetCurrentProcessId();
            string returnValue = string.Format("The current process ID is {0}", myPId);
            return returnValue;
        }

        public void Start()
        {
            var msgBoxRet = MessageBox(new IntPtr(0), "Process Starts Now!", "Message Dialog", 0);

            throw new NotImplementedException();
        }

        public string Is64BitProcessMessage()
        {
            bool is64Bit = IsWow64Process();

            if (is64Bit)
            {
                return string.Format("This is a 64 bit process!");
            }
            return string.Format("This is a 32 bit process!");
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region DLLImport#SUT}}
    Public Class Foo
        <DllImport("Kernel32.dll")>
        Public Shared Function GetCurrentProcessId() As Integer
        End Function

        <DllImport("user32.dll", CharSet:=CharSet.Auto)>
        Public Shared Function MessageBox(hWnd As IntPtr, text As [String], caption As [String], type As UInteger) As Integer
        End Function

        <DllImport("Kernel32.dll")>
        Private Shared Function IsWow64Process() As Boolean
        End Function

        Public Function FormatCurrentProcessId() As String
            Dim myPId = GetCurrentProcessId()
            Dim returnValue As String = String.Format("The current process ID is {0}", myPId)
            Return returnValue
        End Function

        Public Sub Start()
            Dim msgBoxRet = MessageBox(New IntPtr(0), "Process Starts Now!", "Message Dialog", 0)

            Throw New NotImplementedException()
        End Sub

        Public Function Is64BitProcessMessage() As String
            Dim is64Bit As Boolean = IsWow64Process()

            If is64Bit Then
                Return String.Format("This is a 64 bit process!")
            End If
            Return String.Format("This is a 32 bit process!")
        End Function
    End Class
  {{endregion}}


> To mock DLL imported members and types you first need to go to elevated mode by enabling JustMock from the menu. Learn how to do that in the [How to Enable/Disable Telerik JustMock](./advanced-usage#how-to-enabledisable-telerik-justmock) topic.


## Mocking the Standard DLL Import

> **Important**
>
>  __TelerikJustMock will not instrument assemblies, containing only DLL import functions.__ 

We will start by testing the `FormatCurrentProcessId` function. As you can see, this method is getting the current process ID, formatting it in a way and finally returning the formatted string. To write the necessary tests for these units should not be hard. You will need: 

1. To pass certain parameters.
1. To execute the function.
1. To check if the results are the same as the expected.

The interesting part comes right with the first point.

### How can you control the current process ID?

In the past this was achieved by wrapping the static `GetCurrentProcessId` method and arranging against the wrapper. However, this can become a tiresome operation.

With JustMock you are now able to directly arrange against the imported method just as you are arranging static functions (check [Static Mocking]({%slug justmock/advanced-usage/static-mocking%}) for more details). 

Next is an example, demonstrating how easy is to arrange functions, decorated with the `[DLLImport()]` attribute:

  #### __[C#]__

  {{region DLLImport#CurrentPorcessId}}
    [TestMethod]
    public void FormatCurrentProcessId_OnExecute_ShouldReturnExpected()
    {
        var expected = 3500;

        // Arrange
        Mock.Arrange(() => Foo.GetCurrentProcessId()).Returns(expected);

        // Act
        var myFoo = new Foo();
        var actual = myFoo.FormatCurrentProcessId();

        // Assert
        Assert.AreEqual(string.Format("The current process ID is {0}", expected), actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region DLLImport#CurrentPorcessId}}
    <TestMethod>
    Public Sub FormatCurrentProcessId_OnExecute_ShouldReturnExpected()
        Dim expected = 3500

        ' Arrange
        Mock.Arrange(Function() Foo.GetCurrentProcessId()).Returns(expected)

        ' Act
        Dim myFoo = New Foo()
        Dim actual = myFoo.FormatCurrentProcessId()

        ' Assert
        Assert.AreEqual(String.Format("The current process ID is {0}", expected), actual)
    End Sub
  {{endregion}}


Here we setup that `Foo.GetCurrentProcessId()` should return expected value when called. Thus we override the original method behavior with a one that we specify. Then we act against our method under test. And finally we assert the expectations.
        

## Isolating the System Under Test

Let's assume we need to isolate certain function which is depending on an extern imported method. Such is the `Start` method from our example (`Foo`) class. In it, we aim to execute some process, but warn with message before that. To test the execution logic (which in this case is insignificant), we will need to prevent the showing of the `MessageBox`. Again, using the commercial version of JustMock you are able to arrange directly against it, like so:

  #### __[C#]__

  {{region DLLImport#MessageBox}}
    [TestMethod]
    [ExpectedException(typeof(NotImplementedException))]
    public void Start_OnExecute_ShouldThrowNotImplementedException()
    {
        // Arrange
        Mock.Arrange(() => Foo.MessageBox(Arg.IsAny<IntPtr>(), Arg.AnyString, Arg.AnyString, Arg.IsAny<uint>())).DoNothing();

        // Act
        var myFoo = new Foo();
        myFoo.Start();
    }
  {{endregion}}

  #### __[VB]__

  {{region DLLImport#MessageBox}}
    <TestMethod>
    <ExpectedException(GetType(NotImplementedException))>
    Public Sub Start_OnExecute_ShouldThrowNotImplementedException()
        ' Arrange
        Mock.Arrange(Function() Foo.MessageBox(Arg.IsAny(Of IntPtr)(), Arg.AnyString, Arg.AnyString, Arg.IsAny(Of UInteger)())).DoNothing()

        ' Act
        Dim myFoo = New Foo()
        myFoo.Start()
    End Sub
  {{endregion}}


Here we specify that when `Foo.MessageBox` is called it should do nothing. Note that we are using the same matchers as in any other JustMock arrangement. Having this, we can continue with the rest of our test.

## Mock Private DLL Import

In many cases, you will find that the imported method/member needs to be set to non-public (`private`, `protected`, `internal`, etc.). Such scenarios is applied in the `Is64BitProcessMessage()` method from our example (`Foo`) class. To test this function we will directly arrange against the imported method, but using the [Mocking LINQ Queries]({%slug justmock/advanced-usage/mocking-linq-queries%}) feature of JustMock. 

Here are the complete tests:

  #### __[C#]__

  {{region DLLImport#IsWiw64ProcessFirst}}
    [TestMethod]
    public void Is64BitProcessMessage_On64BitExecute_ShouldReturnExpected()
    {
        // Arrange
        Mock.NonPublic.Arrange<bool>(typeof(Foo), "IsWow64Process").Returns(true);

        // Act
        var myFoo = new Foo();
        var actual = myFoo.Is64BitProcessMessage();

        // Assert
        Assert.AreEqual("This is a 64 bit process!", actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region DLLImport#IsWiw64ProcessFirst}}
    <TestMethod>
    Public Sub Is64BitProcessMessage_On64BitExecute_ShouldReturnExpected()
        ' Arrange
        Mock.NonPublic.Arrange(Of Boolean)(GetType(Foo), "IsWow64Process").Returns(True)

        ' Act
        Dim myFoo = New Foo()
        Dim actual = myFoo.Is64BitProcessMessage()

        ' Assert
        Assert.AreEqual("This is a 64 bit process!", actual)
    End Sub
  {{endregion}}


  #### __[C#]__

  {{region DLLImport#IsWiw64ProcessSecond}}
    [TestMethod]
    public void Is64BitProcessMessage_On32BitExecute_ShouldReturnExpected()
    {
        // Arrange
        Mock.NonPublic.Arrange<bool>(typeof(Foo), "IsWow64Process").Returns(false);

        // Act
        var myFoo = new Foo();
        var actual = myFoo.Is64BitProcessMessage();

        // Assert
        Assert.AreEqual("This is a 32 bit process!", actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region DLLImport#IsWiw64ProcessSecond}}
    <TestMethod>
    Public Sub Is64BitProcessMessage_On32BitExecute_ShouldReturnExpected()
        ' Arrange
        Mock.NonPublic.Arrange(Of Boolean)(GetType(Foo), "IsWow64Process").Returns(False)

        ' Act
        Dim myFoo = New Foo()
        Dim actual = myFoo.Is64BitProcessMessage()

        ' Assert
        Assert.AreEqual("This is a 32 bit process!", actual)
    End Sub
  {{endregion}}


Note that, in this case we are mocking a non-public, static, imported function. This clearly shows what can be achieved and on what cost, with JustMock.
        

## See Also

 * [Static Mocking]({%slug justmock/advanced-usage/static-mocking%})
 * [Mocking LINQ Queries]({%slug justmock/advanced-usage/mocking-linq-queries%})
