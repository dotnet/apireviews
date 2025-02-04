# API Review 02/04/2025

## Random.Get{Hex}String

**Approved** | [#runtime/111956](https://github.com/dotnet/runtime/issues/111956#issuecomment-2634728595) | [Video](https://www.youtube.com/watch?v=0v4cLS4stVk&t=0h0m0s)

* When we added these to the CSPRNG we believed no one would need them on System.Random.  New data has proven otherwise.
* "Should these be virtual?" GetItems and Shuffle aren't. These are matching that. If we change any to virtual we should change all to virtual.
* Should we add GetString that writes to a span?  No, it's redundant with GetItems.

```c#
namespace System;

public partial class Random
{
    // same signatures as on RandomNumberGenerator, but instance instead of static
    public string GetString(ReadOnlySpan<char> choices, int length);
    public string GetHexString(int stringLength, bool lowercase = false);
    public void   GetHexString(Span<char> destination, bool lowercase = false);
}
```
## Recommend using JsonElement.Parse

**Approved** | [#runtime/111957](https://github.com/dotnet/runtime/issues/111957#issuecomment-2634735924) | [Video](https://www.youtube.com/watch?v=0v4cLS4stVk&t=0h12m55s)

Looks good as proposed

Category: Performance
Severity: Info
## `System.Linq.Shuffle()`

**Approved** | [#runtime/111221](https://github.com/dotnet/runtime/issues/111221#issuecomment-2634781919) | [Video](https://www.youtube.com/watch?v=0v4cLS4stVk&t=0h16m11s)

* No one seems to be comfortable with reservoir sampling, so the implementation of this is always going to be spiritually equivalent to `return random.Shuffle(source.ToArray())`
* We have reservations about the overload accepting a Random instance.  It's difficult (if not impossible) for IQueryable.
* We should do AsyncEnumerable and Queryable at the same time

```c#
namespace System.Linq;

public static class Enumerable 
{
    public static IEnumerable<T> Shuffle<T>(this IEnumerable<T> source) { }
}

public static class AsyncEnumerable 
{
    public static IAsyncEnumerable<T> Shuffle<T>(this IAsyncEnumerable<T> source) { }
}

public static class Queryable 
{
    public static IQueryable<T> Shuffle<T>(this IQueryable<T> source) { }
}
```

We also discussed doing an analyzer for patterns like `enumerable.OrderBy(e => Guid.NewGuid())`; it seems reasonable, and would be Category:Performance, Severity:Info.

## Proposal: allow exception marshallers via '[return: MarshalUsing]' on generated COM interfaces methods without '[PreserveSig]'

**Approved** | [#runtime/109522](https://github.com/dotnet/runtime/issues/109522#issuecomment-2634813932) | [Video](https://www.youtube.com/watch?v=0v4cLS4stVk&t=0h45m30s)

* Looks good as proposed.
* For a reference example of what one of these types would be, see `ExceptionAsHResultMarshaller<T>`

```c#
namespace System.Runtime.InteropServices.Marshalling;

public partial class GeneratedComInterfaceAttribute
{
    public Type? ExceptionToUnmanagedMarshaller { get; set; }
}
```

## Expose a way to get either directly to the backing array of an ImmutableArray.Builder, or get it as a span.

**Approved** | [#runtime/111813](https://github.com/dotnet/runtime/issues/111813#issuecomment-2634834461) | [Video](https://www.youtube.com/watch?v=0v4cLS4stVk&t=0h54m53s)

* The closest equivalents always return bounded data, such as getting a Span from List, or the correctly sized array from ImmutableArray.  So the return here shouldn't be the array itself, but Memory, so that the offset and length are conveyed with it.
  * Callers who really want the array can then use the TryGetArray method on Memory.
  * Since it's not returning an array, renamed AsArray to AsMemory

```c#
namespace System.Runtime.InteropServices;

public static class ImmutableCollectionsMarshal
{
    public static Memory<T> AsMemory<T>(ImmutableArray<T>.Builder builder);
}
```

## Consider adding a Clone() Method to the hash implementations

**Approved** | [#runtime/111908](https://github.com/dotnet/runtime/issues/111908#issuecomment-2634840960) | [Video](https://www.youtube.com/watch?v=0v4cLS4stVk&t=1h5m31s)

* Looks good as proposed

```C#
namespace System.IO.Hashing;

public sealed partial class Crc32
{
    /// <summary> Returns a clone of the current instance, with a copy of the internal state. </summary>
    public Crc32 Clone();
}

public sealed partial class Crc64
{
    public Crc64 Clone();
}

public sealed partial class XxHash3
{
    public XxHash3 Clone();
}

public sealed partial class XxHash32
{
    public XxHash32 Clone();
}

public sealed partial class XxHash64
{
    public XxHash64 Clone();
}

public sealed partial class XxHash128
{
    public XxHash128 Clone();
}
```
## ConditionalWeakTable<TKey,TValue>.Remove overload

**Approved** | [#runtime/111925](https://github.com/dotnet/runtime/issues/111925#issuecomment-2634850673) | [Video](https://www.youtube.com/watch?v=0v4cLS4stVk&t=1h8m44s)

* Looks good as proposed
* We raised the question of should it be "TryRemove" instead of "Remove", but left it as "Remove" to be consistent with the same overload pair on Dictionary`2.

```C#
namespace System.Runtime.CompilerServices;

public sealed partial class ConditionalWeakTable<TKey, TValue>
{
    public bool Remove(TKey key, [MaybeNullWhen(false)] out TValue value);
}
```
## DefaultInterpolatedStringHandler - expose buffer and reset

**Approved** | [#runtime/110505](https://github.com/dotnet/runtime/issues/110505#issuecomment-2634884748) | [Video](https://www.youtube.com/watch?v=0v4cLS4stVk&t=1h13m34s)

* We discussed what might happen if we ever changed the implementation to use a paging strategy, and feel that we can make this work.
* Looks good as proposed.

```c#
namespace System.Runtime.CompilerServices;

public ref struct DefaultInterpolatedStringHandler
{
    public void Clear();
    public ReadOnlySpan<char> Text { get; }
}
```

## Add `allows ref struct` to ConfiguredCancelableAsyncEnumerable

**Approved** | [#runtime/112007](https://github.com/dotnet/runtime/issues/112007#issuecomment-2634891862) | [Video](https://www.youtube.com/watch?v=0v4cLS4stVk&t=1h26m46s)

* Should we also do ToBlockingEnumerable at the same time?  No, it's implemented as an iterator, and therefore cannot employ that constraint.
* Looks good as proposed.

```diff
namespace System.Threading.Tasks
{
    public static class TaskAsyncEnumerableExtensions
    {
-       public static ConfiguredCancelableAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> source, bool continueOnCapturedContext);
+       public static ConfiguredCancelableAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> source, bool continueOnCapturedContext) where T : allows ref struct;
-       public static ConfiguredCancelableAsyncEnumerable<T> WithCancellation<T>(this IAsyncEnumerable<T> source, CancellationToken cancellationToken);
+       public static ConfiguredCancelableAsyncEnumerable<T> WithCancellation<T>(this IAsyncEnumerable<T> source, CancellationToken cancellationToken) where T : allows ref struct;
}

namespace System.Runtime.CompilerServices
{
-    public readonly struct ConfiguredCancelableAsyncEnumerable<T>;
+    public readonly struct ConfiguredCancelableAsyncEnumerable<T> where T : allows ref struct;
}
```
