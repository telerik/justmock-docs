---
title: Extension Methods Mocking
page_title: Extension Methods Mocking | JustMock Documentation
description: Extension Methods Mocking
previous_url: /advanced-usage-extension-methods-mocking.html
slug: justmock/advanced-usage/extension-methods-mocking
tags: extension,methods,mocking
published: True
position: 20
---

# Extension Methods Mocking

Mocking extension methods is one of the advanced features supported in __Telerik®  JustMock__. In this topic, we will go through some examples that show how easy and straightforward it is to assert expectations related to extension methods in your tests.

> This feature is available only in the commercial version of Telerik JustMock. Refer to [this]({%slug justmock/licensing/license-agreement%}#commercial-vs-free-version) topic to learn more about the differences between both the commercial and free versions of Telerik JustMock.

## How To Mock Extension Methods?

You can mock any extension method as you would do it with any other instance method. There is no need to add specific setup or use a dedicated API for extension methods. The `Mock.Arrange()` method will help you set up the behavior or expectations you need. 

To illustrate this in more examples we will use the following code:

#### __[C#] Sample setup__

{{region ExtensionMethodsMocking#SampleCode1}}
    
    public class Foo
    {
        public void Execute()
        {
            throw new NotImplementedException();
        }
    
        public string Title { get; set; }
    }
    
    public static class FooExtensions
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


When you have to assert some expectations related to extension methods, you can use everything you already know. Let's go through a very simple example.

#### __[C#] Example 1: Arrange extension method behavior__

{{region ExtensionMethodsMocking#SampleCode2}}

    [TestMethod]
    public void ShouldAssertExtensionMethodMockingWithArguments()
    {
        var foo = new Foo();
    
        string expected = "bar";
        // Arrange that when the `Foo.Echo` extension method is called, the returned value should be a specific value ("bar").
        Mock.Arrange(() => foo.Echo(Arg.IsAny<string>())).Returns(expected);
    
        // Call the extension method with another value
        string result = foo.Echo("hello");
    
        // Assert that the correct value is returned
        Assert.AreEqual(expected, result);
    }
{{endregion}}


Next, we have a more complex sample where the extension method has more arguments.

#### __[C#] Example 2: Arrange the behavior of extension method with several parameters__

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

>For more information on dealing with method parameters, check the [Matchers]({%slug justmock/basic-usage/matchers%}) help topic.

## Assert Occurrences of Extension Methods

Another thing we can assert is the occurrence of the extension methods. Let's start with an example.

#### __[C#] Example 3: Arrange extension method occurrence__

{{region ExtensionMethodsMocking#SampleCode4}}

	[TestMethod]
	[ExpectedException(typeof(AssertFailedException))]
	public void ShouldAssertOccurrencesForExtenstionMethod()
	{
	    var foo = Mock.Create<Foo>();
	
	    string expected = "test";
	
	    // Arrange that the extension method invocation should never occur.
	    Mock.Arrange(() => foo.Echo(Arg.IsAny<string>())).Returns(expected).OccursNever();
	
	    foo.Echo(expected);
	
	    Mock.Assert(foo));
	}
{{endregion}}


The sample code arranges that the extension method should never occur as described in the [Asserting Occurrence]({%slug justmock/basic-usage/asserting-occurrence%}) topic. However, we then call the extension method and assert that an exception is thrown.

## Mock Interface Extension Method Calls

This section will show that we can easily mock interface extension method calls when needed. In the example, we will use the following classes:

#### __[C#] Sample setup__

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

#### __[C#] Example 4: Arrange the behavior of extension method on interface__
  
{{region ExtensionMethodsMocking#SampleCode6}}

	[TestMethod]
	public void ShouldAssertInterfaceExtensionMethodCall()
	{
	    var provider = Mock.Create<DataProvider>();
	    var objectScope = Mock.Create<IObjectScope>();
	
	    // When called, GetScope should return objectScope.
	    Mock.Arrange(() => provider.GetScope()).Returns(objectScope);
	
	    var ret = provider.GetScope();
	
	    Assert.IsTrue(ret.Equals(objectScope));
	}
{{endregion}}


Here we have arranged that the call to the `GetScope` method should return a specific mocked instance of the `IObjectScope` interface. We act and assert that the correct value is returned.

## See Also

* [Asserting Occurrence]({%slug justmock/basic-usage/asserting-occurrence%})
* [Matchers]({%slug justmock/basic-usage/matchers%})
* [Static Mocking]({%slug justmock/advanced-usage/static-mocking%})