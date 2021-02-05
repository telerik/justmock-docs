---
title: Returns
page_title: Returns | JustMock Documentation
description: Returns
previous_url: /basic-usage-mock-returns.html
slug: justmock/basic-usage/mock/returns
tags: returns
published: True
position: 7
---

# Returns

The `Returns` method is used with non void calls to ignore the actual call and return a custom value. This topic goes through a number of scenarios where the `Returns` method is useful. For them, we will be using the following interface:

  #### __[C#]__

  {{region Returns#IFooSUT}}
    public interface IFoo
    {
        int Bar { get; set; }
        int Echo(int myInt);
        int Execute(int myInt1, int myInt2);
    }
  {{endregion}}

  #### __[VB]__

  {{region Returns#IFooSUT}}
    Public Interface IFoo
        Function Execute(ByVal int1 As Integer, ByVal int2 As Integer)
        Property Bar As Integer
        Function Echo(ByVal int As Integer) As Integer
    End Interface
  {{endregion}}

You may additionally want to check the[Create Mocks By Example]({%slug justmock/basic-usage/create-mocks-by-example%}) article.

## Assert Property Get Call

With `Returns` method you can change the return value from a property get call.

  #### __[C#]__

  {{region Returns#AssertPropertyGetCall}}
    [TestMethod]
    public void ShouldAssertPropertyGetCall()
    {
        // Arrange
        var foo = Mock.Create<IFoo>();

        Mock.Arrange(() => foo.Bar).Returns(10);


        // Act
        var actual = 0;
        actual = foo.Bar;

        // Assert
        Assert.AreEqual(10, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region Returns#AssertPropertyGetCall}}
    <TestMethod()>
    Public Sub ShouldAssertPropertyGetCall()
        ' Arrange
        Dim foo = Mock.Create(Of IFoo)()

        Mock.Arrange(Function() foo.Bar).Returns(10)

        ' Act
        Dim actual = 0
        actual = foo.Bar

        ' Assert
        Assert.AreEqual(10, actual)
    End Sub
  {{endregion}}

In this example we arrange the `Bar` property get to return `10` when called. By acting with `actual = foo.Bar;` we assign `10` to `actual`, as `foo.Bar` will result in `10`. Finally, a verification is asserted.

## Assert Method Call with Matcher

A common case is to mock a method call to return a custom value in conjunction with a [matcher]({%slug justmock/basic-usage/matchers%}).

  #### __[C#]__

  {{region Returns#AssertCallWithMatcher}}
    [TestMethod]
    public void ShouldAssertMethodCallWithMatcher1()
    {
        // Arrange
        var foo = Mock.Create<IFoo>();

        Mock.Arrange(() => foo.Echo(Arg.IsAny<int>())).Returns((int i) => ++i);

        // Act
        var actual = foo.Echo(10); 

        // Assert
        Assert.AreEqual(11, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region Returns#AssertCallWithMatcher}}
    <TestMethod()>
    Public Sub ShouldAssertMethodCallWithMatcher1()
        ' Arrange
        Dim foo = Mock.Create(Of IFoo)()

        Mock.Arrange(Function() foo.Echo(Arg.IsAny(Of Integer)())).Returns(Function(i As Integer) System.Threading.Interlocked.Increment(i))

        ' Act
        Dim actual = foo.Echo(10)

        ' Assert
        Assert.AreEqual(11, actual)
    End Sub
  {{endregion}}

Here we use an `Arg.IsAny` matcher for the call to match calls to `foo.Echo` with any `int` argument. In the `Returns` method, instead of using a simple `int` value, we use a function to return the passed value incremented with 1.

You may also use a more complicated matcher. For example, you can make arrangement for passing exactly `10` to `Echo` method. With the following line of code we return exactly what has been passed:

  #### __[C#]__

  {{region Returns#AssertCallWithMatcher2}}
    [TestMethod]
    public void ShouldAssertMethodCallWithMatcher2()
    {
        // Arrange
        var foo = Mock.Create<IFoo>();

        Mock.Arrange(() => foo.Echo(Arg.Matches<int>(x => x == 10))).Returns((int i) => i);

        // Act
        var actual = foo.Echo(10);

        // Assert
        Assert.AreEqual(10, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region Returns#AssertCallWithMatcher2}}
    <TestMethod()>
    Public Sub ShouldAssertMethodCallWithMatcher2()
        ' Arrange
        Dim foo = Mock.Create(Of IFoo)()

        Mock.Arrange(Function() foo.Echo(Arg.Matches(Of Integer)(Function(x) x = 10))).Returns(Function(i As Integer) i)

        ' Act
        Dim actual As Integer = foo.Echo(10)

        ' Assert
        Assert.AreEqual(10, actual)
    End Sub
  {{endregion}}

The following example mocks the `Execute` method so that it returns its second argument. We use *lambda expression* in the `Returns` body to select the desired argument. After we have arranged we act by calling `foo.Execute(100, 10)` and verify that it has returned actually what we expect, namely `10`. Additionally, we verify that a call with any `int` as first argument and exactly `10` as second has been made.

  #### __[C#]__

  {{region Returns#AssertCallWithMatcherAndReturn}}
    [TestMethod]
    public void ShouldReturnWhateverSecondArgIs()
    {
        // Arrange
        var foo = Mock.Create<IFoo>();

        Mock.Arrange(() => foo.Execute(Arg.IsAny<int>(), Arg.IsAny<int>())).Returns((int id, int i) => i);

        // Act, Assert
        Assert.AreEqual(foo.Execute(100, 10), 10);

        Mock.Assert(() => foo.Execute(Arg.IsAny<int>(), Arg.Matches<int>(x => x == 10)));
    }
  {{endregion}}

  #### __[VB]__

  {{region Returns#AssertCallWithMatcherAndReturn}}
    <TestMethod()>
    Public Sub ShouldReturnWhateverSecondArgIs()
        ' Arrange
        Dim foo = Mock.Create(Of IFoo)()

        Mock.Arrange(Function() foo.Execute(Arg.AnyInt, Arg.AnyInt)).Returns(Function(id As Integer, i As Integer) i)

        ' Act, Assert
        Assert.AreEqual(foo.Execute(100, 10), 10)

        Mock.Assert(Sub() foo.Execute(Arg.AnyInt, Arg.Matches(Of Integer)(Function(x) x = 10)))
    End Sub
  {{endregion}}

Follows an example, showing mocking method in F#:

  #### __[F#]__

  {{region Returns#AssertCallWithMatcherAndReturn}}
    [<Test()>]
	member this.ShouldMockMethodCallsWithReturn() =

		let monkey = Mock.Create<IMonkey>()
	
		Mock.Arrange(monkey, fun ignore -> monkey.Echo()).Returns(10).MustBeCalled()  
	
		let result = monkey.Echo()
	
		Mock.Assert(monkey)
  {{endregion}}

We arrange the `Echo` method to return `10` and expect that it will actually be called in our test. After acting by calling the method, we verify the arrangement. Furthermore, you can assert that `result` has the value of `10`.

> Refer to the [Matchers]({%slug justmock/basic-usage/matchers%}) help topic for more information about using matchers.

## Executing Mocked Method In Same Test Method

Assume we have the following class:

  #### __[C#]__

  {{region Returns#FooSUT}}
    public class Foo
    {
        public int Echo(int myInt)
        {
            throw new NotImplementedException();
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region Returns#FooSUT}}
    Public Class Foo
        Public Function Echo(num As Integer) As Integer
            Throw New NotImplementedException
        End Function
    End Class
  {{endregion}}

JustMock supports executing mocked method in same test method without necessarily passing the type as an argument.

  #### __[C#]__

  {{region Returns#SameTestMethod}}
    [TestMethod]
    public void ShouldExecuteMockForSameInstanceInSameContext()
    {
        // Arrange
        var foo = Mock.Create<Foo>();

        Mock.Arrange(() => foo.Echo(Arg.AnyInt)).IgnoreInstance().Returns((int arg1) => arg1);

        // Assert
        Assert.AreEqual(new Foo().Echo(10), 10);
    }
  {{endregion}}

  #### __[VB]__

  {{region Returns#SameTestMethod}}
    <TestMethod()>
    Public Sub ShouldExecuteMockForSameInstanceInSameContext()
        ' Arrange
        Dim foo = Mock.Create(Of Foo)()

        Mock.Arrange(Function() foo.Echo(Arg.AnyInt)).IgnoreInstance().Returns(Function(arg1 As Integer) arg1)

        ' Assert
        Assert.AreEqual(New Foo().Echo(10), 10)
    End Sub
  {{endregion}}

In this example, `Echo` is called on a new `Foo` instance, instead of a mocked one. However, the arrangement is applied.

# See Also

 *  __[Matchers]({%slug justmock/basic-usage/matchers%})__

 * [Call Original]({%slug justmock/basic-usage/mock/call-original%})

 * [Do Nothing]({%slug justmock/basic-usage/mock/do-nothing%})

 * [Do Instead]({%slug justmock/basic-usage/mock/do-instead%})

 * [Must Be Called]({%slug justmock/basic-usage/mock/must-be-called%})

 * [Raise]({%slug justmock/basic-usage/mock/raise%})

 * [Raises]({%slug justmock/basic-usage/mock/raises%})

 * [Throws]({%slug justmock/basic-usage/mock/throws%})

 * [Create Mocks By Example]({%slug justmock/basic-usage/create-mocks-by-example%})
