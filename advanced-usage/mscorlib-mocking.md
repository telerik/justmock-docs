---
title:  System API / MsCorLib
page_title: System API / MsCorLib Mocking | JustMock Documentation
description:  System API / MsCorLib Mocking
previous_url: /advanced-usage-mscorlib-mocking.html
slug: justmock/advanced-usage/mscorlib-mocking
tags: mscorlib, system, mock, fake, .net, class, method, property, datetime, stream, fileinfo
published: True
position: 14
---

# System API / MsCorLib Mocking

**Telerik® JustMock** allows you to mock everything, including the .NET system types as part of mscorlib.dll or any other library or DLL. Even more, with JustMock you are able to arrange any System member/function without additional setups. 
> This feature is available only in the commercial version of Telerik JustMock. Refer to [this]({%slug justmock/licensing/commercial-vs-free-version%}) topic to learn more about the differences between the commercial and free versions of Telerik JustMock.


## Mocking System.DateTime Class

In **Example 1** you can see how to mock the `DateTime.Now` property to always return the date `April 12, 2010`.

#### Example 1: Mock DateTime.Now
```C#
[TestMethod]
public void ShouldAssertCustomValueForDateTimeNow()
{
    // Arrange
    Mock.Arrange(() => DateTime.Now).Returns(new DateTime(1900, 4, 12));
    
    // Act
    var now = DateTime.Now;
    
    // Assert
    Assert.AreEqual(1900, now.Year);
    Assert.AreEqual(4, now.Month);
    Assert.AreEqual(12, now.Day);
}
```
```VB
<TestMethod> _
Public Sub ShouldAssertCustomValueForDateTimeNow()
    ' Arrange
    Mock.Arrange(Function() DateTime.Now).Returns(New DateTime(1900, 4, 12))
    
    ' Act
    Dim now = DateTime.Now
    
    ' Assert
    Assert.AreEqual(1900, now.Year)
    Assert.AreEqual(4, now.Month)
    Assert.AreEqual(12, now.Day)
End Sub
```

Note that you can directly arrange the `DateTime.Now` without any prerequisites.

The setup in **Example 1** will also work even if the call to `DateTime.Now` is in a member in another class. For example, take the `NestedDateTime` class:

#### Example 2: Nested DateTime sample usage
```C#
public class NestedDateTime
{
    public DateTime GetDateTime()
    {
        return DateTime.Now;
    }
}
```
```VB
Public Class NestedDateTime
    Public Function GetDateTime() As DateTime
        Return DateTime.Now
    End Function
End Class
```

In this case, the test will look like this:

#### Example 3: Arrange DateTime in nested class
```C#
[TestMethod]
public void ShouldArrangeCustomValueForDateTimeNowInCustomClass()
{
    // Arrange
    Mock.Arrange(() => DateTime.Now).Returns(new DateTime(1900, 4, 12));
    
    // Act
    var now = new NestedDateTime().GetDateTime();
    
    // Assert
    Assert.AreEqual(1900, now.Year);
    Assert.AreEqual(4, now.Month);
    Assert.AreEqual(12, now.Day);
}
```
```VB
<TestMethod> _
Public Sub ShouldArrangeCustomValueForDateTimeNowInCustomClass()
    ' Arrange
    Mock.Arrange(Function() DateTime.Now).Returns(New DateTime(1900, 4, 12))
    
    ' Act
    Dim now = New NestedDateTime().GetDateTime()
    
    ' Assert
    Assert.AreEqual(1900, now.Year)
    Assert.AreEqual(4, now.Month)
    Assert.AreEqual(12, now.Day)
End Sub
```


## Mocking System.IO.FileInfo Class

Using JustMock, you can mock virtually any System type from MsCorLib without any prerequisites. To demonstrate mocking of framework methods, we will take the [FileInfo](https://docs.microsoft.com/en-us/dotnet/api/system.io.fileinfo) class and mock its `Delete` method.

#### Example 4: Mock FileInfo.Delete method
```C#
[TestMethod]
public void ShouldMockFileInfoDelete()
{
    var filename = this.GetType().Assembly.ManifestModule.FullyQualifiedName;
    var myFileInfo = new FileInfo(filename);
    
    bool isCalled = false;
    
    // Arrange
    // When myFileInfo.Delete is called, set the isCalled variable to true
    Mock.Arrange(() => myFileInfo.Delete()).DoInstead(() => isCalled = true);
    
    //Act
    myFileInfo.Delete();
    
    //Assert
    Assert.IsTrue(isCalled);
}
```
```VB
<TestMethod> _
Public Sub ShouldMockFileInfoDelete()
    'Arrange
    Dim filename = Me.[GetType]().Assembly.ManifestModule.FullyQualifiedName
    Dim myFileInfo = New FileInfo(filename)
    
    Dim isCalled As Boolean = False
    
    ' Arrange
    ' When myFileInfo.Delete is called, set the isCalled variable to true
    Mock.Arrange(Sub() myFileInfo.Delete()).DoInstead(Sub() isCalled = True)
    
    'Act
    myFileInfo.Delete()
    
    'Assert
    Assert.IsTrue(isCalled)
End Sub
```

## Mocking System.IO.DriveInfo Class

Let's look at another example - we want to mock the static GetDrives method of [DriveInfo](https://docs.microsoft.com/en-us/dotnet/api/system.io.driveinfo):

#### Example 5: Mock DriveInfo.GetDrives static method
```C#
[TestMethod]
public void ShouldMockDriveInfoGetDrives_StaticMember()
{
    bool isCalled = false;
    
    // Arrange
    // When GetDrives() is called, set isCalled to true
    Mock.Arrange(() => DriveInfo.GetDrives()).DoInstead(() => isCalled = true);
    
    // Act
    DriveInfo.GetDrives();
    
    // Assert
    Assert.IsTrue(isCalled);
}
```
```VB
<TestMethod> _
Public Sub ShouldMockDriveInfoGetDrives_StaticMember()
    Dim isCalled As Boolean = False
    
    ' Arrange
    ' When GetDrives() is called, set isCalled to true
    Mock.Arrange(Sub() DriveInfo.GetDrives()).DoInstead(Sub() isCalled = True)
    
    ' Act
    DriveInfo.GetDrives()
    
    ' Assert
    Assert.IsTrue(isCalled)
End Sub
```


## Mocking System.IO.File Class

This section shows how to mock the `File.ReadAllBytes` method. The setup you can see in **Example 6** ignores the actual implementation of `ReadAllBytes` and returns a custom fake content for the file.

#### Example 6: Mock File.ReadAllBytes method
```C#
[TestMethod]
public void ShouldMockStaticFileForReadOperaton()
{
    byte[] fakeBytes = "byte content".ToCharArray().Select(c => (byte)c).ToArray();
    
    // Arrange 
    // When File.ReadAllBytes is invoked with any string parameter, return fakeBytes as a result
    Mock.Arrange(() => File.ReadAllBytes(Arg.IsAny<string>())).Returns(fakeBytes);
    
    // Act
    var returnedBytes = File.ReadAllBytes("ping");
    
    // Assert - both have the same length
    Assert.AreEqual(fakeBytes.Length, returnedBytes.Length);
    
    // Assert - both have the same content
    var same = true;
    
    for (var i = 0; i < fakeBytes.Length; i++)
    {
        if (fakeBytes[i] != returnedBytes[i])
        {
            same = false;
            break;
        }
    }
    
    Assert.AreEqual(true, same);
}
```
```VB
<TestMethod> _
Public Sub ShouldMockStaticFileForReadOperaton()
    Dim fakeBytes As Byte() = "byte content".ToCharArray().[Select](Function(c) CByte(AscW(c))).ToArray()
    
    ' Arrange 
    ' When File.ReadAllBytes is invoked with any string parameter, return fakeBytes as a result
    Mock.Arrange(Function() File.ReadAllBytes(Arg.IsAny(Of String)())).Returns(fakeBytes)
    
    ' Act
    Dim returnedBytes = File.ReadAllBytes("ping")
    
    ' Assert - both have the same length
    Assert.AreEqual(fakeBytes.Length, returnedBytes.Length)
    
    ' Assert - both have the same content
    Dim same = True
    
    For i As Integer = 0 To fakeBytes.Length - 1
        If fakeBytes(i) <> returnedBytes(i) Then
            same = False
            Exit For
        End If
    Next
    
    Assert.AreEqual(True, same)
End Sub
```


## Mocking System.IO.FileStream Class

This section shows a more complicated example. It demonstrates how to mock the `FileStream.Write` method.

#### Example 7: Mock the FileStream.Write method
```C#
[TestMethod]
public void ShouldMockFileOpenWithCustomFileStream()
{
    byte[] actual = new byte[255];
    
    // Writing fake bytes into the actual variable
    using (StreamWriter writer = new StreamWriter(new MemoryStream(actual)))
    {
        writer.WriteLine("Hello world");
        writer.Flush();
    }
    
    // Create a mocked instance of FileStream
    FileStream fs = Mock.Create<FileStream>(Constructor.Mocked);
    
    // Mocking the specific call and setting up expectations. 
    // Calling the Write method will result in copying the content 
    // of 'actual' to the passed byte array.
    Mock.Arrange(() => fs.Write(null, 0, 0)).IgnoreArguments()
        .DoInstead((byte[] content, int offset, int len) => actual.CopyTo(content, offset));
    
    // Arranging File.Open. Invoking the method will always return a custom file stream.
    Mock.Arrange(() => File.Open(Arg.AnyString, Arg.IsAny<FileMode>())).Returns(fs);
    
    // Act - 'fileStream' is assigned with the custom stream returned from File.Open.
    var fileStream = File.Open("hello.txt", FileMode.Open);
    byte[] fakeContent = new byte[actual.Length];
    
    // Original task
    // After this, as arranged, the content of 'actual' and 'fakeContent' will be the same.
    fileStream.Write(fakeContent, 0, actual.Length);
    
    // Assert
    Assert.AreEqual(fakeContent.Length, actual.Length);
    
    for (var i = 0; i < fakeContent.Length; i++)
    {
        Assert.AreEqual(fakeContent[i], actual[i]);
    }
}
```
```VB
<TestMethod>
Public Sub ShouldMockFileOpenWithCustomFileStream()
    Dim actual As Byte() = New Byte(254) {}
    
    ' Writing locally, can be done from resource manifest as well.
    
    Using writer As New StreamWriter(New MemoryStream(actual))
        writer.WriteLine("Hello world")
        writer.Flush()
    End Using
    
    ' Arrange the file stream.
    Dim fs As FileStream = Mock.Create(Of FileStream)(Constructor.Mocked)
    
    ' Mocking the specific call and setting up expectations. 
    ' We replace the actual implementation.
    ' Calling the Write method will result in copying the content 
    ' of 'actual' to the passed byte array.
    Mock.Arrange(Sub() fs.Write(Nothing, 0, 0)).IgnoreArguments() _
        .DoInstead(Sub(content As Byte(), offset As Integer, len As Integer) actual.CopyTo(content, offset))
    
    ' Arranging File.Open. We return a custom file stream.
    Mock.Arrange(Function() File.Open(Arg.AnyString, Arg.IsAny(Of FileMode)())).Returns(fs)
    
    ' Act - 'fileStream' is assigned with the custom stream returned from File.Open.
    Dim fileStream = File.Open("hello.txt", FileMode.Open)
    Dim fakeContent As Byte() = New Byte(actual.Length - 1) {}
    
    ' Original task
    ' After this, as arranged, the content of 'actual' and 'fakeContent' will be the same.
    fileStream.Write(fakeContent, 0, actual.Length)
    
    ' Assert
    Assert.AreEqual(fakeContent.Length, actual.Length)
    
    For i As Integer = 0 To fakeContent.Length - 1
        Assert.AreEqual(fakeContent(i), actual(i))
    Next
End Sub
```

This is what happens in **Example 7** step by step:

1. Write "hello world" in `actual`.          
1. Set expectation for `Write` method. We replace the actual implementation so that it will result in copying the content of `actual` to the passed byte array.          
1. Arrange `File.Open` to return a custom file stream.          
1. Mimic opening a file - `fileStream` is assigned with the custom stream from the previous step.          
1. Call the `Write` method. This is our original task.          
1. Finally, we assert that both files have the same content.


## See Also

* [Returns]({%slug justmock/basic-usage/mock/returns%})

* [DoInstead]({%slug justmock/basic-usage/mock/do-instead%})

* [Matchers]({%slug justmock/basic-usage/matchers%})