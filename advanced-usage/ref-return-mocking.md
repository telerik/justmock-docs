---
title: Mocking Ref Returns
page_title: Mocking Ref Returns | JustMock Documentation
description: Mocking Ref returns
previous_url: /advanced-usage-ref-return-mocking.html
slug: justmock/advanced-usage/ref-return-mocking
tags: ref,return,mocking
published: True
position: 16
---

# Mocking Ref Returns

Ref return is one of the advanced features supported in __Telerik® JustMock__. It gives you ability to arrange and verify expectations of public and non-public instance and static ref return methods and properties in elevated mode.

> Ref return is a feature specific for C# language and have limited support in other CLR languages. To use ref return mocking, you have to enable profiler for elevated mode. You can refer to [this]({%slug justmock/advanced-usage%}) topic for more information about advanced features and how to enable them.

`ClassUsingRefReturns` sample class is used below to figure out different ref return use cases. Here is its implementation:

  #### __[C#]__

  {{region RefReturnMocking#Sample}}
	class ClassUsingRefReturns
	{
		private static int[] array = { 1, 2, 3, 4 };

		public ref int GetRefReturnInstanceWithArgs(ref int p)
		{
			ref int local = ref array[0];
			local += p;

			return ref local;
		}

		private ref int GetRefReturnPrivInstanceWithArgs(ref int p)
		{
			ref int local = ref array[1];
			local += p;

			return ref local;
		}

		public static ref int RefReturnStatic
		{
			get
			{
				return ref array[2];
			}

		}

		private static ref int RefReturnPrivStatic
		{
			get
			{
				return ref array[3];
			}
		}
	}
  {{endregion}}

## Mocking Public Ref Return Members

The typical example of mocking public instance method could be illustrated as following:

  #### __[C#]__

  {{region RefReturnMocking#PublicInstanceMember}}
	[TestMethod]
	public void MockRefReturnInstanceMethodWithArgs()
	{
		var localRef = LocalRef.WithValue(12);

		// Arrange
		var sut = Mock.Create<ClassUsingRefReturns>();
		Mock.Arrange(sut, s => s.GetRefReturnInstanceWithArgs(ref Arg.Ref(Arg.AnyInt).Value)))
			.Returns(localRef.Handle)
			.OccursOnce();

		// Act
		int param = 10;
		ref int res = ref sut.GetRefReturnInstance(ref param);

		// Assert
		Mock.Assert(sut);
		Assert.AreEqual(localRef.Ref, res);
	}
  {{endregion}}


In this example, there is an arrangement to fake ref return and verify occurrences of instance method using `Mock.Arrange`. The interesting part of this arrangement is to provide a value which can be considered as ref return. JustMock introduces a dedicated delegate type `FuncExpectation<TReturn>.RefDelegate` for this purpose and `LocalRef` helper class which simplifies ref return arrangements and verifications.

> **Important**
>
> Ref return fakes are implemented via delegates. `LocalRef` helper class hides a lot of implementation details and makes your test code easy to maintain.  

Another common use case is mocking of public static member. The next example shows how to achieve that:

  #### __[C#]__

  {{region RefReturnMocking#PublicStaticMember}}
    [TestMethod]
    public void MockRefReturnStaticProperty()
    {
        // Arrange
        Mock.SetupStatic<ClassUsingRefReturns>();
        Mock.Arrange<ClassUsingRefReturns, int>(() =>ClassUsingRefReturns.RefReturnStatic)
            .Returns(LocalRef.WithValue(12).Handle)
            .OccursOnce();

        // Act
        ref int res = ref ClassUsingRefLocalsAndReturns.RefReturnStatic;

        // Assert
        Mock.Assert<ClassUsingRefReturns>();
        Assert.AreEqual(12, res);
    }
  {{endregion}}

## Mocking Non-public Ref Return Members

Non-public API provides several dedicated interfaces for mocking ref returns and accessing members following a well-known JustMock approach. Let's see how this looks like in action:

  #### __[C#]__

  {{region RefReturnMocking#NonPublicInstanceMember}}
    [TestMethod]
    public void MockRefReturnPrivateInstanceMethodWithArgs()
    {
        // Arrange
        var sut = Mock.Create<ClassUsingRefReturns>();
        Mock.NonPublic.RefReturn.Arrange<int>(sut, "GetRefReturnPrivInstanceWithArgs", Arg.Expr.Ref(1))
            .Returns(LocalRef.WithValue(12).Handle)
            .OccursOnce();

        // Act
        var accessor = Mock.NonPublic.MakePrivateAccessor(sut);
        ref int res = ref accessor.RefReturn.CallMethod<int>("GetRefReturnPrivInstanceWithArgs", Arg.Ref(1).Value);

        // Assert
        Mock.Assert(sut);
        Assert.AreEqual(12, res);
    }
  {{endregion}}


The example demonstrates ref return faking and occurrence verification handled by `INonPublicRefReturnExpectation` interface. Accessing non-public members is supported via `IPrivateRefReturnAccessor` interface as a part of `PrivateAccessor`. The next example gives an idea of how you can mock non-public ref return static members:

  #### __[C#]__

  {{region RefReturnMocking#NonPublicStaticMember}}
    [TestMethod]
    public void MockRefReturnPrivateStaticProperty()
    {
        // Arrange
        Mock.SetupStatic<ClassUsingRefReturns>();
        Mock.NonPublic.RefReturn.Arrange<ClassUsingRefReturns, int>("RefReturnPrivStatic")
            .Returns(LocalRef.WithValue(12).Handle)
            .OccursOnce();

        // Act
        var accessor = Mock.NonPublic.MakeStaticPrivateAccessor(typeof(ClassUsingRefReturns));
        ref int res = ref accessor.RefReturn.GetProperty<int>("RefReturnPrivStatic");

        // Assert
        Mock.Assert<ClassUsingRefReturns>();
        Assert.AreEqual(12, res);
    }
  {{endregion}}

## See Also

 * [Static Mocking]({%slug justmock/advanced-usage/static-mocking%})

 * [Mocking Non-public Members and Types]({%slug justmock/advanced-usage/mocking-non-public-members-and-types%})
