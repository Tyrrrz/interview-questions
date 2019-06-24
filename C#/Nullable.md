# Nullable

___

**Q: What's the difference between `int` and `int?` declarations?**

The `int?` notation is a shorthand for writing `Nullable<int>`. This type is a wrapper for value types that allows them to take `null` as a valid value.

This is useful for cases when a variable (or field, or property) is expected to have an indeterminate state, meaning it was not initialized as opposed to just being set to default value.

___

**Q: How is Nullable implemented?**

Generic type `Nullable<T>` is defined as a struct. There are also some helper methods exposed on a non-generic `Nullable` type which is a static class.

Below you can see an example implementation of `Nullable<T>`, the actual implementation in .NET Core/.NET Framework may be slightly different.

```c#
public struct Nullable<T> where T : struct
{
    private readonly bool _hasValue;
    private readonly T _value;

    public bool HasValue => _hasValue;

    public T Value => _hasValue ? _value : throw new InvalidOperationException();

    public Nullable(T value)
    {
        _hasValue = true;
        _value = value;
    }

    public T GetValueOrDefault() => _hasValue ? _value : default(T);

    public T GetValueOrDefailt(T defaultValue) => _hasValue ? _value : defaultValue;

    public override bool Equals(object other) => _hasValue ? value.Equals(other) : other == null;

    public override int GetHashCode() => _hasValue ? value.GetHashCode() : 0;

    public override string ToString() => _hasValue ? value.ToString() : "";

    public static implicit operator Nullable<T>(T value) => new Nullable<T>(value);

    public static explicit operator T(Nullable<T> value) => value.Value;
}
```

The generic constraint `T : struct` makes sure that this type can only be used with value types.

Members of the `Nullable<T>` struct include a property `HasValue` which indicates whether the value was initialized, and `Value` property which contains the actual value. Accessing the `Value` property when `HasValue` is `false` throws an exception.

There is also `GetValueOrDefault()` method that allows safely retrieving the value with a fallback.

To facilitate equality comparison, base methods `Equals()` and `GetHashCode()` have been overridden. The `Equals` method returns `true` if compared with `null` while `HasValue` is `false`.

Finally, this type implements implicit conversion between `T` and `T?` that lets us do `int? a = 5`, and also an explicit conversion between `T?` and `T` which lets us convert the other way around but will throw an exception if the value isn't set.

___

**Q: How is it possible that we can assign `null` to `Nullable<T>` even though it's a struct?**

Compiler provides syntactic sugar when working with `Nullable<T>`:

- When you write `int? a = null` it gets compiled to `Nullable<int> a = new Nullable<int>()`.
- When you write `if (a == 15)` (where `a` is `int?`) it gets compiled to `if (a.GetValueOrDefault() == 15 && a != null)`

___

**Q: What happens when you pass an instance of `Nullable<T>` to a method that expects an `object`?**

In such cases, to avoid "double boxing", the `Nullable<T>` wrapper is discarded and you either get a `T` instance boxed as `object` or a `null` reference, depending on whether `HasValue` is `true` or `false`.

___

**Q: How does `Nullable<T>` work during runtime?**

Behavior of `Nullable<T>` is further optimized by the runtime, which makes its instances appear as instances of `T` when `HasValue` is true.
