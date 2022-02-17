---
title: Static Mocking
page_title: Static Mocking | JustMock Documentation
description: Static Mocking
previous_url: /advanced-usage-static-mocking.html
slug: justmock/advanced-usage/static-mocking
tags: static,mocking
published: True
position: 4
---

# Static Mocking

Static mocking is one of the advanced features supported in __Telerik® JustMock__. It allows you to fake static constructors, methods and properties calls, set expectations and verify results using the [AAA]({%slug justmock/basic-usage/arrange-act-assert%}) principle. 

We can divide static mocking into the following major parts: 

* Static constructor mocking
* Static method mocking
* Extension methods mocking

> This is an Elevated Feature. Refer to [this]({%slug justmock/licensing/license-agreement%}#commercial-vs-free-version) topic to learn more about the differences between both the commercial and free versions of Telerik JustMock.

Further in this topic we will use the following sample code to illustrate how to mock static constructors, methods and properties.

  #### __[C#]__

  {{region StaticMocking#Sample}}
    public class Foo
    {
        static Foo()
        {
            throw new NotImplementedException();
        }

        public static void Submit()
        {
        }

        public static int Execute(int arg)
        {
            throw new NotImplementedException();
        }

        public static int FooProp
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
    }

    internal class FooInternal
    {
        internal static void DoIt()
        {
            throw new NotImplementedException();
        }
    }

    public static class FooStatic
    {
        public static void Do()
        {
            throw new NotImplementedException();
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region StaticMocking#Sample}}
    Public Class Foo
        Shared Sub New()
            Throw New NotImplementedException()
        End Sub

        Public Shared Sub Submit()
            Throw New NotImplementedException()
        End Sub

        Public Shared Function Execute(arg As Integer) As Integer
            Throw New NotImplementedException()
        End Function

        Public Shared Property FooProp As Integer
            Get
                Throw New NotImplementedException()
            End Get
            Set(value As Integer)
                Throw New NotImplementedException()
            End Set
        End Property
    End Class

    Friend Class FooInternal
        Friend Shared Sub DoIt()
            Throw New NotImplementedException()
        End Sub
    End Class

    Public NotInheritable Class FooStatic
        Private Sub New()
        End Sub
        Public Shared Sub [Do]()
            Throw New NotImplementedException()
        End Sub
    End Class
  {{endregion}}


> **Important**
>
> To use static mocking you first need to go to elevated mode by enabling JustMock from the menu. [How to Enable/Disable ]({%slug justmock/advanced-usage%}#EnableDisableJustMock)

## Static Constructor Mocking

In the following example you will see how you can specify the behavior of the static constructor when the target type is mocked. The first thing you need to do is to set up the target type for mocking all static calls. You do this using one of the following calls to the `Mock.SetupStatic` method.

  #### __[C#]__

  {{region StaticMocking#SetupStaticConstructor}}
	void Mock.SetupStatic( Type staticType );
	void Mock.SetupStatic( Type targetType, Behavior behavior );
	void Mock.SetupStatic( Type staticType, StaticConstructor staticConstructor );
	void Mock.SetupStatic( Type targetType, Behavior behavior, StaticConstructor staticConstructor );
  {{endregion}}


As you can see you have a number of choices. You always have to provide the type of the target class you want to set up for mocking. You can also specify the behavior of the mock. The default behavior is [Behavior.Loose]({%slug justmock/basic-usage/mock-behaviors/loose%}).

The `StaticConstructor` parameter defines the default behavior of the static constructor. You can choose from the following values:

* NonMocked
* Mocked

The default value for this property is `NonMocked`.
      	
Let's see a complete example using the class `Foo` from the sample code in the beginning.

  #### __[C#]__

  {{region StaticMocking#StaticConstructor}}
    [TestMethod]
    public void ShouldArrangeStaticFunction()
    {
        // Arrange
        Mock.SetupStatic(typeof(Foo), StaticConstructor.Mocked);
        int expected = 0;

        Mock.Arrange(() => Foo.FooProp).Returns(0);
        
        // Assert
        Assert.AreEqual(expected, Foo.FooProp);  
    }
  {{endregion}}

  #### __[VB]__

  {{region StaticMocking#StaticConstructor}}
    <TestMethod()>
    Public Sub ShouldArrangeStaticFunction()
        ' Arrange
        Mock.SetupStatic(GetType(Foo), StaticConstructor.Mocked)
        Dim expected As Integer = 0

        Mock.Arrange(Function() Foo.FooProp).Returns(0)

        ' Assert            
        Assert.AreEqual(expected, Foo.FooProp)
    End Sub
  {{endregion}}

Here we have set up the static constructor mock of the target type `Foo`. Using the `StaticConstructor` parameter in the call to `SetupStatic` we have specified that we want to mock the call to the static constructor and therefore the call to the `Foo.FooProp` will not throw a `NotImplementedException`. 

## General Static Method Mocking
Let's start with the simplest example, namely how to mock the static `Submit` method. First, we need to call the following:

  #### __[C#]__

  {{region StaticMocking#SetupStaticCS}}
    Mock.SetupStatic(typeof(Foo), StaticConstructor.Mocked);
  {{endregion}}

    #### __[VB]__

  {{region StaticMocking#SetupStaticVB}}
	Mock.SetupStatic(GetType(Foo), StaticConstructor.Mocked)
  {{endregion}}

This call setups the `Foo` type for static mocking and prepares all the static methods as mockable.
        
This is actually the only difference between static and instance mocking. From now on you continue as if you are mocking instance methods.

  #### __[C#]__

  {{region StaticMocking#ActCS}}
    Foo.Submit();
  {{endregion}}

    #### __[VB]__

  {{region StaticMocking#ActVB}}
	Foo.Submit()
  {{endregion}}


In the `Submit` method implementation we throw an exception, but as we mocked the `Foo` class that exception should not be thrown. Finally, we can assert that the method was actually called.
      
  #### __[C#]__

  {{region StaticMocking#AssertCS}}
    Mock.Assert(() => Foo.Submit());
  {{endregion}}

    #### __[VB]__

  {{region StaticMocking#AssertVB}}
	Mock.Assert(Sub() Foo.Submit())
  {{endregion}}

With `Mock.SetupStatic(typeof(Foo), StaticConstructor.Mocked);` we setup that *all* static methods from this class will me mocked. In certain cases you'd need to mock only methods that are setup with `Mock.Arrange`. To achieve this you need to set the M:Telerik.JustMock.Behavior to `Strict` in the `Mock.SetupStatic` call.
        

  #### __[C#]__

  {{region StaticMocking#SetupStaticStrictMode}}
    [TestMethod]
    [ExpectedException(typeof(StrictMockException))]
    public void ShouldThrowWhenNotArranged()
    {
        //Arrange
        Mock.SetupStatic(typeof(Foo), Behavior.Strict, StaticConstructor.Mocked);

        Mock.Arrange(() => Foo.Execute(10)).Returns(10);
        
        //Assert
        Assert.AreEqual(10, Foo.Execute(10));

        // Act
        // throws MockException as there is no arrange associated with the Submit method
        Foo.Submit();
    }
  {{endregion}}

  #### __[VB]__

  {{region StaticMocking#SetupStaticStrictMode}}
    <TestMethod()>
    <ExpectedException(GetType(StrictMockException))>
    Public Sub ShouldThrowWhenNotArranged()
        ' Arrange
        Mock.SetupStatic(GetType(Foo), Behavior.Strict, StaticConstructor.Mocked)

        Mock.Arrange(Function() Foo.Execute(10)).Returns(10)

        ' Assert
        Assert.AreEqual(10, Foo.Execute(10))

        ' Act
        ' Throws MockException as there is no arrange associated with the Submit method.
        Foo.Submit()
    End Sub
  {{endregion}}


Once we set the M:Telerik.JustMock.Behavior to `Strict`, only calls setup through arrange are mocked. Other calls will throw a `MockException`.
        
Follows an example of mocking static method in F#:

  #### __[F#]__

  {{region StaticMocking#SetupStaticStrictMode}}
    [<Test()>]
		member this.ShouldMockStaticCall() =  
	
		Mock.SetupStatic<Foo>()
	
		Mock.Arrange(fun ignore -> Foo.EchoStatic()).Returns(1);
	
		Assert.AreEqual(1, Foo.EchoStatic())
  {{endregion}}

The static `EchoStatic` method is arranged to return `1`.

## Mocking Static Property Get

You can also set up a static property get:

  #### __[C#]__

  {{region StaticMocking#StaticGetOnProperty}}
    [TestMethod]
    public void ShouldFakeStaticPropertyGet()
    {
        // Arrange
        Mock.SetupStatic(typeof(Foo), Behavior.Strict, StaticConstructor.Mocked);
        
        bool called = false;

        Mock.Arrange(() => Foo.FooProp).DoInstead(() => { called = true; }).Returns(1);

        // Assert
        Assert.AreEqual(Foo.FooProp, 1);
        Assert.IsTrue(called);
    }
  {{endregion}}

  #### __[VB]__

  {{region StaticMocking#StaticGetOnProperty}}
    <TestMethod()>
    Public Sub ShouldFakeStaticPropertyGet()
        ' Arrange
        Mock.SetupStatic(GetType(Foo), Behavior.Strict, StaticConstructor.Mocked)

        Dim called As Boolean = False

        Mock.Arrange(Function() Foo.FooProp).DoInstead(Sub() called = True).Returns(1)

        ' Act
        Assert.AreEqual(Foo.FooProp, 1)
        Assert.IsTrue(called)
    End Sub
  {{endregion}}

We replace the actual implementation of `Foo.FooProp` with `called = true;` and return `1`. After acting we verify that the method was actually called with return value `1`.

## Mocking Static Property Set
Now, let's mock a static property set. In the example below, we arrange the `Foo.FooProp` property. We use `DoInstead` to set a local boolean variable to `true` once it is assigned `10`.  After that, we verify that what we have expected actually happened in our test.

  #### __[C#]__

  {{region StaticMocking#PropertySet}}
    [TestMethod]
    public void ShouldFakeStaticPropertySet()
    {
        // Arrange
        Mock.SetupStatic(typeof(Foo), Behavior.Strict, StaticConstructor.Mocked);

        bool called = false;

        Mock.ArrangeSet(() => { Foo.FooProp = 10; }).DoInstead(() => { called = true; });

        // Act - this line should not throw any mockexception.
        Foo.FooProp = 10;

        // Assert
        Assert.IsTrue(called);
    }
  {{endregion}}

  #### __[VB]__

  {{region StaticMocking#PropertySet}}
    <TestMethod()>
    Public Sub ShouldFakeStaticPropertySet()
        ' Arrange
        Mock.SetupStatic(GetType(Foo), Behavior.Strict, StaticConstructor.Mocked)

        Dim called As Boolean = False

        Mock.ArrangeSet(Sub() Foo.FooProp = 10).DoInstead(Sub() called = True)

        ' Act - this line should not throw any mockexception.
        Foo.FooProp = 10

        ' Assert
        Assert.IsTrue(called)
    End Sub
  {{endregion}}

Go to [Mock Properties]({%slug justmock/basic-usage/mock-properties%}) topic to learn more about mocking properties.

## Mocking Internal Static Call
Going further, you can mock internal methods like in the following example.

  #### __[C#]__

  {{region StaticMocking#InternalClassStaticMethod}}
    [TestMethod]
    public void ShouldFakeInternalStaticCall()
    {
        // Arrange
        Mock.SetupStatic<FooInternal>();

        // Act
        FooInternal.DoIt();
    }
  {{endregion}}

  #### __[VB]__

  {{region StaticMocking#InternalClassStaticMethod}}
    <TestMethod()>
    Public Sub ShouldFakeInternalStaticCall()
        ' Arrange
        Mock.SetupStatic(Of FooInternal)()

        ' Act
        FooInternal.DoIt()
    End Sub
  {{endregion}}

Here, calling `FooInternal.DoIt` should not throw an exception as you are  allowed to setup static internal methods.

## Mocking Static Class

In the demonstrated examples, the class itself that we mock is a *non static* class - only the methods are static. To mock a static class you need to use the non generic version of the `Mock.SetupStatic` method, i.e.
          	

  #### __[C#]__

  {{region StaticMocking#StaticClass}}
    [TestMethod]
    public void ShouldMockStaticClass()
    {
        // Arrange
        Mock.SetupStatic(typeof(FooStatic));

        // Act - doesn't throw MockException
        FooStatic.Do();
    }
  {{endregion}}

  #### __[VB]__

  {{region StaticMocking#StaticClass}}
    <TestMethod()>
    Public Sub ShouldMockStaticClass()
        ' Arrange
        Mock.SetupStatic(GetType(FooStatic))

        ' Act - doesn't throw MockException
        FooStatic.[Do]()
    End Sub
  {{endregion}}


## Mocking Static Members Across Threads

Mocking static members across all threads is an unsafe operation that may compromise the stability of the testing framework. Arrangements on static members are valid only for the current thread by default. To make an arrangement on a static member valid on all threads, add the .OnAllThreads() clause to the arrangement:

  #### __[C#]__

  {{region }}
    Mock.Arrange(() => DateTime.Now).Returns(new DateTime()).OnAllThreads();
  {{endregion}}


## Mocking Current HttpContext
Here is an example how to mock the *current HTTP context*.

We arrange a call to `HttpContext.Current` to set a local variable to `true`. Note that the original implementation of `HttpContext.Current` won`t be executed.
    		

  #### __[C#]__

  {{region StaticMocking#MockHTTPContext}}
    [TestMethod]
    public void ShouldAssertMockingHttpContext()
    {
        // Arrange
        bool called = false;
        Mock.Arrange(() => HttpContext.Current).DoInstead(() => called = true);

        // Act
        var ret = HttpContext.Current;

        // Assert
        Assert.IsTrue(called);
    }
  {{endregion}}

  #### __[VB]__

  {{region StaticMocking#MockHTTPContext}}
    <TestMethod> _
    Public Sub ShouldAssertMockingHttpContext()
        ' Arrange
        Dim called As Boolean = False
        Mock.Arrange(Function() HttpContext.Current).DoInstead(Sub() called = True)

        ' Act
        Dim ret = HttpContext.Current

        ' Assert
        Assert.IsTrue(called)
    End Sub
  {{endregion}}

After acting we verify against our expectations.

## Mocking Extension Methods

Extension methods enable you to "add" methods to existing types without creating a new derived type, recompiling, or otherwise modifying the original type. Extension methods are a special kind of static methods, but they are called as if they were instance methods on the extended type.

Mocking extension method is similar to mocking any instance methods. The only difference is that we don’t need `Mock.Create<T>()` call to initialize the class for mocking as extension mocking is by default partial.
        
Let's see an example of how to mock extension methods. Consider the following class:

  #### __[C#]__

  {{region StaticMocking#SampleClass}}
    public class Bar
    {
        public void Execute()
        {
            throw new NotImplementedException();
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region StaticMocking#SampleClass}}
    Public Class Bar
        Public Sub Execute()
            Throw New NotImplementedException()
        End Sub
    End Class
  {{endregion}}


And a class that contains extension methods for the `Foo` class:
        

  #### __[C#]__

  {{region StaticMocking#FooExtensions}}
    public static class BarExtensions
    {
        public static int Echo(this Bar foo, int arg)
        {
            return default(int);
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region StaticMocking#FooExtensions}}
    Public Module BarExtensions
        <System.Runtime.CompilerServices.Extension()>
        Public Function Echo(foo As Bar, arg As Integer) As Integer
            Return 0
        End Function
    End Module
  {{endregion}}


Let's mock the `Echo` extension method.           
        

  #### __[C#]__

  {{region StaticMocking#MockExtensionMethod}}
    [TestMethod]
    public void ShouldFakeExtensionMethod()
    {
        // Arrange
        var foo = new Bar();

        Mock.Arrange(() => foo.Echo(10)).Returns(11);

        // Act
        var actual = foo.Echo(10);

        // Assert
        Assert.AreEqual(11, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region StaticMocking#MockExtensionMethod}}
    <TestMethod()>
    Public Sub ShouldFakeExtensionMethod()
        ' Arrange
        Dim foo = New Bar()

        Mock.Arrange(Function() foo.Echo(10)).Returns(11)

        ' Act
        Dim actual = foo.Echo(10)

        ' Assert
        Assert.AreEqual(11, actual)
    End Sub
  {{endregion}}


First we create an instance of the `Foo` class. Notice that we create a standard instance of the class, not a mock instance. Then we setup the call to `Echo` through `Arrange` in the same way we do it for non static methods. Finally, we assert the return value as usual.

## See Also

 * [Behavior.Loose]({%slug justmock/basic-usage/mock-behaviors/loose%})

 * [Mock Properties]({%slug justmock/basic-usage/mock-properties%})
