---
title: Setup Callbacks With REF And OUT Parameters
page_title: Setup Callbacks With REF And OUT Parameters | JustMock Documentation
description: Setup Callbacks With REF And OUT Parameters
previous_url: /scenarios-setup-callbacks-with-ref-and-out-parameters.html
slug: justmock/scenarios/setup-callbacks-with-ref-and-out-parameters
tags: setup,callbacks,with,ref,and,out,parameters
published: True
position: 0
---

# Setup Callbacks With REF And OUT Parameters

By using __Telerik® JustMock__ you can mock methods that take `out` or `ref` parameters.

In the following examples, we will use the following sample code to test:

  #### __[C#]__

  {{region OutRef#SampleClass}}
    public interface IFoo
	{
	    bool Execute(string arg1, out string arg2);
	    void Execute(ref string arg1);
	}  
  {{endregion}}


## Expect Out Arguments

Let's arrange that an `out` argument should have a particular value.

  #### __[C#]__

  {{region OutRef#Out}}
    // Arrange
	var iFoo = Mock.Create<IFoo>();
	
	string expected = "pong";
	Mock.Arrange(() => iFoo.Execute("ping", out expected)).Returns(true);
	
	// Act
	string actual;
	bool called = false;
	called = iFoo.Execute("ping", out actual);
	
	// Assert
	Assert.IsTrue(called);
	Assert.AreEqual(expected, actual);  
  {{endregion}}


In the arrange statement we use the `out` keyword and pass the already initialized variable. Thus, we arrange that a call to the `Execute` method, passing `"ping"` as first argument, should set the `out` argument to `"pong"`.

## Assert Ref Arguments

We use the same technique as in the `out` argument example to set a value for the `ref` argument.

  #### __[C#]__

  {{region OutRef#Ref}}
    // Arrange
	var iFoo = Mock.Create<IFoo>();
	
	string expected = "foo";
	Mock.Arrange(() => iFoo.Execute(ref expected)).DoNothing();
	
	// Act
	string actual = string.Empty;
	iFoo.Execute(ref actual);
	
	// Assert
	Assert.AreEqual(expected, actual);  
  {{endregion}}


In the arrange statement we use the `ref` keyword and pass the already initialized variable. Thus, we arrange that a call to the `Execute` method should set the `ref` argument to `"foo"`.
