---
title: PostSharp
page_title: PostSharp | JustMock Documentation
description: Integrate PostSharp with Telerik JustMock
previous_url: /integration-postsharp.html
slug: justmock/integration/postsharp
tags: postsharp
published: True
position: 6
---

# PostSharp

This topic describes how you can use Telerik JustMock to mock member calls inside PostSharp aspects.

In brief [PostSharp](http://www.sharpcrafters.com/) is a tool that makes aspect-oriented programming easy. Note that the JustMock integration is available for PostSharp version 2.1.2.8 and above. If you are new to the AOP model you can look through the PostSharp [documentation](http://www.sharpcrafters.com/postsharp/documentation) describing the key principles involved.

## Example

Consider that you want to authorize an __ASP.NET MVC__ controller action to enshure that it validates the currently logged in user’s identity. For the purpose of this example we will need the following classes:

  #### __[C#]__

  {{region PostSharp#SampleCode1}}
	public class Identity
	{
	    public static bool IsAuthenticated
	    {
	        get
	        {
	            throw new NotImplementedException();
	        }
	    }
	}
  {{endregion}}


  #### __[C#]__

  {{region PostSharp#SampleCode2}}
	public class Order
	{
	    public Order(string productName, int quantity)
	    {
	        this.ProductName = productName;
	        this.Quantity = quantity;
	    }
	
	    public string ProductName { get; private set; }
	    public int Quantity { get; private set; }
	    public bool IsFilled { get; private set; }
	    public DateTime OrderDate { get; set; }
	    public DateTime FillDate { get; set; }
	}
	
	public class Inventory
	{
	    [Authorize]
	    public bool PlaceOrder(Order order)
	    {
	        orders.Add(order);
	        return true;
	    }
	
	    public bool HasOrders()
	    {
	        return orders.Count > 0;
	    }
	
	    private IList<Order> orders = new List<Order>();
	}
  {{endregion}}


The `Inventory` class contains the `Orders` of an authenticated user. To ensure that the user is authenticated we have used the `AuthorizeAttribute` which has the following definition:

  #### __[C#]__

  {{region PostSharp#AuthorizeAttribute}}
	[Serializable]
	public class AuthorizeAttribute : MethodInterceptionAspect
	{
	    public override void OnInvoke(MethodInterceptionArgs args)
	    {
	        if (Identity.IsAuthenticated)
	            base.OnInvoke(args);
	    }
	}
  {{endregion}}


The `MethodInterceptionAspect` provides a virtual method `OnInvoke`, for us to implement. This method is called in place of the target method call. It is here where we determine if and how the target method is going to be invoked. We do this using the provided `MethodInterceptionArgs`. In our case we want to check whether the user is authenticated.

Note that we have marked the class with the `SerializableAttribute` which is a required attribute by PostSharp. Once you compile the code it will actually write the necessary hooks inside the method based on the aspects you have declared.

Next is a simple test using [MSpec](https://codebetter.com/aaronjensen/2008/05/08/introducing-machine-specifications-or-mspec-for-short/) where we ensured that an order is placed to the inventory only if the identity is valid.

  #### __[C#]__

  {{region PostSharp#Identity}}
	[Subject(typeof(Inventory))]
	public class Authorization_Ensures_a_Valid_Identity
	{
	    Establish context = () =>
	    {
	        Mock.Arrange(() => Identity.IsAuthenticated).Returns(true);
	        inventory = new Inventory();
	    };
	
	    private Because of = () =>
	    {
	        inventory.PlaceOrder(new Order("Taslisker", 1) { OrderDate = DateTime.Now });
	    };
	
	    private It should_assert_that_order_place_was_successful = () =>
	      inventory.HasOrders().ShouldBeTrue();
	
	    static Inventory inventory;
	}
  {{endregion}}


Here we have mocked the static `Identity` call using JustMock which intercepts the calls on top of the PostSharp hooks during authorization. For the purpose of this example we have used the PostSharp starter edition.
