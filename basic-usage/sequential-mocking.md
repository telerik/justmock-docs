---
title: Sequential Mocking
page_title: Sequential Mocking | JustMock Documentation
description: Sequential Mocking
previous_url: /basic-usage-sequential-mocking.html
slug: justmock/basic-usage/sequential-mocking
tags: sequential,mocking
published: True
position: 10
---

# Sequential Mocking

Sequential mocking allows you to return different values on the same or different consecutive calls to one and the same type. In other words, you can set up expectations for successive calls of the same type.

For the examples in this article we will use the following sample code to test:

  #### __[C#]__

  {{region SequentialMocking#SampleCode}}
    public interface IFoo
    {
        string Execute(string arg);
        int Echo(int arg1);
        int GetIntValue();
    }
  {{endregion}}

  #### __[VB]__

  {{region SequentialMocking#SampleCode}}
    Public Interface IFoo
        Function Execute(arg As String) As String
        Function Echo(arg1 As Integer) As Integer
        Function GetIntValue() As Integer
    End Interface
  {{endregion}}


## Step by step example
Consider the above code. We have a simple interface named `IFoo` and it has a method named `GetIntValue` that returns `int`. We want to make sure that on three different successive calls, it returns three different predefined values.

  #### __[C#]__

  {{region SequentialMocking#StepByStepExample}}
    [TestMethod]
        public void ShouldArrangeAndAssertInASequence()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            Mock.Arrange(() => foo.GetIntValue()).Returns(0).InSequence();
            Mock.Arrange(() => foo.GetIntValue()).Returns(1).InSequence();
            Mock.Arrange(() => foo.GetIntValue()).Returns(2).InSequence();

            // Act
            int actualFirstCall = foo.GetIntValue();
            int actualSecondCall = foo.GetIntValue();
            int actualThirdCall = foo.GetIntValue();

            // Assert
            Assert.AreEqual(0, actualFirstCall);
            Assert.AreEqual(1, actualSecondCall);
            Assert.AreEqual(2, actualThirdCall);  
        }
  {{endregion}}

  #### __[VB]__

  {{region SequentialMocking#StepByStepExample}}
    <TestMethod()>
        Public Sub ShouldArrangeAndAssertInASequence()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Function() foo.GetIntValue()).Returns(0).InSequence()
            Mock.Arrange(Function() foo.GetIntValue()).Returns(1).InSequence()
            Mock.Arrange(Function() foo.GetIntValue()).Returns(2).InSequence()

            ' Act
            Dim actualFirstCall As Integer = foo.GetIntValue()
            Dim actualSecondCall As Integer = foo.GetIntValue()
            Dim actualThirdCall As Integer = foo.GetIntValue()

            ' Assert
            Assert.AreEqual(0, actualFirstCall)
            Assert.AreEqual(1, actualSecondCall)
            Assert.AreEqual(2, actualThirdCall)
        End Sub
  {{endregion}}

In the arrange section, we setup that: 

*  the first call to `GetIntValue` returns `0`
*  the second call returns `1`
*  the third call returns `2`

 We achieve that by just calling `InSequence()` on the `Arrange`. This modifier instructs TelerikJustMock to return the expected result in the order you specify.

> **Note**
>
> In this example, every subsequent call to `foo.GetIntValue` will result in returning the last specified arrangement, namely `2`.


## Assert Sequence with a Matcher
You can arrange consecutive calls to one and the same method passing different arguments to return different values.

  #### __[C#]__

  {{region SequentialMocking#AssertSequenceWithMatcher}}
    [TestMethod]
        public void ShouldAssertSequentlyWithAMatchers()
        {
            // Arrange
            var iFoo = Mock.Create<IFoo>();

            Mock.Arrange(() => iFoo.Execute("foo")).Returns("hello").InSequence();
            Mock.Arrange(() => iFoo.Execute(Arg.IsAny<string>())).Returns("bye").InSequence();

            // Act
            string actualFirstCall = iFoo.Execute("foo");
            string actualSecondCall = iFoo.Execute("bar");
            string actualThirdCall = iFoo.Execute("foobar"); ;

            // Assert
            Assert.AreEqual("hello", actualFirstCall);
            Assert.AreEqual("bye", actualSecondCall);
            Assert.AreEqual("bye", actualThirdCall);  
        }
  {{endregion}}

  #### __[VB]__

  {{region SequentialMocking#AssertSequenceWithMatcher}}
    <TestMethod()>
        Public Sub ShouldAssertSequentlyWithAMatchers()
            ' Arrange
            Dim iFoo = Mock.Create(Of IFoo)()

            Mock.Arrange(Function() iFoo.Execute("foo")).Returns("hello").InSequence()
            Mock.Arrange(Function() iFoo.Execute(Arg.IsAny(Of String)())).Returns("bye").InSequence()

            ' Act
            Dim actualFirstCall As String = iFoo.Execute("foo")
            Dim actualSecondCall As String = iFoo.Execute("bar")
            Dim actualThirdCall As String = iFoo.Execute("foobar")

            ' Assert
            Assert.AreEqual("hello", actualFirstCall)
            Assert.AreEqual("bye", actualSecondCall)
            Assert.AreEqual("bye", actualThirdCall)
        End Sub
  {{endregion}}

In sequential mocking you can still use the power of other features, like [Matchers]({%slug justmock/basic-usage/matchers%}), to write more complete and precise tests.

> **Important**
>
> Remember that if you arrange two calls, but actually perform more than two calls, all calls after the second one will return the value specified to be returned in the second arrange.

## Assert Multiple Calls with Different Matcher
To extend the previous example, we can arrange consecutive calls to methods to return different values depending on a condition specified with a [matcher]({%slug justmock/basic-usage/matchers%}).

  #### __[C#]__

  {{region SequentialMocking#AssertMultipleForDifferentMatchers}}
    [TestMethod]
        public void ShouldAssertMultipleCallsWithDifferentMatchers()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            Mock.Arrange(() => foo.Echo(Arg.Matches<int>(x => x > 10))).Returns(10).InSequence();
            Mock.Arrange(() => foo.Echo(Arg.Matches<int>(x => x > 20))).Returns(20).InSequence();

            // Act
            int actualFirstCall = foo.Echo(11);
            int actualSecondCall = foo.Echo(21);

            // Assert
            Assert.AreEqual(10, actualFirstCall);
            Assert.AreEqual(20, actualSecondCall);
        }
  {{endregion}}

  #### __[VB]__

  {{region SequentialMocking#AssertMultipleForDifferentMatchers}}
    <TestMethod()>
        Public Sub ShouldAssertMultipleCallsWithDifferentMatchers()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Function() foo.Echo(Arg.Matches(Of Integer)(Function(x) x > 10))).Returns(10).InSequence()
            Mock.Arrange(Function() foo.Echo(Arg.Matches(Of Integer)(Function(x) x > 20))).Returns(20).InSequence()

            ' Act
            Dim actualFirstCall As Integer = foo.Echo(11)
            Dim actualSecondCall As Integer = foo.Echo(21)

            ' Assert
            Assert.AreEqual(10, actualFirstCall)
            Assert.AreEqual(20, actualSecondCall)
        End Sub
  {{endregion}}


## Returns and InSequence
When using `Returns` you can chain calls like in the following example of `InSequence` alternative.

> This feature is enabled when Telerik.JustMock.Helpers namespace is included.


  #### __[C#]__

  {{region SequentialMocking#ReturnsAlternative}}
    [TestMethod]
        public void ShouldArrangeInSequencedReturns()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            Mock.Arrange(() => foo.Echo(Arg.AnyInt)).Returns(10).Returns(11).MustBeCalled();

            // Assert
            Assert.AreEqual(10, foo.Echo(1));
            Assert.AreEqual(11, foo.Echo(2));
            Mock.Assert(foo);
        }
  {{endregion}}

  #### __[VB]__

  {{region SequentialMocking#ReturnsAlternative}}
    <TestMethod()>
        Public Sub ShouldArrangeInSequencedReturns()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Function() foo.Echo(Arg.AnyInt)).Returns(10).Returns(11).MustBeCalled()

            ' Assert
            Assert.AreEqual(10, foo.Echo(1))
            Assert.AreEqual(11, foo.Echo(2))
            Mock.Assert(foo)
        End Sub
  {{endregion}}

The effect is the same as using separate arranges for the `foo` object. The first call will return 10 and the second - 11.

Another possible approach is to use the `ReturnsMany` helper method, like so:

  #### __[C#]__

  {{region SequentialMocking#ReturnsManyAlternative}}
    [TestMethod]
        public void ShouldArrangeReturnsMany()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            int[] returnValues = new int[3] { 1, 2, 3 };

            Mock.Arrange(() => foo.Echo(Arg.AnyInt)).ReturnsMany(returnValues);

            // Act
            var actualFirstCall = foo.Echo(10);
            var actualSecondCall = foo.Echo(10);
            var actualThirdCall = foo.Echo(10);
            var actualFourthCall = foo.Echo(10);

            // Assert
            Assert.AreEqual(1, actualFirstCall);
            Assert.AreEqual(2, actualSecondCall);
            Assert.AreEqual(3, actualThirdCall);
        }
  {{endregion}}

  #### __[VB]__

  {{region SequentialMocking#ReturnsManyAlternative}}
    <TestMethod>
        Public Sub ShouldArrangeReturnsMany()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Dim returnValues As Integer() = New Integer(2) {1, 2, 3}

            Mock.Arrange(Function() foo.Echo(Arg.AnyInt)).ReturnsMany(returnValues)

            ' Act
            Dim actualFirstCall = foo.Echo(10)
            Dim actualSecondCall = foo.Echo(10)
            Dim actualThirdCall = foo.Echo(10)

            ' Assert
            Assert.AreEqual(1, actualFirstCall)
            Assert.AreEqual(2, actualSecondCall)
            Assert.AreEqual(3, actualThirdCall)
        End Sub
  {{endregion}}

The `ReturnsMany` method will arrange certain function/property to sequentially return the members of a predefined array (`returnValues`). If the calls to that function/property exceed the array members during the act phase, the last arranged value will become default. This is shown in the next example:

  #### __[C#]__

  {{region SequentialMocking#ReturnsManyAlternative2}}
    [TestMethod]
        public void ShouldAutoArrangeForExceededValues()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            int[] returnValues = new int[3] { 1, 2, 3 };

            Mock.Arrange(() => foo.Echo(Arg.AnyInt)).ReturnsMany(returnValues);

            // Act
            var actualFirstCall = foo.Echo(10);
            var actualSecondCall = foo.Echo(10);
            var actualThirdCall = foo.Echo(10);
            var actualFourthCall = foo.Echo(10);
            var actualFifthCall = foo.Echo(10);

            // Assert
            Assert.AreEqual(1, actualFirstCall);
            Assert.AreEqual(2, actualSecondCall);
            Assert.AreEqual(3, actualThirdCall);
            Assert.AreEqual(3, actualFourthCall);
            Assert.AreEqual(3, actualFifthCall);
        }
  {{endregion}}

  #### __[VB]__

  {{region SequentialMocking#ReturnsManyAlternative2}}
    <TestMethod>
        Public Sub ShouldAutoArrangeForExceededValues()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Dim returnValues As Integer() = New Integer(2) {1, 2, 3}

            Mock.Arrange(Function() foo.Echo(Arg.AnyInt)).ReturnsMany(returnValues)

            ' Act
            Dim actualFirstCall = foo.Echo(10)
            Dim actualSecondCall = foo.Echo(10)
            Dim actualThirdCall = foo.Echo(10)
            Dim actualFourthCall = foo.Echo(10)
            Dim actualFifthCall = foo.Echo(10)

            ' Assert
            Assert.AreEqual(1, actualFirstCall)
            Assert.AreEqual(2, actualSecondCall)
            Assert.AreEqual(3, actualThirdCall)
            Assert.AreEqual(3, actualFourthCall)
            Assert.AreEqual(3, actualFifthCall)
        End Sub
  {{endregion}}


## See Also


 * [Returns]({%slug justmock/basic-usage/mock/returns%})

 * [Matchers]({%slug justmock/basic-usage/matchers%})
