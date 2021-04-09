---
title: Assert and Verification
page_title: Assert and Verification | JustMock Documentation
description: Learn how to assert and verify the desired behavior of a mocked object using JustMock.
slug: justmock/getting-started/basics/assert
tags: mock, test, method, property, assert, verify
published: True
position: 3
---

# Assert and Verification

While the .NET gives you convenient methods for asserting different conditions into your unit tests, it still doesn’t cover some behaviors that can be set up when using **JustMock**. That is why, **JustMock** exposes several asserting methods to help you verify the behavior is what you expect.

When setting that a specific method must be called, or a set of methods should be called in a specific order, the actual execution must be then verified with the **`Mock.Assert`** method. To illustrate the exact usage, we will use the following setup: 

#### [C#] Sample setup

{{region justmock-getting-started-basics-assert_0}}

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
{{endregion}}


**Example 1** shows how you can set up a test verifying that the `Complete` method is first invoked, followed by `HasInventory` and `Remove` is last in the chain:

#### [C#] Example 1: Verify the order of invocation for several methods

{{region justmock-getting-started-basics-assert_1}}

    Order order = new Order("testProduct", 10);
    IWarehouse warehouse = Mock.Create<IWarehouse>();
     
    // Define that the Complete method should execute its original implementation
    // and should be invoked once in the order it appears in the arrange phase.
    Mock.Arrange(() => order.Complete(warehouse))
        .CallOriginal()
        .InOrder()
        .OccursOnce();
     
    Mock.Arrange(() => warehouse.HasInventory(order.ProductName, order.Quantity))
        .Returns(true)
        .InOrder()
        .OccursOnce();
     
    Mock.Arrange(() => warehouse.Remove(order.ProductName, order.Quantity))
        .DoNothing() // Skip the method logic.
        .InOrder() // Define that the method should be invoked in the order it appears in the Arrange phase.
        .OccursOnce(); // Define that there should be a single call to this method during the test execution.
     
    order.Complete(warehouse);
     
    // Assert that all the arranged methods of the two classes conform to the expectations set on them.
    Mock.Assert(warehouse); 
    Mock.Assert(order); 
{{endregion}}

>More information on how you can set up a specific occurrence of members is available in the [Asserting Occurrence]({%slug justmock/basic-usage/asserting-occurrence%}) topic.

In addition to the behavior of the class members, you can also assert a property setter.

#### [C#] Example 2: Verify property setter

{{region justmock-getting-started-basics-assert_1}}

    // Create a mock object that you can setup.
    // Using Behavior.CallOriginal ensures that all members that are not arranged will follow their original implementation.
    Order order = Mock.Create<Order>(Behavior.CallOriginal, "testProduct", 10);
    IWarehouse warehouse = Mock.Create<IWarehouse>();
     
    Mock.Arrange(() => warehouse.HasInventory(order.ProductName, order.Quantity)).Returns(true);
    Mock.Arrange(() => warehouse.Remove(order.ProductName, order.Quantity)).DoNothing();
                           
    // Define that the setter of the IsCompleted must be invoked at least once with value = true
    Mock.ArrangeSet(() => order.IsCompleted = true).MustBeCalled();
     
    order.Complete(warehouse);
     
    // Assert that the setter has been invoked
    Mock.AssertSet(() => order.IsCompleted = true);
{{endregion}}

## Next Steps

As a next step we recommend you to check the [Mock Static or Extension Members]({%slug justmock/getting-started/basics/static%}) article that shows how you can test any static implementation.

## See Also

* [Arrange Act Assert]({%slug justmock/basic-usage/arrange-act-assert%})

* [Asserting Occurence]({%slug justmock/basic-usage/asserting-occurrence%})