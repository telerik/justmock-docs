---
title: Mock Non-Public API
page_title: Mock Non-Public API | JustMock Documentation
description: Check out this topic to learn how you can mock private or internal methods and properties with JustMock.
slug: justmock/getting-started/basics/non-public
tags: mock, test, method, property, non-public, private, internal
published: True
position: 5
---

# Mock Non-Public API

The non-public API like the private members are essential parts of the implementation of each system. A developer won’t be able to fully control the behavior of all the classes or dependencies that they might need to if they are unable to mock the private implementations. 

To provide you with full control on the behavior of the different private implementations, **JustMock** exposes the **`Mock.NonPublic`** option. With this option, you can arrange the expectations and behavior for private and internal members. 

To illustrate its usage, we will be using the following sample setup that takes care of gifts distribution for orders containing more than 5 products:

#### [C#] Sample setup

{{region justmock-getting-started-basics-private_0}}

    public class Product
    {
        public Product(string name, int quantity)
        {
            this.Name = name;
            this.Quantity = quantity;
        }
     
        public string Name { get; internal set; }
        public int Quantity { get; internal set; }
    }
     
    public class Order
    {
        private int minimalProductsCountForGift = 5;
        private bool isCompleted;
     
        public Order(List<Product> products)
        {
            this.Products = products;
        }
     
        public List<Product> Products { get; private set; }
     
        public void Complete(IWarehouse warehouse)
        {
            if (this.isCompleted)
            {
                return;
            }
     
            if (this.IsEligibleForGift())
            {
                GiftsDistributor.AddGift(this);
            }
     
            foreach (var product in this.Products)
            {
                if (warehouse.HasInventory(product.Name, product.Quantity))
                {
                    warehouse.Remove(product.Name, product.Quantity);
                }
                else
                {
                    this.UpdateStatus(false);
                    return;
                }
            }
     
            this.UpdateStatus(true);
        }
     
        private void UpdateStatus(bool isCompleted)
        {
            this.isCompleted = isCompleted;
        }
     
        private bool IsEligibleForGift()
        {
            int currentProductsCount = 0;
            foreach (var product in this.Products)
            {
                currentProductsCount += product.Quantity;
            }
     
            if (currentProductsCount >= minimalProductsCountForGift)
            {
                return true;
            }
     
            return false;
        }
    }
     
    public static class GiftsDistributor
    {
        public static Product Gift { get; set; }
     
        static GiftsDistributor()
        {
            Gift = new Product("Hat (gift)", 1);
        }
     
        public static void AddGift(Order order)
        {
            order.Products.Add(Gift);
        }
    }

{{endregion}}


With the implementation above, testing that the gifts distribution is correctly performed in scenario without using mocks would require us to investigate the implementation of the `IsEligibleForGift` method and setup our order in a way that ensures the method will return `true`. With **JustMock** you can skip these details and just define the value you need to be returned so that the execution of the logic can continue in the desired way.

#### [C#] Example 1: Arrange private field

{{region justmock-getting-started-basics-private_1}}
    
    // Create mocks for the class under test and its dependency
    Order orderMock = Mock.Create<Order>(Behavior.CallOriginal, new List<Product>());
    IWarehouse warehouseMock = Mock.Create<IWarehouse>();
     
    // Arrange your expectations with Mock.NonPublic
    Mock.NonPublic.Arrange<bool>(orderMock, "IsEligibleForGift").Returns(true);
     
    // Any invocation of HasInventory(string, int) and Remove(string, int) will return true
    Mock.Arrange(() => warehouseMock.HasInventory(Arg.IsAny<string>(), Arg.IsAny<int>())).Returns(true);
    Mock.Arrange(() => warehouseMock.Remove(Arg.IsAny<string>(), Arg.IsAny<int>())).DoNothing();
     
    orderMock.Complete(warehouseMock);
         
    // Ensure that a gift hat is added to the order
    Assert.AreEqual(1, orderMock.Products.Count);
{{endregion}}


With `Mock.NonPublic` you can use the same familiar approach for arranging the getter and setter of properties and the behavior of methods that have restrictive access modifier.

## Calling Private Methods and Properties

**JustMock** gives you the ability to easily invoke private members from your tests as well. To achieve that, you will need to use the **`PrivateAccessor`** class. It represents a wrapper for objects that allows you invoke their private or internal members without using complex queries to select them. While `PrivateAccessor` uses the .NET Reflection API internally, it exposes convenient and easy to read methods that hide the complexity of the reflection in a single line of code.

To see `PrivateAccessor` in action, let’s say that we would like to test that invoking the `Complete` method from our sample setup doesn’t add gifts to orders that have been already processed. An order is considered processed when its `isCompleted` field is set to `true`. There is also exposed a method changing the status and we can use it to avoid additional unneeded processing.


#### [C#] Example 2: Call a private method and obtain the value of a private field

{{region justmock-getting-started-basics-private_1}}

    // Create mocks for the class under test and its dependency
    Order orderMock = Mock.Create<Order>(Behavior.CallOriginal, new List<Product>());
    IWarehouse warehouseMock = Mock.Create<IWarehouse>();
     
    PrivateAccessor accessor = new PrivateAccessor(orderMock);
    accessor.CallMethod("UpdateStatus", true); // Invoke the UpdateStatus() method to set the isCompleted field to true
    bool isCompleted = (bool)accessor.GetMember("isCompleted");
              
    Assert.AreEqual(true, isCompleted);
     
    orderMock.Complete(warehouseMock);
     
    // Ensure that a gift is not added to the order
    Assert.AreEqual(0, orderMock.Products.Count);
{{endregion}}

## Next Steps

The [Mock System API]({%slug justmock/getting-started/basics/system-api%}) topic will provide you with more details on how you can arrange members defined inside the .NET.

## See Also

* [Private Accessor]({%slug justmock/advanced-usage/private-accessor%})

* [Mocking Non-public Members and Types]({%slug justmock/advanced-usage/mocking-non-public-members-and-types%})