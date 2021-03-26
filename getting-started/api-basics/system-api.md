---
title: Mock System API
page_title: Mock System API | JustMock Documentation
description: Check out this topic to learn how you can mock methods and properties defined in .NET with JustMock.
slug: justmock/getting-started/basics/system-api
tags: mock, test, method, property, system, mscorlib, file, stream, date, time
published: True
position: 6
---

# Mock System API

**JustMock** enables you to not only mock the implementations inside your app but allows you to mock the APIs provided by the .NET. You can control the behavior and arrange the members of the framework classes using the already familiar approaches without needing any additional setup.

You can mock any class or member – no matter whether you need a mock for `DateTime`, `Stream`, `File`, `String`, dictionaries, collections or any other framework implementation – with **JustMock** you can customize it.

> This feature is available only in the commercial version of JustMock. Refer to [this]({%slug justmock/getting-started/commercial-vs-free-version%}) topic to learn more about the differences between the commercial and free versions of Telerik JustMock.

The following `Order` class uses a `DateTime` value to construct the receipt for a specific order. This value is assigned when creating the order.

#### [C#] Sample setup

{{region justmock-getting-started-basics-system-api_0}}

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
        private DateTime orderDate;
        public Order(List<Product> products)
        {
            this.Products = products;
            this.orderDate = DateTime.Now;
        }
     
        public List<Product> Products { get; private set; }
     
        public string GetReceipt()
        {
            StringBuilder result = new StringBuilder();
            foreach (var product in this.Products)
            {
                result.AppendLine(string.Format("Purchased {0} {1}", product.Quantity, product.Name));
            }
     
            result.Append("Order Date: " + this.orderDate.ToString("d"));
     
            return result.ToString();
        }
    }

{{endregion}}

This is one of the hardest to test scenarios as the `DateTime` values can contain seconds or milliseconds that can also frequently change. Furthermore, using `DateTime.Now` in an internal logic guarantees that a test won’t be stable enough if you depend on that particular value as `DateTime.Now` will return different result when the order is created and when you try to verify the value. 

**JustMock** gives you a way to control how the `DateTime` (or any other class) should behave. **Example 1** shows you a test ensuring that the receipt is properly generated. The invocation of `DateTime.Now` is arranged to always return the same value no matter who calls the property getter.

#### [C#] Example 1: Mock DateTime.Now

{{region justmock-getting-started-basics-system-api_1}}
    
    DateTime testDateTime = new DateTime(2020, 1, 1);
    // Arrange the return value of DateTime.Now
    Mock.Arrange(() => DateTime.Now).Returns(testDateTime);
     
    // Create a new Order - its constructor will pick up the test DateTime value as arranged above
    Order order = new Order(new List<Product>() { new Product("shirt", 1) });
     
    string receipt = order.GetReceipt();
    string expected = "Purchased 1 shirt\r\nOrder Date: " + testDateTime.ToString("d");
     
    Assert.AreEqual(expected, receipt);
{{endregion}}

Using that approach, you can mock the behavior of any method or property defined in the framework. For more examples on the topic, refer to the [MsCorlib Mocking]({%slug justmock/advanced-usage/mscorlib-mocking%}) article.

## See Also

* [MsCorlib Mocking]({%slug justmock/advanced-usage/mscorlib-mocking%})