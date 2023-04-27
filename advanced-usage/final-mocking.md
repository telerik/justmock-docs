---
title: Final Mocking
page_title: Final Mocking | JustMock Documentation
description: Final Mocking
previous_url: /advanced-usage-final-mocking.html
slug: justmock/advanced-usage/final-mocking
tags: final,mocking
published: False
position: 1
---

# Final Mocking

Final mocking is one of the advanced features supported in __Telerik® JustMock__. It allows you to fake final method/property calls, set expectations and verify results using the Arrange Act Assert principle. 

Faking final or virtual method/property calls doesn't affect the way you write your tests, i.e. the same syntax is used for mocking both final and non-final calls.

>This is an Elevated Feature. Refer to [this]({%slug justmock/licensing/license-agreement%}#commercial-vs-free-version) topic to learn more about the differences between the commercial and free versions of Telerik JustMock.


In the next examples we will use the following sample classes to test:

  #### __[C#]__

  {{region FinalMocking#SampleClasses}}
    public class Foo
    {
      public int Execute(int arg1, int arg2)
      {
          throw new NotImplementedException();
      }

      public int Execute(int arg1)
      {
          throw new NotImplementedException();
      }

      public int Echo(int arg1)
      {
          return arg1;
      }

      public string FooProp { get; set; }

      public delegate void EchoEventHandler(bool echoed);
      public event EchoEventHandler OnEchoCallback;
    }

    public class FooGeneric
    {
      public TRet Echo<T, TRet>(T arg1)
      {
          throw new NotImplementedException();
      }
    }
  {{endregion}}

  #### __[VB]__

  {{region FinalMocking#SampleClasses}}
    Public Class Foo
        Public Function Execute(arg1 As Integer, arg2 As Integer) As Integer
            Throw New NotImplementedException()
        End Function

        Public Function Execute(arg1 As Integer) As Integer
            Throw New NotImplementedException()
        End Function

        Public Function Echo(arg1 As Integer) As Integer
            Return arg1
        End Function

        Public Property FooProp() As String
            Get
                Return m_FooProp
            End Get
            Set(value As String)
                m_FooProp = value
            End Set
        End Property
        Private m_FooProp As String

        Public Delegate Sub EchoEventHandler(echoed As Boolean)
        Public Event OnEchoCallback As EchoEventHandler
    End Class

    Public Class FooGeneric
        Public Function Echo(Of T, TRet)(arg1 As T) As TRet
            Throw New NotImplementedException()
        End Function
    End Class
  {{endregion}}



> **Important**
>
> To use final mocking you first need to go to elevated mode by enabling TelerikJustMock from the menu. [ How to Enable/Disable ](30af2f74-f30f-4e92-9668-64a7add42b63#EnableDisableJustMock)

## Assert Final Method Setup

Set up a call to a final method and assert its return value.

  #### __[C#]__

  {{region FinalMocking#FinalMethodSetup}}
    [TestMethod]
    public void ShouldSetupACallToAFinalMethod()
    {
        // Arrange
        var foo = Mock.Create<Foo>();

        Mock.Arrange(() => foo.Echo(Arg.IsAny<int>())).Returns(10);

        // Act
        var actual = foo.Echo(1);

        // Assert
        Assert.AreEqual(10, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region FinalMocking#FinalMethodSetup}}
    <TestMethod()>
    Public Sub ShouldSetupACallToAFinalMethod()
        ' Arrange
        Dim foo = Mock.Create(Of Foo)()

        Mock.Arrange(Function() foo.Echo(Arg.IsAny(Of Integer)())).Returns(10)

        ' Act
        Dim actual = foo.Echo(1)

        ' Assert
        Assert.AreEqual(10, actual)
    End Sub
  {{endregion}}

Here we setup that a call to the final `foo.Echo` method with any `int` argument should return 10.

## Assert Property Get

Set up a call to a final property and assert its return value.

  #### __[C#]__

  {{region FinalMocking#FinalPropertyGet}}
    [TestMethod]
    public void ShouldSetupACallToAFinalProperty()
    {
        // Arrange
        var foo = Mock.Create<Foo>();

        Mock.Arrange(() => foo.FooProp).Returns("bar");

        // Act
        var actual = string.Empty;
        actual = foo.FooProp;

        // Assert
        Assert.AreEqual("bar", actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region FinalMocking#FinalPropertyGet}}
    <TestMethod()>
    Public Sub ShouldSetupACallToAFinalProperty()
        ' Arrange
        Dim foo = Mock.Create(Of Foo)()

        Mock.Arrange(Function() foo.FooProp).Returns("bar")

        ' Act
        Dim actual = String.Empty
        actual = foo.FooProp

        ' Assert
        Assert.AreEqual("bar", actual)
    End Sub
  {{endregion}}

Here we setup that a call to the final `foo.FooProp` property should return the "bar" literal.

## Assert Property Set
In this example we arrange the `FooProp` property of the `Foo` class.

  #### __[C#]__

  {{region FinalMocking#FinalPropertySet}}
    [TestMethod]
    [ExpectedException(typeof(StrictMockException))]
    public void ShouldAssertPropertySet()
    {
        // Arrange
        var foo = Mock.Create<Foo>(Behavior.Strict);

        Mock.ArrangeSet(() => foo.FooProp = "ping");

        // Act
        foo.FooProp = "foo";
    }
  {{endregion}}

  #### __[VB]__

  {{region FinalMocking#FinalPropertySet}}
    <TestMethod()>
    <ExpectedException(GetType(StrictMockException))>
    Public Sub ShouldAssertPropertySet()
        ' Arrange
        Dim foo = Mock.Create(Of Foo)(Behavior.Strict)

        Mock.ArrangeSet(Function() foo.FooProp = "ping")

        ' Act
        foo.FooProp = "foo"
    End Sub
  {{endregion}}

Here we setup that setting the final `foo.FooProp` property to any `string` value different from "ping" will cause an exception of type `MockException` to be thrown.

## Assert Method Overloads

The following example example shows how to mock final method overloads. `foo.Execute` has two overloads - accepting one or two *integers* as arguments. Here we mock them both by `Returns`. In the first case we return just the argument that has been passed. In the second case we return the sum of the two *integer* values. After that, we act by calling both overloads and assert what we have arranged.

  #### __[C#]__

  {{region FinalMocking#FinalMethodOverloads}}
    [TestMethod]
    public void ShouldAssertOnMethodOverload()
    {
        // Arrange
        var foo = Mock.Create<Foo>();

        Mock.Arrange(() => foo.Execute(Arg.IsAny<int>())).Returns((int result) => result);
        Mock.Arrange(() => foo.Execute(Arg.IsAny<int>(), Arg.IsAny<int>())).Returns((int arg1, int arg2) => arg1 + arg2);

        // Assert
        Assert.AreEqual(foo.Execute(1), 1);
        Assert.AreEqual(foo.Execute(1, 1), 2);
    }
  {{endregion}}

  #### __[VB]__

  {{region FinalMocking#FinalMethodOverloads}}
    <TestMethod()>
    Public Sub ShouldAssertOnMethodOverload()
        ' Arrange
        Dim foo = Mock.Create(Of Foo)()

        Mock.Arrange(Function() foo.Execute(Arg.IsAny(Of Integer)())).Returns(Function(result As Integer) result)
        Mock.Arrange(Function() foo.Execute(Arg.IsAny(Of Integer)(), Arg.IsAny(Of Integer)())).Returns(Function(arg1 As Integer, arg2 As Integer) arg1 + arg2)

        ' Assert
        Assert.AreEqual(foo.Execute(1), 1)
        Assert.AreEqual(foo.Execute(1, 1), 2)
    End Sub
  {{endregion}}


## Assert Method Callbacks
Follows an example of how to use the `Raises` method to fire a callback and pass event arguments once a final method is called.

  #### __[C#]__

  {{region FinalMocking#FinalMethodCallback}}
    [TestMethod]
    public void ShouldAssertOnMethodCallbacks()
    {
        // Arrange
        var foo = Mock.Create<Foo>();

        Mock.Arrange(() => foo.Echo(Arg.IsAny<int>())).Raises(() => foo.OnEchoCallback += null, true);

        bool called = false;

        foo.OnEchoCallback += delegate(bool echoed)
        {
            called = echoed;
        };

        // Act
        foo.Echo(10);

        // Assert
        Assert.IsTrue(called);
    }
  {{endregion}}

  #### __[VB]__

  {{region FinalMocking#FinalMethodCallback}}
    <TestMethod()>
    Public Sub ShouldAssertOnMethodCallbacks()
        ' Arrange
        Dim foo = Mock.Create(Of Foo)()



        Mock.Arrange(Function() foo.Echo(Arg.IsAny(Of Integer)())).Raises(Sub() AddHandler foo.OnEchoCallback, Nothing, True)

        Dim called As Boolean = False

        AddHandler foo.OnEchoCallback, Sub(echoed As Boolean) called = echoed

        ' Act
        foo.Echo(10)

        ' Assert
        Assert.IsTrue(called)
    End Sub
  {{endregion}}

When the `foo.Echo()` method is called with any *integer* value as argument, the `OnEchoCallback` is raised with parameter `true`. We attach a delegate to the event to set a local boolean variable to `true` once the method is called. We act by calling `foo.Echo(10)`. Finally, we verify that `foo.Echo()` has been called in our test.

## Assert Generic Types and Methods
You can also assert generic types and methods. In the example below we mock generic method in non-generic type. We arrange that it should return the *string* value that has been passed to it.

  #### __[C#]__

  {{region FinalMocking#FinalGenerics}}
    [TestMethod]
    public void ShouldAssertOnGenericTypesAndMethod()
    {
        // Arrange
        string expected = "ping";

        var foo = Mock.Create<FooGeneric>();

        Mock.Arrange(() => foo.Echo<string, string>(expected)).Returns((string s) => s);

        // Act
        string ret = foo.Echo<string, string>(expected);

        // Assert
        Assert.AreEqual(expected, ret);
    }
  {{endregion}}

  #### __[VB]__

  {{region FinalMocking#FinalGenerics}}
    <TestMethod()>
    Public Sub ShouldAssertOnGenericTypesAndMethod()
        ' Arrange
        Dim expected As String = "ping"

        Dim foo = Mock.Create(Of FooGeneric)()

        Mock.Arrange(Function() foo.Echo(Of String, String)(expected)).Returns(Function(s As String) s)

        ' Act
        Dim ret As String = foo.Echo(Of String, String)(expected)

        ' Assert
        Assert.AreEqual(expected, ret)
    End Sub
  {{endregion}}

For more information about mocking generic types and methods go to [Generics]({%slug justmock/basic-usage/generics%}) topic.
