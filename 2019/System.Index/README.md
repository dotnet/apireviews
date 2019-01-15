# System.Index and System.Range

Status: **Review not complete** | 
[API Ref](https://github.com/dotnet/corefx/issues/34076)
[Video](https://www.youtube.com/watch?v=NYliXLGGBwc)

## Notes

* `Index` has an implicit conversion to `int`. However, it will fail for
  negative numbers. We normally don't allow implicit conversions to fail;
  however, we believe the developer experience is virtually identical as all
  methods accepting an `int`-based index will throw an
  `ArgumentOutOfRangeException` when being called with a negative number. Now
  the call site will fail with the conversion, but it's the same line of code in
  the developer's program.
    - We should `ArgumentOutOfRangeException` from the implicit operator (as
      opposed to `InvalidCastException`)
* Do the types `Index` and `Range` any value over conventions for method/indexer parameter list?
    - Yes, because they allow languages to model the special syntax as
      expressions, which makes them composable, like being able to extract them
      as variables.
* We still have some open questions, such as:
    - We need a method that can *close* the range, given a container size
    - Performance issues (see below)

## Convenient use of high-performance APIs

Indexers and slice methods are sometimes performance critical, especially for
low-level types, for example, `Span<T>`. We generally don't believe we'll be
able to get them to parity with `int`-based overloads. We can make them additive
in the sense that we can add overloads, but this also means callers cannot use
language syntax. An alternative would be to allow attributing the method (or
parameters) that allows the caller to use syntax at the call site.

Given this method:

```C#
[RangeMethod]
public Slice(int start, int length)
```

Code like this could work:

```C#
x.Slice(start..end);
```

There would be limitations though:

* The `^` syntax wouldn't be supported
* Use of `Index` and `Range` types wouldn't be supported
* Extracting the expression `start..end` into a local variable wouldn't be supported

We could make those things, with an understanding that they would impact
performance, which seem to make them somewhat questionable.