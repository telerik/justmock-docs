---
title: Entity Framework Mocking
page_title: Entity Framework Mocking | JustMock Documentation
description: Entity Framework Mocking
previous_url: /advanced-usage-entity-framework-mocking.html
slug: justmock/advanced-usage/entity-framework-mocking
tags: entity,framework,mocking
published: True
position: 15
---

# Entity Framework Mocking

With __Microsof tEntity Framework__ you develop data access application by using a conceptual application model instead of relational storage schema.

__Telerik® JustMock__ can be used in conjunction with Microsoft Entity Framework, with the [Telerik.JustMock.EntityFramework](https://github.com/tailsu/Telerik.JustMock.EntityFramework) package. With it, you can easily create in-memory mocks of the DbSet and DbContext types. It also provides some additional mocking amenities for JustMock.

In this topic we will cover some scenarios in unit testing Microsoft EntityF ramework. In the examples below the `DbContext` class is used, along with the following methods:

  #### __[C#]__

  {{region EntityFramework#Samples}}
	public class NerdDinners : DbContext
	{
	    public DbSet<Dinner> Dinners { get; set; }
	    public DbSet<RSVP> RSVPs { get; set; }
	}
	
	public class Dinner
	{
	    public int DinnerID { get; set; }
	    public string Title { get; set; }
	    public DateTime EventDate { get; set; }
	    public string Address { get; set; }
	    public string HostedBy { get; set; }
	}
	
	public class RSVP
	{
	    public int RSVPID { get; set; }
	    public int DinnerID { get; set; }
	    public string AtendeeEmail { get; set; }
	}
   
  {{endregion}}


## Returning A Fake Collection

First we need a method that returns a fake collection of `Dinner`s. We will use the example below:

  #### __[C#]__

  {{region EntityFramework#FakeDinnersReturn}}
    public IList<Dinner> FakeDinners()
	{ 
	    List<Dinner> fakeDin = new List<Dinner>
	    {
	        new Dinner { Address = "1 Microsoft way", DinnerID = 1, EventDate =DateTime.Now, HostedBy = "Telerik" , Title = "Telerik Dinner"}
	    };
	
	    return fakeDin;
	}
  {{endregion}}

After creating a new instance of the `NerdDinners` class, we will __Arrange__ that a call to `nerdDinners.Dinners()` method, will return our fake collection. Then, in the __Act__, we call the `nerdDinners.Dinners()` and search for a `dinner` with a certain `DinnerID`. Finally we __Assert__ that there is only one item in our collection and this item has `DinnerID` equal to one.

> **Important**
>
> Note that when you use `ReturnsCollection()` you must be using the `Telerik.JustMock.Helpers;`.

  #### __[C#]__

  {{region EntityFramework#MockingCollectionReturn}}
	[TestMethod]
	public void ShouldReturnFakeCollectionWhenExpected()
	{
	    NerdDinners nerdDinners = new NerdDinners();
	
	    // Arrange
	    Mock.Arrange(() => nerdDinners.Dinners).ReturnsCollection(FakeDinners());
	
	    // Act
	    var query = from d in nerdDinners.Dinners
	                where d.DinnerID == 1
	                select d;
	
	    // Assert
	    Assert.AreEqual(1, query.Count());
	    Assert.AreEqual(1, query.First().DinnerID);
	
	}
  {{endregion}}

## Returning A Fake Collection With Future Mocking

In this example we will return the same fake collection.

  {{region EntityFramework#FakeDinnersReturn}}
    public IList<Dinner> FakeDinners()
	{ 
	    List<Dinner> fakeDin = new List<Dinner>
	    {
	        new Dinner { Address = "1 Microsoft way", DinnerID = 1, EventDate =DateTime.Now, HostedBy = "Telerik" , Title = "Telerik Dinner"}
	    };
	
	    return fakeDin;
	}
  {{endregion}}

To assure that the instance does not matter during the __Act__ phase we will make a repository class:

  {{region EntityFramework#DinnersRepository}}
	public class DinnerRepository
	{
	    public Dinner GetById(int dinnerId)
	    {
	        NerdDinners nerdDinners = new NerdDinners();
	        var query = from d in nerdDinners.Dinners
	                    where d.DinnerID == 1
	                    select d;
	
	        return query.First();
	    }
	}
  {{endregion}}

As you see, in the test below we are acting with a `new DinnerRepository()`, but still we are meeting the expectations and the test passes. This behavior is known and expected in [Future Mocking]({%slug justmock/advanced-usage/future-mocking%}).

> Note that when you use `ReturnsCollection()` you must be using the `Telerik.JustMock.Helpers;` namespace.

  {{region EntityFramework#FutureMockingCollectionReturn}}
	[TestMethod]
	public void ShouldReturnFakeCollectionForFutureInstance()
	{
	    NerdDinners nerdDinners = new NerdDinners();
	
	    Mock.Arrange(() => nerdDinners.Dinners).IgnoreInstance().ReturnsCollection(FakeDinners());
	
	    Assert.AreEqual(1, new DinnerRepository().GetById(1).DinnerID);
	}	
  {{endregion}}

## Faking the Add of an Entity

In the next example we will __Arrange__ the calling of the `Add()` method to actually add an item to a previously created local collection.

  {{region EntityFramework#FakingAdd}}
	[TestMethod]
	public void ShouldReturnFakeCollectionForFutureInstance()
	{
	    NerdDinners nerdDinners = new NerdDinners();
	
	    Mock.Arrange(() => nerdDinners.Dinners).IgnoreInstance().ReturnsCollection(FakeDinners());
	
	    Assert.AreEqual(1, new DinnerRepository().GetById(1).DinnerID);
	}	
  {{endregion}}

Here are the steps:

1. Create an instance of the `NerdDinners` class.
1. Create a new `Dinner` with some ID and a `List` of `Dinner` instances.
1. Arrange `nerdDinners.Dinners.Add()` method to add the object from *step 2.* to the local collection from the same step.
1. Arrange that the `SaveChanges()` method is doing nothing.
1. Act by calling `Add(dinner)` and `SaveChanges()`.
1. Verify that:
	* the collection has exactly *one* item
	* this item is exactly the object from *step 2.*
