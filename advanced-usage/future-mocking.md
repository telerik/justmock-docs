---
title: Future Mocking
page_title: Future Mocking | JustMock Documentation
description: Future Mocking
previous_url: /advanced-usage-future-mocking.html
slug: justmock/advanced-usage/future-mocking
tags: future,mocking
published: True
position: 8
---

# Future Mocking

Future Mocking allows you to mock members without passing the dependency through a constructor or calling a method. You can apply future mocking automatically based on an expectation rather than applying it to every instance explicitly. You will find this handy especially in cases when you have to mock third party controls and tools where you have little control over the way in which they are created.

## Ignore instance for an expectation explicitly

By default all mocks are tied to an instance and this is the expected behavior. For instance in the following sample we have created an object of type `UserData` which has a single method called `ReturnFive()` which returns an integer. Then we mock the call to the `ReturnFive()` method for the `fakeUsed` instance of the `UserData` class. But note that the `ReturnFive()` method will not be mocked for any of the other instances of the `UserData` class.

  #### __[C#]__

  {{region FutureMocking#SampleCode1}}
    public class UserData
        {
            public int ReturnFive()
            {
                return 5;
            }
        }

        [TestMethod]
        public void ShouldArrangeReturnOnlyForSpecificInstance()
        {
            // Arrange
            var fakeUsed = Mock.Create<UserData>();
            Mock.Arrange(() => fakeUsed.ReturnFive()).Returns(7);

            // Assert
            Assert.AreEqual(7, fakeUsed.ReturnFive());
            Assert.AreEqual(5, new UserData().ReturnFive());    
        }
  {{endregion}}

  #### __[VB]__

  {{region FutureMocking#SampleCode1}}
    Public Class UserData
            Public Function ReturnFive() As Integer
                Return 5
            End Function
        End Class

        <TestMethod>
        Public Sub ShouldArrangeReturnOnlyForSpecificInstance()
            ' Arrange
            Dim fakeUsed = Mock.Create(Of UserData)()
            Mock.Arrange(Function() fakeUsed.ReturnFive()).Returns(7)

            ' Assert
            Assert.AreEqual(7, fakeUsed.ReturnFive())
            Assert.AreEqual(5, New UserData().ReturnFive())
        End Sub
  {{endregion}}

If we want to apply future mocking and assure that all `UserData` instances use the same mocked version of the `ReturnFive()` method, we can call `IgnoreInstance()`.

  #### __[C#]__

  {{region FutureMocking#SampleCode2}}
    [TestMethod]
        public void ShouldArrangeReturnForFutureUserDataInstances()
        {
            // Arrange
            var fakeUsed = Mock.Create<UserData>();
            Mock.Arrange(() => fakeUsed.ReturnFive()).IgnoreInstance().Returns(7);

            // Assert
            Assert.AreEqual(7, fakeUsed.ReturnFive());
            Assert.AreEqual(7, new UserData().ReturnFive());
        }
  {{endregion}}

  #### __[VB]__

  {{region FutureMocking#SampleCode2}}
    <TestMethod>
        Public Sub ShouldArrangeReturnForFutureUserDataInstances()
            ' Arrange
            Dim fakeUsed = Mock.Create(Of UserData)()
            Mock.Arrange(Function() fakeUsed.ReturnFive()).IgnoreInstance().Returns(7)

            ' Assert
            Assert.AreEqual(7, fakeUsed.ReturnFive())
            Assert.AreEqual(7, New UserData().ReturnFive())
        End Sub
  {{endregion}}

When `IgnoreInstance` is applied, the `ReturnFive()` method will work according to the expectation you have set in `Mock.Arrange`.

When setting future expectations you can also use the [Fluent Mocking]({%slug justmock/basic-usage/fluent-mocking%}) API as in this example:

  #### __[C#]__

  {{region FutureMocking#SampleCode2Fluent}}
    [TestMethod]
        public void ShouldArrangeReturnForFutureUserDataInstances_Fluent()
        {
            // Arrange
            var fakeUsed = Mock.Create<UserData>();
            fakeUsed.Arrange(mock => mock.ReturnFive()).IgnoreInstance().Returns(7);

            // Assert
            Assert.AreEqual(7, fakeUsed.ReturnFive());
            Assert.AreEqual(7, new UserData().ReturnFive());
        }
  {{endregion}}

  #### __[VB]__

  {{region FutureMocking#SampleCode2Fluent}}
    <TestMethod>
        Public Sub ShouldArrangeReturnForFutureUserDataInstances_Fluent()
            ' Arrange
            Dim fakeUsed = Mock.Create(Of UserData)()
            fakeUsed.Arrange(Function(x) x.ReturnFive()).IgnoreInstance().Returns(7)

            ' Assert
            Microsoft.VisualStudio.TestTools.UnitTesting.Assert.AreEqual(7, fakeUsed.ReturnFive())
            Microsoft.VisualStudio.TestTools.UnitTesting.Assert.AreEqual(7, New UserData().ReturnFive())
        End Sub
  {{endregion}}


## Using IgnoreInstance for Virtual Methods

You can also apply `IgnoreInstance` to virtual methods. Imagine that we have a simple class `Calculus` which has a `virtual` method `Sum`:

  #### __[C#]__

  {{region FutureMocking#VirtualMethods}}
    public class Calculus
        {
            public virtual int Sum()
            {
                return 0;
            }
        }
  {{endregion}}

  #### __[VB]__

  {{region FutureMocking#VirtualMethods}}
    Public Class Calculus
            Public Overridable Function Sum() As Integer
                Return 0
            End Function
        End Class
  {{endregion}}

Now we can use `IgnoreInctance` in the exact same way:

  #### __[C#]__

  {{region FutureMocking#VirtualMethods2}}
    [TestMethod]
        public void ShouldApplyIgnoreInstanceToVirtual()
        {
            //Arrange
            var calculus = Mock.Create<Calculus>();
            Mock.Arrange(() => calculus.Sum()).IgnoreInstance().Returns(10);

            //Assert
            Assert.AreEqual(10, calculus.Sum());
            Assert.AreEqual(10, new Calculus().Sum());
        }
  {{endregion}}

  #### __[VB]__

  {{region FutureMocking#VirtualMethods2}}
    <TestMethod>
        Public Sub ShouldApplyIgnoreInstanceToVirtual()
            'Arrange
            Dim calculus = Mock.Create(Of Calculus)()
            Mock.Arrange(Function() calculus.Sum()).IgnoreInstance().Returns(10)

            'Assert
            Assert.AreEqual(10, calculus.Sum())
            Assert.AreEqual(10, New Calculus().Sum())
        End Sub
  {{endregion}}


## Using IgnoreInstance for Collections

There is an example where we want to __Arrange__ a property to return fake collection. To demonstrate this, we will use the `Foo` class below.

  #### __[C#]__

  {{region FutureMocking#SampleCode7}}
    public class Foo
        {
            public Foo()
            {
            }

            public List<object> RealCollection
            {
                get { return collection; }
            }
            private List<object> collection;
        }
  {{endregion}}

  #### __[VB]__

  {{region FutureMocking#SampleCode7}}
    Public Class Foo
            Public Sub New()
            End Sub

            Public ReadOnly Property RealCollection() As List(Of Object)
                Get
                    Return collection
                End Get
            End Property
            Private collection As List(Of Object)
        End Class
  {{endregion}}

We will also use a public method that returns our fake collection.

  #### __[C#]__

  {{region FutureMocking#SampleCode7FakeCol}}
    public IList<object> FakeCollection()
        {
            List<object> resultCollection = new List<object>();

            resultCollection.Add("asd");
            resultCollection.Add(123);
            resultCollection.Add(true);

            return resultCollection;
        }
  {{endregion}}

  #### __[VB]__

  {{region FutureMocking#SampleCode7FakeCol}}
    Public Function FakeCollection() As IList(Of Object)
            Dim resultCollection As New List(Of Object)()

            resultCollection.Add("asd")
            resultCollection.Add(123)
            resultCollection.Add(True)

            Return resultCollection
        End Function
  {{endregion}}

This is how the test will look like:

  #### __[C#]__

  {{region FutureMocking#SampleCode7Test}}
    [TestMethod]
        public void ShouldReturnFakeCollectionForFutureCall()
        {
            var fooMocked = Mock.Create<Foo>();

            var expectedCollection = FakeCollection();

            // Arrange
            Mock.Arrange(() => fooMocked.RealCollection).IgnoreInstance().ReturnsCollection(expectedCollection);

            // Act
            var actualArrangedCollection = fooMocked.RealCollection;
            var actualUnArrangedCollection = new Foo().RealCollection;

            // Assert
            // Asserting for the arranged instance
            Assert.AreEqual(expectedCollection.Count, actualArrangedCollection.Count);
            Assert.AreEqual(expectedCollection.FirstOrDefault(), actualArrangedCollection.FirstOrDefault());

            // Asserting for a new unarranged instance
            Assert.AreEqual(expectedCollection.Count, actualUnArrangedCollection.Count);
            Assert.AreEqual(expectedCollection.FirstOrDefault(), actualUnArrangedCollection.FirstOrDefault());
        }
  {{endregion}}

  #### __[VB]__

  {{region FutureMocking#SampleCode7Test}}
    <TestMethod>
        Public Sub ShouldReturnFakeCollectionForFutureCall()
            Dim fooMocked = Mock.Create(Of Foo)()

            Dim expectedCollection = FakeCollection()

            ' Arrange
            Mock.Arrange(Function() fooMocked.RealCollection).IgnoreInstance().ReturnsCollection(expectedCollection)

            ' Act
            Dim actualArrangedCollection = fooMocked.RealCollection
            Dim actualUnArrangedCollection = New Foo().RealCollection

            ' Assert
            ' Asserting for the arranged instance
            Assert.AreEqual(expectedCollection.Count, actualArrangedCollection.Count)
            Assert.AreEqual(expectedCollection.FirstOrDefault(), actualArrangedCollection.FirstOrDefault())

            ' Asserting for a new unarranged instance
            Assert.AreEqual(expectedCollection.Count, actualUnArrangedCollection.Count)
            Assert.AreEqual(expectedCollection.FirstOrDefault(), actualUnArrangedCollection.FirstOrDefault())
        End Sub
  {{endregion}}

You can see that the test logic is the same as in the previous tests with the only difference that you are returning a collection this time.

## Future Constructor Mocking

With JustMock you are even able to future mock the constructor of an instance. Let`s assume we have the following class:

  #### __[C#]__

  {{region FutureMocking#SampleCodeFutureCtorMocking}}
    public class FooWithNotImplementedConstructor
        {
            public FooWithNotImplementedConstructor()
            {
                throw new NotImplementedException();
            }
        }
  {{endregion}}

  #### __[VB]__

  {{region FutureMocking#SampleCodeFutureCtorMocking}}
    Public Class FooWithNotImplementedConstructor
            Public Sub New()
                Throw New NotImplementedException()
            End Sub
        End Class
  {{endregion}}

We can easily arrange its constructor like this:

  #### __[C#]__

  {{region FutureMocking#SampleCodeFutureCtorMockingTest}}
    [TestMethod]
        public void ShouldMockConstructorForFutureInstances()
        {
            // Arrange
            Mock.Arrange(() => new FooWithNotImplementedConstructor()).DoNothing(); // Directly arranging the constructor

            // Act
            var myNewInstance = new FooWithNotImplementedConstructor(); // This will not throw an exception

            // Assert
            Assert.IsNotNull(myNewInstance);
            Assert.IsInstanceOfType(myNewInstance, typeof(FooWithNotImplementedConstructor));
        }
  {{endregion}}

  #### __[VB]__

  {{region FutureMocking#SampleCodeFutureCtorMockingTest}}
    <TestMethod>
        Public Sub ShouldMockConstructorForFutureInstances()
            ' Arrange
            Mock.Arrange(Function() New FooWithNotImplementedConstructor()).DoNothing() ' Directly arranging the constructor

            ' Act
            Dim myNewInstance = New FooWithNotImplementedConstructor() ' This will not throw an exception

            ' Assert
            Assert.IsNotNull(myNewInstance)
            Assert.IsInstanceOfType(myNewInstance, GetType(FooWithNotImplementedConstructor))
        End Sub
  {{endregion}}

This will apply __DoNothing__ to the constructor of every new instance of type `FooWithNotImplementedConstructor` called during the test method.

## Mocking the New Operator

With JustMock you can arrange a return value for a new object creation. Let's assume we have the following class:

  #### __[C#]__

  {{region FutureMocking#SampleCodeNewObjMocking}}
    public class FooWithProp
        {
            public string MyProp { get; set; }
        }

        public FooWithProp GetNewInstance()
        {
            return new FooWithProp();
        }
  {{endregion}}

  #### __[VB]__

  {{region FutureMocking#SampleCodeNewObjMocking}}
    Public Class FooWithProp
            Public Property MyProp As String
        End Class

        Public Function GetNewInstance() As FooWithProp
            Return New FooWithProp()
        End Function
  {{endregion}}

We can easily arrange each new instance of the `FooWithProp` class, to return a predefined object of the same type:

  #### __[C#]__

  {{region FutureMocking#SampleCodeNewObjMockingTest}}
    [TestMethod]
        public void ShouldReturnNewObjectForFutureInstances()
        {
            // Arrange
            var testObj = new FooWithProp() { MyProp = "Test" };

            // Directly arranging the expression to return our predefined object
            Mock.Arrange(() => new FooWithProp()).Returns(testObj); 

            // Act
            var myNewInstance = GetNewInstance();

            // Assert
            Assert.IsNotNull(myNewInstance);
            Assert.IsInstanceOfType(myNewInstance, typeof(FooWithProp));
            // Assert that the returned instance is equal to the predefined
            Assert.AreEqual("Test", myNewInstance.MyProp);
        }
  {{endregion}}

  #### __[VB]__

  {{region FutureMocking#SampleCodeNewObjMockingTest}}
    <TestMethod>
        Public Sub ShouldReturnNewObjectForFutureInstances()
            ' Arrange
            Dim testObj = New FooWithProp()
            testObj.MyProp = "Test"

            ' Directly arranging the expression to return our predefined object
            Mock.Arrange(Function() New FooWithProp()).Returns(testObj)

            ' Act
            Dim myNewInstance = GetNewInstance()

            ' Assert
            Assert.IsNotNull(myNewInstance)
            Assert.IsInstanceOfType(myNewInstance, GetType(FooWithProp))
            ' Assert that the returned instance is equal to the predefined
            Assert.AreEqual("Test", myNewInstance.MyProp)
        End Sub
  {{endregion}}

This will work for each new instance of the `FooWithProp` type outside the test method. Still, it applies only for code, executed under the test context.

## Future Mocking Across Threads

Future mocking across all threads is an unsafe operation because it may compromise the stability of the testing framework. Arrangements that ignore the instance are valid only for the current thread by default. To make an arrangement that ignores the instance valid on all threads, add the .OnAllThreads() clause to the arrangement: 

  {{region }}
    Mock.Arrange(() => new DBContext()).DoNothing().OnAllThreads();
  {{endregion}}


## Advanced Example

Let's see a UI example where we have a form. Based on some action against the form, it raises an event which needs to be handled in a specific way, therefore in the unit test we want to assert the expected value of the handler execution.

  #### __[C#]__

  {{region FutureMocking#SampleCode3}}
    public Form()
	{
		InitializeComponent();
	
		this.service = new EntryService();
	
		service.Saved += new EventHandler<EntrySavedEventArgs>(service_Saved);
	}
  {{endregion}}

Imagine that we have defined an `EntryService` the purpose of which is to save some entries to a database when the user has made any changes. For instance, we can have a button on the form and when the user is finished editing the entries pressing this button will trigger a call to the following method:

  {{region FutureMocking#SampleCode4}}
    public void SaveToDatabase(string value)
	{
		try
		{
			this.service.Save(value);
		}
		catch (DuplicateEntryException ex)
		{
			MessageBox.Show("Entry Duplicated " + ex.DuplicatedValue);
		}
	}
  {{endregion}}


Here is the handler for the `service.Saved` event:
	
{{region FutureMocking#SampleCode5}}
	public void service_Saved(object sender, EntrySavedEventArgs e)
	{
		this.label1.Text = "Saved string : " + e.EntryValue;
	}
{{endregion}}

Next, is a simple test using [MSpec](https://codebetter.com/aaronjensen/2008/05/08/introducing-machine-specifications-or-mspec-for-short/) where we have created an event handler which will receive as an argument the value which was passed to the `service.Save` call in the `SaveToDatabase` method.

{{region FutureMocking#SampleCode6}}
	[Subject(typeof(Form))]
	public class when_save_to_database_is_invoked_on_form
	{
	    Establish context = () =>
	    {
	        IEntryService serviceMock = Mock.Create<EntryService>();
	        Mock.Arrange(() => serviceMock.Save(valueToSave)).Raises(() => serviceMock.Saved += null, new EntrySavedEventArgs(valueToSave));
	    };
	
	    private Because of = () =>
	    {
	        sut = new Form();
	        sut.SaveToDatabase(valueToSave);
	    };
	
	    private It should_assert_that_label_contains_expected_valueToSave = () =>
	        sut.label1.Text.ShouldEqual("Saved string : " + valueToSave);
	
	    static Form sut;
	    const string valueToSave = "Raise Event";
	}
{{endregion}}

Here we can see that although no instance is supplied to the target UI class, JustMock picks up the intended setup from the context.

## See Also

 * [Fluent Mocking]({%slug justmock/basic-usage/fluent-mocking%})
