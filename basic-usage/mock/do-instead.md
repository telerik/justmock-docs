---
title: Do Instead
page_title: Do Instead | JustMock Documentation
description: Do Instead
previous_url: /basic-usage-mock-do-instead.html
slug: justmock/basic-usage/mock/do-instead
tags: do,instead
published: True
position: 3
---

# Do Instead

The `DoInstead` method is used to replace the actual implementation of a method with a mocked one. This topic goes through a number of scenarios where the `DoInstead` method is useful.

The following system under test will be used for the examples in this article:

  #### __[C#]__

  {{region DoInstead#IFooSUT}}
    public interface IFoo
    {
        int Bar { get; set; }
        string Execute(string str);
        int Submit(int arg1, int arg2, int arg3, int arg4);
    }
  {{endregion}}

  #### __[VB]__

  {{region DoInstead#IFooSUT}}
    Public Interface IFoo
        Property Bar() As Integer
        Function Execute(ByVal str As String) As String
        Function Submit(num1 As Integer, num2 As Integer, num3 As Integer, num4 As Integer) As Integer
    End Interface
  {{endregion}}


## Assert DoInstead

Let's see how to replace a method behavior, verify its call and its return value.

  #### __[C#]__

  {{region DoInstead#AssertDoInstead}}
    [TestMethod]
        public void ShouldAssertIfItsCalledAndReturnArgument()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            bool called = false;

            Mock.Arrange(() => foo.Execute(Arg.IsAny<string>()))
                      .DoInstead(() => { called = true; })
                      .Returns((string s) => s);

            // Act
            var actual = string.Empty;
            actual = foo.Execute("bar");

            // Assert
            Assert.AreEqual("bar", actual);
            Assert.IsTrue(called);    
        }
  {{endregion}}

  #### __[VB]__

  {{region DoInstead#AssertDoInstead}}
    <TestMethod()>
        Public Sub ShouldAssertIfItsCalledAndReturnArgument()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Dim called As Boolean = False

            Mock.Arrange(Function() foo.Execute(Arg.AnyString)).
                DoInstead(Sub() called = True).
                Returns(Function(s As String) s)

            ' Act
            Dim actual = String.Empty
            actual = foo.Execute("bar")

            ' Assert
            Assert.AreEqual("bar", actual)
            Assert.IsTrue(called)
        End Sub
  {{endregion}}

First, we arrange with `DoInstead` to execute `called = true;` instead of the actual implementation of `foo.Execute` method. Also, we set up that the call should return the passed argument directly. We act by calling the `foo.Execute` method with argument "bar" and then verify that the method actually returns what we expect.

## Assert DoInstead for Several Arguments

You can assert `DoInstead` for more than one argument in the method. Follows an example for asserting four arguments:

  #### __[C#]__

  {{region DoInstead#AssertDoInsteadSeveralArguments}}
    [TestMethod]
        public void ShouldReturnSumOfArguments()
        {
            // Arrange
            int expected = 0;

            var foo = Mock.Create<IFoo>();
            Mock.Arrange(() => foo.Submit(Arg.IsAny<int>(), Arg.IsAny<int>(), Arg.IsAny<int>(), Arg.IsAny<int>()))
                .DoInstead((int arg1, int arg2, int arg3, int arg4) => { expected = arg1 + arg2 + arg3 + arg4; });

            // Act
            foo.Submit(10, 10, 10, 10);

            // Assert
            Assert.AreEqual(40, expected);
        }
  {{endregion}}

  #### __[VB]__

  {{region DoInstead#AssertDoInsteadSeveralArguments}}
    <TestMethod()>
        Public Sub ShouldReturnSumOfArguments()
            ' Arrange
            Dim expected As Integer = 0

            Dim foo = Mock.Create(Of IFoo)()
            Mock.Arrange(Function() foo.Submit(Arg.AnyInt, Arg.AnyInt, Arg.AnyInt, Arg.AnyInt)).
                DoInstead(Function(arg1 As Integer, arg2 As Integer, arg3 As Integer, arg4 As Integer)
                              expected = arg1 + arg2 + arg3 + arg4
                          End Function)

            ' Act
            foo.Submit(10, 10, 10, 10)

            ' Assert
            Assert.AreEqual(40, expected)
        End Sub
  {{endregion}}

Here we replace the actual implementation of the `Submit` method and return the sum of the specified arguments.

## Assert DoInstead On Property Set

`DoInstead` can also be used to change the behavior of a property set. Here we arrange to set a boolean value instead of actually setting the `foo.Bar` property.

  #### __[C#]__

  {{region DoInstead#AssertDoInsteadOnPropertySet}}
    [TestMethod]
        public void ShouldCheckIfPropertySetIsCalledWithZero()
        {
            // Arrange
            var foo = Mock.Create<IFoo>(Behavior.Strict);

            bool expected = false;
            
            Mock.ArrangeSet(() => { foo.Bar = 1; }).DoInstead(() => expected = true);

            // Act
            foo.Bar = 1;

            // Assert
            Assert.IsTrue(expected);    
        }
  {{endregion}}

  #### __[VB]__

  {{region DoInstead#AssertDoInsteadOnPropertySet}}
    <TestMethod()>
        Public Sub ShouldCheckIfPropertySetIsCalledWithZero()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)(Behavior.Strict)

            Dim expected As Boolean = False

            Mock.ArrangeSet(Sub() foo.Bar = 0).DoInstead(Sub() expected = True)

            ' Act
            foo.Bar = 0

            ' Assert
            Assert.IsTrue(expected)
        End Sub
  {{endregion}}

First, we arrange with `DoInstead` to execute `expected = true;` instead of the actual implementation of `foo.Bar` property setter. We act by setting `foo.Bar` to `1` and verify that `expected` is true.

> Notice that we use `Behavior.Strict` for the mocked object.

## Using Parameters Passed To The Original Call In DoInstead

`DoInstead` can also use the parameters passed to the original call. Assume the following examples, with the methods `Echo` and `Bar` previously added to the `Foo` class:

  #### __[C#]__

  {{region DoInstead#Add}}
    public class Foo
    {
        public int Echo(int num)
        {
            return num;
        }

        public void Bar(Action action)
        {
            action();
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region DoInstead#Add}}
    Public Class Foo
        Public Shared Function Echo(num As Integer) As Integer
            Throw New NotImplementedException
        End Function

        Public Sub Bar(action As Action)
            action()
        End Sub
    End Class
  {{endregion}}

Now, in `DoInstead`, we will use the integer value passed to `Echo` to replace the actual implementation:

  #### __[C#]__

  {{region DoInstead#ParametersUsed}}
    [TestMethod]
        public void ShouldUseParametersPassedToOriginalCall()
        {
            // Arrange
            var foo = Mock.Create<Foo>();

            int expected = 4;
            int actual = 0;
            
            Mock.Arrange(() => foo.Echo(Arg.AnyInt)).DoInstead((int a) =>
            {
                actual = a;
            });
            
            // Act
            foo.Echo(expected);
            
            // Assert
            Assert.AreEqual(expected, actual);
        }
  {{endregion}}

  #### __[VB]__

  {{region DoInstead#ParametersUsed}}
    <TestMethod()>
        Public Sub ShouldUseParametersPassedToOriginalCall()
            ' Arrange
            Dim foo = Mock.Create(Of Foo)()

            Dim expected As Integer = 4
            Dim actual As Integer = 0

            Mock.Arrange(Function() foo.Echo(Arg.AnyInt)).DoInstead(Function(a As Integer)
                                                                        actual = a
                                                                    End Function)

            ' Act
            foo.Echo(expected)

            ' Assert
            Assert.AreEqual(expected, actual)
        End Sub
  {{endregion}}

In order to parameterise the `DoInstead` call to apply the `Echo` method, simply use an action in the body accepting `int` to access parameters passed to `Echo`. In the example we replace the actual implementation with `actual = a;`. We act by calling `foo.Echo( expected );` and verify the result.

Additionaly, you can test actions passed to the `DoInstead` call. Look at the following example:

  #### __[C#]__

  {{region DoInstead#ActionUsed}}
    [TestMethod]
        public void ShouldUseActionInDoInsteadCall()
        {
            // Arrange
            var foo = Mock.Create<Foo>();

            int expected = 4;
            int actual = 0;

            Mock.Arrange(() => foo.Bar(Arg.IsAny<Action>())).DoInstead((Action action) => action());

            // Act
            foo.Bar(new Action(() =>
            {
                actual = expected;
            }));

            // Assert
            Assert.AreEqual(expected, actual);
        }
  {{endregion}}

  #### __[VB]__

  {{region DoInstead#ActionUsed}}
    <TestMethod()>
        Public Sub ShouldUseActionInDoInsteadCall()
            ' Arrange
            Dim foo = Mock.Create(Of Foo)()

            Dim expected As Integer = 4
            Dim actual As Integer = 0

            Mock.Arrange(Sub() foo.Bar(Arg.IsAny(Of Action))).DoInstead(Function(action As Action)
                                                                            action()
                                                                        End Function)

            ' Act
            foo.Bar(New Action(Function()
                                   actual = expected
                               End Function))

            ' Assert
            Assert.AreEqual(expected, actual)
        End Sub
  {{endregion}}

We act by calling `foo.Bar` with new `Action`, changing the `actual` variable. In `DoInstead` we just execute the action. Finally, we verify that our expectations are met, namely `actual` and `expected` variables have the same value.

## Using out and ref Parameters Passed To The Original Call In DoInstead

In order to mock methods containing `out` and `ref` parameters, you need to specify a `delegate` describing your method's prototype. Here is the method we are going to mock. Note that it is `virtual` and the second parameter is declared as a `ref` parameter.

  #### __[C#]__

  {{region DoInstead#AssertDoInsteadRefParamMockTarget}}
    public class DoInsteadWithCustomDelegate
    {
        public virtual void AddTo(int arg1, ref int arg2)
        {
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region DoInstead#AssertDoInsteadRefParamMockTarget}}
    Public Class DoInsteadWithCustomDelegate
        Public Overridable Sub AddTo(arg1 As Integer, ByRef arg2 As Integer)
        End Sub
    End Class
  {{endregion}}

The idea behind this method is to have one parameter (`arg1`) the value of which is added to the second parameter(`arg2`). As a result of the method call the value of the second parameter will have changed as long as `arg1` is not 0. Let's create a test method that performs this logic using `DoInstead`.

  #### __[C#]__

  {{region DoInstead#AssertDoInsteadRefParam}}
    [TestMethod]
        public void ShouldTakeOutValueFromDoInsteadWhenDefinedWithCustomDelegate()
        {
            // Arrange
            int refArg = 1;

            var mock = Mock.Create<DoInsteadWithCustomDelegate>();

            Mock.Arrange(() => mock.AddTo(10, ref refArg)).DoInstead(new RefAction<int, int>((int arg1, ref int arg2) =>
            {
                arg2 += arg1;
            })); 

            // Act
            mock.AddTo(10, ref refArg);

            // Assert
            Assert.AreEqual(11, refArg);
        }
  {{endregion}}

  #### __[VB]__

  {{region DoInstead#AssertDoInsteadRefParam}}
    <TestMethod()>
        Public Sub ShouldTakeOutValueFromDoInsteadWhenDefinedWithCustomDelegate()
            ' Arrange
            Dim refArg As Integer = 1

            Dim DoInsteadWithCustomDelegateMock = Mock.Create(Of DoInsteadWithCustomDelegate)()


            Mock.Arrange(Sub() DoInsteadWithCustomDelegateMock.AddTo(10, refArg)) _
                .DoInstead(New RefAction(Of Integer, Integer)(Sub(arg As Integer, ByRef arg2 As Integer)
                                                                  arg2 += arg
                                                              End Sub))

            ' Act
            DoInsteadWithCustomDelegateMock.AddTo(10, refArg)

            ' Assert
            Assert.AreEqual(11, refArg)
        End Sub
  {{endregion}}

And here is the delegate we referenced in the `DoInstead` call:

  #### __[C#]__

  {{region DoInstead#AssertDoInsteadRefParamDelegate}}
    public delegate void RefAction<T1, T2>(T1 arg1, ref T2 arg2);
  {{endregion}}

  #### __[VB]__

  {{region DoInstead#AssertDoInsteadRefParamDelegate}}
    Public Delegate Sub RefAction(Of T1, T2)(arg1 As T1, ByRef arg2 As T2)
  {{endregion}}


## See Also


 * [Call Original]({%slug justmock/basic-usage/mock/call-original%})

 * [Do Nothing]({%slug justmock/basic-usage/mock/do-nothing%})[](b9461116-b200-4739-aff1-af8458c7095e)

 * [Must Be Called]({%slug justmock/basic-usage/mock/must-be-called%})

 * [Raise]({%slug justmock/basic-usage/mock/raise%})

 * [Raises]({%slug justmock/basic-usage/mock/raises%})

 * [Returns]({%slug justmock/basic-usage/mock/returns%})

 * [Throws]({%slug justmock/basic-usage/mock/throws%})
