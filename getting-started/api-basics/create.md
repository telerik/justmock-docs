---
title: Create Mock Instances
page_title: Create Mock Instances | JustMock Documentation
description: Learn how you can create instances of abstract classes or interfaces without caring about the specific implementations they might have using JustMock.
slug: justmock/getting-started/basics/create
tags: mock, test, create
published: True
position: 1
---

# Create Mock Instances

The [Mock the behavior of a public method]({%slug justmock/getting-started/basics/basics%}) topic demonstrated how to mock the behavior of a method using a plain object instance of the tested object. Coding using the best practices always involves creating abstract classes or interfaces. This topic shows you how you can test a functionality dependent on abstract classes or interfaces without caring about the specific implementations of the dependency. 

To illustrate that case, we will use a slightly extended version of the example with orders and a warehouse. Now, we will define the warehouse as an interface and will enable a product to be delivered from multiple warehouses.

#### [C#] Sample setup

{{region justmock-getting-started-basics-create_0}}

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
        public bool IsCompleted { get; private set; }
     
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


What would you need to do in order to verify that the `Complete` method of the `Order` class properly sets the `IsCompleted` property? Let’s take a closer look - our target method depends on an instance implementing `IWarehouse` that has some products currently present and internally invokes other two methods - `HasInventory` and `Remove`. Testing this case without using a mocking framework will force you to find a specific instance of a warehouse and control its data so that the `Complete` method of `Order` can perform the needed actions. 

**JustMock** enables you to create instances of abstract classes and interfaces without providing their exact implementations. You also have control over how they should be mocked. **Example 1** demonstrates how you can use the `**Mock.Create()**` method to get an instance representing `IWarehouse`.

#### [C#] Example 1: Create mock from an interface

{{region justmock-getting-started-basics-create_1}}

    [TestMethod]
    public void Order_IsCompleted_WhenWarehouseHasInventory()
    {
        // Create an instance of the class
        Order order = new Order("testProduct", 10);
     
        // Get an instance of the interface dependency
        IWarehouse warehouse = Mock.Create<IWarehouse>();
     
        // Setup the warehouse - the HasInventory method called with the specified parameters will always return true.
        Mock.Arrange(() => warehouse.HasInventory(order.ProductName, order.Quantity)).Returns(true);
     
        // We are not maintaining a collection of products, so there isn't anything to remove.
        Mock.Arrange(() => warehouse.Remove(order.ProductName, order.Quantity)).DoNothing();
     
        order.Complete(warehouse);
     
        Assert.IsTrue(order.IsCompleted);
    }

{{endregion}}


The parameterless overload of the `Create()` method generates an instance of the mocked class or interface using the **RecursiveLoose** behavior. The following list will give you a brief overview of the different mock behaviors you can use. 

- **RecursiveLoose behavior**: Generate **empty stubs** for all members and dependencies. Even if nothing is arranged, the mocks with Behavior.RecursiveLoose never return null. This is the default behavior. Read more in the [Behaviors|RecursiveLoose]({%slug justmock/basic-usage/mock-behaviors/recursiveloose%}) article.

* **Loose behavior**: When nothing arranged, **the members return the default** value of their return type - null for reference types and the default value for value types. Read more in the [Behaviors|Loose]({%slug justmock/basic-usage/mock-behaviors/loose%}) article

- **CallOriginal behavior**: All members **follow the original implementation** of the mocked instance. [Read more...]({%slug justmock/basic-usage/mock-behaviors/calloriginal%})

- **Strict behavior**: Allows you to call **only arranged methods**. Every call to other members results in MockException. Read more in the [Behaviors|Strict]({%slug justmock/basic-usage/mock-behaviors/strict%}) article

## Create mock of a type using non-default constructor

In addition to passing the mock behavior, the overloads of the `Create` method allow you specify the constructor you would like to use. Depending on the specific class implementation, it might miss a default constructor, or you might need to test the behavior of an object when created with specific arguments. 

In the sample setup used in this topic, the Order class doesn't provide a default constructor, so we will need to provide the required parameters if we need to create an object of that type.

#### [C#] Example 2: Create mock using non-default constructor

{{region justmock-getting-started-basics-create_2}}

    Order orderMock = Mock.Create<Order>(Behavior.CallOriginal, "test product", 1);
{{endregion}}


## Next Steps

Check the [Arrange]({%slug justmock/getting-started/basics/arrange%}) topic to learn more on how you can setup the desired behavior of different members.

## See Also

 * [Automocking]({%slug justmock/basic-usage/automocking%})



