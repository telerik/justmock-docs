---
title: Basics of Mocking 
page_title: Basics of Mocking  | JustMock Documentation
description: Check out the sample example in this topic to get a better understanding of how you can start testing your system with JustMock.
slug: justmock/getting-started/basics/basics
tags: mock, test, method, property, behavior
published: True
position: 0
---

# Basics of Mocking

Mocking is a process used in unit testing when the unit being tested has external dependencies. The purpose of mocking is to isolate and focus on the code being tested and not on the behavior or state of external dependencies. In mocking, the dependencies are replaced by closely controlled replacement objects that simulate the behavior of the real ones. **JustMock** creates the replacement objects and manages their lifetime for you.

In the topics of this Getting Started section, you will get familiar with the API of JustMock and will see example scenarios of its basic usage.

## Mock the Behavior of a Public Method

To start from the basics, in the first example we will show you how to mock the behavior of a public method for a specific instance. To demonstrate the case, we will use a sample setup representing an order and a warehouse keeping all the inventory for the different orders:

#### [C#] Sample setup

{{region justmock-getting-started-basics-first-step_0}}

    public class Warehouse
    {
        public Dictionary<string, int> Products { get; set; }

        public bool HasInventory(string productName, int quantity)
        {
            int quantityInWarehouse;
            if (this.Products.TryGetValue(productName, out quantityInWarehouse))
            {
                if (quantityInWarehouse >= quantity)
                {
                    return true;
                }
            }

            return false;
        }

        public void Remove(string productName, int quantity)
        {
            if (this.HasInventory(productName, quantity))
            {
                this.Products[productName] -= quantity;
            }
        }
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
        public bool IsCompleted { get; private set; }

        public void Complete(Warehouse warehouse)
        {
            if (warehouse.HasInventory(this.ProductName, this.Quantity))
            {
                warehouse.Remove(this.ProductName, this.Quantity);
                this.IsCompleted = true;
            }
        }
    }

{{endregion}}


Let’s first verify that the `Complete` method of the `Order` class properly sets the `IsCompleted` property to avoid duplication. Our target method depends on the products currently present in the `Products` collection and internally invokes other two methods - `HasInventory` and `Remove`. To test the code without mocking, you would need to setup a `Warehouse` object with a collection keeping the products and maintain that collection during the execution of the different tests. You would have to call the `Complete` method using specific arguments for the different cases. 

With **JustMock**, you can directly control the behavior of the methods and ignore the specific data preserved in `Warehouse`. **Example 1** shows how you can setup the `HasInventory` method to always return `true` and ignore the logic of `Remove`. The demonstrated setup enables you to focus on the logic and ignore the specific data or requirements for the logic to operate.

#### [C#] Example 1: Test IsCompleted marked as expected by controlling the method behavior

{{region justmock-getting-started-basics-first-step_1}}

    [TestMethod]
    public void Order_IsCompleted_WhenWarehouseHasInventory()
    {
        Order order = new Order("testProduct", 10);
        Warehouse warehouse = new Warehouse();

        // Setup the warehouse - the HasInventory method called with the specified parameters will always return true. 
        Mock.Arrange(() => warehouse.HasInventory(order.ProductName, order.Quantity)).Returns(true);

        // We are not maintaining a collection of products, so there isn't anything to remove. 
        Mock.Arrange(() => warehouse.Remove(order.ProductName, order.Quantity)).DoNothing();

        order.Complete(warehouse);

        Assert.IsTrue(order.IsCompleted);
    }
{{endregion}}


Another option that you can use is to create a collection of products and ensure the `Products` property uses it instead of messing up the real data.

#### [C#] Example 2: Test IsCompleted marked as expected by controlling the Products property behavior

{{region justmock-getting-started-basics-first-step_2}}

    private Dictionary<string, int> GetTestProducts()
    {
        return new Dictionary<string, int>()
        {
            { "shirt", 23},
            { "trousers", 3},
            { "hat", 12},
            { "jeans", 36},
            { "skirt", 9}
        };
    }
    
    [TestMethod]
    public void Order_IsCompleted_WhenWarehouseHasSampleInventory()
    {
        Order order = new Order("shirt", 1);
        Warehouse warehouse = new Warehouse();

        // Setup the warehouse - the Products property will always return the test products. 
        Mock.Arrange(() => warehouse.Products).Returns(this.GetTestProducts());

        order.Complete(warehouse);

        Assert.IsTrue(order.IsCompleted);
    }

{{endregion}}


## Next Steps

Now that you know how a simple public class with public members can be tested using **JustMock**, go ahead and check the next topic of the Getting Started series to learn about the numerous features and scenarios you can easily setup and test. In the [Create Mock Instances]({%slug justmock/getting-started/basics/create%}) topic, you will learn how to obtain mocked instances of different implementations.


## See Also

 * [Call Original]({%slug justmock/basic-usage/mock/call-original%})

 * [Do Instead]({%slug justmock/basic-usage/mock/do-instead%})

 * [Do Nothing]({%slug justmock/basic-usage/mock/do-nothing%})

 * [Returns]({%slug justmock/basic-usage/mock/returns%})

