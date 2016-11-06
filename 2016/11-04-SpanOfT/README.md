# API Review for `Span<T>`

Status: **Approved** | [Issue](https://github.com/dotnet/corefxlab/issues/951)

We agreed on the following API shape for `Span<T>`:

```C#
public struct Span<T>
{
    public unsafe Span(void* ptr, int length);
    public Span(T[] array);
    public Span(T[] array, int start);
    public Span(T[] array, int start, int length);
    public static Span<T> Empty { get; }
    public bool IsEmpty { get; }
    public ref T this[int index] { get; }
    public int Length { get; }
    public void CopyTo(Span<T> destination);
    [Obsolete("This operation is not supported.", IsError=True)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public override bool Equals(object obj);
    [Obsolete("This operation is not supported.", IsError=True)]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public override int GetHashCode();
    [EditorBrowsable(EditorBrowsableState.Never)]
    public ref T int DangerousGetPinnableReference();
    public static Span<T> DangerousCreate(object obj, IntPtr offset, int length); 
    public static implicit operator Span<T> (ArraySegment<T> arraySegment);
    public static implicit operator Span<T> (T[] array);
    public static bool operator ==(Span<T> left, Span<T> right);
    public static bool operator !=(Span<T> left, Span<T> right);
    public Span<T> Slice(int start);
    public Span<T> Slice(int start, int length);
    public T[] ToArray();
}
```

## `Span<T>` and 64-bit

It was suggested to allow `Span<T>` to represent spans that are larger than
32-bit.

Before discussion the options it's important to consider that even on 64-bit
machines, most apps should (and thus will) run as 32-bit because they don't need
a 64-bit address space and will therefore benefit from smaller working sets as
the pointers aren't inflated.

In order to support 64-bit spans we have these options:

1. **Only expose 64-bit APIs**. This would mean having the indexer and `Length`
   would return `long`.

2. **Expose 32-bit and 64-bit APIs**. This would mean we'd have two indexers
   (one accepting `int`, the other `long`) and a `Length` property returning
   `int` as well as a `LongLength` property returning `long`.

3. **Use an architecture specific type**. This would mean the indexer and length
   would return a type that either is either 32-bit or 64-bit, depending on the
   process architecture (akin to how `IntPtr` works).

4. **Separate Span<T> type**. In this model, we'd have `Span32<T>` and
   `Span64<T>`.

The options (1) and (2) are quite cumbersome to use with our existing BCL and
much of any of the existing managed code that deals with arrays as virtually all
APIs that deal with lengths and offsets are encoded using `int`. This penalizes
the 90% case due to the need for conversions, which makes the developers code
more complicated, which might also impact the ability for the code gen to emit
performant code.

Option (3) seems like a big hammer that also makes it harder to convert types
back and forth. And option (4) is pretty much a non-starter as it bifurcates the
APIs dealing with spans.

We don't believe we actually need to have spans that are larger than what a
signed-int can represent, which is 2 GB. If the need arises in the future, we
will take another look. Chances are this also requires changes in other parts,
specifically arrays.

## `Span<T>.Slice()` with unsigned data types

The current prototype has slicing methods that take `uint`:

```C#
public Span<T> Slice(uint start);
public Span<T> Slice(uint start, uint length);
```

We decided to remove them because we believe their value is low; the argument is
similar as for 64-bit. Virtually everything deals in `int`.

Developers that prefer their algorithms to be implemented using `uint` can
define these methods as extension methods.

## `Span<T>.Slice()` and language syntax

The language team is considering adding syntax for specifying ranges. The
early thinking is that the an expression like `n..m` will be of a to-be-added
type, such as `Range`. We think we'll likely use this type in indexers to
provide functionality that is today provided by the `Slice` method:

This would allow code like:

```C#
Span<byte> buffer = ...

Span<byte> view1 = buffer.Slice(3, 2);
Span<byte> view2 = buffer[3..4];
 
// view1 == view2
```

We don't think this means we should change anything in the design of `Span<T>`.
For existing types (such as `string` and `Array`) it will also be normal to have
both, a method as well as an indexer.

## `Span<T>` and `foreach`

We currently shared the struct-based enumerator with `ReadOnlySpan<T>` which
looks weird. We decided that the enumerator is of dubious value in general as
the optimizer will unlikely be able to emit code that is as good as a `for`
loop, especially with bounce checking removal.

Since `Span<T>` will have to be special cased by the C# compiler we believe it's
better to support `foreach` by emitting a regular `for` loop (akin to what the
compiler does for `string`). We could even generalize this and make it a general
pattern so that any type that offers an `int`-based `Length` and indexer will
become enumerable with `foreach`.

Also, it's worth pointing out that there is no point in implementing the
`IEnumerable<T>` interface as both boxing a instatiating a generic type or
method with `Span<T>` will be disallowed by the type system.

## `Span<T>.Set()`

These methods are equivalent to the `CopyTo` methods and simply allow swapping
the operands:

```C#
Span<byte> left = ...
Span<byte> right = ...

// This:
//
// right.Set(left);
//
// is the same as 

left.CopyTo(right);
```

Since the BCL usually doesn't have this pattern we've decided to remove the
`Set()` methods.

## `Span<T>.CopyTo(T[])`

We decided to remove the API as it adds zero expressiveness or performance
gains. The implicit conversion from `T[]` to `Span<T>` will allow passing in an
array.

## `Span<T>.TryCopyTo(Span<T>)`

The `CopyTo` method has to check the destination span is large enough. If it's
not, an exception will be thrown. However, when dealing with buffers having to
reallocate is quite common. Since exception can be expensive, calling code will
have to be written defensively like this:

```C#
Span<byte> source = ...
Span<byte> target = ...

if (target.Length < source.Length)
    target = GrowBuffer();

source.CopyTo(target);
```

First of all, this results in duplicate checks (which are probably not too bad,
given that it's a bulk operation) but more importantly it's quite error prone
to get the comparison correct. We suggest it's easier to add a `TryCopyTo`
method that allows code like this:

```C#
Span<byte> source = ...
Span<byte> target = ...

if (!source.TryCopyTo(target))
{
    target = GrowBuffer();
    source.CopyTo(target);
}
```

## `Span<T>(object, UIntPtr, length)`

The scenario makes sense, but it shouldn't be a constructor as it's quite
advanced. We'd like to make it a method so that we can convey that's unsafe.
Thus, we decided to make this `Span<T>.DangerousCreate(object, IntPtr, length)`.

## `Span<T>(T[], int start)`

Other BCL APIs with similar functionality provide a way to simply construct the
range representing the remainder from a given position, for example `Substring`.
For consistency, we should add this method. It's also quite useful.

## `Span<T>` Indexer

In order to align with the semantics of arrays, the return type of the indexer
should be `ref T` instead of just `T`. This allows code like this:

```C#
Parse(ref span[index]);
```

## `Span<T>.Length`

There is no reason why this needs to be a field and the general BCL pattern is
to expose this as a property.

## `Span<T>` and the underlying pointer

In order to use `Span<T>` with native code, we need a mechanism to extract a
`ref T` that can be used with the `fixed` statment. We could use `span[0]` but
that doesn't work for zero-length spans as it will cause an index of range
error.

Instead, we'll introduce an API like `ref T DangerousGetPinnableReference()`.
Since the API doesn't return a pointer, it doesn't require an `unsafe` context.
However, the API is inherently unsafe as assigning to the returned value is
possible but incorrect in case the span has zero-length. Thus, we follow the
convention and use the term `Dangerous` in the API.

We don't expect user code to call this API directly. Thus, we'll make this API
as hidden in IntelliSense. Instead, we'd like to add a compiler feature that
enables the `fixed` statement to work for any types that offer this method:

```C#
Span<byte> buffer = ...;

fixed (byte* pBuffer = &buffer)
{
    // Use the pointer
}
```

This allows other structs to offer the same behavior, which is especially useful
for structs that simply wrap spans.
