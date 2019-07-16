# Synchronization primitives

___

**Q: What synchronization objects do you know in C#?**

There are various types available:

- `Mutex`, which can be used to facilitate exclusive access to a shared resource.
- `Semaphore`, which can be used to limit the number of threads that can access a shared resource at the same time.
- `AutoResetEvent`, which can transition from non-signaled to signaled state, allowing one thread to access a shared resource, before automatically going back to non-signaled state.
- `ManualResetEvent`, which can transition from non-signaled to signaled state, allowing any thread to access to a shared resource, before being manually reset to non-signaled state.

___

**Q: What's the difference between `Semaphore` and `SemaphoreSlim` and similar?**

Base class library offers lightweight (slim) versions of some synchronization primitives. The difference is that slim variants don't use native resources, don't reply on underlying OS, and typically provide better performance.

On the other hand, non-slim synchronization objects can be named, which allows them to be used between multiple processes, rather than just between threads in a single process.

If you plan on using these synchronization primitives within single application, slim variants are a better choice.
