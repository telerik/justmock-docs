---
title: Matchers
page_title: Matchers | JustMock Documentation
description: Matchers
previous_url: /basic-usage-matchers.html
slug: justmock/basic-usage/matchers
tags: matchers
published: True
position: 8
---

# Matchers

Matchers let you ignore passing actual values as arguments used in mocks. Instead, they give you the possibility to pass just an expression that satisfies the argument type or the expected value range. For example, if a method accepts a string as a first parameter, you don’t need to pass a specific string like "Camera". Instead, you can use __Arg.IsAny<string>()__. 

There are several types of matchers supported in TelerikJustMock:

1. Defined Matchers (`Arg.AnyBool`, `Arg.AnyDouble`, `Arg.AnyFloat`, `Arg.AnyGuid`, `Arg.AnyInt`, `Arg.AnyLong`, `Arg.AnyObject`, `Arg.AnyShort`, `Arg.AnyString`, `Arg.NullOrEmpty`)
1. `Arg.IsAny<T>();`
1. `Arg.IsInRange(`[FromValue : int], [ToValue : int], [RangeKind]`)`
1. `Arg.Matches<T>(Expression<Predicate<T>> expression)`
1. `Arg.Ref()`

Matchers also let you ignore all arguments in your mocks by a single call to `IgnoreArguments()` (in the arrangement) or `Args.Ignore()` (in the assertion).

To take a look at each one of these features in details, we will use the following interfaces:

  #### __[C#]__

  {{region Matchers#IFooClass}}
    public interface IFoo
    {
        int Echo(int intArg1);
        int Echo(int intArg1, int intArg2);
        int Bar(ref int intArg1);
    }
  {{endregion}}

  #### __[VB]__

  {{region Matchers#IFooClass}}
    Public Interface IFoo
        Function Echo(ByVal int As Integer) As Integer
        Function Echo(ByVal int1 As Integer, ByVal int As Integer) As Integer
        Function Bar(ByRef int1 As Integer) As Integer
    End Interface
  {{endregion}}


  #### __[C#]__

  {{region Matchers#IPaymentService}}
    public interface IPaymentService
    {
        void ProcessPayment(DateTime dateTi, decimal deci);
    }
  {{endregion}}

  #### __[VB]__

  {{region Matchers#IPaymentService}}
    Public Interface IPaymentService
        Sub ProcessPayment(ByVal dateTi As DateTime, ByVal deci As Decimal)
    End Interface
  {{endregion}}


## Arg.IsAny<T>();
We've already used this matcher in one of our examples earlier.

  #### __[C#]__

  {{region Matchers#ArgAnyCS}}
	Mock.Arrange(() => warehouse.HasInventory(Arg.IsAny<string>(), Arg.IsAny<int>())).Returns(true);
  {{endregion}}

  #### __[VB]__

  {{region Matchers#ArgAnyVB}}
    Mock.Arrange(Function() warehouse.HasInventory(Arg.IsAny(Of String)(), Arg.IsAny(Of Integer)())).Returns(True)
  {{endregion}}

This matcher specifies that when the __HasInventory__ method is called with any string as a first argument and any int as a second it should return __true__.

## Arg.IsInRange(int from, int to, RangeKind range)

The IsInRange matcher lets us arrange a call for an expected value range. With the __RangeKind__ argument we can specify whether the given range includes or excludes its boundaries.
Consider the following example:

  #### __[C#]__

  {{region Matchers#ArgRangeInclusiveCS}}
	Mock.Arrange(() => foo.Echo(Arg.IsInRange(0, 5, RangeKind.Inclusive))).Returns(true);
  {{endregion}}

  #### __[VB]__

  {{region Matchers#ArgRangeInclusiveVB}}
    Mock.Arrange(Function() foo.Echo(Arg.IsInRange(0, 5, RangeKind.Inclusive))).Returns(True)
  {{endregion}}


With the first line we specify that when the __foo.Echo__ method is called with an argument value ranging from 0 to 5, the method will return __true__. We specify the __RangeKind__ to be Inclusive which means that calling the method with 0 or 5 satisfies the range condition and it will return __true__ for these values.

  #### __[C#]__

  {{region Matchers#ArgRangeExclusiveCS}}
	Mock.Arrange(() => foo.Echo(Arg.IsInRange(0, 5, RangeKind.Exclusive))).Returns(true);
  {{endregion}}

  #### __[VB]__

  {{region Matchers#ArgRangeExclusiveVB}}
    Mock.Arrange(Function() foo.Echo(Arg.IsInRange(0, 5, RangeKind.Exclusive))).Returns(True)
  {{endregion}}


On the second line we specify the __RangeKind__ to be __Exclusive__ so calling the method with 6 or 10 doesn’t satisfy the condition because these values are excluded from the range.

## Arg.Matches<T> (Expression<Predicate<T>> expression)

This is the most flexible matcher and it allows you to specify your own matching expression. Let's illustrate it with a simple example:

  #### __[C#]__

  {{region Matchers##ArgMatchesCS}}
	Mock.Arrange(() => foo.Echo(Arg.Matches<int>( x => x < 10)).Returns(true);
  {{endregion}}

  #### __[VB]__

  {{region Matchers##ArgMatchesVB}}
    Mock.Arrange(Function() foo.Echo(Arg.Matches(Of Integer)(Function(x) x < 10))).Returns(True)
  {{endregion}}


With our expression (or predicate) __x => x < 10__ we specify that a call to __foo.Echo__ with an argument less than 10 should return __true__.

## Ignoring All Arguments in Arrangement

To set argument independent expectations against certain method, you have to apply `IgnoreArguments` to its arrangement. The next example demonstrates this functionality:

  #### __[C#]__

  {{region Matchers#IgnoringAllArgumentsForASpecificExpectation}}
    [TestMethod]
        public void IgnoringAllArgumentsForASpecificExpectation()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            Mock.Arrange(() => foo.Echo(0, 0)).IgnoreArguments().Returns(10);

            // Act
            int actual = foo.Echo(10, 200);

            // Assert
            Assert.AreEqual(10, actual);
        }
  {{endregion}}

  #### __[VB]__

  {{region Matchers#IgnoringAllArgumentsForASpecificExpectation}}
    <TestMethod>
        Public Sub IgnoringAllArgumentsForASpecificExpectation()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Function() foo.Echo(0, 0)).IgnoreArguments().Returns(10)

            ' Act
            Dim actual As Integer = foo.Echo(10, 200)

            ' Assert
            Assert.AreEqual(10, actual)
        End Sub
  {{endregion}}

After creating mock of the `IFoo`, we arrange that every `Echo` call, no matter its arguments should return 10. For this, in the arrangement we pass arguments from their corresponding types and then we apply the `IgnoreArguments` functionality.

## Using Matchers in Assert
Matchers are also useful in assertion. Consider a fictitious payment service where we don't care what the payment date is, but we want to make sure that the payment amount which is passed to the service is $54.44. We can easily achieve this by using the following statement:

  #### __[C#]__

  {{region Matchers#Assertion}}
    [TestMethod]
        public void ShouldUseMatchersInAssert()
        {
            // Arrange
            var paymentService = Mock.Create<IPaymentService>();

            // Act
            paymentService.ProcessPayment(DateTime.Now, 54.44M);

            // Assert
            Mock.Assert(() => paymentService.ProcessPayment(
                Arg.IsAny<DateTime>(),
                Arg.Matches<decimal>(paymentAmount => paymentAmount == 54.44M)));
        }
  {{endregion}}

  #### __[VB]__

  {{region Matchers#Assertion}}
    <TestMethod()>
        Public Sub ShouldUseMatchersInAssert()
            ' Arrange
            Dim paymentService = Mock.Create(Of IPaymentService)()

            ' Act
            paymentService.ProcessPayment(DateTime.Now, 54.44D)

            ' Assert
            Mock.Assert(Sub() paymentService.ProcessPayment(
                            Arg.IsAny(Of DateTime)(), 
                            Arg.Matches(Of Decimal)(Function(paymentAmount) paymentAmount = 54.44D)))
        End Sub
  {{endregion}}

We assert for calling `ProcessPayment` with whatever `DateTime` argument and payment amount exactly `$54.44`.

Here is another example. We specify that an `Echo` call with arguments `10` and `20` should return `30`. We use the matchers `Arg.Matches<int>(x => x == 10)` and `Arg.Matches<int>(x => x == 20)` to handle the case when we pass `10` and `20` to the `Echo` method.

  #### __[C#]__

  {{region Matchers#Assertion2}}
    [TestMethod]
        public void ShouldUseMatchersInArrange()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            Mock.Arrange(() => foo.Echo(Arg.Matches<int>(x => x == 10), Arg.Matches<int>(x => x == 20))).Returns(30);

            // Act
            int ret = foo.Echo(10, 20);

            // Assert
            Assert.AreEqual(30, ret);
        }
  {{endregion}}

  #### __[VB]__

  {{region Matchers#Assertion2}}
    <TestMethod()>
        Public Sub ShouldUseMatchersInArrange()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Function() foo.Echo(Arg.Matches(Of Integer)(Function(x) x = 10), Arg.Matches(Of Integer)(Function(x) x = 20))).Returns(30)

            ' Act
            Dim ret As Integer = foo.Echo(10, 20)

            ' Assert
            Assert.AreEqual(30, ret)

        End Sub
  {{endregion}}


## Using Matchers to Ignore All Arguments in Assert
You already saw how you can use matchers in your assertion calls. If you need you can specify that you want to ignore all arguments in the assert call. You do this by adding the `Args.Ignore()` argument to the `Mock.Assert` call. Here is an example:

  #### __[C#]__

  {{region Matchers#IgnoreAll}}
    [TestMethod]
        public void UsingMatchersInAssertExample2()
        {
            // Arrange
            var paymentService = Mock.Create<IPaymentService>();

            // Act
            paymentService.ProcessPayment(DateTime.Now, 54.44M);

            // Assert
            Mock.Assert(() => paymentService.ProcessPayment(new DateTime(), 0), Args.Ignore());
        }
  {{endregion}}

  #### __[VB]__

  {{region Matchers#IgnoreAll}}
    <TestMethod()>
        Public Sub ShouldIgnoreArgumentsInAssert()
            ' Arrange
            Dim paymentService = Mock.Create(Of IPaymentService)()

            ' Act
            paymentService.ProcessPayment(DateTime.Now, 54.44D)

            ' Assert
            Mock.Assert(Sub() paymentService.ProcessPayment(New DateTime(), 0), Args.Ignore())
        End Sub
  {{endregion}}

In this way we assert for calling `ProcessPayment` no matter the arguments.

## Using Matchers and Specializations
In an arrangement you can define more than one matcher. Consider the following example:

  #### __[C#]__

  {{region Matchers#Specialization}}
    [TestMethod]
        [ExpectedException(typeof(ArgumentException))]
        public void UsingMatchersAndSpecializations()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            Mock.Arrange(() => foo.Echo(Arg.AnyInt))
                .Returns(10)
                .OccursOnce();
            Mock.Arrange(() => foo.Echo(Arg.Matches<int>(x => x > 10)))
                .Throws(new ArgumentException());

            // Act
            int actual = foo.Echo(1);

            // Assert
            Assert.AreEqual(10, actual);

            // Act
            foo.Echo(11);
        }
  {{endregion}}

  #### __[VB]__

  {{region Matchers#Specialization}}
    <TestMethod()> _
        <ExpectedException(GetType(ArgumentException))> _
        Public Sub ShouldUseMatchersAndSpecializations()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Function() foo.Echo(Arg.AnyInt)).Returns(10)
            Mock.Arrange(Function() foo.Echo(Arg.Matches(Of Integer)(Function(x) x > 10))).Throws(New ArgumentException())

            ' Act
            Dim actual As Integer = foo.Echo(1)

            ' Assert
            Assert.AreEqual(10, actual)

            ' Act
            foo.Echo(11)
        End Sub
  {{endregion}}

In the case when a specialization is used among with `Arg.IsAny<T>`, JustMock will select the arrangement with the proper matcher. `foo.Echo(1)` will use the first matcher and will return `10`. But `foo.Echo(11)` will use the second matcher because it is a specialization of the first and applies the call, therefore `ArgumentException` will be thrown.

## Using Matchers for ref Arguments (Arg.Ref())

>  __`Arg.Ref` is used only in C# assemblies, as for Visual Basic you can directly match `ByRef` arguments without it.__ 

With JustMock you are able to use matchers for functions that take ref arguments. For this purpose, you need to use the `Arg.Ref().Value` syntax. Check the next examples for further guidance:

* Here we will use the `Arg.Ref` matcher, in order to match function that will be called with a ref argument with known __value__.

	  #### __[C#]__
	
	  {{region Matchers#MatchingCertainRefParams}}
	    [TestMethod]
	        public void MatchingCertainRefParameters()
	        {
	            int myRefArg = 5;
	
	            // Arrange
	            var foo = Mock.Create<IFoo>();
	
	            Mock.Arrange(() => foo.Bar(ref Arg.Ref(5).Value)).Returns(10);
	
	            // Act
	            int actual = foo.Bar(ref myRefArg);
	
	            // Assert
	            Assert.AreEqual(10, actual);
	            Assert.AreEqual(5, myRefArg);
	        }
	  {{endregion}}
	
	  #### __[VB]__
	
	  {{region Matchers#MatchingCertainRefParams}}
	    <TestMethod>
	        Public Sub MatchingCertainRefParameters()
	            Dim myRefArg As Integer = 5
	
	            ' Arrange
	            Dim foo = Mock.Create(Of IFoo)()
	
	            Mock.Arrange(Function() foo.Bar(5)).Returns(10)
	
	            ' Act
	            Dim actual As Integer = foo.Bar(myRefArg)
	
	            ' Assert
	            Assert.AreEqual(10, actual)
	            Assert.AreEqual(5, myRefArg)
	        End Sub
	  {{endregion}}
	
	Note, the using of the *Value* field at the end of the matcher (`Arg.Ref(5)`__.Value__) is intended and needed in order to use this feature.

* Here we will use the `Arg.Ref` matcher, in order to match function that will be called with a ref argument with known __type__. To match the type, we will use a defined matcher.

	  #### __[C#]__
	
	  {{region Matchers#MatchingRefParamsOfAnyType}}
	    [TestMethod]
	        public void MatchingRefParametersOfAnyType()
	        {
	            int myRefArg = 5;
	
	            // Arrange
	            var foo = Mock.Create<IFoo>();
	
	            Mock.Arrange(() => foo.Bar(ref Arg.Ref(Arg.AnyInt).Value)).Returns(10);
	
	            // Act
	            int actual = foo.Bar(ref myRefArg);
	
	            // Assert
	            Assert.AreEqual(10, actual);
	            Assert.AreEqual(5, myRefArg);
	        }
	  {{endregion}}
	
	  #### __[VB]__
	
	  {{region Matchers#MatchingRefParamsOfAnyType}}
	    <TestMethod>
	        Public Sub MatchingRefParametersOfAnyType()
	            Dim myRefArg As Integer = 5
	
	            ' Arrange
	            Dim foo = Mock.Create(Of IFoo)()
	
	            Mock.Arrange(Function() foo.Bar(Arg.AnyInt)).Returns(10)
	
	            ' Act
	            Dim actual As Integer = foo.Bar(myRefArg)
	
	            ' Assert
	            Assert.AreEqual(10, actual)
	            Assert.AreEqual(5, myRefArg)
	        End Sub
	  {{endregion}}
	
	Note, the using of the *Value* field at the end of the matcher (`Arg.Ref(Arg.AnyInt)`__.Value__) is intended and needed in order to use this feature.

* Here we will use the `Arg.Ref` matcher, in order to match function that will be called with a ref argument with known __type__ and value interval. We will use `Arg.Matches<T>()` for this.

	  #### __[C#]__
	
	  {{region Matchers#MatchingRefParamsWithSpecialization}}
	    [TestMethod]
	        public void MatchingRefParametersWithSpecialization()
	        {
	            int myRefArg = 11;
	
	            // Arrange
	            var foo = Mock.Create<IFoo>();
	
	            Mock.Arrange(() => foo.Bar(ref Arg.Ref(Arg.Matches<int>(x=> x > 10)).Value)).Returns(10);
	
	            // Act
	            int actual = foo.Bar(ref myRefArg);
	
	            // Assert
	            Assert.AreEqual(10, actual);
	            Assert.AreEqual(11, myRefArg);
	        }
	  {{endregion}}
	
	  #### __[VB]__
	
	  {{region Matchers#MatchingRefParamsWithSpecialization}}
	    <TestMethod>
	        Public Sub MatchingRefParametersWithSpecialization()
	            Dim myRefArg As Integer = 11
	
	            ' Arrange
	            Dim foo = Mock.Create(Of IFoo)()
	
	            Mock.Arrange(Function() foo.Bar(Arg.Matches(Of Integer)(Function(x) x > 10))).Returns(10)
	
	            ' Act
	            Dim actual As Integer = foo.Bar(myRefArg)
	
	            ' Assert
	            Assert.AreEqual(10, actual)
	            Assert.AreEqual(11, myRefArg)
	        End Sub
	  {{endregion}}
	
	Note, the using of the *Value* field at the end of the matcher (`Arg.Ref(Arg.Matches<int>(x => x > 10))`__.Value__) is intended and needed in order to use this feature.

