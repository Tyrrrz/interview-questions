# Heap vs stack

___

**Q: Where are objects allocated in C#?**

In C# there are two places where an object can be stored -- the heap and the stack.

Objects allocated on the stack are available only inside of a stack frame (execution of a method), while objects allocated on the heap can be accessed from anywhere.

___

**Q: Which objects are allocated on the stack and which objects are allocated on the heap?**

_Note: never say "reference types are allocated on the heap while value types are allocated on the stack", this is a commonly repeated mistake and sets off a red flag for an experienced interviewer._

Reference types (classes, interfaces, delegates, `object` and `string`) are always allocated on the heap and never on the stack.

When you pass a reference object as a parameter or assign it to a variable, you're in fact passing its reference, which itself can also be allocated on the stack.

By passing a reference to such an object, you're effectively telling where that object is located on the heap so that the invoked code can access it.

Every time an object is passed as a reference, the reference itself is copied. This means that you can change the reference to point to a different object without affecting the previous object itself or other references pointing to it. A reference is lightweight and is always constant size (32 bit or 64 bit depending on OS bitness) so copying it (and thus passing around reference types) is considered cheap.

The `string` type deserves a special mention because it's a reference type but it's primarily passed by copy, not by reference. This is the only such type.

Value types (derived from `System.ValueType`, e.g. `int`, `bool`, `char`, `enum` and any `struct`) can be allocated on the heap or on the stack, depending where they were declared.

- If the value type was declared inside a method then it's stored on the *stack*.
- If the value type was declared as a member of a class then it's stored on the *heap*, along with its parent.
- If the value type was declared as a member of another value type then it's stored wherever that value type is stored.

Starting with C#7.2, a `struct` can be declared as `ref struct`, in which case it will *always* be allocated on the stack, preventing it from being declared inside reference types.

Instances of value types are passed by copy (unless used with reference semantics, see below). This means that every time a value type is assigned to a variable or passed as parameter, the value is copied.

Because copying value types can get expensive depending on the size of the object, it's not recommended to declare memory-heavy objects as value types.

Since every type in C# derives from `System.Object`, value types can be assigned to variables or passed to methods that expect an `object`. In such cases, the value is copied and stored on the heap wrapped as a reference type, an operation known as *boxing*.

___

**Q: Can we use value types with reference semantics?**

Keywords such as `ref` and `out`, `ref return` and `ref local` (C#7.0), `in` (C#7.2) allow accessing value types by reference. This means that instead of copying the value, the consuming code will receive a reference to the value instead, be it on a stack or on a heap, as long as the lifetime of that value type is longer than that of consuming code.

___

**Q: How is the heap memory freed up?**

While the objects stored on the stack are gone when the containing stack frame is popped, memory used by objects stored on the heap needs to be freed up by the *garbage collector*.

When an object stored on the heap no longer has any references pointing to it, it's considered eligible for garbage collection.

At a certain point, garbage collector kicks in, interrupts all running threads, invokes the finalizers of the objects it's trying to get rid of (on a special finalizer thread), and then marks the memory as free to use.

___

**Q: What issue may happen due to allocation and de-allocation of memory on the heap?**

As the memory on the heap is allocated and de-allocated, it becomes fragmented. See the following diagram:

```
HEAP:
---][-------][----------][-----]........
      obj 1      obj 2    obj 3   free
```

When `obj 2` is de-allocated, its memory becomes free:

```
HEAP:
---][-------]............[-----]........
      obj 1      free     obj 3   free
```

Now, if the runtime needs to allocate another object on the heap, it may use the memory freed up by `obj 2`, but only if the new object actually "fits". If that memory is not enough, the runtime may request more contiguous memory from the operating system by expanding its working set, as shown here:

```
HEAP:
---][-------]............[-----][--------------------]...
      obj 1      free     obj 3         obj 4
```

As a result of the fragmentation, the memory usage becomes less efficient. To deal with this, garbage collector may rearrange the memory so that there are no gaps. This is done by simply copying the bytes around, in an operation called "defragmentation".

```
HEAP:
---][-------][-----][--------------------]...............
      obj 1   obj 3         obj 4               free
```

___

**Q: What is Large Object Heap and what is it used for?**

Depending on the size of the consumed memory, memory defragmentation can be expensive, that's why the heap is further separated into *Small Object Heap* (SOH) and *Large Object Heap* (LOH).

An object is stored on the SOH if it's smaller than 85kbytes, otherwise it's stored on the LOH. This cut off point of 85000 bytes was empirically devised as the point after which defragmentation no longer provides performance benefits.

Due to how CPUs deal with `double`s, arrays of `double` are an exception, such objects are stored on the LOH if there are more than 1000 elements in the array.

Memory in LOH is (normally) not defragmented, providing better performance at the cost of less efficient memory usage.
