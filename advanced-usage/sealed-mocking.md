---
title: Sealed Mocking
page_title: Sealed Mocking | JustMock Documentation
description: Sealed Mocking
previous_url: /advanced-usage-sealed-mocking.html
slug: justmock/advanced-usage/sealed-mocking
tags: sealed,mocking
published: True
position: 3
---

# Sealed Mocking

Sealed mocking is one of the advanced features supported in __Telerik® JustMock__. It allows you to fake sealed classes and calls to their methods/properties, set expectations and verify results using the AAA principle. Faking sealed classes and calls to their methods/properties doesn't affect the way you write your tests, i.e. the same syntax is used for mocking non sealed classes.

> This feature is available only in the commercial version of Telerik JustMock. Refer to [this]({%slug justmock/getting-started/commercial-vs-free-version%}) topic to learn more about the differences between the commercial and free versions.

In the examples in this article we will use the following sample classes and interface to test:

  #### __[C#]__

  {{region SealedMocking#SampleClasses}}
    public sealed class FooSealed
    {
        public int Echo(int arg1)
        {
            return arg1;
        }
    }

    public sealed class FooSealedInternal
    {
        internal FooSealedInternal()
        {

        }

        public int Echo(int arg1)
        {
            return arg1;
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region SealedMocking#SampleClasses}}
    Public NotInheritable Class FooSealed
        Public Function Echo(arg1 As Integer) As Integer
            Return arg1
        End Function
    End Class

    Public NotInheritable Class FooSealedInternal

        Friend Sub New()
        End Sub

        Public Function Echo(arg1 As Integer) As Integer
            Return arg1
        End Function
    End Class
  {{endregion}}


  #### __[C#]__

  {{region SealedMocking#SampleClassInterface}}
    public interface IFoo
    {
        void Execute();
        void Execute(int arg1);
    }

    public sealed class Foo : IFoo
    {
        public void Execute()
        {
            throw new NotImplementedException();
        }

        void IFoo.Execute(int arg1)
        {
            throw new NotImplementedException();
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region SealedMocking#SampleClassInterface}}
    Public Interface IFoo
        Sub Execute()
        Sub Execute(arg1 As Integer)
    End Interface

    Public NotInheritable Class Foo
    Implements IFoo
        Public Sub Execute() Implements IFoo.Execute
            Throw New NotImplementedException()
        End Sub

        Private Sub Execute(arg1 As Integer) Implements IFoo.Execute
            Throw New NotImplementedException()
        End Sub
    End Class
  {{endregion}}

> **Important**
>
> To use sealed mocking you first need to go to elevated mode by enabling JustMock from the menu.  [How to Enable/Disable](./advanced-usage#how-to-enabledisable-telerikjustmock)

## Assert Final Method Call on a Sealed Class

Set up a call to a final method on a sealed class and assert its return value.

  #### __[C#]__

  {{region SealedMocking#AsssertFinalCall}}
    [TestMethod]
    public void ShouldAssertFinalMethodCallOnASealedClass()
    {
        // Arrange
        var foo = Mock.Create<FooSealed>();

        Mock.Arrange(() => foo.Echo(Arg.IsAny<int>())).Returns(10);

        // Act
        var actual = foo.Echo(1);

        // Assert
        Assert.AreEqual(10, actual);    
    }
  {{endregion}}

  #### __[VB]__

  {{region SealedMocking#AsssertFinalCall}}
    <TestMethod()>
    Public Sub ShouldAssertFinalMethodCallOnASealedClass()
        ' Arrange
        Dim foo = Mock.Create(Of FooSealed)()

        Mock.Arrange(Function() foo.Echo(Arg.IsAny(Of Integer)())).Returns(10)

        ' Act
        Dim actual = foo.Echo(1)

        ' Assert
        Assert.AreEqual(10, actual)
    End Sub
  {{endregion}}

Here we setup that a call to the final `foo.Echo` method with any `int` argument should return `10`.

## Create Mock for Sealed Class with Internal Constructor

Even the `sealed` class has only an `internal` constructor we can still create a mock, call its methods and verify the results.

  #### __[C#]__

  {{region SealedMocking#CreateMockForSealedInternal}}
    [TestMethod]
    public void ShouldCreateMockForASealedClassWithInternalConstructor()
    {
        // Arrange
        var foo = Mock.Create<FooSealedInternal>();

        Mock.Arrange(() => foo.Echo(Arg.IsAny<int>())).Returns(10);

        // Assert
        Assert.IsNotNull(foo);

        // Act
        var actual = foo.Echo(1);

        // Assert
        Assert.AreEqual(10, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region SealedMocking#CreateMockForSealedInternal}}
    <TestMethod()>
    Public Sub ShouldCreateMockForASealedClassWithInternalConstructor()
        ' Arrange
        Dim foo = Mock.Create(Of FooSealedInternal)()

        Mock.Arrange(Function() foo.Echo(Arg.IsAny(Of Integer)())).Returns(10)

        ' Assert
        Assert.IsNotNull(foo)

        ' Act
        Dim actual = foo.Echo(1)

        ' Assert
        Assert.AreEqual(10, actual)
    End Sub
  {{endregion}}

Here we setup that a call to the final `foo.Echo` method with any `int` argument should return 10. After we ensure that the object is actually created, we assert for the return value of the `foo.Echo` method.

## Create Mock for Sealed Class with Interface
When a mock is created by using `Mock.Create` all dependencies are also implemented. In the example below we mock the `Foo` class, which implements the `IFoo` interface.

  #### __[C#]__

  {{region SealedMocking#CreateMockForClassWithInterface}}
    [TestMethod]
    public void ShouldAssertCallOnVoid()
    {
        // Arrange
        var foo = Mock.Create<Foo>();

        bool called = false;

        Mock.Arrange(() => foo.Execute()).DoInstead(() => called = true);

        // Act
        foo.Execute();

        // Assert
        Assert.IsTrue(called);
    }
  {{endregion}}

  #### __[VB]__

  {{region SealedMocking#CreateMockForClassWithInterface}}
    <TestMethod()>
    Public Sub ShouldAssertCallOnVoid()
        ' Arrange
        Dim foo = Mock.Create(Of Foo)()

        Dim called As Boolean = False

        Mock.Arrange(Sub() foo.Execute()).DoInstead(Sub() called = True)

        ' Act
        foo.Execute()

        ' Assert
        Assert.IsTrue(called)
    End Sub
  {{endregion}}

Furthermore, if you are interested in `IFoo` interface implementation you can use the `as` operator and call the interface members as shown below.

  #### __[C#]__

  {{region SealedMocking#CreateMockForClassWithInterfaceExtracted}}
    [TestMethod]
    public void ShouldAssertCallOnVoidThroughAnInterface()
    {
        // Arrange
        var foo = Mock.Create<Foo>();

        bool called = false;

        Mock.Arrange(() => foo.Execute()).DoInstead(() => called = true);

        // Act
        IFoo iFoo = foo;
        iFoo.Execute();

        // Assert
        Assert.IsTrue(called);
    }
  {{endregion}}

  #### __[VB]__

  {{region SealedMocking#CreateMockForClassWithInterfaceExtracted}}
    <TestMethod()>
    Public Sub ShouldAssertCallOnVoidThroughAnInterface()
        ' Arrange
        Dim foo = Mock.Create(Of Foo)()

        Dim called As Boolean = False

        Mock.Arrange(Sub() foo.Execute()).DoInstead(Sub() called = True)

        ' Act
        Dim iFoo As IFoo = foo
        iFoo.Execute()

        ' Assert
        Assert.IsTrue(called)
    End Sub
  {{endregion}}

In both examples a local variable is set to `true` once the `Execute` method is called.
