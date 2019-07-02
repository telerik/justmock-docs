---
title: Mocking Non-public Members and Types
page_title: Mocking Non-public Members and Types | JustMock Documentation
description: Mocking Non-public Members and Types
previous_url: /advanced-usage-mocking-non-public-members-and-types.html
slug: justmock/advanced-usage/mocking-non-public-members-and-types
tags: mocking,non-public,members,and,types
published: True
position: 13
---

# Mocking Non-public Members and Types

In elevated mode, you can use __Telerik® JustMock__ to mock non-public members and types. That is useful when you want to isolate calls to non-public members.

> This feature is available only in the commercial version of Telerik JustMock. 
Refer to [this]({%slug justmock/getting-started/commercial-vs-free-version%}) topic to learn more about the differences between the commercial and free versions.

In the further examples we will use the following sample class to test:

  #### __[C#]__

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

  #### __[VB]__

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

## Step by step example

First, create an instance of the type you want to test. To mock a non-public member use the `Mock.NonPublic` modifier and then add the arrange statement. In the arrange statement, first pass the target object to test, then the member name that you want to test as a `string` and eventually  the arguments to be passed in case you test a method.

  #### __[C#]__

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

  #### __[VB]__

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

Here we setup that a call to the `DoPrivate` method of the `Foo` class should set a local variable `called`. Thus, we override the original method behavior with one that we specify.

`Mock.NonPublic` can be also used to mock generic non-public methods. In addition to the non-generic method mock the generic type arguments has to be supplied in the arrangement, the code look as following:

  #### __[C#]__

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

  #### __[VB]__

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

## Mock Private Call with Matcher
Arrange a call to a private method accepting an argument that matches any integer value.

  #### __[C#]__

  {{region NonPublicMocking#ShouldMockPrivateCallsWithMatcherArgument}}
    [TestMethod]
    public void ShouldInvokeNonPublicMemberWithMatcher()
    {
        Foo foo = new Foo();

        int expected = 1;

        // Arrange
        Mock.NonPublic.Arrange<int>(foo, "PrivateEcho", ArgExpr.IsAny<int>()).Returns(expected);

        // Act
        int actual = foo.Echo(5);

        // Assert
        Assert.AreEqual(expected, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region NonPublicMocking#ShouldMockPrivateCallsWithMatcherArgument}}
    <TestMethod>
    Public Sub ShouldInvokeNonPublicMemberWithMatcher()
        Dim foo As New Foo()

        Dim expected As Integer = 1

        ' Arrange
        Mock.NonPublic.Arrange(Of Integer)(foo, "PrivateEcho", ArgExpr.IsAny(Of Integer)()).Returns(expected)

        ' Act
        Dim actual As Integer = foo.Echo(5)

        ' Assert
        Assert.AreEqual(expected, actual)
    End Sub
  {{endregion}}

Here we specify that `PrivateEcho` method called with any `int` argument will return `1`. Acting is by calling it with `5` as argument. Finally, we verify that the return value is actually `1`.

##  Natural Mocking Using a Dynamic Wrapper

JustMock leverages .NET 4 and the DLR to allow you to arrange non-public members as naturally as you can public members.

In the example below, we wrap the mock instance in a dynamic object using Mock.NonPublic.Wrap(). The wrapper can be passed to Mock.NonPublic.Arrange and Mock.NonPublic.Assert together with an operation to specify what you want to arrange. We could also arrange the value of a property getter or the action of a property setter. Matchers are again passed using members of the ArgExpr class.

You need to reference the Microsoft.CSharp assembly when using dynamic expressions in Visual C# projects.

  #### __[C#]__

  {{region NonPublicMocking#StepByStepDynamic}}
    [TestMethod]
    public void ShouldInvokeNonPublicMemberDynamic()
    {
        Foo foo = new Foo();

        // Arrange
        dynamic fooAcc = Mock.NonPublic.Wrap(foo);
        Mock.NonPublic.Arrange<int>(fooAcc.PrivateEcho(ArgExpr.IsAny<int>())).Returns(10);
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

  #### __[VB]__

  {{region NonPublicMocking#StepByStepDynamic}}
    <TestMethod>
    Public Sub ShouldInvokeNonPublicMemberDynamic()
        Dim foo As New Foo()

        ' Arrange
        Dim fooAcc As Object = Mock.NonPublic.Wrap(foo)
        Mock.NonPublic.Arrange(Of Integer)(fooAcc.PrivateEcho(ArgExpr.IsAny(Of Integer)())).Returns(10)
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


## Mock Private Method with Overloads

Here we arrange a call to a *private method with two overloads*. The following class will be used:

  #### __[C#]__

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

  #### __[VB]__

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
> To interact with non-public classes you will need to add the `InternalVisibleTo` property inside the  *AssemblyInfo.cs*  in your project, like this: 
  #### __[C#]__

  {{region }}
    [assembly: InternalsVisibleTo("YourTestProject")]
  {{endregion}}


We mock the `pExecute` method that accepts an *integer* value as argument. We arrange it to set a local boolean variable to `true` once it is called with `10` as argument. After that we act by calling `foo.Execute(10)` and verify that `called` is true.

  #### __[C#]__

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

  #### __[VB]__

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


## Mock Private Interface Implementation Method

This example shows how we can mock an explicit (not public) interface implementation method from current or base class.

The following classes will be used:

  #### __[C#]__

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

  #### __[VB]__

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

We can now mock the `IManager.Provider` call in this way:

  #### __[C#]__

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

  #### __[VB]__

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


## Mock Internal Virtual Method

Arrange a call to an internal virtual method.

  #### __[C#]__

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

  #### __[VB]__

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

Note that in the arrange statement we don't use a mock of `Foo`, but the actual instance.

## Mock Internal Virtual Property Get And Set

Arrange a call to an internal virtual property.

  #### __[C#]__

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

  #### __[VB]__

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

Note that in the arrange statement we don't use a mock of `Foo`, but the actual instance.

Follows an example of mocking internal virtual property set. We override the actual implementation by arranging that the `foo.Value` must be called with certain value.

  #### __[C#]__

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

  #### __[VB]__

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



## Mock Private Static Method

The following example shows how to mock a *private static method*. We use the following class:

  #### __[C#]__

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

  #### __[VB]__

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
> To interact with non-public classes you will need to add the `InternalVisibleTo` property inside the  *AssemblyInfo.cs*  in your project, like this: 
  #### __[C#]__

  {{region }}
    [assembly: InternalsVisibleTo("YourTestProject")]
  {{endregion}}


The method we arrange is `FooInternalStatic.EchoPrivate()`.

  #### __[C#]__

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

  #### __[VB]__

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

We call the `Echo` method, but its implementation calls the `EchoPrivate` method, so our assertion passes. Like an instance non-public generic methods `Mock.NonPublic` can be used to mock non-public generic static ones, here is the sample test:

  #### __[C#]__

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

  #### __[VB]__

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

## Mock Private Static Property

The following example shows how to mock the get function of a *private static property*. We use the *Foo* class from above.

The property we arrange is `Foo.PrivateStaticProperty`. When called, it will return an expected integer value, different from the default:

  #### __[C#]__

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

  #### __[VB]__

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

To act, we call the `GetMyPrivateStaticProperty()` method, but its implementation returns the `PrivateStaticProperty`, so our assertion passes.

## Mock Internal Class

Let's see an example of how to mock an internal class from .NET framework. Consider the `System.Net.HttpRequestCreator` class which is internal, but has a public interface `System.Net.IWebRequestCreate`. Here we mock its `Create` method.

  #### __[C#]__

  {{region NonPublicMocking#ShouldMockNonPublicClassFromFramework}}
    [TestMethod]
    public void ShouldMockInternaldotNETClass()
    {
        // Arrange
        string typeName = "System.Net.HttpRequestCreator";

        var httpRequestCreator = Mock.Create(typeName);

        bool isCalled = false;

        Mock.NonPublic.Arrange(httpRequestCreator, "Create", ArgExpr.IsAny<Uri>()).DoInstead(() => isCalled = true);

        // Act
        System.Net.IWebRequestCreate iWebRequestCreate = (System.Net.IWebRequestCreate)httpRequestCreator;

        iWebRequestCreate.Create(new Uri("https://www.telerik.com"));

        // Assert
        Assert.IsTrue(isCalled);
    }
  {{endregion}}

  #### __[VB]__

  {{region NonPublicMocking#ShouldMockNonPublicClassFromFramework}}
    <TestMethod> _
    Public Sub ShouldMockInternaldotNETClass()
        ' Arrange
        Dim typeName As String = "System.Net.HttpRequestCreator"

        Dim httpRequestCreator = Mock.Create(typeName)

        Dim isCalled As Boolean = False

        Mock.NonPublic.Arrange(httpRequestCreator, "Create", ArgExpr.IsAny(Of Uri)()).DoInstead(Sub() isCalled = True)

        ' Act
        Dim iWebRequestCreate As System.Net.IWebRequestCreate = DirectCast(httpRequestCreator, System.Net.IWebRequestCreate)

        iWebRequestCreate.Create(New Uri("https://www.telerik.com"))

        ' Assert
        Assert.IsTrue(isCalled)
    End Sub
  {{endregion}}

Note the use of `ArgExpr.IsAny<Uri>()` - as we mock a non-public call, we need to know the type of the argument to resolve the method. Thus, instead of using `Arg`, like we do in most of the other cases, we must use `ArgExpr`. 

> **Important**
>
>  To mock internal type, your assembly name must be fully qualified according to the framework design rules, i.e. `assembly name = namespace`. Note that you can't mock types from `mscorlib` in this way. We do a hierarchical search to find the proper qualified name as in the above example. `System.Net.HttpRequestCreator` is found in the `System` assembly, not in `System.Net`. 


## Mock Protected Member

To show how the mocking of protected members is being done, we will use the following class:

  #### __[C#]__

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

To mock protected members in JustMock, we use the same method and logic, as for the rest non-public types, shown above:

First we will arrange that our `IntValue()` method should never occur, like this:

  #### __[C#]__

  {{region NonPublicMocking#FakingProtectedMembersTest1}}
    [TestMethod]
    public void ShouldAssertOccrenceForNonPublicFunction()
    {
        var foo = Mock.Create<FooWithProtectedMembers>(Behavior.CallOriginal);

        Mock.NonPublic.Assert<int>(foo, "IntValue", Occurs.Never());
    }
  {{endregion}}

  #### __[VB]__

  {{region NonPublicMocking#FakingProtectedMembersTest1}}
    <TestMethod> _
    Public Sub ShouldAssertOccrenceForNonPublicFunction()
        Dim foo = Mock.Create(Of FooWithProtectedMembers)(Behavior.CallOriginal)

        Mock.NonPublic.Assert(Of Integer)(foo, "IntValue", Occurs.Never())
    End Sub
  {{endregion}}

The second test will test if our protected `Load()` method is actually been called, when `Init()` is initiated.

  #### __[C#]__

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

  #### __[VB]__

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
>  To mock protected type, your assembly name must be fully qualified according to the framework design rules, i.e. `assembly name = namespace`. Note that you can't mock types from `mscorlib` in this way. 
