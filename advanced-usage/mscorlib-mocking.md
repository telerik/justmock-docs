---
title:  System API / MsCorLib
page_title: System API / MsCorLib Mocking | JustMock Documentation
description:  System API / MsCorLib Mocking
previous_url: /advanced-usage-mscorlib-mocking.html
slug: justmock/advanced-usage/mscorlib-mocking
tags: mscorlib,mocking
published: True
position: 14
---

#  System API / MsCorLib Mocking

__Telerik® JustMock__ enables you to mock methods from the .NET Framework, i.e. from MsCorlib.
Even more, with JustMock you are able to arrange against any MsCorlib member/function without any additional setups. This allows you to mock MsCorlib functions/members by directly arranging their behavior.

> This feature is available only in the commercial version of Telerik JustMock. Refer to [this]({%slug justmock/getting-started/commercial-vs-free-version%}) topic to learn more about the differences between the commercial and free versions of Telerik JustMock.


## Mocking DateTime Class

The following example demonstrates how to mock the `DateTime.Now` method to return the date `April 12, 2010`.

  #### __[C#]__

  {{region MsCorlib#MockStaticMethod}}
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
  {{endregion}}

  #### __[VB]__

  {{region MsCorlib#MockStaticMethod}}
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
  {{endregion}}

Note that you can now arrange the `DateTime.Now` without any per-requisites.

This will also work even if the call to `DateTime.Now` is in a member in another class. For example, take the `NestedDateTime` class:

  #### __[C#]__

  {{region MsCorlib#NestedDateTimeClass}}
    public class NestedDateTime
    {
        public DateTime GetDateTime()
        {
            return DateTime.Now;
        }
    }
  {{endregion}}

  #### __[VB]__

  {{region MsCorlib#NestedDateTimeClass}}
    Public Class NestedDateTime
            Public Function GetDateTime() As DateTime
                Return DateTime.Now
            End Function
        End Class
  {{endregion}}

In this case, the first example will look like this:

  #### __[C#]__

  {{region MsCorlib#MockStaticMethod2}}
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
  {{endregion}}

  #### __[VB]__

  {{region MsCorlib#MockStaticMethod2}}
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
  {{endregion}}


## Mocking FileInfo Class

Using JustMock we can mock virtually any MsCorlib type without any per-requisites. To demonstrate mocking of framework methods we will take the `FileInfo` class and mock its `Delete` method.

  #### __[C#]__

  {{region MsCorlib#Example}}
    [TestMethod]
    public void ShouldMockFileInfoDelete()
    {
        var filename = this.GetType().Assembly.ManifestModule.FullyQualifiedName;
        var MyFileInfo = new FileInfo(filename);

        bool isCalled = false;

        // Arrange
        Mock.Arrange(() => MyFileInfo.Delete()).DoInstead(() => isCalled = true);

        //Act
        MyFileInfo.Delete();

        //Assert
        Assert.IsTrue(isCalled);
    }
  {{endregion}}

  #### __[VB]__

  {{region MsCorlib#Example}}
    <TestMethod> _
    Public Sub ShouldMockFileInfoDelete()
        'Arrange
        Dim filename = Me.[GetType]().Assembly.ManifestModule.FullyQualifiedName
        Dim myFileInfo = New FileInfo(filename)

        Dim isCalled As Boolean = False

        ' Arrange
        Mock.Arrange(Sub() myFileInfo.Delete()).DoInstead(Sub() isCalled = True)

        'Act
        myFileInfo.Delete()

        'Assert
        Assert.IsTrue(isCalled)
    End Sub
  {{endregion}}

We simply arrange when `fi.Delete` is called to set `called` to `true`. Then, after acting we verify it.

## Mocking DriveInfo Class

Let's look at another example - we want to mock DriveInfo.GetDrives():

  #### __[C#]__

  {{region MsCorlib#MockDriverInfo2}}
    [TestMethod]
    public void ShouldMockDriveInfoGetDrives_StaticMember()
    {
        bool isCalled = false;

        // Arrange
        Mock.Arrange(() => DriveInfo.GetDrives()).DoInstead(() => isCalled = true);

        // Act
        DriveInfo.GetDrives();

        // Assert
        Assert.IsTrue(isCalled);
    }
  {{endregion}}

  #### __[VB]__

  {{region MsCorlib#MockDriverInfo2}}
    <TestMethod> _
    Public Sub ShouldMockDriveInfoGetDrives_StaticMember()
        Dim isCalled As Boolean = False

        ' Arrange
        Mock.Arrange(Sub() DriveInfo.GetDrives()).DoInstead(Sub() isCalled = True)

        ' Act
        DriveInfo.GetDrives()

        ' Assert
        Assert.IsTrue(isCalled)
    End Sub
  {{endregion}}

Again, we arrange when `GetDrives()` is called to set `called` to `true`. Then, after acting we verify it.

## Mocking File Class

The following example demonstrates how to mock the `File.ReadAllBytes` method.

We ignore the implementation of the method and arrange it to return our fake content for the file. After acting, we verify that both the arrays have the same length and content.

The comments in the code will guide you through the whole process:

  #### __[C#]__

  {{region MsCorlib#MockFile2}}
    [TestMethod]
    public void ShouldMockStaticFileForReadOperaton()
    {
        // Replace the actual implementation of File.ReadAllBytes methods
        // with returning 'fakeBytes' bytes array
        byte[] fakeBytes = "byte content".ToCharArray().Select(c => (byte)c).ToArray();

        // Arrange 
        Mock.Arrange(() => File.ReadAllBytes(Arg.IsAny<string>())).Returns(fakeBytes);

        // Act
        var ret = File.ReadAllBytes("ping");

        // Assert - both have the same length
        Assert.AreEqual(fakeBytes.Length, ret.Length);

        // Assert - both have the same content
        var same = true;

        for (var i = 0; i < fakeBytes.Length; i++)
        {
            if (fakeBytes[i] != ret[i])
            {
                same = false;
                break;
            }
        }

        Assert.AreEqual(true, same);
    }
  {{endregion}}

  #### __[VB]__

  {{region MsCorlib#MockFile2}}
    <TestMethod> _
    Public Sub ShouldMockStaticFileForReadOperaton()
        ' Replace the actual implementation of File.ReadAllBytes methods
        ' with returning 'fakeBytes' bytes array
        Dim fakeBytes As Byte() = "byte content".ToCharArray().[Select](Function(c) CByte(AscW(c))).ToArray()

        ' Arrange 
        Mock.Arrange(Function() File.ReadAllBytes(Arg.IsAny(Of String)())).Returns(fakeBytes)

        ' Act
        Dim ret = File.ReadAllBytes("ping")

        ' Assert - both have the same length
        Assert.AreEqual(fakeBytes.Length, ret.Length)

        ' Assert - both have the same content
        Dim same = True

        For i As Integer = 0 To fakeBytes.Length - 1
            If fakeBytes(i) <> ret(i) Then
                same = False
                Exit For
            End If
        Next

        Assert.AreEqual(True, same)
    End Sub
  {{endregion}}


## Mocking FileStream Class

This is a more complicated example. It demonstrates how to mock the `FileStream.Write` method.

We arrange the Write method of the FileStream. The comments in the code will guide you through the whole process:

  #### __[C#]__

  {{region MsCorlib#MockFileStream2}}
    [TestMethod]
    public void ShouldMockFileOpenWithCustomFileStream()
    {
        byte[] actual = new byte[255];

        // Writing locally, can be done from resource manifest as well.

        using (StreamWriter writer = new StreamWriter(new MemoryStream(actual)))
        {
            writer.WriteLine("Hello world");
            writer.Flush();
        }

        // Arrange the file stream.
        FileStream fs = Mock.Create<FileStream>(Constructor.Mocked);

        // Mocking the specific call and setting up expectations. 
        // We replace the actual implementation.
        // Calling the Write method will result in coping the content 
        // of 'actual' byte array to the passed byte array.
        Mock.Arrange(() => fs.Write(null, 0, 0)).IgnoreArguments()
            .DoInstead((byte[] content, int offset, int len) => actual.CopyTo(content, offset));

        // Arranging File.Open. We return a custom file stream.
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
  {{endregion}}

  #### __[VB]__

  {{region MsCorlib#MockFileStream2}}
    <TestMethod> _
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
        ' Calling the Write method will result in coping the content 
        ' of 'actual' byte array to the passed byte array.
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
  {{endregion}}

This is what happens here:

1. Write "hello world" in `actual`.          
1. Set expectation for `Write` method. We replace the actual implementation so that it will result in coping the content of `actual` byte array to the passed byte array.          
1. Arrange `File.Open` to return a custom file stream.          
1. Mimic opening a file - `fileStream` is assigned with the custom stream from previous step.          
1. Call the `Write` method. This is our original task.          
1. Finally, we assert that the both files have the same content.
