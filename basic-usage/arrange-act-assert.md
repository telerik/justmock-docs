---
title: Arrange Act Assert
page_title: Arrange Act Assert | JustMock Documentation
description: The Arrange Act Assert pattern and how to apply it when using Telerik JustMock.
previous_url: /basic-usage-arrange-act-assert.html
slug: justmock/basic-usage/arrange-act-assert
tags: arrange,act,assert
published: True
position: 1
---

# Arrange Act Assert

Arrange/Act/Assert (AAA) is a pattern for arranging and formatting code in Unit Test methods.

It is a best practice to author your tests in more natural and convenient way. The idea is to develop a unit test by following these 3 simple steps:

* __Arrange__ – setup the testing objects and prepare the prerequisites for your test.
* __Act__ – perform the actual work of the test.
* __Assert__ – verify the result.


## Benefits of Using Arrange Act Assert

* Clearly separates what is being tested from the setup and verification steps.
* Clarifies and focuses attention on a historically successful and generally necessary set of test steps.
* Makes some test smells more obvious:
    * Assertions intermixed with "Act" code.
    * Test methods that try to test too many different things at once.

## Arrange/Act/Assert with JustMock

Lets illustrate the benefits of the pattern with an example. We will test our warehouse to ensure that it returns correct results when an order is placed.

>The classes used for the next example are defined in the [Testing Your Application with JustMock](../getting-started/quick-start) article.

## Arrange

First we need an order:

  #### __[C#]__

  {{region }}
    var order = new Order("Camera", 2);
  {{endregion}}

  #### __[VB]__

  {{region }}
    Dim order = New Order("Camera", 2)
  {{endregion}}

Now let’s mock the warehouse:

  #### __[C#]__

  {{region }}
    var warehouse = Mock.Create<IWarehouse>();
  {{endregion}}

  #### __[VB]__

  {{region }}
    Dim warehouse = Mock.Create(Of IWarehouse)()
  {{endregion}}

We want to ensure that when an order of 2 cameras is placed, the warehouse returns that it has enough quantity of the product.

  #### __[C#]__

  {{region }}
    Mock.Arrange(() => warehouse.HasInventory("Camera", 2)).Returns(true);
  {{endregion}}

  #### __[VB]__

  {{region }}
    Mock.Arrange(Function() warehouse.HasInventory("Camera", 2)).Returns(true)
  {{endregion}}

>note You can also check the [Create Mocks By Example](./create-mocks-by-example) topic that demonstrates how you can arrange mock objects in more complex scenarios.

That’s it. We set up the testing objects for our test. Now let’s act.


## Act

Fill our order from the warehouse.

  #### __[C#]__

  {{region }}
    order.Fill(warehouse);
  {{endregion}}

  #### __[VB]__

  {{region }}
    order.Fill(warehouse)
  {{endregion}}

Once we have executed the desired action, we need to ensure that it has been completed successfully and our order was actually filled, meaning that the warehouse really had inventory of 2 cameras.

## Assert 

We will use the __Assert__ class from Microsoft.VisualStudio.TestTools.UnitTesting namespace (found in Microsoft.VisualStudio.QualityTools.UnitTestFramework assembly – automatically added as a reference from Visual Studio while creating a Test Project) to ensure that the `IsFilled` property of the order is set to `true`.

  #### __[C#]__

  {{region }}
    Assert.IsTrue(order.IsFilled);
  {{endregion}}

  #### __[VB]__

  {{region }}
    Assert.IsTrue(order.IsFilled)
  {{endregion}}

With this simple example we illustrated the use of the AAA pattern and showed how easy it is to test your code with JustMock. Notice that you don’t need to change even a single line of your original code to set up, execute and verify its correctness.

## Verify Interaction

Now let's take it a little further and verify not only the final result, but also the interaction while executing the test.

We arranged that when the warehouse’s `HasInventory` method is called with specific parameters, it should return `true`, but we never ensured that this method is actually called. Let's change the `Arrange` method and mark that `warehouse.HasInventory` must be called.

  #### __[C#]__

  {{region }}
    Mock.Arrange(() => warehouse.HasInventory("Camera", 2)).Returns(true).MustBeCalled();
  {{endregion}}

  #### __[VB]__

  {{region }}
    Mock.Arrange(Function() warehouse.HasInventory("Camera", 2)).Returns(true).MustBeCalled()
  {{endregion}}

To verify this we need to call `Mock.Assert` in the `Assert` phase with the warehouse object.

  #### __[C#]__

  {{region }}
    Mock.Assert(warehouse);
  {{endregion}}

  #### __[VB]__

  {{region }}
    Mock.Assert(warehouse)
  {{endregion}}

## Verify Order of Calls

Furthermore you may want to ensure that a set of method calls are executed in a particular order. Let\`s assume we have the following `IFoo` interface:

  #### __[C#]__

  {{region BUAAA#IFooSUT}}
    public interface IFoo
    {
        void Submit();
        void Echo();
    }
  {{endregion}}

  #### __[VB]__

  {{region BUAAA#IFooSUT}}
    Public Interface IFoo
        Sub Submit()
        Sub Echo()
    End Interface
  {{endregion}}

You use the `Arrange` method to define the methods invocation order.

  #### __[C#]__

  {{region BUAAA#VerifyInvocationOrder1}}
    [TestMethod]
    public void ShouldVerifyCallsOrder()
    {
        // Arrange
        var foo = Mock.Create<IFoo>();

        Mock.Arrange(() => foo.Submit()).InOrder();
        Mock.Arrange(() => foo.Echo()).InOrder();

        // Act
        foo.Submit();
        foo.Echo();

        // Assert
        Mock.Assert(foo);
    }
  {{endregion}}

  #### __[VB]__

  {{region BUAAA#VerifyInvocationOrder1}}
    <TestMethod()>
    Public Sub ShouldVerifyCallsOrder()
        ' Arrange
        Dim foo = Mock.Create(Of IFoo)()

        Mock.Arrange(Sub() foo.Submit()).InOrder()
        Mock.Arrange(Sub() foo.Echo()).InOrder()

        ' Act
        foo.Submit()
        foo.Echo()

        ' Assert
        Mock.Assert(foo)
    End Sub
  {{endregion}}

Again to verify this we need to call `Mock.Assert` in the `Assert` phase with the foo object.

Note that the `InOrder` option also supports asserting the order of mock calls regardless of the instance within the test scope. Imagine that you have to validate that the user has logged in before using their shopping cart in your application.

  #### __[C#]__

  {{region BUAAA#VerifyInvocationOrder2}}
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

  {{region BUAAA#VerifyInvocationOrder2}}
    Public Interface IUserValidationService
        Function ValidateUser(ByVal userName As String, ByVal password As String) As Integer
    End Interface

    Public Interface IShoppingCartService
        Function LoadCart(ByVal userID As Integer) As IList(Of String)
    End Interface
  {{endregion}}

Here we have defined the `IUserValidationService` and the `IShoppingCartService` services whose invocation order we are going to assert in the following test:

  #### __[C#]__

  {{region BUAAA#VerifyInvocationOrder3}}
    [TestMethod]
    public void ShouldAssertInOrderForDifferentInstancesInTestMethodScope()
    {
        string userName = "Bob";
        string password = "Password";
        int userID = 5;
        var cart = new List<string> {"Foo", "Bar"};

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

  {{region BUAAA#VerifyInvocationOrder3}}
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

In the arrange phase we defined that the `ValidateUser` call should be made only once and before the `LoadCart` service call. The `LoadCart` call should also occur only once and should follow the `ValidateUser` service call. We act and then assert our expectations.

>noteRefer to the [Asserting Occurrence](./asserting-occurrence) topic to learn more about asserting occurrence. The example also uses the [Returns](./mock/returns) option in order to ignore the actual call and return a custom value.


## See Also

 * [Testing Your Application with JustMock]({%slug justmock/getting-started/quick-start%})

 * [Create Mocks By Example]({%slug justmock/basic-usage/create-mocks-by-example%})
