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

__Telerik® JustMock__ allows you to perform unit testing in conjunctions with the Microsoft Entity Framework.

## Introduction

With __Microsoft Entity Framework__, you develop data access applications by using a conceptual application model instead of a relational storage schema.

__JustMock__ supports the Microsoft Entity Framework thanks to the [Telerik.JustMock.EntityFramework](https://github.com/tailsu/Telerik.JustMock.EntityFramework) package. This package allows you to easily create in-memory mocks of the DbSet and DbContext types. It also provides additional mocking amenities for JustMock.

In this topic, we will cover some scenarios in unit testing Microsoft Entity Framework. In the examples below, we use the `DbContext` class along with the following methods:

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

The following steps demonstrate how to return a fake collection:

1. Create a method that returns a fake collection of `Dinner`s. For this example, we use the code below:

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

1. Create a new instance of the `NerdDinners` class.

1. __Arrange__ that a call to the `nerdDinners.Dinners()` method will return our fake collection.

1. Call the `nerdDinners.Dinners()` and search for a `dinner` with a certain `DinnerID` in the __Act__.

1. __Assert__ that there is only one item in our collection and this item has `DinnerID` equal to one.

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

## Returning A Fake Collection with Future Mocking

In this example we will return the same fake collection.

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

To assure that the instance does not matter during the __Act__ phase we will make a repository class:

  #### __[C#]__

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

  #### __[C#]__

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

  #### __[C#]__
  
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
