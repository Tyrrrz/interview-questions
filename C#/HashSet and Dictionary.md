# HashSet and Dictionary

___

**Q: What are `HashSet<T>` and `Dictionary<TKey, TValue` used for?**

`HashSet<T>` is a collection of unique elements.

`Dictionary<TKey, TValue>` is a collection of key-value pairs, where keys are unique.

There are also `Hashtable` which is a non-generic version of `Dictionary`.

___

**Q: How does it verify that an element is unique?**

When adding a new element to a `HashSet` or a new key to a `Dictionary`, its hash code is used to check for uniqueness.

Every type in .NET inherits `GetHashCode()` method from `System.Object` which can be used to uniquely represent internal state of an object as an `Int32` value. If there are two objects of the same type, their `GetHashCode()` *must* return the same value if they are equal and it *should* return different values if they are not equal. In other words, non-equal objects may have identical hash codes.

If you will be trying to add an element whose hash code matches one of the elements already present in the collection, these two elements will be checked for equality using `Equals()` method. If the result is `true`, the element will *not* be added to the collection, and in case with `Dictionary` an exception will be thrown.

In order for `HashSet` and `Dictionary` to function properly, the elements/keys have to properly override `GetHashCode()` and `Equals()` methods. Alternatively, when you instantiate these collections, you can delegate equality checks and hash code calculation to an instance of `IEqualityComparer<T>`.

___

**Q: What happens if `Equals()`/`GetHashCode()` are not overridden and a custom comparer is not specified?**

In case of `struct` types, their default implementation of `Equals()` and `GetHashCode()` uses reflection to compare instances and calculate hash code based on the values of declared fields. This means you can use `struct` types inside `HashSet<T>` and `Dictionary<TKey, TValue>` without having to implement these methods yourself.

As for `class` types, their default implementation of `Equals()` uses reference comparison instead of value comparison, while `GetHashCode()` returns unpredictable values. This means that all instances of `class` types that don't override `Equals()` and `GetHashCode()` will be considered unique in regards to `HashSet` and `Dictionary`.

___

**Q: What if the type is not immutable?**

You shouldn't use mutable types as dictionary keys or elements in `HashSet<T>`. These collection types have no way of knowing that an element's state was modified, so the stored hash code values `GetHashCode()` will become outdated.

___

**Q: How does `Dictionary<TKey, TValue>` perform element lookup?**

Elements in a dictionary are split into a growing set of buckets according to their hash code. The implementation tries to minimize the average number of elements in a bucket. In an ideal case, each bucket contains only one element.

When you attempt to get an element by its key, e.g. using `TryGetValue()` method, it will determine which bucket may contain this element using modulus of the hash code over the number of buckets. If the bucket exists and it contains only one element, it will simply return it; if the bucket exists but there are multiple elements in it, it will use `Equals()` to find the exact match for given key; if the bucket doesn't exist, the element is not present.
