---
title: Arrange Mocks
page_title: Arrange Mocks | JustMock Documentation
description: Learn how to set up the desired behavior of a mocked object using JustMock and its Arrange method.
slug: justmock/getting-started/basics/arrange
tags: mock, test, method, property, setup, behavior, arrange, return
published: True
position: 2
---

# Arrange Mocks

Arranging a mock is the phase that is used to set up the behavior of the mocked objects. The previous topics slightly touched the arranging of a mock to show you complete examples. This article describes the different options and approaches you can use to arrange how the mocked members should behave.

**JustMock** exposes a rich set of options for defining the behavior of your mocks. To set up a behavior, you should first invoke the **`Arrange()`** method. It accepts a lambda function or an action describing the member you would like to mock and returns a [`CommonExpectation`](https://docs.telerik.com/devtools/justmock/api/Telerik.JustMock.Expectations.CommonExpectation-1.html) object that allows you set different behaviors and expectations. 

For the examples in this topic, we will use the already familiar from the previous topics scenario with a warehouse and orders:

#### Sample setup
```C#
public interface IWarehouse
{
    bool HasInventory(string productName, int quantity);
     
    void Remove(string productName, int quantity);
}
     
public class Order
{
    public Order(string productName, int quantity)
    {
        this.ProductName = productName;
        this.Quantity = quantity;
    }
     
    public string ProductName { get; private set; }
    public int Quantity { get; private set; }
    public bool IsCompleted { get; set; }
     
    public void Complete(IWarehouse warehouse)
    {
        if (warehouse.HasInventory(this.ProductName, this.Quantity))
        {
            warehouse.Remove(this.ProductName, this.Quantity);
            this.IsCompleted = true;
        }
    }
}
```

>The examples in this topic use actual values for the arrange of the methods for simplicity. However, **JustMock** enables you to ignore the actual values and pass an expression that satisfies the argument type or the expected value range. For more details on the topic, refer to the [Matchers]({%slug justmock/basic-usage/matchers%}) article.

## Returns

The `Returns` method allows you to control the return value of a property or method. When using this method, the invocation of the real code is skipped and it directly returns the specified value.

If you check the implementation of the `Complete` method from our sample setup, you will notice that the `HasInventory` method must return `true` so that the order can be processed. With **JustMock**, you shouldn’t care about the specific products currently available in the warehouse and their quantity. You can directly mark that the `HasInventory` method returns always `true` and skip adding products in the warehouse to just test the execution of the `Complete` method.

#### Example 1: Modify the return value of a method
```C#
// Create an instance of the class
Order order = new Order("testProduct", 10);
    
// Get an instance of the interface dependency
IWarehouse warehouse = Mock.Create<IWarehouse>();
    
// Set up the warehouse - the HasInventory method called with the specified parameters will always return true.
Mock.Arrange(() => warehouse.HasInventory(order.ProductName, order.Quantity)).Returns(true);
```

## CallOriginal

The `CallOriginal` method specifies that the original implementation must be executed. This method is useful when dealing with mock objects (created with `Mock.Create()`) to ensure that the real implementation of the member is invoked.

#### Example 2: Invoke the original implementation of a method
```C#
// Create a mock object for the class under test
Order order = Mock.Create<Order>("testProduct", 10);
    
// Get an instance of the interface dependency
IWarehouse warehouse = Mock.Create<IWarehouse>();
    
// Set up the warehouse - the HasInventory method called with the specified parameters will always return true.
Mock.Arrange(() => warehouse.HasInventory(order.ProductName, order.Quantity)).Returns(true);
    
// Define that the Complete method should execute its original implementation when called
Mock.Arrange(() => order.Complete(warehouse)).CallOriginal();
// Call the method
order.Complete(warehouse);
```

## DoInstead

With the `DoInstead` method you can have full control over what will be executed when the arranged method is called. In the context of the `Order` class, you can directly define that, instead of checking the available quantity in the warehouse and removing the ordered one, the `IsCompleted` property will be directly set to `true`.

#### Example 3: Change the logic executed by a method
```C#
// Create a mock object for the class under test
Order order = new Order("testProduct", 10);
    
// Get an instance of the interface dependency
IWarehouse warehouse = Mock.Create<IWarehouse>();
    
// Define the logic that should be executed when the Complete is called
Mock.Arrange(() => order.Complete(warehouse)).DoInstead(() => { order.IsCompleted = true; });
    
// Call the method
order.Complete(warehouse);
Assert.IsTrue(order.IsCompleted);
```

## DoNothing

This method marks the member it is arranged for to skip all its logic. It is useful when you would like to skip the execution of the logic but would need to verify that the method or property is actually called.

In the `Order` class, a good member to skip its execution is the `Remove` method. When we use a mocked warehouse, it doesn’t contain anything in its products, thus there is nothing to be removed.

#### Example 4: Skip the implementation logic of a method
```C#
// Create an instance of the class
Order order = new Order("testProduct", 10);
    
// Get an instance of the interface dependency
IWarehouse warehouse = Mock.Create<IWarehouse>();
    
// Set up the warehouse - the HasInventory method called with the specified parameters will always return true.
Mock.Arrange(() => warehouse.HasInventory(order.ProductName, order.Quantity)).Returns(true);
    
// We are not maintaining a collection of products, so there isn't anything to remove.
Mock.Arrange(() => warehouse.Remove(order.ProductName, order.Quantity)).DoNothing();
    
order.Complete(warehouse);
    
Assert.IsTrue(order.IsCompleted);
```

## MustBeCalled

This method marks a member that it must be called at least once during the execution of the test logic. It helps you verify that the specific member is indeed called in the case under test. **Example 5** demonstrates how you can mark the `Remove` method to execute no logic but still ensure it is being called.

#### Example 5: Ensure a method is being called
```C#
Order order = new Order("testProduct", 10);
IWarehouse warehouse = Mock.Create<IWarehouse>();
Mock.Arrange(() => warehouse.HasInventory(order.ProductName, order.Quantity)).Returns(true);
    
Mock.Arrange(() => warehouse.Remove(order.ProductName, order.Quantity))
    .DoNothing() // Skip the method logic
    .MustBeCalled(); // Mark that the method must be invoked at least once
    
order.Complete(warehouse);
    
Mock.Assert(() => warehouse.Remove(order.ProductName, order.Quantity)); // Assert that the Remove method has been invoked
```

> Note that verifying that the `Remove` method has been actually called is done through **`Mock.Assert`**. You can learn more about the assertion functionalities of JustMock in the [Asserting Occurrence]({%slug justmock/basic-usage/asserting-occurrence%}) help topic.

## Matchers

Matchers let you ignore passing actual values as arguments used in mocks. Instead, they give you the possibility to pass just an expression that satisfies the argument type or expected value range. For example, if a method accepts string as a first parameter, you don’t need to pass a specific string like "Camera", instead you can use `Arg.IsAny<string>()`. 

There are 3 types of matchers supported in JustMock:

1. **Arg.IsAny<[Type]>()**: Can be used to arrange a call that matches any value of the specified type.
1. **Arg.IsInRange([FromValue : int], [ToValue : int], [RangeKind])**: Lets you arrange a call that matches a specific range of values. The range can be inclusive or exclusive.
1. **Arg.Matches&lt;T&gt;(Expression&lt;Predicate&lt;T&gt;&gt; expression)**: Allows you to specify your own matching expression.

**Example 6** shows a simple usage of the matchers - it demonstrates how you can arrange the behavior of a method no matter of the specific argument values passed to it.

#### Example 6: Arrange method using Matchers 
```C#
// The HasInventory method will always return true when invoked with any string and any int values for its parameters.
Mock.Arrange(() => warehouse.HasInventory(Arg.IsAny<string>(), Arg.IsAny<int>())).Returns(true); 
```

> For more details and examples of how you can use matchers, refer to the [Matchers]({%slug justmock/basic-usage/matchers%}) help topic.

## Raise

The `Raise` method enables you to raise an event so that you can ensure that the event is working as expected and the logic properly handles it. 

To demonstrate the usage of the `Raise` and `Raises` options, we will use a slightly modified version of the sample setup.

#### Sample setup with events

```C#
public class Order
{
    public event EventHandler TimedOut;
    
    private bool hasTimedOut;
    private bool isCompleting;
    
    public Order(string productName, int quantity)
    {
        this.ProductName = productName;
        this.Quantity = quantity;
        this.TimedOut += this.Order_TimedOut;
    }
    
    public string ProductName { get; private set; }
    public int Quantity { get; private set; }
    public bool IsCompleted { get; set; }
    
    internal bool HasTimedOut
    {
        get
        {
            return this.hasTimedOut;
        }
        set
        {
            if (this.hasTimedOut != value)
            {
                hasTimedOut = value;
                if (hasTimedOut)
                {
                    this.OnTimedOut(this, new EventArgs());
                }
            }
        }
    }
    
    private void OnTimedOut(object sender, EventArgs e)
    {
        // If the order is currently completing, do not time it out
        if (CanTimeOut() && this.TimedOut != null)
        {
            this.TimedOut.Invoke(sender, e);
        }
    }
    
    private bool CanTimeOut()
    {
        return !isCompleting;
    }
    
    private void Order_TimedOut(object sender, EventArgs e)
    {
        if (CanTimeOut())
        {
            this.ProductName = string.Empty;
            this.Quantity = 0;
        }
    }
    
    public void Complete(IWarehouse warehouse)
    {
        this.isCompleting = true;
        if (warehouse.HasInventory(this.ProductName, this.Quantity))
        {
            warehouse.Remove(this.ProductName, this.Quantity);
            this.IsCompleted = true;
        }
        this.isCompleting = false;
    }
}
```


If you take a deeper look into the implementation above, you will notice that the `TimedOut` event is fired only when the HasTimedOut property is set to true. This property is internal so that it can be changed only by project members and not from outside. So, how we can verify that everything goes as expected? **Example 7** shows a test verifying that the product is cleared from the order when it has been timed out.

#### Example 7: Raise an event

```C#
Order orderMock = Mock.Create<Order>(Behavior.CallOriginal, "test product", 1);

// Raise the TimedOut event
Mock.Raise(() => orderMock.TimedOut += null, orderMock, new EventArgs());

// Verify that the product is cleared
Assert.AreEqual(string.Empty, orderMock.ProductName);
Assert.AreEqual(0, orderMock.Quantity);   
```

## Raises

The `Raises` option allows you to configure that when a member is invoked, it will raise a specific event. With the last sample setup, we can use `Raises` to ensure that an order won't be marked as timed out and its product won't be removed in case it is currently being completed. **Example 8** shows how you can implement the test for this case.

#### Example 8: Raise an event when a method is called
```C#
Order orderMock = Mock.Create<Order>(Behavior.CallOriginal, "test product", 1);
IWarehouse warehouseMock = Mock.Create<IWarehouse>();

// Arrange that calling th HasInventory method with any parameters will always raise the TimedOut event
Mock.Arrange(() => warehouseMock.HasInventory(Arg.AnyString, Arg.AnyInt))
    .Raises(() => orderMock.TimedOut += null, orderMock, new EventArgs());

// Act
orderMock.Complete(warehouseMock);

// Assert that the product is not cleared
Assert.AreEqual("test product", orderMock.ProductName);
Assert.AreEqual(1, orderMock.Quantity);
```


## Throws

With `Throws` you can force a member to throw exception so you can ensure your app can properly handle the error. **Example 9** shows how you can arrange the `IsCompleted` property to always throw an `InvalidOperationException` when its setter is invoked.

#### Example 9: Force a property setter to throw exception
```C#
Order orderMock = Mock.Create<Order>(Behavior.CallOriginal, "test product", 1);
Mock.ArrangeSet(() => orderMock.IsCompleted = Arg.AnyBool).Throws(new InvalidOperationException());
```

## Next Steps

As a next step we recommend you to check the [Assert and Verification]({%slug justmock/getting-started/basics/assert%}) article to get a better understanding of the API JustMock provides for verifying the arranged behavior.

## See Also

 * [Matchers]({%slug justmock/basic-usage/matchers%})
 
 * [Mock Properties]({%slug justmock/basic-usage/mock-properties%})

