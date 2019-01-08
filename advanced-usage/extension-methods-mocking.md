---
title: Extension Methods Mocking
page_title: Extension Methods Mocking | JustMock Documentation
description: Extension Methods Mocking
previous_url: /advanced-usage-extension-methods-mocking.html
slug: justmock/advanced-usage/extension-methods-mocking
tags: extension,methods,mocking
published: True
position: 21
---

# Extension Methods Mocking

Extension Methods mocking is one of the advanced features supported in __Telerik®  JustMock__. In this topic we will go through some examples that show how easy and straightforward it is to assert expectations related to extension methods in your tests.

> This feature is available only in the commercial version of Telerik JustMock. Refer to [this]({%slug justmock/getting-started/commercial-vs-free-version%}) topic to learn more about the differences between both the commercial and free versions of Telerik JustMock.

To illustrate this in more examples we will use the following code:

  #### __[C#]__

  {{region ExtensionMethodsMocking#SampleCode1}}
	public class Foo
	{
	    public void Execute()
	    {
	        throw new NotImplementedException();
	    }
	
	    public string Title { get; set; }
	}
	
	public static class ExtendFoo
	{
	    public static int Echo(this Foo foo)
	    {
	        return default(int);
	    }
	
	    public static string Echo(this Foo foo, string value)
	    {
	        return value;
	    }
	
	    public static int Echo(this Foo foo, int arg1, int arg2)
	    {
	        return default(int);
	    }
	}
  {{endregion}}


## Mocking Extension Methods

When you have to assert some expectations related to extension methods, you can use everything you already know. Let's go through a very simple example.

  #### __[C#]__

  {{region ExtensionMethodsMocking#SampleCode2}}
    [TestMethod]
	public void ShouldAssertExtensionMethodMockingWithArguments()
	{
	    var foo = new Foo();
	
	    string expected = "bar";
	
	    Mock.Arrange(() => foo.Echo(Arg.IsAny<string>())).Returns(expected);
	
	    string result = foo.Echo("hello");
	
	    Assert.AreEqual(expected, result);
	}
  {{endregion}}

Here we have used an object of type `Foo`. We first arrange that when its `Echo` extension method is called the returned value should be a specific value. Then we act - we call the extension method with another value and finally assert that the correct value is returned.

Next we have a more complex sample where the extension method has more arguments.

  #### __[C#]__

  {{region ExtensionMethodsMocking#SampleCode3}}
    [TestMethod]
	public void ShouldAssertExtensionMethodWithMultipleArguments()
	{
	    var foo = new Foo();
	
	    Mock.Arrange(() => foo.Echo(Arg.IsAny<int>(), Arg.Matches<int>(x => x == 10))).Returns((int arg1, int arg2) => arg1 + arg2);
	
	    int ret = foo.Echo(1, 10);
	
	    Assert.AreEqual(11, ret);
	}
  {{endregion}}


Similarly, we arrange that when the `Echo` extension method is called, it should return the sum of its arguments if the second argument is equal to 10.

## Assert Occurrences of Extension Methods

Another thing we can assert is the occurrence of the extension methods. Let's start with an example.

  #### __[C#]__

  {{region ExtensionMethodsMocking#SampleCode4}}
	[TestMethod]
	[ExpectedException(typeof(AssertFailedException))]
	public void ShouldAssertOccurrencesForExtenstionMethod()
	{
	    var foo = Mock.Create<Foo>();
	
	    string expected = "test";
	
	    Mock.Arrange(() => foo.Echo(Arg.IsAny<string>())).Returns(expected).OccursNever();
	
	    foo.Echo(expected);
	
	    Mock.Assert(foo));
	}
  {{endregion}}

Here we first arrange that the extension method should never occur as described in the [Asserting Occurrence]({%slug justmock/basic-usage/asserting-occurrence%}) topic. However we then call the extension method and assert that an exception is thrown.

## Mock Interface Extension Method calls

This last section will show that we can easily mock interface extension method calls when needed. In the example we will use the following classes:

  {{region ExtensionMethodsMocking#SampleCode5}}
	public interface IDataProvider
	{
	}
	
	public interface IObjectScope
	{
	}
	
	public class DataProvider : IDataProvider
	{
	}
	
	public static class DataExtensions
	{
	    public static IObjectScope GetScope(this IDataProvider provider)
	    {
	        throw new NotImplementedException();
	    }
	}
  {{endregion}}

We have the `IDataProvider` interface which we extend with the `GetScope` extension method. We can now write tests that mock the extension method's execution:

  {{region ExtensionMethodsMocking#SampleCode6}}
	[TestMethod]
	public void ShouldAssertInterfaceExtensionMethodCall()
	{
	    var libararies = Mock.Create<DataProvider>();
	    var objectScope = Mock.Create<IObjectScope>();
	
	    Mock.Arrange(() => libararies.GetScope()).Returns(objectScope);
	
	    var ret = libararies.GetScope();
	
	    Assert.IsTrue(ret.Equals(objectScope));
	}
  {{endregion}}

Here we have arranged that the call to the `GetScope` method should return a specific mocked instance of the `IObjectScope` interface. We act and assert that the correct value is returned.
