---
title: Mocking Local Functions
page_title: Mocking Local Functions | JustMock Documentation
description: Mocking Local Functions
previous_url: /advanced-usage-mocking-local-functions.html
slug: justmock/advanced-usage/mocking-local-functions
tags: mocking,local,function
published: True
position: 17
---

# Mocking Local Functions

JustMock provides comprehensive API for mocking local function.  In most cases, mocking of local functions is not different than mocking class methods. Though there are some limitations that are caused by local functions syntax restrictions and compiler behavior.
To mock a local function, you need to provide the following information:

  1.	Name of the local function.
  2.	Containing method name and, if the method receives parameters, type parameters array.

> This feature is available only in the commercial version of Telerik JustMock. Refer to [this]({%slug justmock/licensing/commercial-vs-free-version%}) topic to learn more about the differences between the commercial and free versions of Telerik JustMock.

## Basic Local Function Mocking

The following code snippet demonstrates a basic scenario where a local function that returns integer result is mocked:

```C#
class Foo
{
	public int GetResult()
	{
		return 100 + GetLocal();
		int GetLocal ()
		{
			return 42;
		}
	}
}
[TestClass]
public class MockLocalFunctions
{
	[TestMethod]
	public void BasicUsage()
	{
		//Arrange
		var sut = Mock.Create<Foo>(Behavior.CallOriginal);
		Mock.Local.Function.Arrange<int>(sut, "GetResult", "GetLocal").DoNothing();

		//Act
		var result = sut. GetResult();

		//Assert
		Assert.AreEqual(100, result);
	}
}
```
  
In the example above, the local function is arranged to “do nothing” and therefore the method `GetResult()` will evaluate as if `GetLocal()` was never declared and used. Sometimes you might want to change the value that is returned by the local function. The next sample code demonstrates how this can be achieved.

```C#
[TestMethod]
public void BasicUsage2()
{
	//Arrange
	var sut = Mock.Create<Foo>(Behavior.CallOriginal);
	Mock.Local.Function.Arrange<int>(sut, "GetResult", "GetLocal").Returns(10);

	//Act
	var result = sut.GetResult();

	//Assert
	Assert.AreEqual(110, result);
}
```
  
## Providing Arguments to Local Functions

You might need to mock behavior of local functions depending on the values of the input parameters. For example, you might want to call the original function in most cases and use the mocked behavior for certain values of the input parameters. This can be easily achieved with JustMock:

```C#
class Foo
{
	public int GetResult(int param)
	{
		return GetLocal(param);

		int GetLocal(int val)
		{
			return val * val;
		}
	}
}

[TestClass]
public class MockLocalFunctions
{
	[TestMethod]
	public void PassArguments()
	{
		//Arrange
		var sut = Mock.Create<Foo>(Behavior.CallOriginal);
		Type[] getResultTypes = new Type[] { typeof(int) };
		Mock.Local.Function.Arrange<int>(sut, "GetResult", getResultTypes, "GetLocal", 10).Returns(42);

		//Act
		var resultOriginal = sut.GetResult(2);
		var resultMocked = sut.GetResult(10);

		//Assert
		Assert.AreEqual(4, resultOriginal);
		Assert.AreEqual(42, resultMocked);
	}
}
```
  
## Avoiding Ambiguity Between Methods Containing Local Functions

Another common scenario is when there are local functions inside overloading methods. In this case, the only difference with the previous examples is that, when a local function is mocked, you must explicitly specify the types of the parameters of the overloaded method that contains it. Let’s use the following class in our example:

```C#
class Foo
{
	public int GetResult()
	{
		return GetLocal();
		int GetLocal()
		{
			return 100;
		}
	}
	public int GetResult(int param)
	{
		return GetLocal();
		int GetLocal()
		{
			return 200;
		}
	}
}
```
  
Note that both overloads of GetResult() have local functions. The case that we want to mock the local function inside GetResult() can look like in the following snippet:
  
```C#
[TestMethod]
public void MockGetResult()
{
	//Arrange
	var sut = Mock.Create<Foo>(Behavior.CallOriginal);
	Mock.Local.Function.Arrange<int>(sut, "GetResult", "GetLocal").Returns(42);

	//Act
	var resultNoParams = sut.GetResult();
	var resultParams = sut.GetResult(10);

	//Assert
	Assert.AreEqual(42, resultNoParams);
	Assert.AreEqual(200, resultParams);
}
```
  
On the other hand, if you want to mock the local function inside `GetResult(int param)`, the code should be reworked as follows:

```C#
[TestMethod]
public void MockGetResultParams()
{
	//Arrange
	var sut = Mock.Create<Foo>(Behavior.CallOriginal);
	Type[] containingMethodParamTypes = new Type[] {typeof(int)};
	Mock.Local.Function.Arrange<int>(sut, "GetResult", containingMethodParamTypes, "GetLocal").Returns(42);

	//Act
	var resultNoParams = sut.GetResult();
	var resultParams = sut.GetResult(10);

	//Assert
	Assert.AreEqual(100, resultNoParams);
	Assert.AreEqual(42, resultParams);
}
```

## Mocking Local Functions Inside Non-public Containing Methods
  
All techniques described above can easily be applied when the local function is contained inside non-public method. The only difference is that you need to use the `PrivateAccssor` if you need to call the non-public containing method inside the test methods. This technique is demonstrated in the following example:

```C#
class Foo
{
	private int GetResult()
	{
		return GetLocal();

		int GetLocal()
		{
			return 10;
		}
	}
}

[TestClass]
public class MockLocalFunctions
{
	[TestMethod]
	public void MockInNonPublic()
	{
		//Arrange
		var sut = Mock.Create<Foo>(Behavior.CallOriginal);
		Mock.Local.Function.Arrange<int>(sut, "GetResult", "GetLocal").Returns(42);
				
		//Act
		var result = Mock.NonPublic.MakePrivateAccessor(sut).CallMethod("GetResult");

		//Assert
		Assert.AreEqual(42, result);
	}
}
```

## Local Functions Inside Static Methods

Everything above applies to mocking local functions inside static methods with the only difference that we need to specify the `Type` that defines the containing method. Besides that, the general setup for mocking static methods is required. A straightforward example would look like this:

```C#
class Foo
{
	static public int GetResult()
	{		
return GetLocal();

		int GetLocal()
		{
			return 10;
		}
	}
}

[TestClass]
public class MockLocalFunctions
{
	[TestMethod]
	public void MockInStatic()
	{
		//Arrange
		Type type = typeof(Foo);
		Mock.SetupStatic(type, Behavior.CallOriginal, StaticConstructor.NonMocked);
		Mock.Local.Function.Arrange<int>(type, "GetResult", "GetLocal").Returns(42);

		//Act
		var result = SomeClass.GetResult();

		//Assert
		Assert.AreEqual(42, result);
	}
}
```
  
There is not much difference when the containing method is static and non-public. For example, consider the following class:

```C#
class Foo
{
	static private int GetResult()
	{
		return GetLocal();

		int GetLocal()
		{
			return 10;
		}
	}
}
```
  
A very simplistic test case, which mocks the behavior of `GetLocal()` would look like the following snippet:

```C#
[TestMethod]
public void MockInStaticNonPublic()
{
	//Arrange
	Type type = typeof(Foo);
	Mock.SetupStatic(type, Behavior.CallOriginal, StaticConstructor.NonMocked);
	Mock.Local.Function.Arrange<int>(type, "GetResult", "GetLocal").Returns(42);

	//Act
	var result = Mock.NonPublic.MakeStaticPrivateAccessor(type).CallMethod("GetResult");

	//Assert
	Assert.AreEqual(42, result);
}
```

## Directly Calling Local Functions

There are scenarios where the local functions may have some complex logic or calls to other class methods. In similar cases, is desired to test the local function in complete isolation from its containing method context. The following sample demonstrates such usage:

```C#
class Foo
{
	bool IsEven(int value)
	{
		bool isEven = (value % 2 == 0)? true: false;
		return isEven;
		
	}
	public void Method()
	{
		bool result = LocalFunction(10);
		bool LocalFunction(int value)
		{
			return IsEven (value);
		}
	}
}
[TestClass]
public class MockLocalFunctions
{
	[TestMethod]
	public void CallLocal()
	{
		//Arrange
		var sut = Mock.Create<Foo>(Behavior.CallOriginal);
		Mock.NonPublic.Arrange<bool>(sut, "IsEven").Returns(false);

		//Act
		var result = Mock.Local.Function.Call(sut, "Method", "LocalFunction", 14);

		//Assert
		Assert.AreEqual(false, result);
	}
}
```
  
  
  
Note that JustMock does not provide support for mocking local functions that are declared but never called in their containing method.
