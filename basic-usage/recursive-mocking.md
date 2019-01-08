---
title: Recursive Mocking
page_title: Recursive Mocking | JustMock Documentation
description: Recursive Mocking
previous_url: /basic-usage-recursive-mocking.html
slug: justmock/basic-usage/recursive-mocking
tags: recursive,mocking
published: True
position: 11
---

# Recursive Mocking

Recursive mocks enable you to mock members that are obtained as a result of "chained" calls on a mock. For example, recursive mocking is useful in the cases when you test code like this: `foo.Bar.Baz.Do("x")`.

In the next examples we will use the following sample code to test:

  #### __[C#]__

  {{region RecursiveMocks#SampleCode}}
    public interface IFoo
    {
        IBar Bar { get; set; }
        string Do(string command);
    }

    public interface IBar
    {
        int Value { get; set; }
        string Do(string command);
        IBaz Baz { get; set; }
    }

    public interface IBaz
    {
        string Do(string command);
    }
  {{endregion}}

  #### __[VB]__

  {{region RecursiveMocks#SampleCode}}
    Public Interface IFoo
        Property Bar() As IBar
        Function [Do](command As String) As String
    End Interface

    Public Interface IBar
        Property Value() As Integer
        Function [Do](command As String) As String
        Property Baz() As IBaz
    End Interface

    Public Interface IBaz
        Function [Do](command As String) As String
    End Interface
  {{endregion}}


## Step by step example
Consider the above code. Let's arrange that a call to `IFoo.Do` method returns a particular string. `IFoo` contains an `IBar` which, in turn, also contains a `Do` method. `IBar.Do` method can be accessed via a recursive call to `IFoo.Bar`. To arrange the call directly from `IFoo`, we just need to chain it considering the fact that TelerikJustMock will automatically create the necessary mock for `IBar`.

  #### __[C#]__

  {{region RecursiveMocks#AssertNestedVeriables}}
    [TestMethod]
        public void ShouldAssertNestedVeriables()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            string ping = "ping";

            Mock.Arrange(() => foo.Do(ping)).Returns("ack");
            Mock.Arrange(() => foo.Bar.Do(ping)).Returns("ack2");

            // Act
            var actualFooDo = foo.Do(ping);
            var actualFooBarDo = foo.Bar.Do(ping);

            // Assert
            Assert.AreEqual("ack", actualFooDo);
            Assert.AreEqual("ack2", actualFooBarDo);
        }
  {{endregion}}

  #### __[VB]__

  {{region RecursiveMocks#AssertNestedVeriables}}
    <TestMethod()>
        Public Sub ShouldAssertNestedVeriables()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Dim ping As String = "ping"

            Mock.Arrange(Function() foo.Do(ping)).Returns("ack")
            Mock.Arrange(Function() foo.Bar.Do(ping)).Returns("ack2")

            ' Act
            Dim actualFooDo = foo.Do(ping)
            Dim actualFooBarDo = foo.Bar.Do(ping)

            ' Assert
            Assert.AreEqual("ack", actualFooDo)
            Assert.AreEqual("ack2", actualFooBarDo)
        End Sub
  {{endregion}}

However, if `foo.Bar` is not arranged, it will still be instantiated. You can verify this by the following example:

  #### __[C#]__

  {{region RecursiveMocks#AssertNotInstantiated}}
    [TestMethod]
        public void ShouldNotInstantiateFooBar()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            // Assert
            Assert.IsNotNull(foo.Bar);
        }
  {{endregion}}

  #### __[VB]__

  {{region RecursiveMocks#AssertNotInstantiated}}
    <TestMethod()>
        Public Sub ShouldNotInstantiateFooBar()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            ' Assert
            Assert.IsNotNull(foo.Bar)
        End Sub
  {{endregion}}


## Nested Property Calls
The following example shows how to assert nested property get calls.

  #### __[C#]__

  {{region RecursiveMocks#AssertNestedPropertyGetSetups}}
    [TestMethod]
        public void ShouldAssertNestedPropertyGet()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();
            Mock.Arrange(() => foo.Bar.Value).Returns(10);

            // Act
            var actual = foo.Bar.Value;

            // Assert
            Assert.AreEqual(10, actual);
        }
  {{endregion}}

  #### __[VB]__

  {{region RecursiveMocks#AssertNestedPropertyGetSetups}}
    <TestMethod()>
        Public Sub ShouldAssertNestedPropertyGet()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Function() foo.Bar.Value).Returns(10)

            ' Act
            Dim actual = foo.Bar.Value

            ' Assert
            Assert.AreEqual(10, actual)
        End Sub
  {{endregion}}

Here we arrange `foo.Bar.Value` to return `10`.

You can also arrange property set. Here is an example:

  #### __[C#]__

  {{region RecursiveMocks#AssertNestedPropertySetSetups}}
    [TestMethod]
        [ExpectedException(typeof(StrictMockException))]
        public void ShouldAssertNestedPropertySet()
        {
            // Arrange
            var foo = Mock.Create<IFoo>(Behavior.Strict);

            Mock.ArrangeSet(() => { foo.Bar.Value = 5; }).DoNothing();

            // Act
            foo.Bar.Value = 5;
            foo.Bar.Value = 10; // This line will throw an exception.
        }
  {{endregion}}

  #### __[VB]__

  {{region RecursiveMocks#AssertNestedPropertySetSetups}}
    <TestMethod()>
        <ExpectedException(GetType(StrictMockException))>
        Public Sub ShouldAssertNestedPropertySet()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)(Behavior.Strict)

            Mock.ArrangeSet(Function() foo.Bar.Value = 5).DoNothing()

            ' Act
            foo.Bar.Value = 5
            foo.Bar.Value = 10 ' This line will throws an exception.
        End Sub
  {{endregion}}

We use `Bahavior.Strict` to enable only arranged calls and to reject any other calls. Only setting `foo.Bar.Value` to `5` is allowed and as we set it to `10`, an exception will be thrown. After assertion we actually set it to `5` and if we verify `foo` at that point an exception won't be thrown.

## Nested Property and Method Calls
We can call properties recursively, or we can do it with methods as well.

  #### __[C#]__

  {{region RecursiveMocks#AssertNestedPropertyHavingMethods}}
    [TestMethod]
        public void NestedPropertyAndMethodCalls()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            Mock.Arrange(() => foo.Bar.Do("x")).Returns("xit");
            Mock.Arrange(() => foo.Bar.Baz.Do("y")).Returns("yit");

            // Act
            var actualFooBarDo = foo.Bar.Do("x");
            var actualFooBarBazDo = foo.Bar.Baz.Do("y");

            // Assert
            Assert.AreEqual("xit", actualFooBarDo);
            Assert.AreEqual("yit", actualFooBarBazDo);
        }
  {{endregion}}

  #### __[VB]__

  {{region RecursiveMocks#AssertNestedPropertyHavingMethods}}
    <TestMethod()>
        Public Sub ShouldAssertNestedPropertyHavingMethods()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Function() foo.Bar.Do("x")).Returns("xit")
            Mock.Arrange(Function() foo.Bar.Baz.Do("y")).Returns("yit")

            ' Act
            Dim actualFooBarDo = foo.Bar.Do("x")
            Dim actualFooBarBazDo = foo.Bar.Baz.Do("y")

            ' Assert
            Assert.AreEqual("xit", actualFooBarDo)
            Assert.AreEqual("yit", actualFooBarBazDo)
        End Sub
  {{endregion}}

In this arrangement we specify that when calling `foo.Bar.Do` and `foo.Bar.Baz.Do` methods `"xit"` and `"yit"` should be returned respectively.

