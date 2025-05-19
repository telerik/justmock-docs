---
title: Mocking LINQ Queries
page_title: Mocking LINQ Queries | JustMock Documentation
description: Mocking LINQ Queries
previous_url: /advanced-usage-mocking-linq-queries.html
slug: justmock/advanced-usage/mocking-linq-queries
tags: mocking,linq,queries
published: True
position: 12
---

# Mocking LINQ Queries

This article provides practical examples that demonstrate how to mock LINQ queries with __Telerik® JustMock__ and custom select.

> This feature is available only in the commercial version of Telerik JustMock. Refer to [this]({%slug justmock/licensing/commercial-vs-free-version%}) topic to learn more about the differences between the commercial and free versions of Telerik JustMock.

If you need a complete Visual Studio project that demonstrates how to mock LINQ queries, refer to our demo. The default installation directory is C:\Program Files (x86)\Progress\Telerik JustMock\Examples.

## Prerequisites

In the further examples, we will use the following method and sample classes to test:

  #### __[C#]__

  {{region LINQ#Samples}}
	public class SimpleData
	{
	    public ExtendedQuery<Product> Products
	    {
	        get
	        {
	            return null;
	        }
	    }
	
	    public ExtendedQuery<Category> Categories
	    {
	        get
	        {
	            return null;
	        }
	    }
	
	    public Product GetProduct( int id )
	    {
	        return this.Products.Where( x => x.ProductID == id ).FirstOrDefault();
	    }
	}
	
	public abstract class ExtendedQuery<T> : IEnumerable<T>
	{
	    public IEnumerator<T> GetEnumerator()
	    {
	        throw new System.NotImplementedException();
	    }
	
	    System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
	    {
	        throw new System.NotImplementedException();
	    }
	}
	
	public class Product
	{
	    public int ProductID { get; set; }
	    public int CategoryID { get; set; }
	    public string ProductName { get; set; }
	    public int UnitsInStock { get; set; }
	    public string QuantityPerUnit { get; set; }
	    public bool Discontinued { get; set; }
	    public int voa_class { get; set; }
	
	    public int GetId()
	    {
	        return this.ProductID;
	    }
	}
	
	public class Category
	{
		public int CategoryID { get; set; }
		public string CategoryName { get; set; }
	}
	
	private List<Product> GetProductLists()
	{
	    var productList = new List<Product>()
	    {
	        new Product() { ProductID = 1, CategoryID = 1, ProductName = "test product", UnitsInStock = 3, QuantityPerUnit = "1", Discontinued = false, voa_class = 1 },
	        new Product() { ProductID = 2, CategoryID = 1, ProductName = "Foo stuff", UnitsInStock = 50, QuantityPerUnit = "1", Discontinued = true, voa_class = 1 },
	        new Product() { ProductID = 3, CategoryID = 2, ProductName = "More Stuff", UnitsInStock = 0, QuantityPerUnit = "1", Discontinued = true, voa_class = 1 }
	    };
	    return productList;
	}
	
	private List<Category> GetCategoriesLists()
	{
	    var categoriesList = new List<Category>()
	    {
	        new Category() { CategoryID = 1, CategoryName = "First" },
	        new Category() { CategoryID = 2, CategoryName = "Second" }
	    };
	    return categoriesList;
	}
  {{endregion}}


> **Important**
>
> To mock LINQ queries you first need to go to elevated mode by enabling TelerikJustMock from the menu. Learn how to do that in the [How to Enable/Disable Telerik JustMock](./advanced-usage#how-to-enabledisable-telerik-justmock) topic.

## Asserting Custom Select Query

In the following example, we mock `simple.Products` to return a custom collection. After that, a LINQ query is executed against the collection to obtain a specifically desired value:

  #### __[C#]__

  {{region LINQ#Example1}}
    [TestMethod]
	public void ShouldAssertCustomSelect()
	{
		var simple = new SimpleData();
		Mock.Arrange(() => simple.Products).ReturnsCollection(GetProductLists());
		
		var expected = (from p in simple.Products
		   	where p.UnitsInStock == 50
		   	select p.ProductID).SingleOrDefault();
		
		Assert.AreEqual(expected, 2);
	}
  {{endregion}}

As `simple.Products` will return the collection defined by `GetProductLists`, the `Product` with `UnitsInStock` property set to `50` has the `ProductID` set to `2`, and therefore the assertion will pass.

## Asserting Projection

Let's extend the previous example. Here, in our LINQ query, we don't get directly the values we need, but construct a new object of type `Product` and verify the `ProductID` property.

  #### __[C#]__

  {{region LINQ#Example2}}
  
    [TestMethod]
	public void ShouldAssertProjectionWhenCombinedWithWhere()
	{
		var simple = new SimpleData();
		Mock.Arrange(() => simple.Products).ReturnsCollection(GetProductLists());
	
		var expected = (from p in simple.Products
			where p.UnitsInStock == 50
			select new { p.ProductID, p.ProductName }).SingleOrDefault();
	
		Assert.AreEqual(expected.ProductID, 2);
	}
  {{endregion}}


There is only one `Product` in the collection returned from `GetProductLists()` with `UnitsInStock` = `50` and its `ProductID` property yields `2`.

## Asserting Queries with Join Operator

Furthermore, you can use Join operations to create associations between sequences that are not explicitly modeled in the data sources. For example, you can perform a join to find the `CategoryName` of each product.

  #### __[C#]__

  {{region LINQ#Example3}}
    [TestMethod]
	public void ShouldAssertUsingJoinClause()
	{
		var simple = new SimpleData();
		Mock.Arrange(() => simple.Products).ReturnsCollection(GetProductLists());
		Mock.Arrange(() => simple.Categories).ReturnsCollection(GetCategoriesLists());
	
		var query = from product in simple.Products
			join category in simple.Categories on product.CategoryID equals category.CategoryID	
			select category.CategoryName;
	
		Assert.AreEqual(3, query.Count());
	}
  {{endregion}}


## Asserting Enumerable Source

With JustMock, you can also mock an enumerable source. Let's add the following method and a class:

  #### __[C#]__

  {{region LINQ#Samples2}}
    public class MyDataContext : DataContext
	{
	    public MyDataContext() : base(string.Empty)
	    {
	
	    }
	
	    public Table<Product> Products
	    {
	        get
	        {
	            return this.GetTable<Product>();
	        }
	    }
	}
	
	private IQueryable<Product> GetFakeProducts()
	{
	    List<Product> productList = GetProductLists();
	
	    return productList.AsQueryable();
	}
  {{endregion}}


We arrange `dataContext.Products` to return the collection defined in the `GetFakeProducts` method. Yet it implements `IQueryable` interface, we are allowed to use it in our assertion. Here we verify it contains `3` elements.

  #### __[C#]__

  {{region LINQ#EnumerableCollection}}
    IList<Product> products;
	
	var dataContext = new MyDataContext();
		
	Mock.Arrange(() => dataContext.Products).ReturnsCollection(GetFakeProducts());
		
	products = dataContext.Products.ToList<Product>();
		            
	Assert.AreEqual(3, products.Count());
  {{endregion}}


## Asserting When Conditional Expression Is Passed in as a Parameter

You can also mock parameters when using the LINQ expressions. Let's walk through an example demonstrating how to mock the behavior of the `==` expression:

  #### __[C#]__

  {{region LINQ#SamplesExpr}}
    var simple = new SimpleData();
	int targetProductId = 2;
	int expectedId = 10;
	
	Mock.Arrange( () => simple.Products ).ReturnsCollection( GetProductLists() );
	Mock.Arrange( () => simple.Products.Where( x => x.ProductID == targetProductId ).First().GetId() ).Returns( expectedId ).MustBeCalled();
	
	Product actual = simple.GetProduct( targetProductId );
	
	Assert.AreEqual( expectedId, actual.GetId() );
  {{endregion}}
 

In this example, we first define that the product collection must be a specific list of products. We then use LINQ to select a specific instance from the `simple.Products` collection. For that instance, we want to mock the call to the `GetId()` method in order to return a different id. Finally, we retrieve the target product and verify that the returned id and the expected id are the same.
