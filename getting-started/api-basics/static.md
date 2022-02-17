---
title: Mock Static or Extension Members
page_title: Mock Static or Extension Members | JustMock Documentation
description: Check out the sample example in this topic to get a better understanding of how you can mock static and extension implementations with JustMock.
slug: justmock/getting-started/basics/static
tags: mock, test, static, method, property, constructor, extension
published: True
position: 4
---

# Mock Static or Extension Members

Working with .NET applications enables you to use static classes, methods and properties. When a method is declared as static, it does not require an instance of its parent class so that it can be invoked — you can directly call it on the class itself.

Testing a non-static method (at least one that has a simple implementation without external dependencies) is straightforward. However, testing a static method is not an easy task at all. This article explains how you can overcome this challenge and test static classes and their members using **JustMock**.

> Mocking static implementations is an **elevated feature**. Refer to the [Commercial vs Free Version]({%slug justmock/licensing/license-agreement%}#commercial-vs-free-version) topic to learn more about the differences between both versions of Telerik JustMock.

To better illustrate different cases, we will be using several examples classes representing an order. The order can contain different products and it might have a gift for the user if they purchased more items. Here is how the implementation looks like:

#### [C#] Sample setup

{{region justmock-getting-started-basics-static_0}}

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
        public Order(List<Product> products)
        {
            this.Products = products;
        }
     
        public List<Product> Products { get; private set; }
        public bool IsCompleted { get; private set; }
    }
     
    public static class GiftsDistributor
    {
        public static int MinimalProductsCount { get; set; }
        public static Product Gift { get; set; }
     
        static GiftsDistributor()
        {
            MinimalProductsCount = 5;
            Gift = new Product("Hat (gift)", 1);
            EnsureGifts(Gift);
        }
     
        private static void EnsureGifts(Product gift)
        {
            // Contact the database to ensure warehouses have enough amount of the gift product to distribute between orders.
            throw new NotImplementedException();
        }
     
        public static void AddGift(Order order)
        {
            order.Products.Add(Gift);
        }
     
        public static bool IsEligibleForGift(Order order)
        {
            int currentProductsCount = 0;
            foreach (var product in order.Products)
            {
                currentProductsCount += product.Quantity;
            }
     
            if (currentProductsCount >= MinimalProductsCount)
            {
                return true;
            }
     
            return false;
        }
    }

{{endregion}}

## Mock Static Constructor

In the sample code above, you will notice that the constructor of `GiftsDistributor` invokes the `EnsureGifts` which in turn is used to contact the warehouses and ensure that they have enough amount of the products used as gifts for the future orders. The `EnsureGifts` method might need a connection to a specific service or database to do his job. This, however, is not a focus on the test we would like to implement – namely, to ensure that gifts are properly distributed for the orders. Here the static constructor mocking of **JustMock** comes handy to eliminate the dependencies we are not interested in. 

The API of **JustMock** provides you with the **`SetupStatic`** method that prepares all the static members of a class and mocks its constructor.

#### [C#] Example 1: Mock a static object constructor

{{region justmock-getting-started-basics-static_1}}

    Mock.SetupStatic(typeof(GiftsDistributor), StaticConstructor.Mocked);
{{endregion}}

With this method, you can specify whether you would like to mock the constructor or not and choose a [mock behavior]({%slug justmock/basic-usage/mock-behaviors%}).

## Mock Static Property

Once you set up the static type, you can mock its properties as you would do with instance properties. 

#### [C#] Example 2: Mock a static property

{{region justmock-getting-started-basics-static_2}}

    Mock.Arrange(() => GiftsDistributor.MinimalProductsCount).Returns(0);
{{endregion}}

## Mock Static Method

Mocking static methods is also similar to how you mock instance methods. Just use the **`Mock.Arrange()`** method to setup the behavior you need.

#### [C#] Example 3: Mock a static method

{{region justmock-getting-started-basics-static_3}}

    Mock.SetupStatic(typeof(GiftsDistributor), StaticConstructor.Mocked);
    Order order = new Order(new List<Product>());
     
    Mock.Arrange(() => GiftsDistributor.IsEligibleForGift(order)).Returns(true);
    // Ensure the original implementation of AddGift is being invoked
    Mock.Arrange(() => GiftsDistributor.AddGift(order)).CallOriginal();
    GiftsDistributor.AddGift(order);
     
    // Assert 
    // Ensure that a gift is added to the order
    Assert.AreEqual(1, order.Products.Count);
    
{{endregion}}

## Mock Extension Method

Extension methods in .NET are also defined as static ones. To demonstrate how you can mock extension methods, we will use the following sample implementation extending the functionality of the `Order` class.

#### [C#] Sample static class

{{region justmock-getting-started-basics-static_4}}

    public static class OrderExtensions
    {
        public static string PrintOrder(this Order order)
        {
            StringBuilder result = new StringBuilder();
            foreach (var product in order.Products)
            {
                result.AppendLine(string.Format("Purchased {0} {1}(s)", product.Quantity, product.Name));
            }
     
            return result.ToString();
        }
    }
{{endregion}}

To mock `PrintOrder` you would again need only to arrange your expectations with `Mock.Arrange()`.

#### [C#] Example 4: Mock extension method

{{region justmock-getting-started-basics-static_5}}

    Order order = new Order(new List<Product>());
     
    string expected = "Mocked";
    Mock.Arrange(() => order.PrintOrder()).Returns(expected);
     
    string actual = order.PrintOrder();
                
    Assert.AreEqual(expected, actual);
{{endregion}}

## Next Steps

Take a look at the [Mock Non-Public API]({%slug justmock/getting-started/basics/non-public%}) article that shows how you can setup private and internal implementations in your tests.

## See Also

 * [Call Original]({%slug justmock/basic-usage/mock/call-original%})

 * [Do Instead]({%slug justmock/basic-usage/mock/do-instead%})

 * [Do Nothing]({%slug justmock/basic-usage/mock/do-nothing%})

 * [Returns]({%slug justmock/basic-usage/mock/returns%})
 
 * [Static Mocking]({%slug justmock/advanced-usage/static-mocking%})
 
 * [Extension Methods Mocking]({%slug justmock/advanced-usage/extension-methods-mocking%})
