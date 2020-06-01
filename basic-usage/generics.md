---
title: Generics
page_title: Generics | JustMock Documentation
description: Generics
previous_url: /basic-usage-generics.html
slug: justmock/basic-usage/generics
tags: generics
published: True
position: 4
---

# Generics

__Telerik® JustMock__ allows you to mock generic classes/interfaces/methods in the same way as you do it for non-generic ones.

In the next examples we will use the following three sample classes to test:

  #### __[C#]__

  {{region Generics#FooGeneric}}
    public class FooGeneric
    {
        public virtual TRet Get<T, TRet>(T arg1)
        {
            return default(TRet);
        }

        public virtual int Get<T>()
        {
            throw new NotImplementedException();
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region Generics#FooGeneric}}
    Public Class FooGeneric
        Public Overridable Function [Get](Of T, TRet)(arg1 As T) As TRet
            Return Nothing
        End Function

        Public Overridable Function [Get](Of T)() As Integer
            Throw New NotImplementedException()
        End Function
    End Class
  {{endregion}}


  #### __[C#]__

  {{region Generics#FooGenericRef}}
    public class FooGenericByRef
    {
        public void Submit<T>(out T arg1)
        {
            arg1 = default(T);
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region Generics#FooGenericRef}}
    Public Class FooGenericByRef
        Public Sub Submit(Of T)(ByRef arg1 As T)
            arg1 = Nothing
        End Sub
    End Class
  {{endregion}}


  #### __[C#]__

  {{region Generics#FooGenericOfT}}
    public class FooGeneric<T>
    {
        public virtual T Get(T arg)
        {
            throw new NotImplementedException();
        }
        public virtual T Get(T arg, T arg2)
        {
            throw new NotImplementedException();
        }

        public virtual void Execute<T1>(T1 arg)
        {
            throw new Exception();
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region Generics#FooGenericOfT}}
    Public Class FooGeneric(Of T)
        Public Overridable Function [Get](arg As T) As T
            Throw New NotImplementedException()
        End Function

        Public Overridable Function [Get](arg As T, arg2 As T) As T
            Throw New NotImplementedException()
        End Function

        Public Overridable Sub Execute(Of T1)(arg As T1)
            Throw New Exception()
        End Sub
    End Class
  {{endregion}}


## Distinguish Generic Method Calls with Different Arguments

Set up a call to a generic method and distinguish calls depending on the argument type.

  #### __[C#]__

  {{region Generics#DistinguiseMethodWithDifferentGenericArgument}}
    [TestMethod]
    public void ShouldDistinguishCallsDependingOnArgumentTypes()
    {
        //Arrange
        var foo = Mock.Create<FooGeneric>();

        int expextedCallWithInt = 0;
        int expextedCallWithString = 1;

        Mock.Arrange(() => foo.Get<int>()).Returns(expextedCallWithInt);
        Mock.Arrange(() => foo.Get<string>()).Returns(expextedCallWithString);

        //Act
        int actualCallWithInt = foo.Get<int>();
        int actualCallWithString = foo.Get<string>();

        //Assert
        Assert.AreEqual(expextedCallWithInt, actualCallWithInt);
        Assert.AreEqual(expextedCallWithString, actualCallWithString);
    }
  {{endregion}}

  #### __[VB]__

  {{region Generics#DistinguiseMethodWithDifferentGenericArgument}}
    <TestMethod()>
    Public Sub ShouldDistinguishCallsDependingOnArgumentTypes()
        ' Arrange
        Dim foo = Mock.Create(Of FooGeneric)()

        Dim expectedCallWithInt As Integer = 0
        Dim expectedCallWithString As Integer = 1
        Mock.Arrange(Function() foo.Get(Of Integer)()).Returns(expectedCallWithInt)
        Mock.Arrange(Function() foo.Get(Of String)()).Returns(expectedCallWithString)

        ' Act
        Dim actualCallWithInt As Integer = foo.Get(Of Integer)()
        Dim actualCallWithString As Integer = foo.Get(Of String)()

        'Assert
        Assert.AreEqual(expectedCallWithInt, actualCallWithInt)
        Assert.AreEqual(expectedCallWithString, actualCallWithString)
    End Sub
  {{endregion}}

We arrange the `Get<T>` method to return different values when called with either `int` or `string` argument.

The syntax for mocking generic method is:

  {{region }}
    Mock.Arrange(() => <class_name>.<method_name><type>())...;
  {{endregion}}

After that you act in the same way as you do it for non-generic methods except that you specify argument type. That is:

  {{region }}
    int actualCallWithInt = foo.Get<int>();
  {{endregion}}

## Mock a Generic Method with Out Argument

Set up a call to a generic method with out argument.

  #### __[C#]__

  {{region Generics#MockGenericMethodWithOut}}
    [TestMethod]
    public void ShouldMockAGenericMethodWithOutArgs()
    {
        //Arrange
        var foo = Mock.Create<FooGenericByRef>();

        string expected = "ping";

        Mock.Arrange(() => foo.Submit<string>(out expected));

        //Act
        string actual = string.Empty;     
        foo.Submit<string>(out actual);

        //Assert
        Assert.AreEqual(expected, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region Generics#MockGenericMethodWithOut}}
    <TestMethod()>
    Public Sub ShouldMockAGenericMethodWithOutArgs()
        ' Arrange
        Dim foo = Mock.Create(Of FooGenericByRef)()

        Dim expected As String = "ping"

        Mock.Arrange(Sub() foo.Submit(Of String)(expected))

        ' Act
        Dim actual As String = String.Empty
        foo.Submit(Of String)(actual)

        ' Assert
        Assert.AreEqual(expected, actual)
    End Sub
  {{endregion}}

In the arrange statement we use the `out` keyword and pass the already initialized variable. It is of type `string` as we specified the type argument of the `Submit` method to `string`. Thus, we arrange that a call to `Submit` should set the out argument to `"ping"`.

## Mock a Generic Class

Set up a call to a method of a generic class.

  #### __[C#]__

  {{region Generics#MockGenericClass}}
    [TestMethod]
    public void ShouldMockGenericClass()
    {
        //Arrange
        var foo = Mock.Create<FooGeneric<int>>();

        int expectedValue = 1;

        Mock.Arrange(() => foo.Get(Arg.IsAny<int>())).Returns(expectedValue);

        //Act
        int actualValue = foo.Get(0);

        //Assert
        Assert.AreEqual(expectedValue, actualValue);
    }
  {{endregion}}

  #### __[VB]__

  {{region Generics#MockGenericClass}}
    <TestMethod()>
    Public Sub ShouldMockGenericClass()
        ' Arrange
        Dim foo = Mock.Create(Of FooGeneric(Of Integer))()

        Dim expectedValue As Integer = 1

        Mock.Arrange(Function() foo.Get(Arg.IsAny(Of Integer)())).Returns(expectedValue)

        ' Act
        Dim actualValue As Integer = foo.Get(0)

        ' Assert
        Assert.AreEqual(expectedValue, actualValue)
    End Sub
  {{endregion}}

In this example we mock the generic class `FooGeneric<int>`. The only difference from mocking non-generic classes is specifying the argument type in the `Mock.Create` call. After that you act in the same manner when calling non-generic methods.

Here is another example for mocking the `Get` method:

  #### __[C#]__

  {{region Generics#MockGenericClassAndReturn}}
    [TestMethod]
    public void ShouldMockPropertyGet()
    {
        //Arrange
        var genericClass = Mock.Create<FooGeneric<int>>();

        Mock.Arrange(() => genericClass.Get(1, 1)).Returns(10);

        //Assert
        Assert.AreEqual(genericClass.Get(1, 1), 10);
    }
  {{endregion}}

  #### __[VB]__

  {{region Generics#MockGenericClassAndReturn}}
    <TestMethod()>
    Public Sub ShouldMockPropertyGet()
        ' Arrange
        Dim genericClass = Mock.Create(Of FooGeneric(Of Integer))()

        Mock.Arrange(Function() genericClass.Get(1, 1)).Returns(10)

        ' Assert
        Assert.AreEqual(genericClass.Get(1, 1), 10)
    End Sub
  {{endregion}}

In this example we specify that the method accepts only two specific arguments and will return `10`.

You can also mock void methods of generic classes. In the next example we mock the `Execute` method by replacing the implementation so that it sets a boolean value to true.

  #### __[C#]__

  {{region Generics#MockGenericClassVoidMethod}}
    [TestMethod]
    public void ShouldMockMethodInGenericClass()
    {
        //Arrange
        var genericClass = Mock.Create<FooGeneric<int>>();

        bool called = false;

        Mock.Arrange(() => genericClass.Execute(1)).DoInstead(() => called = true);

        //Act
        genericClass.Execute(1);

        //Assert
        Assert.IsTrue(called);
    }
  {{endregion}}

  #### __[VB]__

  {{region Generics#MockGenericClassVoidMethod}}
    <TestMethod()>
    Public Sub ShouldMockMethodInGenericClass()
        ' Arrange
        Dim genericClass = Mock.Create(Of FooGeneric(Of Integer))()

        Dim called As Boolean = False

        Mock.Arrange(Sub() genericClass.Execute(1)).DoInstead(Sub() called = True)

        ' Act
        genericClass.Execute(1)

        ' Assert
        Assert.IsTrue(called)
    End Sub
  {{endregion}}


## Mock Generic Method in Non-generic Class

Generic methods in non-generic class can also be mocked.

Here is an example:

  #### __[C#]__

  {{region Generics#MockGenericMethodNonGenericClass}}
    [TestMethod]
    public void ShouldMockGenericMethodInNonGenericClass()
    {
        //Arrange
        var genericClass = Mock.Create<FooGeneric>();

        Mock.Arrange(() => genericClass.Get<int, int>(1)).Returns(10);

        //Assert
        Assert.AreEqual(genericClass.Get<int, int>(1), 10);
    }
  {{endregion}}

  #### __[VB]__

  {{region Generics#MockGenericMethodNonGenericClass}}
    <TestMethod()>
    Public Sub ShouldMockNonGenericMethodInGenericClass()
        ' Arrange
        Dim genericClass = Mock.Create(Of FooGeneric)()

        Mock.Arrange(Function() genericClass.Get(Of Integer, Integer)(1)).Returns(10)

        ' Assert            
        Assert.AreEqual(genericClass.Get(Of Integer, Integer)(1), 10)
    End Sub
  {{endregion}}

First, we arrange that the `Get` method will be called only with `int` argument `1` and return `int` value `10`. After that, we verify.

