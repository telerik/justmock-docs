---
title: Mock Internal Types Via Proxy
page_title: Mock Internal Types Via Proxy | JustMock Documentation
description: Mock Internal Types Via Proxy
previous_url: /basic-usage-mock-internal-types-via-proxy.html
slug: justmock/basic-usage/mock-internal-types-via-proxy
tags: mock,internal,types,via,proxy
published: True
position: 12
---

# Mock Internal Types Via Proxy

With JustMock you can mock internal types with `InternalsVisibleToAttribute` the same way you mock public types. Without `InternalsVisibleToAttribute` you are forced to mock as you mock non-public members.

In the further examples, we will use the following sample classes to test:

  #### __[C#]__

  {{region InternalMocking#SampleClass}}
    internal class FooInternal
    {
        internal FooInternal()
        {
            builder = new StringBuilder();
        }

        public StringBuilder Builder
        {
            get
            {
                return builder;
            }
        }

        private StringBuilder builder;
    }

    public class FooInternalCtor
    {
        internal FooInternalCtor(string name)
        {
            this.name = name;
        }

        public string Name { get { return name; } }

        private string name;
    }
  {{endregion}}

  #### __[VB]__

  {{region InternalMocking#SampleClass}}
    Friend Class FooInternal
        Friend Sub New()
            m_builder = New StringBuilder()
        End Sub

        Public ReadOnly Property Builder() As StringBuilder
            Get
                Return m_builder
            End Get
        End Property

        Private m_builder As StringBuilder
    End Class

    Public Class FooInternalCtor
        Friend Sub New(name As String)
            Me.m_name = name
        End Sub

        Public ReadOnly Property Name() As String
            Get
                Return m_name
            End Get
        End Property

        Private m_name As String
    End Class
  {{endregion}}


## Mock Internal Type Via Proxy
Provided that the `InternalsVisibleTo` attribute is included in the `AssemblyInfo.cs`, let's consider the following example:

  #### __[C#]__

  {{region InternalMocking#Example}}
    [TestMethod]
    public void ShouldMockInternalTypeViaProxy()
    {
        // Arrange
        var foo = Mock.Create<FooInternal>(Behavior.CallOriginal);

        // Assert
        Assert.IsNotNull(foo.Builder);
    }
  {{endregion}}

  #### __[VB]__

  {{region InternalMocking#Example}}
    <TestMethod()>
    Public Sub ShouldMockInternalTypeViaProxy()
        ' Arrange
        Dim foo = Mock.Create(Of FooInternal)(Behavior.CallOriginal)

        ' Assert
        Assert.IsNotNull(foo.Builder)
    End Sub
  {{endregion}}

Here we verify that the mock is created and its constructor is called. Thus, `foo.Builder` will be assigned.

The key to be used in the `InternalsVisibleTo` attribute is as follows:
* __For strong-named assemblies:__
[assembly: InternalsVisibleTo("Telerik.JustMock, PublicKey=0024000004800000940000000602000000240000525341310004000001000100098b1434e598c656b22eb59000b0bf73310cb8488a6b63db1d35457f2f939f927414921a769821f371c31a8c1d4b73f8e934e2a0769de4d874e0a517d3d7b9c36cd0ffcea2142f60974c6eb00801de4543ef7e93f79687b040d967bb6bd55ca093711b013967a096d524a9cadf94e3b748ebdae7947ea6de6622eabf6548448e")]
* __For non strong-named assemblies:__
[assembly: InternalsVisibleTo("Telerik.JustMock")]

## Create Mock With Internal Constructor
You can mock class that exposes internal constructor in the same way you mock public types.

  #### __[C#]__

  {{region InternalMocking#InternalCtor}}
    [TestMethod]
    public void ShouldCreateMockWithInternalCtor()
    {
        // Arrange
        var expected = "hello";

        var foo = Mock.Create<FooInternalCtor>(x =>
        {
            x.CallConstructor(() => new FooInternalCtor(expected));
            x.SetBehavior(Behavior.CallOriginal);
        });

        // Assert
        Assert.AreEqual(foo.Name, expected);
    }
  {{endregion}}

  #### __[VB]__

  {{region InternalMocking#InternalCtor}}
    <TestMethod()>
    Public Sub ShouldCreateMockWithInternalCtor()
        ' Arrange
        Dim expected = "hello"

        Dim foo = Mock.Create(Of FooInternalCtor)(Function(x)
                                                        x.CallConstructor(Function() New FooInternalCtor(expected))
                                                        x.SetBehavior(Behavior.CallOriginal)
                                                    End Function)

        ' Assert
        Assert.AreEqual(foo.Name, expected)
    End Sub
  {{endregion}}

In the example above, we mock the internal constructor of `FooInternalCtor` class. When `foo` is created the internal constructor will be called with `"hello"` as argument and the `Name` property will be assigned `"hello"` respectively. We verify that by calling `foo.Name` in the assertion phase of the [AAA]({%slug justmock/basic-usage/arrange-act-assert%}) pattern.

## Mock An Interface Member, Privately Implemented In Base Class
Here is an interface with a single member in it and a class which is inheriting that interface.

  #### __[C#]__

  {{region InternalMocking#InterfaceMemInBaseClassCUT}}
    public interface IManager
    {
        object Provider { get; }
    }

    public class FooBase : IManager
    {
        object IManager.Provider
        {
            get { throw new NotImplementedException(); }
        }
    }

    public class Bar : FooBase
    {
    }
  {{endregion}}

  #### __[VB]__

  {{region InternalMocking#InterfaceMemInBaseClassCUT}}
    Public Interface IManager
        ReadOnly Property Provider() As Object
    End Interface

    Public Class FooBase
        Implements IManager
        ReadOnly Property Provider() As Object Implements IManager.Provider
            Get
                Throw New NotImplementedException()
            End Get
        End Property
    End Class

    Public Class Bar
        Inherits FooBase
    End Class
  {{endregion}}

The derived class `Bar` will be our object to mock, in the example below.

  #### __[C#]__

  {{region InternalMocking#InterfaceMemInBaseClassTest}}
    [TestMethod]
    public void ShouldMockAnInterfaceMemberPrivatelyImplementedInBaseClass()
    {
        // Arrange
        var bar = Mock.Create<Bar>();

        string expected = "dummy";

        Mock.Arrange(() => ((IManager)bar).Provider).Returns("dummy");

        // Act
        var actual = ((IManager)bar).Provider;

        // Assert
        Assert.AreEqual(expected, actual);
    }
  {{endregion}}

  #### __[VB]__

  {{region InternalMocking#InterfaceMemInBaseClassTest}}
    <TestMethod()>
    Public Sub ShouldMockAnInterfaceMemberPrivatelyImplementedInBaseClass()
        ' Arrange
        Dim bar = Mock.Create(Of Bar)()

        Dim expected As String = "dummy"

        Mock.Arrange(Function() DirectCast(bar, IManager).Provider).Returns("dummy")

        ' Act
        Dim actual = DirectCast(bar, IManager).Provider

        ' Assert
        Assert.AreEqual(expected, actual)
    End Sub
  {{endregion}}

We have mocked the `Bar` class. Then we make arranges about the interface member, `Provider` and we act. Finally, we can assert that the test behavior is as expected.
