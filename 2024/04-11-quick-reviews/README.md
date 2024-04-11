# API Review 04/11/2024

## Add missing Min/MaxNumber generic math APIs on TensorPrimitives

**Approved** | [#runtime/98862](https://github.com/dotnet/runtime/issues/98862#issuecomment-2050150849) | [Video](https://www.youtube.com/watch?v=BKd3mq2XgNk&t=0h0m0s)


* Looks good as proposed

```c#
namespace System.Numerics.Tensors;

public static class TensorPrimitives
{
    // Same surface area as Min/Max{Magnitude}, but for Min/Max{Magnitude}Number
    public static int IndexOfMaxNumber<T>(ReadOnlySpan<T> x) where T : INumber<T>;
    public static int IndexOfMinNumber<T>(ReadOnlySpan<T> x) where T : INumber<T>;
    public static int IndexOfMaxMagnitudeNumber<T>(ReadOnlySpan<T> x) where T : INumber<T>;
    public static int IndexOfMinMagnitudeNumber<T>(ReadOnlySpan<T> x) where T : INumber<T>;

    public static T MaxMagnitudeNumber<T>(ReadOnlySpan<T> x) where T : INumberBase<T>;
    public static void MaxMagnitudeNumber<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, System.Span<T> destination) where T : INumberBase<T>;
    public static void MaxMagnitudeNumber<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : INumberBase<T>;

    public static T MaxNumber<T>(ReadOnlySpan<T> x) where T : INumber<T>;
    public static void MaxNumber<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : INumber<T>;
    public static void MaxNumber<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : INumber<T>;

    public static T MinMagnitudeNumber<T>(ReadOnlySpan<T> x) where T : INumberBase<T>;
    public static void MinMagnitudeNumber<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : INumberBase<T>;
    public static void MinMagnitudeNumber<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : INumberBase<T>;

    public static T MinNumber<T>(ReadOnlySpan<T> x) where T : INumber<T>;
    public static void MinNumber<T>(ReadOnlySpan<T> x, ReadOnlySpan<T> y, Span<T> destination) where T : INumber<T>;
    public static void MinNumber<T>(ReadOnlySpan<T> x, T y, Span<T> destination) where T : INumber<T>;
}
```
## Tensor<T> and supporting types

**NeedsWork** | [#runtime/100924](https://github.com/dotnet/runtime/issues/100924#issuecomment-2050338917) | [Video](https://www.youtube.com/watch?v=BKd3mq2XgNk&t=0h9m54s)


* Added NativeIndex..ctor(Index), NativeIndex.ToIndex, and similar to NativeRange.
* Added a Span-accepting overload to SpanND to mirror the Array one, and similar to ReadOnlySpanND
* We're not sure if System is the right namespace for NativeIndex and NativeRange, but want more people in the room to discuss.
* The name SpanND did not meet with universal love and harmony, needs more people to discuss.
* The namespaces are also something to further discuss
* We added indexers on SpanND/ROSpanND to accept params NativeIndex
* We added (Try)FlattenTo to copy a SpanND into a Span.

```c#
namespace System;

public readonly struct NativeIndex : IEquatable<NativeIndex>
{
    public NativeIndex(nint value, bool fromEnd = false);
    public NativeIndex(Index value);

    public static NativeIndex End { get; }
    public static NativeIndex Start { get; }

    public bool IsFromEnd { get; }
    public nint Value { get; }

    public static implicit operator NativeIndex(nint value);
    public static implicit operator NativeIndex(Index value);

    public static explicit operator Index(NativeIndex value);
    public static explicit operator checked Index(NativeIndex value);

    public Index ToIndex();

    public static NativeIndex FromEnd(nint value);
    public static NativeIndex FromStart(nint value);

    public nint GetOffset(nint length);
}

public readonly struct NativeRange : IEquatable<NativeRange>
{
    public NativeRange(NativeIndex start, NativeIndex end);
    public NativeRange(Range range);

    public static NativeRange All { get; }

    public NativeIndex End { get; }
    public NativeIndex Start { get; }

    public static explicit operator Range(NativeRange value);
    public static explicit operator checked Range(NativeRange value);

    public Range ToRange();

    public static NativeRange EndAt(NativeIndex end);
    public (nint Offset, nint Length) GetOffsetAndLength(nint length);
    public static NativeRange StartAt(NativeIndex start);
}
```

```C#
namespace System;

public readonly ref struct SpanND<T>
{
    public SpanND(void* pointer, ReadOnlySpan<nint> lengths);
    public SpanND(void* pointer, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);

    public SpanND(Span<T> span);
    public SpanND(Span<T> span, ReadOnlySpan<nint> offsets, ReadOnlySpan<nint> lengths);

    public SpanND(Array? array);
    public SpanND(Array? array, ReadOnlySpan<nint> offsets, ReadOnlySpan<nint> lengths);

    // Do we want overloads for common dimensions like `T[,]` and `T[,,]`?

    public static SpanND<T> Empty { get; }

    public bool IsEmpty { get; }
    public ReadOnlySpan<nint> Lengths { get; }
    public nint LinearLength { get; }
    public int Rank { get; }
    public ReadOnlySpan<nint> Strides { get; }

    public ref T this[params ReadOnlySpan<nint> indices] { get; }
    public ref T this[params ReadOnlySpan<NativeIndex> indices] { get; }

    public static bool operator ==(SpanND<T> left, SpanND<T> right);
    public static bool operator !=(SpanND<T> left, SpanND<T> right);

    public static implicit operator SpanND<T>(Array array);
    public static implicit operator SpanND<T>(Span<T> span);
    public static implicit operator ReadOnlySpanND<T>(SpanND<T> span);

    public void Clear();

    public void CopyTo(SpanND<T> destination);
    public bool TryCopyTo(SpanND<T> destination);
    
    public void FlattenTo(Span<T> destination);
    public bool TryFlattenTo(Span<T> destination);
    
    public void Fill(T value);
    public Enumerator GetEnumerator();
    public ref T GetPinnableReference();
    public SpanND<T> Slice(params ReadOnlySpan<NativeIndex> indices);
    public SpanND<T> Slice(params ReadOnlySpan<NativeRange> ranges);

    // Do we want `Array ToArray()` to mirror Span<T>?

    public ref struct Enumerator
    {
        public ref readonly T Current { get; }
        public bool MoveNext();
    }
}

public readonly ref struct ReadOnlySpanND<T>
{
    public ReadOnlySpanND(void* pointer, ReadOnlySpan<nint> lengths);
    public ReadOnlySpanND(void* pointer, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);

    public ReadOnlySpanND(ReadOnlySpan<T> span);
    public ReadOnlySpanND(ReadOnlySpan<T> span, ReadOnlySpan<nint> starts, ReadOnlySpan<nint> lengths);

    public ReadOnlySpanND(Array? array);
    public ReadOnlySpanND(Array? array, ReadOnlySpan<nint> starts, ReadOnlySpan<nint> lengths);

    // Do we want overloads for common dimensions like `T[,]` and `T[,,]`?

    public static ReadOnlySpanND<T> Empty { get; }

    public bool IsEmpty { get; }
    public ReadOnlySpan<nint> Lengths { get; }
    public nint LinearLength { get; }
    public int Rank { get; }
    public ReadOnlySpan<nint> Strides { get; }

    public ref readonly T this[params ReadOnlySpan<nint> indices] { get; }
    public ref readonly T this[params ReadOnlySpan<NativeIndex> indices] { get; }

    public static bool operator ==(ReadOnlySpanND<T> left, ReadOnlySpanND<T> right);
    public static bool operator !=(ReadOnlySpanND<T> left, ReadOnlySpanND<T> right);

    public static implicit operator ReadOnlySpanND<T>(Array array);

    public void CopyTo(SpanND<T> destination);
    public bool TryCopyTo(SpanND<T> destination);

    public void FlattenTo(Span<T> destination);
    public bool TryFlattenTo(Span<T> destination);
    
    public Enumerator GetEnumerator();
    public ref readonly T GetPinnableReference();
    public ReadOnlySpanND<T> Slice(params ReadOnlySpan<NativeIndex> indices);
    public ReadOnlySpanND<T> Slice(params ReadOnlySpan<NativeRange> ranges);

    // Do we want `Array ToArray()` to mirror Span<T>?

    public ref struct Enumerator
    {
        public ref readonly T Current { get; }
        public bool MoveNext();
    }
}
```
