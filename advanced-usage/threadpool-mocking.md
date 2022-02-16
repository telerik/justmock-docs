---
title: Threadpool Mocking
page_title: Threadpool Mocking | JustMock Documentation
description: Threadpool Mocking
previous_url: /advanced-usage-threadpool-mocking.html
slug: justmock/advanced-usage/threadpool-mocking
tags: threadpool,mocking
published: True
position: 20
---

# Threadpool Mocking

Threadpool mocking is one of the advanced features supported in __Telerik® JustMock__. It allows you to access mocked objects inside another thread and work with them as expected.

> This feature is available only in the commercial version of Telerik JustMock. Refer to [this]({%slug justmock/licensing/license-agreement%}#commercial-vs-free-version) topic to learn more about the differences between the commercial and free versions.

## Mocking Inside a Thread

We will use the following sample code to illustrate how you can do this:

  #### __[C#]__

  {{region ThreadpoolMocking#SampleCode1}}
    public class Mockable
	{
	    public bool IsMocked
	    {
	        get
	        {
	            throw new NotImplementedException();
	        }
	    }
	}
	
	internal class WaitLatch : IDisposable
	{
	    public void Wait()
	    {
	        while (autoResetEvent.WaitOne())
	        {
	            if (currentCount >= initialCount)
	                break;
	        }
	    }
	
	    public void Signal()
	    {
	        Interlocked.Increment(ref currentCount);
	        autoResetEvent.Set();
	    }
	
	    public void Dispose()
	    {
	        Dispose(true);
	        GC.SuppressFinalize(this);
	    }
	
	    protected virtual void Dispose(bool disposing)
	    {
	        if (disposing)
	            if (autoResetEvent != null)
	            {
	                autoResetEvent = null;
	            }
	    }
	
	    ~WaitLatch()
	    {
	        Dispose(false);
	    }
	
	    private int currentCount;
	    private const int initialCount = 1;
	
	    AutoResetEvent autoResetEvent = new AutoResetEvent(false);
	}
  {{endregion}}


Here the `WaitLatch` class represents the thread we are going to execute. Inside the thread we will access an instance of the `Mockable` class. Let's see a complete example:

  #### __[C#]__

  {{region ThreadpoolMocking#SampleCode2}}
    [TestMethod]
	public void ShouldInvokeMockInsideAChildThreadFromThreadPool()
	{
	    var mockable = Mock.Create<Mockable>();
	    Mock.Arrange(() => mockable.IsMocked).Returns(true);
	
	    bool mocked = false;
	
	    var latch = new WaitLatch();
	
	    ThreadPool.QueueUserWorkItem((cookie) =>
	    {
	        try
	        {
	            mocked = mockable.IsMocked;
	        }
	        finally
	        {
	            latch.Signal();
	        }
	    });
	
	    latch.Wait();
	
	    Assert.IsTrue(mocked);
	}
  {{endregion}}


We use `Mock.Arrange` to specify that the `IsMocked` property should return true. Then we create another thread using `ThreadPool.QueueUserWorkItem` and access the mocked object. Finally, we assert that the expected value is returned.

> **Note**
>
> Static member mocking and future mocking does not work across threads by default. To make a static or future arrangement valid across all threads, add the .OnAllThreads() clause to the arrangement, e.g. 
>  {{region }}
    Mock.Arrange(() => DateTime.Now).Returns(new DateTime()).OnAllThreads();
  {{endregion}}

