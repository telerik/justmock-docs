---
title: Mocking Non-public Members and Types
page_title: Mocking Non-public Members and Types | JustMock Documentation
description: Learn how you can isolate non-public members and types by mocking them with JustMock.
previous_url: /advanced-usage-mocking-non-public-members-and-types.html
slug: justmock/advanced-usage/mocking-non-public-members-and-types
tags: mocking,non-public,members,and,types
published: True
position: 13
---

# Mocking Non-public Members and Types

This article provides various examples that demonstrate how to mock non-public members and types with __Telerik® JustMock__.

## Introduction

You can use JustMock to mock non-public members and types in elevated mode. That is useful when you want to isolate calls to non-public members and types, such as:

* private calls, methods, and interfaces
* private static methods and properties
* protected members
* internal classes
* internal virtual methods and properties

> This feature is available only in the commercial version of Telerik JustMock. 
Refer to [this]({%slug justmock/getting-started/commercial-vs-free-version%}) topic to learn more about the differences between the commercial and free versions.

>note If you need a complete Visual Studio project that demonstrates how to mock non-public members and types, refer to the examples in the installation directory. The default installation directory is *C:\Program Files (x86)\Progress\Telerik JustMock\Examples*.

## Prerequisites

In the further examples, we will use the following sample class to test:

#### __[C#] Sample setup__

  {{region NonPublicMocking#SampleClass}}
    public class Foo
    {
        private void DoPrivate()
        {
            throw new NotImplementedException();
        }

        private void DoPrivate(int arg)
        {
            throw new NotImplementedException();
        }

        public void DoPublic()
        {
            DoPrivate();
        }

        private void DoPrivateGeneric<T>(T arg)
        {
            throw new NotImplementedException();
        }

        public void DoPublicGeneric<T>(T arg)
        {
            DoPrivateGeneric<T>(arg);
        }

        public void Execute(int arg)
        {
            DoPrivate(arg);
        }

        private int PrivateEcho(int arg)
        {
            return arg;
        }

        public int Echo(int arg)
        {
            return PrivateEcho(arg);
        }

        internal virtual void Do()
        {
            throw new NotImplementedException();
        }

        internal virtual string Value
        {
            get
            {
                throw new NotImplementedException();
            }
            set
            {
                throw new NotImplementedException();
            }
        }

        private static int PrivateStaticProperty { get; set; }

        public int GetMyPrivateStaticProperty()
        {
            return PrivateStaticProperty;
        }
    }
  {{endregion}}

  #### __[VB] Sample setup__

  {{region NonPublicMocking#SampleClass}}
    Public Class Foo
        Private Sub DoPrivate()
            Throw New NotImplementedException()
        End Sub

        Private Sub DoPrivate(arg As Integer)
            Throw New NotImplementedException()
        End Sub

        Public Sub DoPublic()
            DoPrivate()
        End Sub

        Private Sub DoPrivateGeneric(Of T)(ByVal arg As T)
            Throw New NotImplementedException()
        End Sub

        Public Sub DoPublicGeneric(Of T)(ByVal arg As T)
            DoPrivateGeneric(Of T)(arg)
        End Sub

        Public Sub Execute(arg As Integer)
            DoPrivate(arg)
        End Sub

        Private Function PrivateEcho(arg As Integer) As Integer
            Return arg
        End Function

        Public Function Echo(arg As Integer) As Integer
            Return PrivateEcho(arg)
        End Function

        Friend Overridable Sub [Do]()
            Throw New NotImplementedException()
        End Sub

        Friend Overridable Property Value() As String
            Get
                Throw New NotImplementedException()
            End Get
            Set(value As String)
                Throw New NotImplementedException()
            End Set
        End Property

        Private Shared Property PrivateStaticProperty() As Integer
            Get
                Return m_PrivateStaticProperty
            End Get
            Set(value As Integer)
                m_PrivateStaticProperty = Value
            End Set
        End Property
        Private Shared m_PrivateStaticProperty As Integer

        Public Function GetMyPrivateStaticProperty() As Integer
            Return PrivateStaticProperty
        End Function
    End Class
  {{endregion}}


> **Important**
>
> To mock non-public members and types you first need to go to elevated mode by enabling TelerikJustMock from the menu. [How to Enable/Disable](./advanced-usage#how-to-enabledisable-telerikjustmock)

## Step by Step Example

Follow the steps below to get started with this example:

1. Create an instance of the type that you want to test.
1. Use the `Mock.NonPublic` modifier to mock a non-public member.
1. Add the arrange statement:
    1. Pass the target object to test.
    1. Pass the member name that you want to test as a `string`.
    1. If you test a method, pass the arguments.

**Example 1** shows how to set up that a call to the `DoPrivate` method of the `Foo` class must set a local variable `called`. This way you override the original method behavior with the one that you specify.

  #### __[C#] Example 1: Change the behavior of a private method__

  {{region NonPublicMocking#StepByStep}}
    [TestMethod]
    public void ShouldInvokeNonPublicMember()
    {
        Foo foo = new Foo();

        bool called = false;

        // Arrange
        Mock.NonPublic.Arrange(foo, "DoPrivate").DoInstead(() => called = true);

        // Act
        foo.DoPublic();

        // Assert
        Assert.IsTrue(called);
    }
  {{endregion}}

  #### __[VB] Example 1: Change the behavior of a private method__

  {{region NonPublicMocking#StepByStep}}
    <TestMethod>
    Public Sub ShouldInvokeNonPublicMember()
        Dim foo As New Foo()

        Dim called As Boolean = False

        ' Arrange
        Mock.NonPublic.Arrange(foo, "DoPrivate").DoInstead(Sub() called = True)

        ' Act
        foo.DoPublic()

        ' Assert
        Assert.IsTrue(called)
    End Sub
  {{endregion}}



`Mock.NonPublic` can be also used to mock generic non-public methods. In addition to the non-generic method mock, the generic type arguments must be supplied in the arrangement. 

  #### __[C#] Example 2: Change the behavior of a generic non-public method__

  {{region NonPublicMocking#StepByStepGeneric}}
    [TestMethod]
    public void ShouldInvokeNonPublicGenericMember()
    {
        Foo foo = new Foo();

        bool called = false;

        // Arrange
        Mock.NonPublic.Arrange(foo, "DoPrivateGeneric", new Type[] { typeof(int) }, 10).DoInstead(() => called = true);

        // Act
        foo.DoPublicGeneric<int>(10);

        // Assert
        Assert.IsTrue(called);
    }
  {{endregion}}

  #### __[VB] Example 2: Change the behavior of a generic non-public method__

  {{region NonPublicMocking#StepByStepGeneric}}
    <TestMethod>
    Public Sub ShouldInvokeNonPublicGenericMember()
        Dim foo As Foo = New Foo()

        Dim called As Boolean = False

        ' Arrange
        Mock.NonPublic.Arrange(foo, "DoPrivateGeneric", New Type() {GetType(Integer)}, 10).DoInstead(Sub() called = True)

        ' Act
        foo.DoPublicGeneric(Of Integer)(10)

        ' Assert
        Assert.IsTrue(called)
    End Sub
  {{endregion}}

## Non-Public Method with Parameters

**Example 3** shows how you can arrange a call to a private method accepting an argument that matches any integer value. The example arranges the `PrivateEcho` to return `1` when called with any `int` parameter. In the acting phase, the `PrivateEcho` method is called with `5` as argument.

>For more details on how to work with parameters when mocking, check the [Matchers]({%slug justmock/basic-usage/matchers%}) help topic.

  #### __[C#] Example 3: Change the behavior of a non-public method with parameter__

  {{region NonPublicMocking#ShouldMockPrivateCallsWithMatcherArgument}}
  
    [TestMethod]
    public void ShouldInvokeNonPublicMemberWithMatcher()
    {
        Foo foo = new Foo();

        int expected = 1;

        // Arrange
        // PrivateEcho will always return 1 when invoked with an int parameter
        Mock.NonPublic.Arrange<int>(foo, "PrivateEcho", Arg.Expr.IsAny<int>()).Returns(expected);

        // Act
        int actual = foo.Echo(5);

        // Assert
        Assert.AreEqual(expected, actual);
    }
  {{endregion}}

  #### __[VB] Example 3: Change the behavior of a non-public method with parameter__

  {{region NonPublicMocking#ShouldMockPrivateCallsWithMatcherArgument}}
    <TestMethod>
    Public Sub ShouldInvokeNonPublicMemberWithMatcher()
        Dim foo As New Foo()

        Dim expected As Integer = 1

        ' Arrange
        ' PrivateEcho will always return 1 when invoked with an int parameter
        Mock.NonPublic.Arrange(Of Integer)(foo, "PrivateEcho", Arg.Expr.IsAny(Of Integer)()).Returns(expected)

        ' Act
        Dim actual As Integer = foo.Echo(5)

        ' Assert
        Assert.AreEqual(expected, actual)
    End Sub
  {{endregion}}



##  Mocking Using a Dynamic Wrapper

JustMock leverages .NET 4 and the [DLR](https://en.wikipedia.org/wiki/Dynamic_Language_Runtime) to allow you to arrange non-public members as naturally as you can do it with public members.

**Example 4** shows how to wrap the mock instance in a dynamic object using `Mock.NonPublic.Wrap()`. The wrapper can be passed to `Mock.NonPublic.Arrange` and `Mock.NonPublic.Assert` together with an operation to specify what you want to arrange. You could also arrange:
- the value of a property getter 
- the action of a property setter

Argument matchers are specified using `Arg.Expr`.

When using dynamic expressions in Visual C# projects, you should reference the Microsoft.CSharp assembly.

  #### __[C#] Example 4: Using dynamic wrapper__

  {{region NonPublicMocking#StepByStepDynamic}}
    [TestMethod]
    public void ShouldInvokeNonPublicMemberDynamic()
    {
        Foo foo = new Foo();

        // Arrange
        dynamic fooAcc = Mock.NonPublic.Wrap(foo);
        Mock.NonPublic.Arrange<int>(fooAcc.PrivateEcho(Arg.Expr.IsAny<int>())).Returns(10);
        Mock.NonPublic.Arrange<string>(fooAcc.Value).Returns("foo");
        Mock.NonPublic.Arrange(fooAcc.Value = "abc").OccursOnce();

        // Act
        var actual = foo.Echo(5);
        var value = foo.Value;
        foo.Value = "abc";

        // Assert
        Assert.AreEqual(10, actual);
        Assert.AreEqual("foo", value);
        Mock.Assert(foo);
    }
  {{endregion}}

  #### __[VB] Example 4: Using dynamic wrapper__

  {{region NonPublicMocking#StepByStepDynamic}}
    <TestMethod>
    Public Sub ShouldInvokeNonPublicMemberDynamic()
        Dim foo As New Foo()

        ' Arrange
        Dim fooAcc As Object = Mock.NonPublic.Wrap(foo)
        Mock.NonPublic.Arrange(Of Integer)(fooAcc.PrivateEcho(Arg.Expr.IsAny(Of Integer)())).Returns(10)
        Mock.NonPublic.Arrange(Of String)(fooAcc.Value).Returns("foo")
        Mock.NonPublic.Arrange(fooAcc.Value = "abc").OccursOnce()

        ' Act
        Dim actual As Integer = foo.Echo(5)
        Dim value As String = foo.Value
        foo.Value = "abc"

        ' Assert
        Assert.AreEqual(10, actual)
        Assert.AreEqual("foo", value)
        Mock.Assert(foo)
    End Sub
  {{endregion}}


## Private Methods with Overloads

In this section, you will find how to arrange a call to a *private method with two overloads*. The following class will be used as an example:

  #### __[C#] Sample setup__

  {{region NonPublicMocking#FooInternal}}
  
    internal class FooInternal
    {
        private void pExecute(int arg1)
        {
            throw new NotImplementedException();
        }

        private void pExecute()
        {
            throw new NotImplementedException();
        }

        public void Execute(int arg1)
        {
            pExecute(arg1);
        }

        public void Execute()
        {
            pExecute();
        }
    }
  {{endregion}}

  #### __[VB] Sample setup__

  {{region NonPublicMocking#FooInternal}}
    Friend Class FooInternal
    Private Sub pExecute(arg1 As Integer)
        Throw New NotImplementedException()
    End Sub

    Private Sub pExecute()
        Throw New NotImplementedException()
    End Sub

    Public Sub Execute(arg1 As Integer)
        pExecute(arg1)
    End Sub

    Public Sub Execute()
        pExecute()
    End Sub
End Class
  {{endregion}}


> **Important**
>
> To interact with non-public classes, you must add the `InternalVisibleTo` property inside the  *AssemblyInfo.cs*  in the project you need to test, like this: 
  #### __[C#]__

  {{region }}
    [assembly: InternalsVisibleTo("YourTestProject")]
  {{endregion}}


In the sample setup shown above, the `pExecute` method has two overloads - one without arguments and one accepting an integer value as an argument. The code in **Example 5** mocks the overload that accepts an *integer*. The behavior of the method is arranged to set a local boolean variable to `true` once it is called with `10` as argument. After that, it acts by calling `foo.Execute(10)` and verifies that `called` is `true`.

  #### __[C#] Example 5: Mock private method with overloads__

  {{region NonPublicMocking#PrivateMethodWithOverloads}}
    [TestMethod]
    public void ShouldInvokeNonPublicMemberWithOverloads()
    {
        FooInternal foo = new FooInternal();
        bool isCalled = false;

        // Arrange
        Mock.NonPublic.Arrange(foo, "pExecute", 10).DoInstead(() => isCalled = true);

        // Act
        foo.Execute(10);

        // Assert
        Assert.IsTrue(isCalled);
    }
  {{endregion}}

  #### __[VB] Example 5: Mock private method with overloads__

  {{region NonPublicMocking#PrivateMethodWithOverloads}}
    <TestMethod> _
    Public Sub ShouldInvokeNonPublicMemberWithOverloads()
        ' Arrange
        Dim foo As New FooInternal()
        Dim isCalled As Boolean = False

        ' Arrange
        Mock.NonPublic.Arrange(foo, "pExecute", 10).DoInstead(Sub() isCalled = True)

        ' Act
        foo.Execute(10)

        ' Assert
        Assert.IsTrue(isCalled)
    End Sub
  {{endregion}}


## Private Interface Implementation Method

This section shows how you can mock an explicit (not public) interface implementation method from current or base class.

The following classes will be used:

  #### __[C#] Sample setup__

  {{region NonPublicMocking#PrivateInterfaceMethod}}
    public interface IManager
    {
        object Provider { get; }
    }

    public class FooBase : IManager
    {
        object IManager.Provider
        {
            get { throw new NotImplementedException(); }
        }
    }

    public class Bar : FooBase
    {
        //...
    }
  {{endregion}}

  #### __[VB] Sample setup__

  {{region NonPublicMocking#PrivateInterfaceMethod}}
    Public Interface IManager
        ReadOnly Property Provider() As Object
    End Interface
    
    Public Class FooBase
        Implements IManager
        Private ReadOnly Property Provider() As Object Implements IManager.Provider
            Get
                Throw New NotImplementedException()
            End Get
        End Property
    End Class
    
    Public Class Bar
        Inherits FooBase
        '...
    End Class
  {{endregion}}

As you can see from the sample above, the `IManager` interface defines the `Provider` property, which is then implemented in `FooBase`. The concrete implementation we need to test, however, resides in the `Bar` class, which uses the `FooBase.Provider` property. In **Example 6** you will see how you can mock the `Provider` property.

  #### __[C#] Example 6: Mock the implementation of a private method defined in an interface__

  {{region NonPublicMocking#PrivateInterfaceMethod2}}
    [TestMethod]
    public void ShouldMockPrivateInterfaceImplementationMethod()
    {
        const string expected = "dummy";

        // Arrange
        var bar = Mock.Create<Bar>();
        Mock.Arrange(() => ((IManager)bar).Provider).Returns(expected);

        // Act, Assert
        Assert.AreEqual(expected, ((IManager)bar).Provider);
    }
  {{endregion}}

  #### __[VB] Example 6: Mock the implementation of a private method defined in an interface__

  {{region NonPublicMocking#PrivateInterfaceMethod2}}
    <TestMethod> _
    Public Sub ShouldMockPrivateInterfaceImplementationMethod()
        Const expected As String = "dummy"

        ' Arrange
        Dim bar = Mock.Create(Of Bar)()
        Mock.Arrange(Function() DirectCast(bar, IManager).Provider).Returns("dummy")

        ' Act, Assert
        Assert.AreEqual(expected, DirectCast(bar, IManager).Provider)
    End Sub
  {{endregion}}


## Internal Virtual Method

Mocking internal virtual methods uses similar approach to mocking public members. To demonstrate how you can use **JustMock** to mock an internal virtual method, we will be using the `Do` method from the sample setup in the beginning of this topic.

  #### __[C#] Example 7: Mock internal virtual method__

  {{region NonPublicMocking#ShouldMockInternalVirtualMethod}}
    [TestMethod]
    public void ShouldMockInternalVirtualMethod()
    {
        Foo foo = new Foo();

        bool isCalled = false;

        // Arrange
        Mock.Arrange(() => foo.Do()).DoInstead(() => isCalled = true);

        // Act
        foo.Do();

        // Assert
        Assert.IsTrue(isCalled);
    }
  {{endregion}}

  #### __[VB] Example 7: Mock internal virtual method__

  {{region NonPublicMocking#ShouldMockInternalVirtualMethod}}
    <TestMethod> _
    Public Sub ShouldMockInternalVirtualMethod()
        Dim foo As New Foo()

        Dim isCalled As Boolean = False

        ' Arrange
        Mock.Arrange(Sub() foo.Do()).DoInstead(Sub() isCalled = True)

        ' Act
        foo.[Do]()

        ' Assert
        Assert.IsTrue(isCalled)
    End Sub
  {{endregion}}

Note that, in the arrange statement, it is not used a mock of `Foo`, but the actual instance.

## Internal Virtual Property Get And Set

Arranging an internal virtual property is also similar to arranging a public property.

  #### __[C#] Example 8: Mock internal virtual property get__

  {{region NonPublicMocking#ShouldMockInternalVirtualPropertyGet}}
    [TestMethod]
    public void ShouldMockInternalVirtualPropertyGET()
    {
        Foo foo = new Foo();

        // Arrange
        Mock.Arrange(() => foo.Value).Returns("ping");

        // Act
        string actual = foo.Value;

        // Assert
        Assert.AreEqual("ping", actual);
    }
  {{endregion}}

  #### __[VB] Example 8: Mock internal virtual property get__

  {{region NonPublicMocking#ShouldMockInternalVirtualPropertyGet}}
    <TestMethod> _
    Public Sub ShouldMockInternalVirtualPropertyGET()
        Dim foo As New Foo()

        ' Arrange
        Mock.Arrange(Function() foo.Value).Returns("ping")

        ' Act
        Dim actual As String = foo.Value

        ' Assert
        Assert.AreEqual("ping", actual)
    End Sub
  {{endregion}}

Note that, in the arrange statement, it is not used a mock of `Foo`, but the actual instance.

The following code is an example of mocking internal virtual property set. The code overrides the actual implementation by arranging that the `foo.Value` must be called with a certain value.

  #### __[C#] Example 9: Mock internal virtual property set__

  {{region NonPublicMocking#ShouldMockInternalVirtualPropertySet}}
    [TestMethod]
    public void ShouldMockInternalVirtualPropertySET()
    {
        // Arrange
        Foo foo = Mock.Create<Foo>();
        Mock.ArrangeSet(() => foo.Value = "ping").MustBeCalled();

        // Act
        foo.Value = "ping";

        // Assert
        Mock.Assert(foo);
    }
  {{endregion}}

  #### __[VB] Example 9: Mock internal virtual property set__

  {{region NonPublicMocking#ShouldMockInternalVirtualPropertySet}}
    <TestMethod> _
    Public Sub ShouldMockInternalVirtualPropertySET()
        ' Arrange
        Dim foo As Foo = Mock.Create(Of Foo)()
        Mock.ArrangeSet(Sub() foo.Value = "ping").MustBeCalled()

        ' Act
        foo.Value = "ping"

        ' Assert
        Mock.Assert(foo)
    End Sub
  {{endregion}}

## Private Static Method

The following example shows how to mock a *private static method*. We use the following sample class:

  #### __[C#] Sample setup__

  {{region NonPublicMocking#FooInternalStatic}}
  
    internal class FooInternalStatic
    {
        private static int EchoPrivate(int arg1)
        {
            throw new NotImplementedException();
        }

        public static int Echo(int arg1)
        {
            return EchoPrivate(arg1);
        }

        private static int EchoPrivateGeneric<T>(T arg1)
        {
            throw new NotImplementedException();
        }

        public static int EchoGeneric<T>(T arg1)
        {
            return EchoPrivateGeneric<T>(arg1);
        }
    }
  {{endregion}}

#### __[VB] Sample setup__

  {{region NonPublicMocking#FooInternalStatic}}
  
    Friend Class FooInternalStatic
        Private Shared Function EchoPrivate(arg1 As Integer) As Integer
            Throw New NotImplementedException()
        End Function

        Public Shared Function Echo(arg1 As Integer) As Integer
            Return EchoPrivate(arg1)
        End Function

        Private Shared Function EchoPrivateGeneric(Of T)(ByVal arg1 As T) As Integer
            Throw New NotImplementedException()
        End Function

        Public Shared Function EchoGeneric(Of T)(ByVal arg1 As T) As Integer
            Return EchoPrivateGeneric(Of T)(arg1)
        End Function
    End Class
  {{endregion}}


> **Important**
>
> To interact with non-public classes, you should add the `InternalVisibleTo` property inside the  *AssemblyInfo.cs*  in the project you need to test, like this: 
  #### __[C#]__

  {{region }}
    [assembly: InternalsVisibleTo("YourTestProject")]
  {{endregion}}


The method arranged in **Example 10** is `FooInternalStatic.EchoPrivate()`.

  #### __[C#] Example 10: Mock private static method__

  {{region NonPublicMocking#PrivateStaticMethod}}
    [TestMethod]
    public void ShouldMockPrivateStaticMethod()
    {
        // Arrange
        Mock.NonPublic.Arrange<FooInternalStatic, int>("EchoPrivate", 10);

        // Act
        FooInternalStatic.Echo(10);

        // Assert
        Mock.NonPublic.Assert<FooInternalStatic, int>("EchoPrivate", 10);
    }
  {{endregion}}

  #### __[VB] Example 10: Mock private static method__

  {{region NonPublicMocking#PrivateStaticMethod}}
    <TestMethod> _
    Public Sub ShouldMockPrivateStaticMethod()
        ' Arrange
        Mock.NonPublic.Arrange(Of FooInternalStatic, Integer)("EchoPrivate", 10)

        ' Act
        FooInternalStatic.Echo(10)

        ' Assert
        Mock.NonPublic.Assert(Of FooInternalStatic, Integer)("EchoPrivate", 10)
    End Sub
  {{endregion}}

The code in **Example 10** calls the `Echo` method, but its implementation calls the `EchoPrivate` method, so the assertion passes.

Like with non-public instance generic methods, `Mock.NonPublic` can be used to mock non-public static generic ones. 

  #### __[C#] Example 11: Mock non-public static generic method__

  {{region NonPublicMocking#PrivateStaticGenericMethod}}
  
    [TestMethod]
    public void ShouldMockPrivateStaticGenericMethod()
    {
        // Arrange
        Mock.NonPublic.Arrange<FooInternalStatic, int>("EchoPrivateGeneric", new Type[] { typeof(int) }, 10);

        // Act
        FooInternalStatic.EchoGeneric<int>(10);

        // Assert
        Mock.NonPublic.Assert<FooInternalStatic, int>("EchoPrivateGeneric", new Type[] { typeof(int) }, 10);
    }
  {{endregion}}

  #### __[VB] Example 11: Mock non-public static generic method__

  {{region NonPublicMocking#PrivateStaticGenericMethod}}
    <TestMethod>
    Public Sub ShouldMockPrivateStaticGenericMethod()
        ' Arrange
        Mock.NonPublic.Arrange(Of FooInternalStatic, Integer)("EchoPrivateGeneric", New Type() {GetType(Integer)}, 10)

        ' Act
        FooInternalStatic.EchoGeneric(Of Integer)(10)

        ' Assert
        Mock.NonPublic.Assert(Of FooInternalStatic, Integer)("EchoPrivateGeneric", New Type() {GetType(Integer)}, 10)
    End Sub
  {{endregion}}

## Private Static Property

This section shows how to mock the get function of a *private static property*. The *Foo* class defined above is used as a sample setup.

The property arranged in **Example 11** is `Foo.PrivateStaticProperty`. When called, it will return an expected integer value, different from the default one.

  #### __[C#] Example 11: Mock private static property__

  {{region NonPublicMocking#PrivateStaticProperty}}
  
    [TestMethod]
    public void ShouldMockPrivateStaticProperty()
    {
        var expected = 10;

        // Arrange
        Foo foo = new Foo();

        Mock.NonPublic.Arrange<int>(typeof(Foo), "PrivateStaticProperty").Returns(expected);

        // Act
        int actual = foo.GetMyPrivateStaticProperty();

        // Assert
        Assert.AreEqual(expected, actual);
    }
  {{endregion}}

  #### __[VB] Example 11: Mock private static property__

  {{region NonPublicMocking#PrivateStaticProperty}}
    <TestMethod> _
    Public Sub ShouldMockPrivateStaticProperty()
        Dim expected = 10

        ' Arrange
        Dim foo As New Foo()

        Mock.NonPublic.Arrange(Of Integer)(GetType(Foo), "PrivateStaticProperty").Returns(expected)

        ' Act
        Dim actual As Integer = foo.GetMyPrivateStaticProperty()

        ' Assert
        Assert.AreEqual(expected, actual)
    End Sub
  {{endregion}}

To act, the code calls the `GetMyPrivateStaticProperty()` method, but its implementation returns the `PrivateStaticProperty`, so the assertion passes.

## Internal Class

Let's see an example of how to mock an internal class from .NET. Consider the `System.Net.HttpRequestCreator` class, which is internal, but has a public interface `System.Net.IWebRequestCreate`. **Example 12** mocks its `Create` method.

  #### __[C#] Example 12: Mock internal class__

  {{region NonPublicMocking#ShouldMockNonPublicClassFromFramework}}
    [TestMethod]
    public void ShouldMockInternaldotNETClass()
    {
        // Arrange
        string typeName = "System.Net.HttpRequestCreator";

        var httpRequestCreator = Mock.Create(typeName);

        bool isCalled = false;

        Mock.NonPublic.Arrange(httpRequestCreator, "Create", Arg.Expr.IsAny<Uri>()).DoInstead(() => isCalled = true);

        // Act
        System.Net.IWebRequestCreate iWebRequestCreate = (System.Net.IWebRequestCreate)httpRequestCreator;

        iWebRequestCreate.Create(new Uri("https://www.telerik.com"));

        // Assert
        Assert.IsTrue(isCalled);
    }
  {{endregion}}

  #### __[VB] Example 12: Mock internal class__

  {{region NonPublicMocking#ShouldMockNonPublicClassFromFramework}}
    <TestMethod> _
    Public Sub ShouldMockInternaldotNETClass()
        ' Arrange
        Dim typeName As String = "System.Net.HttpRequestCreator"

        Dim httpRequestCreator = Mock.Create(typeName)

        Dim isCalled As Boolean = False

        Mock.NonPublic.Arrange(httpRequestCreator, "Create", Arg.Expr.IsAny(Of Uri)()).DoInstead(Sub() isCalled = True)

        ' Act
        Dim iWebRequestCreate As System.Net.IWebRequestCreate = DirectCast(httpRequestCreator, System.Net.IWebRequestCreate)

        iWebRequestCreate.Create(New Uri("https://www.telerik.com"))

        ' Assert
        Assert.IsTrue(isCalled)
    End Sub
  {{endregion}}

Note the use of `Arg.Expr.IsAny<Uri>()` - as we mock a non-public call, we need to know the type of the argument to resolve the method. Thus, instead of using `Arg`, like we do in most of the other cases, we must use `Arg.Expr`.

> **Important**
>
>  To mock an internal type, your assembly name must be fully qualified according to the framework design rules, i.e. `assembly name = namespace`. Note that you can't mock types from `mscorlib` in this way. JustMock does a hierarchical search to find the proper qualified name as in the above example. `System.Net.HttpRequestCreator` is found in the `System` assembly, not in `System.Net`. 

**Example 13** shows how you can handle an even more complex scenario - mock internal class by calling an original constructor with arguments, then make a call to the original implementation from a public interface. For the purpose of the example, it will be used another internal class - `System.Net.WebSocketHttpRequestCreator` - derived from the public interface `System.Net.IWebRequestCreate`.

The sample test verifies whether the call to `Create` method has been made and the returned `Web.WebRequest` object has an expected value for `RequestUri` property set.

  #### __[C#] Example 13: Complex mocking of internal .NET class with constructor arguments__

  {{region NonPublicMocking#ShouldMockNonPublicClassFromFrameworkWithArgs}}
  
    [TestMethod]
    public void ShouldMockInternaldotNETClassWithArgs()
    {
        // Arrange
        string typeName = "System.Net.WebSocketHttpRequestCreator";

        var httpRequestCreator = Mock.Create(typeName, (config) => config
            .CallConstructor(new object[] {true}));

        Mock.NonPublic.Arrange(httpRequestCreator, "Create", Arg.Expr.IsAny<Uri>())
            .CallOriginal()
            .MustBeCalled();

        // Act
        System.Net.IWebRequestCreate iWebRequestCreate = (System.Net.IWebRequestCreate)httpRequestCreator;

        var result = iWebRequestCreate.Create(new Uri("https://www.telerik.com"));

        // Assert
        Mock.Assert(httpRequestCreator);
        Assert.AreEqual(new Uri("https://www.telerik.com"), result.RequestUri);
    }
  {{endregion}}

  #### __[VB] Example 13: Complex mocking of internal .NET class with constructor arguments__

  {{region NonPublicMocking#ShouldMockNonPublicClassFromFrameworkWithArgs}}
  
    <TestMethod>
    Public Sub ShouldMockInternaldotNETClassWithArgs()
        ' Arrange
        Dim typeName As String = "System.Net.WebSocketHttpRequestCreator"

        Dim httpRequestCreator = Mock.Create(typeName, Sub(config) _
            config.CallConstructor(New Object() {True}))

        Mock.NonPublic.Arrange(httpRequestCreator, "Create", Arg.Expr.IsAny(Of Uri)()) _
            .CallOriginal() _
            .MustBeCalled()

        ' Act
        Dim iWebRequestCreate As System.Net.IWebRequestCreate = DirectCast(httpRequestCreator, System.Net.IWebRequestCreate)

        Dim result = iWebRequestCreate.Create(New Uri("https://www.telerik.com"))

        ' Assert
        Mock.Assert(httpRequestCreator)

        Assert.AreEqual(New Uri("https://www.telerik.com"), result.RequestUri)
    End Sub
  {{endregion}}


## Protected Member

To show how to mock a protected member, we will use the following class:

  #### __[C#] Sample setup__

  {{region NonPublicMocking#PublicClassWithProtectedMembers}}
  
    public class FooWithProtectedMembers
    {
        protected virtual void Load()
        {
            throw new NotImplementedException();
        }

        protected virtual int IntValue
        {
            get
            {
                throw new NotImplementedException();
            }
        }

        public virtual void Init()
        {
            Load();
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region NonPublicMocking#PublicClassWithProtectedMembers}}
  
    Public Class FooWithProtectedMembers
    Protected Overridable Sub Load()
        Throw New NotImplementedException()
    End Sub

    Protected Overridable ReadOnly Property IntValue() As Integer
        Get
            Throw New NotImplementedException()
        End Get
    End Property

    Public Overridable Sub Init()
        Load()
    End Sub
End Class
  {{endregion}}


To mock protected members in JustMock, you can use the same method and logic, as for the rest non-public types, previously shown in this topic.

First, we will arrange that our `IntValue()` method must never occur::

  #### __[C#] Example 14: Arrange protected method__

  {{region NonPublicMocking#FakingProtectedMembersTest1}}
    [TestMethod]
    public void ShouldAssertOccrenceForNonPublicFunction()
    {
        var foo = Mock.Create<FooWithProtectedMembers>(Behavior.CallOriginal);

        Mock.NonPublic.Assert<int>(foo, "IntValue", Occurs.Never());
    }
  {{endregion}}

  #### __[VB] Example 14: Arrange that a protected method must not be called__

  {{region NonPublicMocking#FakingProtectedMembersTest1}}
    <TestMethod> _
    Public Sub ShouldAssertOccrenceForNonPublicFunction()
        Dim foo = Mock.Create(Of FooWithProtectedMembers)(Behavior.CallOriginal)

        Mock.NonPublic.Assert(Of Integer)(foo, "IntValue", Occurs.Never())
    End Sub
  {{endregion}}

The second test will test if the protected `Load()` method is actually been called, when `Init()` is initiated.

  #### __[C#] Example 15: Arrange that a protected method must be called at least once__

  {{region NonPublicMocking#FakingProtectedMembersTest2}}
  
    [TestMethod]
    public void ShouldMockProtectedVirtualMembers()
    {
        var foo = Mock.Create<FooWithProtectedMembers>(Behavior.CallOriginal);

        Mock.NonPublic.Arrange(foo, "Load").MustBeCalled();

        foo.Init();

        Mock.Assert(foo);
    }
  {{endregion}}

  #### __[VB] Example 15: Arrange that a protected method must be called at least once__

  {{region NonPublicMocking#FakingProtectedMembersTest2}}
    <TestMethod> _
    Public Sub ShouldMockProtectedVirtualMembers()
        Dim foo = Mock.Create(Of FooWithProtectedMembers)(Behavior.CallOriginal)

        Mock.NonPublic.Arrange(foo, "Load").MustBeCalled()

        foo.Init()

        Mock.Assert(foo)
    End Sub
  {{endregion}}


> **Important**
>
>  To mock a protected type, your assembly name must be fully qualified according to the framework design rules, i.e. `assembly name = namespace`. Note that you can't mock types from `mscorlib` in this way. 
