# API Review 04/30/2024

## Tensor<T> and supporting types

**NeedsWork** | [#runtime/100924](https://github.com/dotnet/runtime/issues/100924#issuecomment-2086731662) | [Video](https://www.youtube.com/watch?v=AuqUmrNwSLA&t=0h0m0s)

* After a very long debate on names, we ended up with NativeIndex => NIndex and NativeRange => NRange
* After a similarly long debate, we think SpanND => TensorSpan
* NIndex/NRange go to System.Buffers
* NIndex/NRange are probably now considered complete
* For pairing To-prefixed methods with explicit conversions, the type-only name (ToIndex) means `checked`, and the alternative named one gets the suffix -Unchecked (ToIndexUnchecked)
* TensorSpan probably goes to System.Numerics.Tensors
* TensorSpan did not get reviewed today other than its name and namespace, so this issue will need a round 3.

```c#
namespace System.Buffers;

public readonly struct NIndex : IEquatable<NIndex>
{
    public NIndex(nint value, bool fromEnd = false);
    public NIndex(Index value);

    public static NIndex End { get; }
    public static NIndex Start { get; }

    public bool IsFromEnd { get; }
    public nint Value { get; }

    public static implicit operator NIndex(nint value);
    public static implicit operator NIndex(Index value);

    public static explicit operator Index(NIndex value);
    public static explicit operator checked Index(NIndex value);

    public static NIndex FromEnd(nint value);
    public static NIndex FromStart(nint value);

    // Should this always do checked or unchecked, how do users get the other?
    public static Index ToIndex();
    public static Index ToIndexUnchecked();

    public nint GetOffset(nint length);

    // IEquatable<NIndex>
    public bool Equals(NIndex other);
}

public readonly struct NRange : IEquatable<NRange>
{
    public NRange(NIndex start, NIndex end);
    public NRange(Range value);

    public static NRange All { get; }

    public NIndex End { get; }
    public NIndex Start { get; }

    public static explicit operator Range(NRange value);
    public static explicit operator checked Range(NRange value);

    public static NRange EndAt(NIndex end);
    public (nint Offset, nint Length) GetOffsetAndLength(nint length);
    public static NRange StartAt(NIndex start);

    // Should this always do checked or unchecked, how do users get the other?
    public static Range ToRange();
    public static Range ToRangeUnchecked();

    // IEquatable<NRange>
    public bool Equals(NRange other);
}
```




```c#
namespace System.Numerics.Tensors;

public ref struct TensorSpan<T>
{
    // Should this be in the shape of the following instead:
    //   public SpanND(void* pointer, nint length, ReadOnlySpan<nint> shape);
    //   public SpanND(void* pointer, nint length, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);

    public SpanND(void* pointer, ReadOnlySpan<nint> shape);
    public SpanND(void* pointer, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);

    public SpanND(Span<T> span);
    public SpanND(Span<T> span, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);

    public SpanND(T[]? array);
    public SpanND(T[]? array, int start, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);
    public SpanND(T[]? array, Index startIndex, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);

    // Should start just be `nint` and represent the linear index?
    public SpanND(Array? array);
    public SpanND(Array? array, ReadOnlySpan<int> start, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);
    public SpanND(Array? array, ReadOnlySpan<Index> startIndex, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);

    // Should we have variants for common ranks (consider 2-5):
    //   public SpanND(T[,]? array);
    //   public SpanND(T[,]? array, ReadOnlySpan<int> start, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);
    //   public SpanND(T[,]? array, ReadOnlySpan<Index> startIndex, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);

    public static SpanND<T> Empty { get; }

    public bool IsEmpty { get; }
    public nint Length { get; }
    public int Rank { get; }
    public ReadOnlySpan<nint> Shape { get; }
    public ReadOnlySpan<nint> Strides { get; }

    public ref T this[params scoped ReadOnlySpan<nint> indices] { get; }
    public ref T this[params scoped ReadOnlySpan<NativeIndex> indices] { get; }
    public SpanND<T> this[params scoped ReadOnlySpan<NativeRange> ranges] { get; set; }

    // These work like Span<T>, comparing the byref, shape, and strides, not element-wise
    public static bool operator ==(SpanND<T> left, SpanND<T> right);
    public static bool operator !=(SpanND<T> left, SpanND<T> right);

    public static explicit operator SpanND<T>(Array? array);
    public static implicit operator SpanND<T>(T[]? array);

    // Should we have variants for common ranks (consider 2-5):
    //   public static implicit operator SpanND<T>(T[,]? array);

    public static implicit operator ReadOnlySpanND<T>(SpanND<T> span);

    public void Clear();
    public void CopyTo(SpanND<T> destination);
    public void Fill(T value);
    public void FlattenTo(SpanND<T> destination);
    public Enumerator GetEnumerator();
    public ref T GetPinnableReference();
    public SpanND<T> Slice(params ReadOnlySpan<NativeIndex> indices);
    public SpanND<T> Slice(params ReadOnlySpan<NativeRange> ranges);
    public bool TryCopyTo(SpanND<T> destination);
    public bool TryFlattenTo(SpanND<T> destination);

    [ObsoleteAttribute("Equals() on SpanND will always throw an exception. Use the equality operator instead.")]
    public override bool Equals(object? obj);

    [ObsoleteAttribute("GetHashCode() on SpanND will always throw an exception.")]
    public override int GetHashCode();

    // Do we want `Array ToArray()` to mirror Span<T>?

    public ref struct Enumerator
    {
        public ref readonly T Current { get; }
        public bool MoveNext();
    }
}

public ref struct ReadOnlySpanND<T>
{
    // Should this be in the shape of the following instead:
    //   public ReadOnlySpanND(void* pointer, nint length, ReadOnlySpan<nint> shape);
    //   public ReadOnlySpanND(void* pointer, nint length, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);

    public ReadOnlySpanND(void* pointer, ReadOnlySpan<nint> shape);
    public ReadOnlySpanND(void* pointer, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);

    public ReadOnlySpanND(T[]? array);
    public ReadOnlySpanND(T[]? array, int start, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);
    public ReadOnlySpanND(T[]? array, Index startIndex, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);

    // Should start just be `nint` and represent the linear index?
    public ReadOnlySpanND(Array? array);
    public ReadOnlySpanND(Array? array, ReadOnlySpan<int> start, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);
    public ReadOnlySpanND(Array? array, ReadOnlySpan<Index> startIndices, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);

    // Should we have variants for common ranks (consider 2-5):
    //   public ReadOnlySpanND(T[,]? array);
    //   public ReadOnlySpanND(T[,]? array, ReadOnlySpan<int> start, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);
    //   public ReadOnlySpanND(T[,]? array, ReadOnlySpan<Index> startIndices, ReadOnlySpan<nint> shape, ReadOnlySpan<nint> strides);

    public static ReadOnlySpanND<T> Empty { get; }

    public bool IsEmpty { get; }
    public nint Length { get; }
    public int Rank { get; }
    public ReadOnlySpan<nint> Shape { get; }
    public ReadOnlySpan<nint> Strides { get; }

    public ref readonly T this[params scoped ReadOnlySpan<nint> indices] { get; }
    public ref readonly T this[params scoped ReadOnlySpan<NativeIndex> indices] { get; }
    public ReadOnlySpanND<T> this[params scoped ReadOnlySpan<NativeRange> ranges] { get; set; }

    // These work like ReadOnlySpan<T>, comparing the byref, shape, and strides, not element-wise
    public static bool operator ==(ReadOnlySpanND<T> left, ReadOnlySpanND<T> right);
    public static bool operator !=(ReadOnlySpanND<T> left, ReadOnlySpanND<T> right);

    public static explicit operator ReadOnlySpanND<T>(Array? array);
    public static implicit operator ReadOnlySpanND<T>(T[]? array);

    // Should we have variants for common ranks (consider 2-5):
    //   public static implicit operator ReadOnlySpanND<T>(T[,]? array);

    public void CopyTo(ReadOnlySpanND<T> destination);
    public void FlattenTo(ReadOnlySpanND<T> destination);
    public Enumerator GetEnumerator();
    public ref readonly T GetPinnableReference();
    public ReadOnlySpanND<T> Slice(params ReadOnlySpan<NativeIndex> indices);
    public ReadOnlySpanND<T> Slice(params ReadOnlySpan<NativeRange> ranges);
    public bool TryCopyTo(SpanND<T> destination);
    public bool TryFlattenTo(SpanND<T> destination);

    [ObsoleteAttribute("Equals() on ReadOnlySpanND will always throw an exception. Use the equality operator instead.")]
    public override bool Equals(object? obj);

    [ObsoleteAttribute("GetHashCode() on ReadOnlySpanND will always throw an exception.")]
    public override int GetHashCode();

    // Do we want `Array ToArray()` to mirror Span<T>?

    public ref struct Enumerator
    {
        public ref readonly T Current { get; }
        public bool MoveNext();
    }
}

```

