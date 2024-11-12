# API Review 11/12/2024

## Apply [OverloadResolutionPriority] to Span-based overloads

**Approved** | [#runtime/109549](https://github.com/dotnet/runtime/issues/109549#issuecomment-2471278847) | [Video](https://www.youtube.com/watch?v=ehUiLj2KvCg&t=0h0m0s)

Looks good as proposed

```diff
namespace System
{
    public static partial class MemoryExtensions
    {
+       [OverloadResolutionPriority(-1)]
        public static int BinarySearch<T>(this System.Span<T> span, System.IComparable<T> comparable) { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int BinarySearch<T, TComparer>(this System.Span<T> span, T value, TComparer comparer) where TComparer : System.Collections.Generic.IComparer<T>, allows ref struct { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int BinarySearch<T, TComparable>(this System.Span<T> span, TComparable comparable) where TComparable : System.IComparable<T>, allows ref struct { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int CommonPrefixLength<T>(this System.Span<T> span, System.ReadOnlySpan<T> other) { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int CommonPrefixLength<T>(this System.Span<T> span, System.ReadOnlySpan<T> other, System.Collections.Generic.IEqualityComparer<T>? comparer) { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool Contains<T>(this System.Span<T> span, T value) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool ContainsAny(this System.Span<char> span, System.Buffers.SearchValues<string> values) { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool ContainsAny<T>(this System.Span<T> span, System.Buffers.SearchValues<T> values) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool ContainsAny<T>(this System.Span<T> span, System.ReadOnlySpan<T> values) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool ContainsAny<T>(this System.Span<T> span, T value0, T value1) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool ContainsAny<T>(this System.Span<T> span, T value0, T value1, T value2) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool ContainsAnyExcept<T>(this System.Span<T> span, System.Buffers.SearchValues<T> values) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool ContainsAnyExcept<T>(this System.Span<T> span, System.ReadOnlySpan<T> values) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool ContainsAnyExcept<T>(this System.Span<T> span, T value) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool ContainsAnyExcept<T>(this System.Span<T> span, T value0, T value1) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool ContainsAnyExcept<T>(this System.Span<T> span, T value0, T value1, T value2) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool ContainsAnyExceptInRange<T>(this System.Span<T> span, T lowInclusive, T highInclusive) where T : System.IComparable<T> { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool ContainsAnyInRange<T>(this System.Span<T> span, T lowInclusive, T highInclusive) where T : System.IComparable<T> { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int Count<T>(this System.Span<T> span, T value) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int Count<T>(this System.Span<T> span, System.ReadOnlySpan<T> value) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool EndsWith<T>(this System.Span<T> span, System.ReadOnlySpan<T> value) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static System.Text.SpanLineEnumerator EnumerateLines(this System.Span<char> span) { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static System.Text.SpanRuneEnumerator EnumerateRunes(this System.Span<char> span) { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int IndexOfAny(this System.Span<char> span, System.Buffers.SearchValues<string> values) { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int IndexOfAny<T>(this System.Span<T> span, System.Buffers.SearchValues<T> values) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int IndexOfAny<T>(this System.Span<T> span, System.ReadOnlySpan<T> values) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int IndexOfAny<T>(this System.Span<T> span, T value0, T value1) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int IndexOfAny<T>(this System.Span<T> span, T value0, T value1, T value2) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int IndexOfAnyExcept<T>(this System.Span<T> span, T value) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int IndexOfAnyExcept<T>(this System.Span<T> span, T value0, T value1) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int IndexOfAnyExcept<T>(this System.Span<T> span, T value0, T value1, T value2) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int IndexOfAnyExcept<T>(this System.Span<T> span, System.Buffers.SearchValues<T> values) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int IndexOfAnyExcept<T>(this System.Span<T> span, System.ReadOnlySpan<T> values) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int IndexOfAnyExceptInRange<T>(this System.Span<T> span, T lowInclusive, T highInclusive) where T : System.IComparable<T> { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int IndexOf<T>(this System.Span<T> span, System.ReadOnlySpan<T> value) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int IndexOf<T>(this System.Span<T> span, T value) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int IndexOfAnyInRange<T>(this System.Span<T> span, T lowInclusive, T highInclusive) where T : System.IComparable<T> { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int LastIndexOfAny<T>(this System.Span<T> span, System.Buffers.SearchValues<T> values) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int LastIndexOfAny<T>(this System.Span<T> span, System.ReadOnlySpan<T> values) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int LastIndexOfAny<T>(this System.Span<T> span, T value0, T value1) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int LastIndexOfAny<T>(this System.Span<T> span, T value0, T value1, T value2) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int LastIndexOfAnyExcept<T>(this System.Span<T> span, T value) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int LastIndexOfAnyExcept<T>(this System.Span<T> span, T value0, T value1) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int LastIndexOfAnyExcept<T>(this System.Span<T> span, T value0, T value1, T value2) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int LastIndexOfAnyExcept<T>(this System.Span<T> span, System.Buffers.SearchValues<T> values) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int LastIndexOfAnyExcept<T>(this System.Span<T> span, System.ReadOnlySpan<T> values) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int LastIndexOfAnyExceptInRange<T>(this System.Span<T> span, T lowInclusive, T highInclusive) where T : System.IComparable<T> { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int LastIndexOf<T>(this System.Span<T> span, System.ReadOnlySpan<T> value) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int LastIndexOf<T>(this System.Span<T> span, T value) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int LastIndexOfAnyInRange<T>(this System.Span<T> span, T lowInclusive, T highInclusive) where T : System.IComparable<T> { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool Overlaps<T>(this System.Span<T> span, System.ReadOnlySpan<T> other) { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool Overlaps<T>(this System.Span<T> span, System.ReadOnlySpan<T> other, out int elementOffset) { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static int SequenceCompareTo<T>(this System.Span<T> span, System.ReadOnlySpan<T> other) where T : System.IComparable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool SequenceEqual<T>(this System.Span<T> span, System.ReadOnlySpan<T> other) where T : System.IEquatable<T>? { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool SequenceEqual<T>(this System.Span<T> span, System.ReadOnlySpan<T> other, System.Collections.Generic.IEqualityComparer<T>? comparer = null) { throw null; }
+       [OverloadResolutionPriority(-1)]
        public static bool StartsWith<T>(this System.Span<T> span, System.ReadOnlySpan<T> value) where T : System.IEquatable<T>? { throw null; }
    }
}

namespace System.Collections.Immutable
{
    public static partial class ImmutableArray
    {
+       [OverloadResolutionPriority(-1)]
        public static ImmutableArray<T> Create<T>(Span<T> items);
+       [OverloadResolutionPriority(-1)]
        public static ImmutableArray<T> ToImmutableArray<T>(this Span<T> items)
    }
}

namespace System.Numerics.Vectors
{
    public readonly struct Vector<T>
    {
+       [OverloadResolutionPriority(-1)]
        public Vector(Span<T> values);
    }
}
```
## `UnsafeAccessorTypeAttribute` for static or private type access

**Approved** | [#runtime/90081](https://github.com/dotnet/runtime/issues/90081#issuecomment-2471324997) | [Video](https://www.youtube.com/watch?v=ehUiLj2KvCg&t=0h24m2s)


* Removed AttributeTargets.Method from the AttributeUsage based on discussion.
* Changed TypeName to get-only per Attribute design guidelines.

```C#
namespace System.Runtime.CompilerServices;

[AttributeUsage(AttributeTargets.Parameter | AttributeTargets.ReturnValue, AllowMultiple = false, Inherited = false)]
public sealed class UnsafeAccessorTypeAttribute : Attribute
{
    /// <summary>
    /// Instantiates an <see cref="UnsafeAccessorTypeAttribute"/> providing access to a type supplied by <paramref name="typeName"/>.
    /// </summary>
    /// <param name="typeName">A fully qualified or partially qualified type name.</param>
    public UnsafeAccessorTypeAttribute(string typeName)
    {
        TypeName = typeName;
    }

    /// <summary>
    /// Fully qualified or partially qualified type name to target.
    /// </summary>
    public string TypeName { get; }
}
```
## MemoryExtensions.*(..., IEqualityComparer) overloads

**Approved** | [#runtime/28934](https://github.com/dotnet/runtime/issues/28934#issuecomment-2471376723) | [Video](https://www.youtube.com/watch?v=ehUiLj2KvCg&t=0h49m26s)

* This is a breaking change for CompareAny(T, T, T) when the last argument is the literal `null`, so we should document that edge case (and the similar ones) as breaking.
* Otherwise, looks good as proposed.

```c#
namespace System;

public static class MemoryExtensions
{
   public static bool Contains<T>(this ReadOnlySpan<T> span, T value, IEqualityComparer<T>? comparer = null);
   public static bool ContainsAny<T>(this ReadOnlySpan<T> span, T value0, T value1, IEqualityComparer<T>? comparer = null);
   public static bool ContainsAny<T>(this ReadOnlySpan<T> span, T value0, T value1, T value2, IEqualityComparer<T>? comparer = null);
   public static bool ContainsAny<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> values, IEqualityComparer<T>? comparer = null);

   public static bool ContainsAnyExcept<T>(this ReadOnlySpan<T> span, T value, IEqualityComparer<T>? comparer = null);
   public static bool ContainsAnyExcept<T>(this ReadOnlySpan<T> span, T value0, T value1, IEqualityComparer<T>? comparer = null);
   public static bool ContainsAnyExcept<T>(this ReadOnlySpan<T> span, T value0, T value1, T value2, IEqualityComparer<T>? comparer = null);
   public static bool ContainsAnyExcept<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> values, IEqualityComparer<T>? comparer = null);

   public static int Count<T>(this ReadOnlySpan<T> span, T value, IEqualityComparer<T>? comparer = null);
   public static int Count<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value, IEqualityComparer<T>? comparer = null);

   public static bool EndsWith<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value, IEqualityComparer<T>? comparer = null);
   public static bool EndsWith<T>(this ReadOnlySpan<T> span, T value, IEqualityComparer<T>? comparer = null);

   public static bool StartsWith<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value, IEqualityComparer<T>? comparer = null);
   public static bool StartsWith<T>(this ReadOnlySpan<T> span, T value, IEqualityComparer<T>? comparer = null);

   public static int IndexOf<T>(this ReadOnlySpan<T> span, T value, IEqualityComparer<T>? comparer = null);
   public static int IndexOfAny<T>(this ReadOnlySpan<T> span, T value0, T value1, IEqualityComparer<T>? comparer = null);
   public static int IndexOfAny<T>(this ReadOnlySpan<T> span, T value0, T value1, T value2, IEqualityComparer<T>? comparer = null);
   public static int IndexOfAny<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> values, IEqualityComparer<T>? comparer = null);

   public static int IndexOf<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value, IEqualityComparer<T>? comparer = null);

   public static int IndexOfAnyExcept<T>(this ReadOnlySpan<T> span, T value, IEqualityComparer<T>? comparer = null);
   public static int IndexOfAnyExcept<T>(this ReadOnlySpan<T> span, T value0, T value1, IEqualityComparer<T>? comparer = null);
   public static int IndexOfAnyExcept<T>(this ReadOnlySpan<T> span, T value0, T value1, T value2, IEqualityComparer<T>? comparer = null);
   public static int IndexOfAnyExcept<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value, IEqualityComparer<T>? comparer = null);

   public static int LastIndexOf<T>(this ReadOnlySpan<T> span, T value, IEqualityComparer<T>? comparer = null);
   public static int LastIndexOfAny<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> values, IEqualityComparer<T>? comparer = null);
   public static int LastIndexOfAny<T>(this ReadOnlySpan<T> span, T value0, T value1, IEqualityComparer<T>? comparer = null);
   public static int LastIndexOfAny<T>(this ReadOnlySpan<T> span, T value0, T value1, T value2, IEqualityComparer<T>? comparer = null);

   public static int LastIndexOf<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value, IEqualityComparer<T>? comparer = null);

   public static int LastIndexOfAnyExcept<T>(this ReadOnlySpan<T> span, T value, IEqualityComparer<T>? comparer = null);
   public static int LastIndexOfAnyExcept<T>(this ReadOnlySpan<T> span, T value0, T value1, IEqualityComparer<T>? comparer = null);
   public static int LastIndexOfAnyExcept<T>(this ReadOnlySpan<T> span, T value0, T value1, T value2, IEqualityComparer<T>? comparer = null);
   public static int LastIndexOfAnyExcept<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> values, IEqualityComparer<T>? comparer = null);

   public static void Replace<T>(this ReadOnlySpan<T> source, Span<T> destination, T oldValue, T newValue, IEqualityComparer<T>? comparer = null);

   public static int SequenceCompareTo<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other, IComparer<T>? comparer = null);
}
```
## Add formatting support to `System.Net.ServerSentEvents`

**Approved** | [#runtime/109294](https://github.com/dotnet/runtime/issues/109294#issuecomment-2471472685) | [Video](https://www.youtube.com/watch?v=ehUiLj2KvCg&t=1h17m59s)


* Removed the custom delegate type in favor of Action
* SseItem.EventType will stay non-nullable even though the ctor now accepts null
* Replaced instance SseFormatter with a static class.

```diff
namespace System.Net.ServerSentEvents;

public readonly struct SseItem<T>
{
-   public SseItem(T data, string eventType);
+   public SseItem(T data, string? eventType);
```

```c#
namespace System.Net.ServerSentEvents;

// Extends the existing type to support writing
public readonly partial struct SseItem<T>
{
    public string? EventId { get; init; }
}

public static class SseFormatter
{
    public static Task WriteAsync(IAsyncEnumerable<SseItem<string>> source, Stream destination, CancellationToken cancellationToken = default);
    public static Task WriteAsync(IAsyncEnumerable<SseItem<T>> source, Stream destination, Action<T, IBufferWriter<byte>> itemFormatter, CancellationToken cancellationToken = default);
}
```
