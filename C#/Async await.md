# Async await

___

**Q: What is the purpose of `async`/`await` keywords?**

These keywords allow writing asynchronous non-blocking code in a synchronous fashion.

This feature is facilitated by the `Task`/`Task<T>` classes or `ValueTask`/`ValueTask<T>` structs. These types represent an abstraction around an operation that may execute asynchronously.

We use `await` keyword to materialize the task into resulting value. Methods that contain `await` must be marked with the `async` keyword.

___

**Q: What's the difference between asynchronous programming and multithreaded programming?**

An asynchronous task does not necessarily represent execution on a separate thread. You can think of an asynchronous operation as just an aggregation of two events -- start and finish.

A good example of an asynchronous operation is reading a file from a hard drive. To read individual bytes of a file, operating system issues requests to the driver software, which in turn tells the drive to seek to a specific position by moving its mechanical head. The process of moving the head around is asynchronous, it's not an operation that runs on CPU, it's just a physical task you have to wait for completing. This is the type of "pure" async operation that can be represented with the `async`/`await` pattern.

That said, an asynchronous task may also represent some CPU-bound calculation happening on a separate thread, but this is an implementation detail. This is useful when you want to delegate execution to a different thread in order to not block the calling thread, while disguising it as an asynchronous operation. You can do that by calling `Task.Run()`.

Overall, it's fair to say that every multithreaded execution can be represented as an asynchronous operation, but not every asynchronous operation necessarily employs additional threads.

___

**Q: How does it work?**

Let's take the following method as an example:

```c#
public async Task DoAsync()
{
    Console.WriteLine("Before await");

    await Task.Delay(TimeSpan.FromSeconds(1));

    Console.WriteLine("Between awaits");

    await Task.Delay(TimeSpan.FromSeconds(1));

    Console.WriteLine("After await");
}
```

Functionally, this code works by printing "Before await" to the console, waiting 1 second, printing "Between awaits", waiting 1 second again, and then printing "After await".

Everything until the first `await` is executed synchronously just like it would in a normal method. That means "Before await" will be printed on the same context that called this method.

The next thing that happens is that we create and run a new task using a static helper method `Task.Delay()`. This is a simple task that does nothing and automatically turns into completed after a specified delay.

Upon reaching the `await` keyword, the runtime will return control back to the calling method, which may or may not await it. If it does await, the same happens again, returning control to the caller of that method in turn, until it reaches a method in the call stack that either does not await (usually event handlers on a message loop thread) or materializes the task synchronously (e.g. entry point in a console application).

Once the task completes (after 1 second), the execution is returned back to our method, which continues by printing "Between awaits". By default, the execution will continue on the same context as the one that started executing this method.

Then the story repeats again by running and waiting on a new task, finally printing "After await". After the last message, the execution is returned to the caller method (if it awaited on `DoAsync`) so that it can continue executing further.

___

**Q: What happens if we execute an asynchronous method but don't await it?**

Nothing special, the operation represented by the task will continue its lifetime normally, but the result won't be observed. The task object itself will eventually be reclaimed by garbage collector.

Note that we can also use `.ContinueWith()` to handle the result in a call-back fashion.

___

**Q: What happens if an exception is thrown within an asynchronous method?**

If the method is awaited, the exception will instantly propagate to the calling method, then to the caller of that method, and so on, as long as the whole chain is awaited.

Otherwise, the exception will be considered unobserved, which can lead (in some versions of the framework) to the application crashing as soon as the task is disposed by the finalizer.

___

**Q: Is it possible to make a lambda that executes asynchronously?**

Yes.

```c#
var result = await new Func<Task>(async () => await Task.Delay(100));
```

___

**Q: What happens when a method returns a `Task` without awaiting it?**

Example:

```c#
public Task WaitAsync() => SomeOtherMethodAsync();
```

In this case, if an exception is thrown within asynchronous part of `SomeOtherMethodAsync()`, the `WaitAsync()` method will not be listed in the stack trace.

___

**Q: `Task` type implements `IDisposable`, when are we supposed to dispose tasks?**

The `Dispose()` method is not supposed to be invoked manually, a task will be disposed automatically when you await it or when it's reclaimed by the GC.

___

**Q: What is the purpose of `ConfigureAwait()`?**

By default, after the awaited task is completed, the execution continues on the originally captured context, i.e. the same thread that invoked the method. You can override that behavior by specifying `ConfigureAwait(false)`, indicating that the execution may continue on a different context.

It's generally recommended using `ConfigureAwait(false)` wherever possible as it can offer minor performance gain and help prevent deadlocks.

___

**Q: Why is the default behavior to continue on the captured context?**

For compatibility reasons, due to constraints when working with Windows UI and classic ASP.NET applications. In one you can only interact with controls from the main thread, in the other you can only serve the response on the same thread that handled the request, so it was important that the execution continued on the captured context.

___

**Q: What is the difference between `Task` and `ValueTask`?**

The main difference is that `Task` is a class while `ValueTask` is a struct. Latter was added to the BCL to alleviate unnecessary pressure on the garbage collector caused by asynchronous methods that often return synchronously (e.g. cached result). You can think of `ValueTask` as a discriminated union of `Task` or a synchronous result. Since recently, `ValueTask` can also represent a result signaled by `ValueTaskSource`.

___

**Q: What are the main downsides of using asynchronous methods compared to synchronous methods?**

Asynchronous methods can be harder to debug, especially because exceptions thrown from asynchronous methods have difficult to read stack traces. Also, running many asynchronous tasks in a tight loop can put pressure on the garbage collector.
