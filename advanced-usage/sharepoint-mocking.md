---
title: SharePoint Mocking
page_title: SharePoint Mocking | JustMock Documentation
description: SharePoint Mocking
previous_url: /advanced-usage-sharepoint-mocking.html
slug: justmock/advanced-usage/sharepoint-mocking
tags: sharepoint,mocking
published: True
position: 18
---

# SharePoint Mocking

__Microsoft SharePoint__ is a browser based web platform where you can set up websites to share and manage information and documents, and publish reports in order to make it easier for people to work together.

Unit testing in Microsoft SharePoint faces several problems that __Telerik® JustMock__ overcomes:
* *Interfaces are rarely used* - most objects in Microsoft SharePoint don't implement public interfaces.
* *Sealed classes* - many Microsoft SharePoint elements are sealed.
* *Internal constructors* - many Microsoft SharePoint elements have internal constructors. Even if there is no behavior that we need to mock, we still need to create an instance of it.
* 
In this topic we will cover some scenarios in unit testing Microsoft SharePoint.

## Disallow SPContext.Current

Disallow calling `SPContext.Current` property.

```C#
[TestMethod]
[ExpectedException(typeof(ApplicationException))]
public void ShouldThrowApplicationExceptionOnCurrent()
{
	// Arrange
	Mock.Arrange(() => SPContext.Current).Throws(new ApplicationException("Not allowed to call SPContext.Current"));

	// Act
	SPContext currentContext = SPContext.Current;
}
```

In this example we mock the static `SPContext` class. We arrange the `Current` property to throw an `ApplicationException` once it is called. The constructor will initialize the `Message` property of the exception using the "Not allowed to call SPContext.Current" string.

## Mocking SPContext.Current

Arranging `SPContext.Current` property to return mock fake object.

```C#
// Arrange
var fakeContext = Mock.Create<SPContext>();
Mock.Arrange(() => SPContext.Current).Returns(fakeContext);

// Act
SPContext currentContext = SPContext.Current;

// Assert
Assert.IsNotNull(currentContext, "The current SPContext should not be null");
```


Here we arrange the `Current` property getter to return fake object when called. After acting with `SPContext currentContext = SPContext.Current` we verify that our fake object was actually returned initializing the `Current` property. If the assertion fails, we specify that the message "The current SPContext should not be null" should be displayed.

## Mocking SPContext.Current.Site

In this example we will mock the `Site` property get of the `SPContext.Current` object. We will use the following class:

```C#
public class Site
{
	public static string GetHomePageUrl()
	{
		return SPContext.Current.Site.Url;
	}
}
```


Follows the actual test code:

```C#
// Arrange
var fakeSiteUrl = "https://www.telerik.com";
var fakeSharepointSite = Mock.Create<SPSite>();

Mock.Arrange(() => SPContext.Current.Site).Returns(fakeSharepointSite);
Mock.Arrange(() => fakeSharepointSite.Url).Returns(fakeSiteUrl);

// Act
string actualUrl = Site.GetHomePageUrl();

Assert.AreEqual(fakeSiteUrl, actualUrl);
```


We arrange that once the `Url` property of the static `SPContext.Current.Site` is called, "https://www.telerik.com" is returned as a result. We act by calling the `GetHomePageUrl` method in our sample class. It returns the *ulr of the current site* and thus "https://www.telerik.com". Finally, this behavior is verified.

## Mocking AllowAnonymousAccess Property

In the next two examples we mock the `AllowAnonymousAccess` property of `SPContext.Web` and `SPContext.Current.Web` objects.

Firstly, let's mock `SPContext.Web`:

```C#
// Arrange
var fakeContext = Mock.Create<SPContext>();
Mock.Arrange(() =>fakeContext.Web.AllowAnonymousAccess).Returns(true);

// Assert
Assert.IsTrue(fakeContext.Web.AllowAnonymousAccess, "Our SPWeb should allow anonymous access");
```

Here we create fake instance of `SPContext` and then arrange the `AllowAnonymousAccess` property to return `true`.

Now, let's mock `SPContext.Current.Web`:

```C#
// Arrange
var fakeContext = Mock.Create<SPContext>();
Mock.Arrange(() => SPContext.Current).Returns(fakeContext);

var fakeWeb = Mock.Create<SPWeb>();
Mock.Arrange(() => fakeContext.Web).Returns(fakeWeb);
Mock.Arrange(() => fakeWeb.AllowAnonymousAccess).Returns(true);

// Assert
Assert.IsTrue(SPContext.Current.Web.AllowAnonymousAccess, "Anonymous access should be allowed on our current SPWeb");
```

We create a fake instance of `SPContext` and pass it to the `SPContext.Current` property. Then a fake instance of `SPWeb` is created that is arranged to be returned as a result once its `Web` property getter is called. Then, as in the previous example, `AllowAnonymousAccess` is arranged to return `true`. Finally, we verify.

In both examples, if the assertion fails, a message is displayed.

## Mocking SPList Class

Follows a slightly more complicated example. Here we mock `SPList`.
 
```C#
// Arrange
var spWeb = Mock.Create<SPWeb>();
var spList = Mock.Create<SPList>();

var spListCollection = Mock.Create<SPListCollection>();
var spListItemCollection = Mock.Create<SPListItemCollection>();

Mock.Arrange(() => spWeb.Lists).Returns(spListCollection);
Mock.Arrange(() => spListCollection[Arg.AnyString]).Returns(spList);

Mock.Arrange(() => spList.GetItems(Arg.IsAny<SPQuery>())).Returns(spListItemCollection);

// Assert
Assert.AreEqual(spListCollection, spWeb.Lists);
Assert.AreEqual(spList, spWeb.Lists["myList"]);
Assert.AreEqual(spListItemCollection, spWeb.Lists["myList"].GetItems(new SPQuery()));
```


This is what we do:

1. Create a fake instance of `SPWeb`
1. Create a fake instance of `SPList`
1. Create a fake instance of `SPListCollection`
1. Create a fake instance of `SPListItemCollection`
1. Mock the fake instance of `SPWeb` from *step 1.* to return the fake instance of `SPListCollection` from *step 3.* once the `List` property get is called.
1. Next, arrange that a call to any index in the fake object of `SPListCollection` returned in the previous step should result in the fake object of `SPList` from *step 2.*
1. Finally in our arrangement, specify that getting any item from our `SPList` from the previous step should result in the fake object of `SPListItemCollection` from *step 4.*
1. Act/Assert. The last thing to do in this example is to verify that every call will result in the proper fake object as specified above:
	* `spWeb.Lists` will result in `spListCollection`
	* `spWeb.Lists` accessed by any string index will result in `spList`
	* getting any item from any `SPList` will result in `spListItemCollection`

To further understand the example, consider the following class hierarchy:

* `SPWeb.List` returns `SPListCollection`
	* `SPListCollection[`"myList"`]` returns `SPList`
		* `SPList.GetItems(new SPQuery())` returns `SPListItemCollection`
			* `SPListItemCollection`

