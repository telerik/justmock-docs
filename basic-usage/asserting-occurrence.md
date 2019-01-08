---
title: Asserting Occurrence
page_title: Asserting Occurrence | JustMock Documentation
description: Asserting Occurrence
previous_url: /basic-usage-asserting-occurrence.html
slug: justmock/basic-usage/asserting-occurrence
tags: asserting,occurrence
published: True
position: 7
---

# Asserting Occurrence

Occurrence is used in conjunction with `Mock.Assert` and `Mock.AssertSet` to determine how many times a call has occurred.

There are 6 types of occurrence:

* [Occurs.Never()](#occursnever) - Specifies that a particular call is never made on a mock. 
* [Occurs.Once()](#occursonce) - Specifies that a call has occurred only once on a mock. 
* [Occurs.AtLeastOnce()](#occursatleastonce) - Specifies that a call has occurred at least once on a mock. 
* [Occurs.AtLeast(numberOfTimes)](#occursatleastnumberoftimes) - Specifies the least number of times a call should occur on a mock. 
* [Occurs.AtMost(numberOfTimes)](#occursatmostnumberoftimes) - Specifies the most number of times a call should occur on a mock. 
* [Occurs.Exactly(numberOfTimes)](#occursexactlynumberoftimes) - Specifies exactly the number of times a call should occur on a mock. 

Furthermore, you can set occurrence directly in the arrangement of a method.

You can use one of 5 different constructs of Occur: 

* [Occurs(numberOfTimes)](#occursnumberoftimes) - Specifies the exact number of times a call should occur on a mock. 
* [OccursOnce()](#occursonce) - Specifies that a call should occur only once on a mock. 
* [OccursNever()](#occursnever) - Specifies that a particular call should never be made on a mock. 
* [OccursAtLeast(numberOfTimes)](#occursatleastnumberoftimes) - Specifies that a call should occur at least once on a mock. 
* [OccursAtMost(numberOfTimes)](#occursatmostnumberoftimes) - Specifies the number of times at most a call should occur on a mock. 

JustMock also enables you to verify the calls order, with:

* [InOrder()](#verifying-calls-order) - Specifies exactly the order, a call should occur on the mock. 

__In the following examples we will use the following interface to test:__

  #### __[C#]__

  {{region Occurrence#IFoo}}
    public interface IFoo
    {
        void Submit();
        int Echo(int intArg);
    }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#IFoo}}
    Public Interface IFoo
        Sub Submit()
        Function Echo(intArg As Integer) As Integer
    End Interface
  {{endregion}}


## Occurs.Never
`Occurs.Never` is used to verify that a method/property hasn't been called during the execution of the test.

  #### __[C#]__

  {{region Occurrence#Never}}
    [TestMethod]
        public void ShouldOccursNever()
        {
            //Arrange
            var foo = Mock.Create<IFoo>();

            //Assert
            Mock.Assert(() => foo.Submit(), Occurs.Never());
        }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#Never}}
    <TestMethod()>
        Public Sub ShouldOccursNever()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            ' Assert
            Mock.Assert(Sub() foo.Submit(), Occurs.Never())
        End Sub
  {{endregion}}

In the example above, we verify that `foo.Submit()` has never been called.

## Occurs.Once
`Occurs.Once` is used to verify that a method/property has been called exactly one time during the execution of the test.

  #### __[C#]__

  {{region Occurrence#Once}}
    [TestMethod]
        public void ShouldOccursOnce()
        {
            //Arrange
            var foo = Mock.Create<IFoo>();

            //Act
            foo.Submit();

            //Assert
            Mock.Assert(() => foo.Submit(), Occurs.Once());
        }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#Once}}
    <TestMethod()>
        Public Sub ShouldOccursOnce()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            ' Act
            foo.Submit()

            ' Assert
            Mock.Assert(Sub() foo.Submit(), Occurs.Once())
        End Sub
  {{endregion}}

In the example above, we verify that `foo.Submit()` has been called once.

## Occurs.AtLeastOnce
`Occurs.AtLeastOnce` is used to verify that a method/property has been called at least once during the execution of the test.

  #### __[C#]__

  {{region Occurrence#AtLeastOnce}}
    [TestMethod]
        public void ShouldOccursAtLeastOnce()
        {
            //Arrange
            var foo = Mock.Create<IFoo>();

            //Act
            foo.Submit();

            //Assert 
            Mock.Assert(() => foo.Submit(), Occurs.AtLeastOnce());

            // Act - call Submit more than once
            foo.Submit();
            foo.Submit();

            // Assert - that should pass again
            Mock.Assert(() => foo.Submit(), Occurs.AtLeastOnce());
        }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#AtLeastOnce}}
    <TestMethod()>
        Public Sub ShouldOccursAtLeastOnce()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            ' Act
            foo.Submit()

            ' Assert
            Mock.Assert(Sub() foo.Submit(), Occurs.AtLeastOnce())

            ' Act - call Submit more than once
            foo.Submit()
            foo.Submit()

            ' Assert - that should pass again
            Mock.Assert(Sub() foo.Submit(), Occurs.AtLeastOnce())
        End Sub
  {{endregion}}

The example above shows that the `Occurs.AtLeastOnce` requirement is met whenever the method is called one or more times.

## Occurs.AtLeast(numberOfTimes)
With `Occurs.AtLeast` you specify at least how many times you want to verify that a given method/property has been called during the execution of the test.

> The next example uses NUnit Testing Framework.


  #### __[C#]__

  {{region Occurrence#AtLeastNTimes}}
    [TestMethod]
        public void ShouldOccursAtLeastCertainNumberOfTimes()
        {
            //Arrange
            var foo = Mock.Create<IFoo>();

            //Act
            foo.Submit();
            foo.Submit();

            //Assert
            NUnit.Framework.Assert.Throws<AssertFailedException>(() => Mock.Assert(() => foo.Submit(), Occurs.AtLeast(3)));

            foo.Submit();

            Mock.Assert(() => foo.Submit(), Occurs.AtLeast(3));
        }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#AtLeastNTimes}}
    <TestMethod()>
        Public Sub ShouldOccursAtLeastCertainNumberOfTimes()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            ' Act
            foo.Submit()
            foo.Submit()

            ' Assert - foo.Submit was called only twice so this throws an exception
            NUnit.Framework.Assert.Throws(Of AssertFailedException)(Sub() Mock.Assert(Sub() foo.Submit(), Occurs.AtLeast(3)))

            ' Act - now, foo.Submit is called 3 times
            foo.Submit()

            ' Assert - that one passes
            Mock.Assert(Sub() foo.Submit(), Occurs.AtLeast(3))
        End Sub
  {{endregion}}

In the example above, the first `Assert` throws an exception as the `foo.Submit` method has been called only twice at that time, but the `Occurs.AtLeast` requires the method to be called at least 3 times. Afterwards, the method is called once again which causes the conditions in the second `Assert` to be satisfied.

## Occurs.AtMost(numberOfTimes)
With `Occurs.AtMost` you specify at most how many times you want to verify that a given method/property has been called during the execution of the test.

  #### __[C#]__

  {{region Occurrence#AtMostNTimes}}
    [TestMethod]
        [ExpectedException(typeof(AssertFailedException))]
        public void ShouldOccursCertainNumberOfTimesAtMost()
        {
            //Arrange
            var foo = Mock.Create<IFoo>();

            //Act
            foo.Submit();
            foo.Submit();

            //Assert
            Mock.Assert(() => foo.Submit(), Occurs.AtMost(2));

            // Act - now we call Submit once again - 3 times in total
            foo.Submit();

            // Assert - that throws an exception
            Mock.Assert(()=>foo.Submit(),Occurs.AtMost(2));
        }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#AtMostNTimes}}
    <TestMethod()>
        <ExpectedException(GetType(AssertFailedException))>
        Public Sub ShouldOccursCertainNumberOfTimesAtMost()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            ' Act
            foo.Submit()
            foo.Submit()

            ' Assert
            Mock.Assert(Sub() foo.Submit(), Occurs.AtMost(2))

            ' Act - now we call Submit once again - 3 times in total
            foo.Submit()

            ' Assert - that throws an exception
            Mock.Assert(Sub() foo.Submit(), Occurs.AtMost(2))
        End Sub
  {{endregion}}


In the example above, the first `Assert` passes as the `foo.Submit` method has been called only twice. However, the second `Assert` throws an exception because the same method has been called 3 times in total, while the condition is to be called 2 times at most.
      
## Occurs.Exactly(numberOfTimes)
With `Occurs.Exactly` you specify exactly how many times you want to verify that a given method/property has been called during the execution of the test.

> The next example uses NUnit Testing Framework.


  #### __[C#]__

  {{region Occurrence#Exactly}}
    [TestMethod]
        public void ShouldOccursExactly()
        {
            //Arrange
            var foo = Mock.Create<IFoo>();

            //Act - call foo.Submit twice
            foo.Submit();
            foo.Submit();

            //Assert - throws an exception - foo.Submit was called only twice
            NUnit.Framework.Assert.Throws<AssertFailedException>(() => Mock.Assert(() => foo.Submit(), Occurs.Exactly(3)));

            // Act - call foo.Submit once again - 3 times in total
            foo.Submit();

            // Assert - that should pass
            Mock.Assert(() => foo.Submit(), Occurs.Exactly(3));

            // Act - call foo.Submit once again - 4 times in total
            foo.Submit();

            // Assert - fails because foo.Submit was called more times than specified
            NUnit.Framework.Assert.Throws<AssertFailedException>(() => Mock.Assert(() => foo.Submit(), Occurs.Exactly(3)));
        }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#Exactly}}
    <TestMethod()>
        Public Sub ShouldOccursExactly()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            ' Act - call foo.Submit twice
            foo.Submit()
            foo.Submit()

            ' Assert - throws an exception - foo.Submit was called only twice
            NUnit.Framework.Assert.Throws(Of AssertFailedException)(Sub() Mock.Assert(Sub() foo.Submit(), Occurs.Exactly(3)))

            ' Act - call foo.Submit once again - 3 times in total
            foo.Submit()

            ' Assert - that should pass
            Mock.Assert(Sub() foo.Submit(), Occurs.Exactly(3))

            ' Act - call foo.Submit once again - 4 times in total
            foo.Submit()

            ' Assert - fails because foo.Submit was called more times than specified
            NUnit.Framework.Assert.Throws(Of AssertFailedException)(Sub() Mock.Assert(Sub() foo.Submit(), Occurs.Exactly(3)))
        End Sub
  {{endregion}}

In the example above, the first `Assert` throws an exception because we call `foo.Submit` less times than the `Occurs.Exactly` condition specifies. The second `Assert` passes because there are exactly 3 calls to `foo.Submit` method. The third `Assert` throws an exception again, because at than time the `foo.Submit` method is called 4 times, which is more that what is specified in the `Occurs.Exactly` condition.

## Occurs(numberOfTimes)
With `Occurs(numberOfTimes)` you specify exactly how many times you want to verify that a given method/property has been called during the execution of the test.

> The next example uses NUnit Testing Framework.


  #### __[C#]__

  {{region Occurrence#NumberOfTimes}}
    [TestMethod]
        public void ShouldFailOnAssertAllWhenExpectionIsNotMet()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            Mock.Arrange(() => foo.Submit()).Occurs(2);

            // Assert
            NUnit.Framework.Assert.Throws<AssertFailedException>(() => Mock.Assert(foo));
        }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#NumberOfTimes}}
    <TestMethod()>
        Public Sub ShouldFailOnAssertAllWhenExpectionIsNotMet()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Sub() foo.Submit()).Occurs(2)

            ' Assert
            NUnit.Framework.Assert.Throws(Of AssertFailedException)(Sub() Mock.Assert(foo))
        End Sub
  {{endregion}}

In the example above, `Occurs(2)` marks the call is required to be executed 2 times, therefore `Mock.Assert` will fail if not expected properly.

## OccursOnce()
`OccursOnce()` is used to verify that a method/property has been called exactly one time during the execution of the test.

  #### __[C#]__

  {{region Occurrence#OccursOnce}}
    [TestMethod]
        public void ShouldArrangeOccursOnce()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            Mock.Arrange(() => foo.Submit()).OccursOnce();

            // Act
            foo.Submit();

            // Assert
            Mock.Assert(foo);
        }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#OccursOnce}}
    <TestMethod()>
        Public Sub ShouldArrangeOccursOnce()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Sub() foo.Submit()).OccursOnce()

            ' Act
            foo.Submit()

            ' Assert
            Mock.Assert(foo)
        End Sub
  {{endregion}}

In the example above, we arrange that `foo.Submit()` must be called exactly once.

## OccursNever()
`OccursNever()` is used to verify that a method/property hasn't been called during the execution of the test.

  #### __[C#]__

  {{region Occurrence#OccursNever}}
    [TestMethod]
        public void ShouldArrangeOccursNever()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();
            
            Mock.Arrange(() => foo.Submit()).OccursNever();

            // Assert
            Mock.Assert(foo);
        }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#OccursNever}}
    <TestMethod()>
        Public Sub ShouldArrangeOccursNever()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Sub() foo.Submit()).OccursNever()

            ' Assert
            Mock.Assert(foo)
        End Sub
  {{endregion}}

In the example above, we verify that `foo.Submit()` has never been called.

## OccursAtLeast(numberOfTimes)
With `OccursAtLeast(numberOfTimes)` you specify at least how many times you want to verify that a given method/property has been called during the execution of the test.

  #### __[C#]__

  {{region Occurrence#OccursAtLeast}}
    [TestMethod]
        public void ShouldArrangeOccursAtLeast()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            Mock.Arrange(() => foo.Submit()).OccursAtLeast(2);

            // Act
            foo.Submit();
            foo.Submit();
            foo.Submit();

            // Assert
            Mock.Assert(foo);
        }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#OccursAtLeast}}
    <TestMethod()>
        Public Sub ShouldArrangeOccursAtLeast()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Sub() foo.Submit()).OccursAtLeast(2)

            ' Act
            foo.Submit()
            foo.Submit()
            foo.Submit()

            ' Assert
            Mock.Assert(foo)
        End Sub
  {{endregion}}

In the example above, `foo.Submit()` is called `3` times. The verification will not fail as it is expected to execute at least `2` times.

## OccursAtMost(numberOfTimes)
With `OccursAtMost(numberOfTimes)` you specify at most how many times you want to verify that a given method/property has been called during the execution of the test.

  #### __[C#]__

  {{region Occurrence#OccursAtMost}}
    [TestMethod]
        [ExpectedException(typeof(AssertFailedException))]
        public void ShouldFailWhenInvokedMoreThanRequried()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();
            
            Mock.Arrange(() => foo.Submit()).OccursAtMost(2);

            // Act
            foo.Submit();
            foo.Submit();
            foo.Submit();
        }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#OccursAtMost}}
    <TestMethod()>
         <ExpectedException(GetType(AssertFailedException))>
        Public Sub ShouldFailWhenInvokedMoreThanRequried()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()
            Mock.Arrange(Sub() foo.Submit()).OccursAtMost(2)

            ' Act
            foo.Submit()
            foo.Submit()
            foo.Submit()
        End Sub
  {{endregion}}

In the example above, we specify that `foo.Submit()` must be called at most 2 times. As it is called 3 times, `AssertFailedException` will be thrown immediately after the third call.

## Asserting Multiple Occurrences
JustMock enables you to assert multiple occurrences of similar type of call using Matcher in Mock.Assert rather than asserting each of them separately.

Here is an example:

  #### __[C#]__

  {{region Occurrence#MultipleOccurrences}}
    [TestMethod]
        public void ShouldBeAbleToAssertOccursUsingMatcherForSimilarCallAtOneShot()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();
	
            Mock.Arrange(() => foo.Echo(1)).Returns((int arg) => arg);
            Mock.Arrange(() => foo.Echo(2)).Returns((int arg) => arg);
            Mock.Arrange(() => foo.Echo(3)).Returns((int arg) => arg);
	
            // Act
            foo.Echo(1);
            foo.Echo(2);
            foo.Echo(3);
	
            // Assert
            Mock.Assert(() => foo.Echo(Arg.AnyInt), Occurs.Exactly(3));
        }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#MultipleOccurrences}}
    <TestMethod()>
        Public Sub ShouldBeAbleToAssertOccursUsingMatcherForSimilarCallAtOneShot()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Function() foo.Echo(1)).Returns(Function(arg__1 As Integer) arg__1)
            Mock.Arrange(Function() foo.Echo(2)).Returns(Function(arg__1 As Integer) arg__1)
            Mock.Arrange(Function() foo.Echo(3)).Returns(Function(arg__1 As Integer) arg__1)

            ' Act
            foo.Echo(1)
            foo.Echo(2)
            foo.Echo(3)

            ' Assert
            Mock.Assert(Function() foo.Echo(Arg.AnyInt), Occurs.Exactly(3))
        End Sub
  {{endregion}}

Our arrangement is for calling `Echo` method with `1`, `2` or `3` as argument. In assertion we use a matcher to cover all calls with *integer* as argument. As calling `foo.Echo` with `1`, `2` and `3` in acting phase applies that matcher, a call with *integer* occurred exactly `3` times.
Refer to [Matchers]({%slug justmock/basic-usage/matchers%}) for more information about using matchers.

## Verifying Calls Order
To ensure that a set of method calls are executed in a particular order, you can use the `InOrder()` method. To achieve this, you will need to use the following workflow: 

1. You arrange the methods to define their invocation order.
1. You act.
1. You assert on the mock, to check if the method execution order is as expected.

 Next is an example, showing `InOrder()` use case: 


  #### __[C#]__

  {{region Occurrence#VerifyInvocationOrder1}}
    [TestMethod]
        public void ShouldVerifyCallsOrder()
        {
            // Arrange
            var foo = Mock.Create<IFoo>();

            Mock.Arrange(() => foo.Submit()).InOrder();
            Mock.Arrange(() => foo.Echo(Arg.AnyInt)).InOrder();

            // Act
            foo.Submit();
            foo.Echo(5);

            // Assert
            Mock.Assert(foo);
        }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#VerifyInvocationOrder1}}
    <TestMethod()>
        Public Sub ShouldVerifyCallsOrder()
            ' Arrange
            Dim foo = Mock.Create(Of IFoo)()

            Mock.Arrange(Sub() foo.Submit()).InOrder()
            Mock.Arrange(Sub() foo.Echo(Arg.AnyInt)).InOrder()

            ' Act
            foo.Submit()
            foo.Echo(5)

            ' Assert
            Mock.Assert(foo)
        End Sub
  {{endregion}}

Note that the `InOrder` option also supports asserting the order of mock calls regardless of the instance within the test scope. Imagine that you have to validate that the user has logged in before using his/hers shopping cart in your application.
        

  #### __[C#]__

  {{region Occurrence#VerifyInvocationOrder2}}
    public interface IUserValidationService
    {
        int ValidateUser(string userName, string password);
    }

    public interface IShoppingCartService
    {
        IList<string> LoadCart(int userID);
    }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#VerifyInvocationOrder2}}
    Public Interface IUserValidationService
        Function ValidateUser(ByVal userName As String, ByVal password As String) As Integer
    End Interface

    Public Interface IShoppingCartService
        Function LoadCart(ByVal userID As Integer) As IList(Of String)
    End Interface
  {{endregion}}

Here we have defined the `IUserValidationService` and the `IShoppingCartService` services whose invocation order we are going to assert in the following test:
        

  #### __[C#]__

  {{region Occurrence#VerifyInvocationOrder3}}
    [TestMethod]
        public void ShouldAssertInOrderForDifferentInstancesInTestMethodScope()
        {
            string userName = "Bob";
            string password = "Password";
            int userID = 5;
            var cart = new List<string> { "Foo", "Bar" };

            // Arrange
            var userServiceMock = Mock.Create<IUserValidationService>();
            var shoppingCartServiceMock = Mock.Create<IShoppingCartService>();

            Mock.Arrange(() => userServiceMock.ValidateUser(userName, password)).Returns(userID).InOrder().OccursOnce();
            Mock.Arrange(() => shoppingCartServiceMock.LoadCart(userID)).Returns(cart).InOrder().OccursOnce();

            // Act
            userServiceMock.ValidateUser(userName, password);
            shoppingCartServiceMock.LoadCart(userID);

            // Assert
            Mock.Assert(userServiceMock);
            Mock.Assert(shoppingCartServiceMock);
        }
  {{endregion}}

  #### __[VB]__

  {{region Occurrence#VerifyInvocationOrder3}}
    <TestMethod()>
        Public Sub ShouldAssertInOrderForDifferentInstancesInTestMethodScope()
            Dim userName As String = "Bob"
            Dim password As String = "Password"
            Dim userID As Integer = 5
            Dim cart As IList(Of String) = {"Foo", "Bar"}

            ' Arrange
            Dim userServiceMock = Mock.Create(Of IUserValidationService)()
            Dim shoppingCartServiceMock = Mock.Create(Of IShoppingCartService)()

            Mock.Arrange(Function() userServiceMock.ValidateUser(userName, password)).Returns(userID).InOrder().OccursOnce()
            Mock.Arrange(Function() shoppingCartServiceMock.LoadCart(userID)).Returns(cart).InOrder().OccursOnce()

            ' Act
            userServiceMock.ValidateUser(userName, password)
            shoppingCartServiceMock.LoadCart(userID)

            ' Assert
            Mock.Assert(userServiceMock)
            Mock.Assert(shoppingCartServiceMock)
        End Sub
  {{endregion}}


In the arrange phase we have defined that the `ValidateUser` call should be made only once and before the `LoadCart` service call. The `LoadCart` call should also occur only once and should follow the `ValidateUser` service call. We act and then assert our expectations.
    
