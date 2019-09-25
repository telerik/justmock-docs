---
title: Private Accessor
page_title: Private Accessor | JustMock Documentation
description: Private Accessor
previous_url: /advanced-usage-private-accessor.html
slug: justmock/advanced-usage/private-accessor
tags: private,accessor
published: True
position: 6
---

# Private Accessor

The __Telerik JustMock PrivateAccessor__ feature allows you to call non-public members of the tested code right in your unit tests. The feature is enabled for both *Free* and *Commercial* versions of JustMock.

To give examples of how the __PrivateAccessor__ can be used within your tests, we will use the following sample class:

  #### __[C#]__

  {{region PrivateAccessor#SampleClass}}
    public class ClassWithNonPublicMembers
    {
        private int Prop { get; set; }

        private int MePrivate()
        {
            return 1000;
        }

        private static int MeStaticPrivate()
        {
            return 2000;
        }

        private T EchoPrivateGeneric<T>(T arg)
        {
            return arg;
        }

        private static T EchoStaticPrivateGeneric<T>(T arg)
        {
            return arg;
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region PrivateAccessor#SampleClass}}
    Public Class ClassWithNonPublicMembers
        Private Property Prop() As Integer
            Get
                Return m_Prop
            End Get
            Set(value As Integer)
                m_Prop = Value
            End Set
        End Property
        Private m_Prop As Integer

        Private Function MePrivate() As Integer
            Return 1000
        End Function

        Private Shared Function MeStaticPrivate() As Integer
            Return 2000
        End Function

        Private Function EchoPrivateGeneric(Of T)(ByVal arg As T) As T
            Return arg
        End Function

        Private Shared Function EchoStaticPrivateGeneric(Of T)(ByVal arg As T) As T
            Return arg
        End Function
    End Class
  {{endregion}}


## Calling Private Methods
The next example will show how to invoke non-public method with the __PrivateAccessor__:

  #### __[C#]__

  {{region PrivateAccessor#CallPrivateMethod}}
    [TestMethod]
    public void PrivateAccessor_ShouldCallMethod()
    {
        // Act
        var inst = new PrivateAccessor(new ClassWithNonPublicMembers());
        var actual = inst.CallMethod("MePrivate");

        // Assert
        Assert.AreEqual(1000, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region PrivateAccessor#CallPrivateMethod}}
    <TestMethod> _
    Public Sub PrivateAccessor_ShouldCallMethod()
        ' Act
        Dim inst = New PrivateAccessor(New ClassWithNonPublicMembers())
        Dim actual = inst.CallMethod("MePrivate")

        ' Assert
        Assert.AreEqual(1000, actual)
    End Sub
  {{endregion}}

To call non-public methods with  __PrivateAccessor__ you must: 

1.  Wrap the instance holding the private method
1.  Call the non-public method by giving its exact name
1.  Finally, you can assert against its value

__Next, we will show how to call non-public methods from a mocked instance:__

  #### __[C#]__

  {{region PrivateAccessor#CallArrangedPrivateMethod}}
    [TestMethod]
    public void ShouldCallArrangedPrivateMethod()
    {
        // Arrange
        var mockedClass = Mock.Create<ClassWithNonPublicMembers>(Behavior.CallOriginal);

        Mock.NonPublic.Arrange<int>(mockedClass, "MePrivate").Returns(5);

        // Act
        var inst = new PrivateAccessor(mockedClass);
        var actual = inst.CallMethod("MePrivate");

        // Assert
        Assert.AreEqual(5, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region PrivateAccessor#CallArrangedPrivateMethod}}
    <TestMethod> _
    Public Sub ShouldCallArrangedPrivateMethod()
        ' Arrange
        Dim mockedClass = Mock.Create(Of ClassWithNonPublicMembers)(Behavior.CallOriginal)

        Mock.NonPublic.Arrange(Of Integer)(mockedClass, "MePrivate").Returns(5)

        ' Act
        Dim inst = New PrivateAccessor(mockedClass)
        Dim actual = inst.CallMethod("MePrivate")

        ' Assert
        Assert.AreEqual(5, actual)
    End Sub
  {{endregion}}

1.  Create a mocked instance of your class under test (you can also use original instance object and perform [partial mocking]({%slug justmock/advanced-usage/partial-mocking%}) later on)
1.  Arrange your expectations
1.  Then, create new PrivateAccessor with the mocked instance as an argument
1.  Call the non-public method by giving its exact name
1.  Finally, you can assert against its expected return value

Using the steps above and supplying the type arguments you can call non-public generic methods, the sample test looks as following:

  #### __[C#]__

  {{region PrivateAccessor#CallArrangedPrivateGenericMethod}}
    [TestMethod]
    public void ShouldCallArrangedPrivateGenericMethod()
    {
        // Arrange
        var mockedClass = Mock.Create<ClassWithNonPublicMembers>(Behavior.CallOriginal);

        Mock.NonPublic.Arrange<int>(mockedClass, "EchoPrivateGeneric", new Type[] { typeof(int) }, 10).Returns(5);

        // Act
        var inst = new PrivateAccessor(mockedClass);
        var actual = inst.CallMethodWithTypeArguments("EchoPrivateGeneric", new Type[] { typeof(int) }, 10);

        // Assert
        Assert.AreEqual(5, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region PrivateAccessor#CallArrangedPrivateGenericMethod}}
    <TestMethod>
    Public Sub ShouldCallArrangedPrivateGenericMethod()
        ' Arrange
        Dim mockedClass = Mock.Create(Of ClassWithNonPublicMembers)(Behavior.CallOriginal)

        Mock.NonPublic.Arrange(Of Integer)(mockedClass, "EchoPrivateGeneric", New Type() {GetType(Integer)}, 10).Returns(5)

        ' Act
        Dim inst = New PrivateAccessor(mockedClass)
        Dim actual = inst.CallMethodWithTypeArguments("EchoPrivateGeneric", New Type() {GetType(Integer)}, 10)

        ' Assert
        Assert.AreEqual(5, actual)
    End Sub
  {{endregion}}

## Calling Static Private Methods
The next example will show how to invoke non-public static method with the __Telerik JustMock PrivateAccessor__:

  #### __[C#]__

  {{region PrivateAccessor#CallStaticPrivateMethod}}
    [TestMethod]
    public void PrivateAccessor_ShouldCallStaticMethod()
    {
        // Act
        var inst = PrivateAccessor.ForType(typeof(ClassWithNonPublicMembers));
        var actual = inst.CallMethod("MeStaticPrivate");

        // Assert
        Assert.AreEqual(2000, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region PrivateAccessor#CallStaticPrivateMethod}}
    <TestMethod> _
    Public Sub PrivateAccessor_ShouldCallStaticMethod()
        ' Act
        Dim inst = PrivateAccessor.ForType(GetType(ClassWithNonPublicMembers))
        Dim actual = inst.CallMethod("MeStaticPrivate")

        ' Assert
        Assert.AreEqual(2000, actual)
    End Sub
  {{endregion}}

To call non-public static methods with the __PrivateAccessor__ you must: 

1.  Wrap the instance holding the private method by type
1.  Call the non-public static method by giving its exact name
1.  Finally, you can assert against its value

__Next, we will show how to call non-public static methods from a mocked instance:__

  #### __[C#]__

  {{region PrivateAccessor#CallArrangedStaticPrivateMethod}}
    [TestMethod]
    public void ShouldCallArrangedStaticPrivateMethod()
    {
        // Arrange
        Mock.SetupStatic(typeof(ClassWithNonPublicMembers));

        Mock.NonPublic.Arrange<int>(typeof(ClassWithNonPublicMembers), "MeStaticPrivate").Returns(5);

        // Act
        var inst = PrivateAccessor.ForType(typeof(ClassWithNonPublicMembers));
        var actual = inst.CallMethod("MeStaticPrivate");

        // Assert
        Assert.AreEqual(5, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region PrivateAccessor#CallArrangedStaticPrivateMethod}}
    <TestMethod> _
    Public Sub ShouldCallArrangedStaticPrivateMethod()
        ' Arrange
        Dim mockedClass = Mock.Create(Of ClassWithNonPublicMembers)(Behavior.CallOriginal)

        Mock.NonPublic.Arrange(Of Integer)(mockedClass, "MeStaticPrivate").IgnoreInstance().Returns(5)

        ' Act
        Dim inst = PrivateAccessor.ForType(GetType(ClassWithNonPublicMembers))
        Dim actual = inst.CallMethod("MeStaticPrivate")

        ' Assert
        Assert.AreEqual(5, actual)
    End Sub
  {{endregion}}

1.  Setup your class for static mocking (this is not needed if you are to perform [partial mocking]({%slug justmock/advanced-usage/partial-mocking%}) later on)
1.  Arrange your expectations
1.  Then, create new PrivateAccessor with the mocked instance type as an argument
1.  Call the non-public method by giving its exact name
1.  Finally, you can assert against its expected return value

Like a non-public generic instance methods, you can use __PrivateAccessor__ to call non-public generic static ones, here is the sample test:

  #### __[C#]__

  {{region PrivateAccessor#CallArrangedStaticPrivateGenericMethod}}
    [TestMethod]
    public void ShouldCallArrangedStaticPrivateGenericMethod()
    {
        // Arrange
        Mock.SetupStatic(typeof(ClassWithNonPublicMembers));

        Mock.NonPublic.Arrange<int>(typeof(ClassWithNonPublicMembers), "EchoStaticPrivateGeneric", new Type[] { typeof(int) }, 10).Returns(5);

        // Act
        var inst = PrivateAccessor.ForType(typeof(ClassWithNonPublicMembers));
        var actual = inst.CallMethodWithTypeArguments("EchoStaticPrivateGeneric", new Type[] { typeof(int) }, 10);

        // Assert
        Assert.AreEqual(5, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region PrivateAccessor#CallArrangedStaticPrivateGenericMethod}}
    <TestMethod>
    Public Sub ShouldCallArrangedStaticPrivateGenericMethod()
        ' Arrange
        Mock.SetupStatic(GetType(ClassWithNonPublicMembers))

        Mock.NonPublic.Arrange(Of Integer)(GetType(ClassWithNonPublicMembers), "EchoStaticPrivateGeneric", New Type() {GetType(Integer)}, 10).Returns(5)

        ' Act
        Dim inst = PrivateAccessor.ForType(GetType(ClassWithNonPublicMembers))
        Dim actual = inst.CallMethodWithTypeArguments("EchoStaticPrivateGeneric", New Type() {GetType(Integer)}, 10)

        ' Assert
        Assert.AreEqual(5, actual)
    End Sub
  {{endregion}}

## Get or Set Private Properties
The next example shows how to get or set the value of a non-public property.

  #### __[C#]__

  {{region PrivateAccessor#GetSetProperty}}
    [TestMethod]
    public void PrivateAccessor_ShouldGetSetProperty()
    {
        // Act
        var inst = new PrivateAccessor(new ClassWithNonPublicMembers());
        inst.SetProperty("Prop", 555);

        // Assert
        Assert.AreEqual(555, inst.GetProperty("Prop"));
    }
  {{endregion}}

  #### __[VB]__

  {{region PrivateAccessor#GetSetProperty}}
    <TestMethod> _
    Public Sub PrivateAccessor_ShouldGetSetProperty()
        ' Act
        Dim inst = New PrivateAccessor(New ClassWithNonPublicMembers())
        inst.SetProperty("Prop", 555)

        ' Assert
        Assert.AreEqual(555, inst.GetProperty("Prop"))
    End Sub
  {{endregion}}

In the above example, we are: 
1.  Passing the instance holding the private property to the __PrivateAccessor__
1.  Setting the value of the private property *Prop* to *555*
1.  Asserting that, the getter of *Prop* must return the above set value

__Next, we will show how to get an arranged non-public property from a mocked instance:__

  #### __[C#]__

  {{region PrivateAccessor#ShouldGetArrangedPrivateProperty}}
    [TestMethod]
    public void ShouldGetArrangedPrivateProperty()
    {
        // Arrange
        var mockedClass = Mock.Create<ClassWithNonPublicMembers>(Behavior.CallOriginal);

        Mock.NonPublic.Arrange<int>(mockedClass, "Prop").Returns(5);

        // Act
        var inst = new PrivateAccessor(mockedClass);
        var actual = inst.GetProperty("Prop");

        // Assert
        Assert.AreEqual(5, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region PrivateAccessor#ShouldGetArrangedPrivateProperty}}
    <TestMethod> _
    Public Sub ShouldGetArrangedPrivateProperty()
        ' Arrange
        Dim mockedClass = Mock.Create(Of ClassWithNonPublicMembers)(Behavior.CallOriginal)

        Mock.NonPublic.Arrange(Of Integer)(mockedClass, "Prop").Returns(5)

        ' Act
        Dim inst = New PrivateAccessor(mockedClass)
        Dim actual = inst.GetProperty("Prop")

        ' Assert
        Assert.AreEqual(5, actual)
    End Sub
  {{endregion}}

1.  Create a mocked instance of your class under test (you can also use original instance object and perform [partial mocking]({%slug justmock/advanced-usage/partial-mocking%}) later on)
1.  Arrange your expectations
1.  Then, create new PrivateAccessor with the mocked instance as an argument
1.  Set the non-public property by giving its exact name
1.  Finally, you can assert against its expected return value

## Throw the original exception when calling method

__PrivateAccessor__ is using the reflection API to invoke the required method. When that method throws exception the reflection API is automatically wrapping it in a [TargetInvocationException](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.targetinvocationexception?view=netframework-4.8). This could cause inconvenience in determining what the original problem is.
__PrivateAccessor.RethrowOriginalOnCallMethod__ is a boolean property that solves that problem by controling whether the original exception should be thrown or not. For backward compatibility the default value is __false__.
To better illustrate the usage of this property consinder the following code sample:

  #### __[C#]__

  {{region PrivateAccessor#RethrowOriginalOnCallMethod_ClassExample}}
    public class Foo
    {
        private void ThrowsException()
        {
            throw new NotImplementedException("There is no implementation for this method.");
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region PrivateAccessor#RethrowOriginalOnCallMethod_ClassExample}}
    Public Class Foo
        Private Sub ThrowsException()
            Throw New NotImplementedException("There is no implementation for this method.")
        End Sub
    End Class
  {{endregion}}

And here is how a test for the ThrowsException method will look like:
 
  #### __[C#]__

  {{region PrivateAccessor#RethrowOriginalOnCallMethod_TestExample}}
    [TestMethod]
    [ExpectedException(typeof(NotImplementedException))]
    public void PrivateAccessor_CallMethod_ThrowsException()
    {
        var instance = new PrivateAccessor(new Foo());
        instance.RethrowOriginalOnCallMethod = true;
        instance.CallMethod("ThrowsException");
    }
  {{endregion}}

  #### __[VB]__

  {{region PrivateAccessor#RethrowOriginalOnCallMethod_TestExample}}
    <TestMethod>
    <ExpectedException(GetType(NotImplementedException))>
    Public Sub PrivateAccessor_CallMethod_ThrowsException()
        Dim instance = New PrivateAccessor(New Foo())
        instance.RethrowOriginalOnCallMethod = True
        instance.CallMethod("ThrowsException")
    End Sub
  {{endregion}}

## See Also

 * [Partial Mocking]({%slug justmock/advanced-usage/partial-mocking%})
