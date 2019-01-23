# System.Index and System.Range

Status: **Review not complete** | 
[API Ref](https://github.com/dotnet/corefx/issues/34076) |
[Video](https://www.youtube.com/watch?v=NYliXLGGBwc)

## Round 1

* `Index` has an implicit conversion to `int`. However, it will fail for
  negative numbers. We normally don't allow implicit conversions to fail;
  however, we believe the developer experience is virtually identical as all
  methods accepting an `int`-based index will throw an
  `ArgumentOutOfRangeException` when being called with a negative number. Now
  the call site will fail with the conversion, but it's the same line of code in
  the developer's program.
    - We should `ArgumentOutOfRangeException` from the implicit operator (as
      opposed to `InvalidCastException`)
* Do the types `Index` and `Range` add any value over conventions for
  method/indexer parameter list?
    - Yes, because they allow languages to model the special syntax as
      expressions, which makes them composable, like being able to extract them
      as variables.
* We still have some open questions, such as:
    - We need a method that can *close* the range, given a container size
    - Performance issues (see below)

## Round 2

* Andy pointed out that the proposal with the attribute (see blow) might not
  easily work if we ship syntax support for high performance APIs in .NET Core
  3.0. We might want to review the plan at least enough to make a decision on
  this.
* Andy asked whether the compiler should consolidate all constructions of
  `Range` into create; however, we feel it might be cheaper at runtime if the
  compiler calls specialized methods so we can avoid bounds checking for the
  missing side. On the other hand, we believe we can add them later if
  necessary. Not making them known to the compiler has the advantage that we can
  decide on naming without involving the compiler.
* Having the implicit operator throw is an artifact of the encoding (we use the
  sign to indicate `fromEnd`) which prevents us from delaying the exception to
  the argument validation of the indexer. However, we still believe the
  developer experience doesn't suffer meaningfully.

### Index

We need this method:

```C#
struct Index
{
    static Index Start => new Index(0, false);
    static Index End => new Index(0, true);
    Index(int value, bool isFromEnd = false) {}
    static Index FromStart(int value) {}
    static Index FromEnd(int value) {}
    bool IsFromEnd { get; }
    int GetOffset(int length) {}
}
```

* No operators. We believe having them throw would be problematic; let's wait
  and see.
* No deconstruct for `Index`

### Range

We need a method to compute the effective values.

```C#
var (start, length) = range.GetOffsetLength(Length);
```

* Replace `Create` with a regular constructor that takes two `Index`
    - We'll keep the `All`, `FromStart` and `ToEnd` methods for VB/F#
    - `All` should be a field/property
    - Rename `FromStart` to `StartAt`, `ToEnd` to `EndAt`
* We should add a `Range` based indexer to the memory classes

## Convenient use of high-performance APIs

Indexers and slice methods are sometimes performance critical, especially for
low-level types, for example, `Span<T>`. We generally don't believe we'll be
able to get them to parity with `int`-based overloads. We can make them additive
in the sense that we can add overloads, but this also means callers cannot use
language syntax. An alternative would be to allow attributing the method (or
parameters) that allows the caller to use syntax at the call site.

Given this method:

```C#
[IndexMethod]
public T this[int index];

[RangeMethod]
public Slice(int start, int length);
```

Code like this could work:

```C#
x.Slice(start..end);
x.Slice(start..^1);
x[^index]
```

The idea would be to code spit the appropriate translation, using the receiver's
`Length`/`Count` properties.

It's unclear how or if this would work when the expressions aren't inline but
are extracted to local variables. However, this probably require the compiler to
synthesize appropriate members that take `Index` and `Range` so that
IntelliSense and symbol lookup works in a sane way (similar to how the compiler
deals with extension methods).

The primary reason why this would be cheaper is the elimination of multiple
layers of bounds checking and throw statements that the JIT isn't likely to be
able to eliminate easily.