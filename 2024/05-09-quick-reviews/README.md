# API Review 05/09/2024

## Tensor<T> and supporting types

**Approved** | [#runtime/100924](https://github.com/dotnet/runtime/issues/100924#issuecomment-2103257232)


* TensorSpan: `Shape` => `Lengths`
* TensorSpan: `Length` => `FlattenedLength`
* TensorSpan..ctor: `(void* pointer, ...)` => `(T* data, nint dataLength, ...)` to apply a total range limit on the pointer (effectively being `BiggerSpan<T>`, but flattened out)
* `indices` => `indexes`, to match FDG
* All of the mdArray overloads are cut for now
* ToArray is cut for now
* We had a debate around the defaulted parameters on ITensor.Create
  * whether pinned needed to be that high of a concept: yes
  * whether we wanted to violate the guideline of not defaulting parameters in interfaces: yes, because statics in interfaces are different from "interface methods".

```c#
namespace System.Buffers
{
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

        public static Range ToRange();
        public static Range ToRangeUnchecked();

        // IEquatable<NRange>
        public bool Equals(NRange other);
    }
}

namespace System.Numerics.Tensors
{
    public ref struct TensorSpan<T>
    {
        // Should this be in the shape of the following instead:
        //   public TensorSpan(void* pointer, nint length, ReadOnlySpan<nint> lengths);
        //   public TensorSpan(void* pointer, nint length, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);

        public TensorSpan(T* data, nint dataLength, ReadOnlySpan<nint> lengths);
        public TensorSpan(T* data, nint dataLength, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);

        public TensorSpan(Span<T> span);
        public TensorSpan(Span<T> span, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);

        public TensorSpan(T[]? array);
        public TensorSpan(T[]? array, int start, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);
        public TensorSpan(T[]? array, Index startIndex, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);

        // Should start just be `nint` and represent the linear index?
        public TensorSpan(Array? array);
        public TensorSpan(Array? array, ReadOnlySpan<int> start, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);
        public TensorSpan(Array? array, ReadOnlySpan<Index> startIndex, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);

        public static TensorSpan<T> Empty { get; }

        public bool IsEmpty { get; }
        public nint FlattenedLength { get; }
        public int Rank { get; }
        public ReadOnlySpan<nint> Lengths { get; }
        public ReadOnlySpan<nint> Strides { get; }

        public ref T this[params scoped ReadOnlySpan<nint> indexes] { get; }
        public ref T this[params scoped ReadOnlySpan<NIndex> indexes] { get; }
        public TensorSpan<T> this[params scoped ReadOnlySpan<NRange> ranges] { get; set; }

        public static bool operator ==(TensorSpan<T> left, TensorSpan<T> right);
        public static bool operator !=(TensorSpan<T> left, TensorSpan<T> right);

        public static explicit operator TensorSpan<T>(Array? array);
        public static implicit operator TensorSpan<T>(T[]? array);

        public static implicit operator ReadOnlyTensorSpan<T>(TensorSpan<T> span);

        public void Clear();
        public void CopyTo(TensorSpan<T> destination);
        public void Fill(T value);
        public void FlattenTo(Span<T> destination);
        public Enumerator GetEnumerator();
        public ref T GetPinnableReference();
        public TensorSpan<T> Slice(params ReadOnlySpan<NIndex> indexes);
        public TensorSpan<T> Slice(params ReadOnlySpan<NRange> ranges);
        public bool TryCopyTo(TensorSpan<T> destination);
        public bool TryFlattenTo(Span<T> destination);

        [EditorBrowsable(EditorBrowsableState.Never)]
        [ObsoleteAttribute("Equals() on TensorSpan will always throw an exception. Use the equality operator instead.")]
        public override bool Equals(object? obj);

        [EditorBrowsable(EditorBrowsableState.Never)]
        [ObsoleteAttribute("GetHashCode() on TensorSpan will always throw an exception.")]
        public override int GetHashCode();

        public ref struct Enumerator
        {
            public ref readonly T Current { get; }
            public bool MoveNext();
        }
    }

    public ref struct ReadOnlyTensorSpan<T>
    {
        public ReadOnlyTensorSpan(T* data, nint dataLength, ReadOnlySpan<nint> lengths);
        public ReadOnlyTensorSpan(T* data, nint dataLength, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);

        public ReadOnlyTensorSpan(ReadOnlySpan<T> span);
        public ReadOnlyTensorSpan(ReadOnlySpan<T> span, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);

        public ReadOnlyTensorSpan(T[]? array);
        public ReadOnlyTensorSpan(T[]? array, int start, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);
        public ReadOnlyTensorSpan(T[]? array, Index startIndex, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);

        public ReadOnlyTensorSpan(Array? array);
        public ReadOnlyTensorSpan(Array? array, ReadOnlySpan<int> start, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);
        public ReadOnlyTensorSpan(Array? array, ReadOnlySpan<Index> startIndexes, ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides);

        public static ReadOnlyTensorSpan<T> Empty { get; }

        public bool IsEmpty { get; }
        public nint FlattenedLength { get; }
        public int Rank { get; }
        public ReadOnlySpan<nint> Lengths { get; }
        public ReadOnlySpan<nint> Strides { get; }

        public ref readonly T this[params scoped ReadOnlySpan<nint> indexes] { get; }
        public ref readonly T this[params scoped ReadOnlySpan<NIndex> indexes] { get; }
        public ReadOnlyTensorSpan<T> this[params scoped ReadOnlySpan<NRange> ranges] { get; }

        public static bool operator ==(ReadOnlyTensorSpan<T> left, ReadOnlyTensorSpan<T> right);
        public static bool operator !=(ReadOnlyTensorSpan<T> left, ReadOnlyTensorSpan<T> right);

        public static explicit operator ReadOnlyTensorSpan<T>(Array? array);
        public static implicit operator ReadOnlyTensorSpan<T>(T[]? array);

        public void CopyTo(TensorSpan<T> destination);
        public void FlattenTo(Span<T> destination);
        public Enumerator GetEnumerator();
        public ref readonly T GetPinnableReference();
        public ReadOnlyTensorSpan<T> Slice(params ReadOnlySpan<NIndex> indexes);
        public ReadOnlyTensorSpan<T> Slice(params ReadOnlySpan<NRange> ranges);
        public bool TryCopyTo(TensorSpan<T> destination);
        public bool TryFlattenTo(Span<T> destination);

        [EditorBrowsable(EditorBrowsableState.Never)]
        [ObsoleteAttribute("Equals() on ReadOnlyTensorSpan will always throw an exception. Use the equality operator instead.")]
        public override bool Equals(object? obj);

        [EditorBrowsable(EditorBrowsableState.Never)]
        [ObsoleteAttribute("GetHashCode() on ReadOnlyTensorSpan will always throw an exception.")]
        public override int GetHashCode();

        public ref struct Enumerator
        {
            public ref readonly T Current { get; }
            public bool MoveNext();
        }
    }

    public interface IReadOnlyTensor<TSelf, T> : IEnumerable<T>
        where TSelf : IReadOnlyTensor<TSelf, T>
    {
        // Should there be APIs for creating from an array, TensorSpan, etc

        static abstract TSelf Empty { get; }

        bool IsEmpty { get; }
        bool IsPinned { get; }
        nint Length { get; }
        int Rank { get; }

        T this[params ReadOnlySpan<nint> indexes] { get; }
        T this[params ReadOnlySpan<NIndex> indexes] { get; }
        TSelf this[params scoped ReadOnlySpan<NRange> ranges] { get; }

        ReadOnlyTensorSpan<T> AsReadOnlyTensorSpan();
        ReadOnlyTensorSpan<T> AsReadOnlyTensorSpan(params scoped ReadOnlySpan<nint> start);
        ReadOnlyTensorSpan<T> AsReadOnlyTensorSpan(params scoped ReadOnlySpan<NIndex> startIndex);
        ReadOnlyTensorSpan<T> AsReadOnlyTensorSpan(params scoped ReadOnlySpan<NRange> range);

        void CopyTo(TensorSpan<T> destination);
        void FlattenTo(Span<T> destination);

        // These are not properties so that structs can implement the interface without allocating:
        void GetLengths(Span<nint> destination);
        void GetStrides(Span<nint> destination);

        ref readonly T GetPinnableReference();
        TSelf Slice(params scoped ReadOnlySpan<nint> start);
        TSelf Slice(params scoped ReadOnlySpan<NIndex> startIndex);
        TSelf Slice(params scoped ReadOnlySpan<NRange> range);
        bool TryCopyTo(TensorSpan<T> destination);
        bool TryFlattenTo(Span<T> destination);
    }

    public interface ITensor<TSelf, T> : IReadOnlyTensor<TSelf, T>
        where TSelf : ITensor<TSelf, T>
    {
        static TSelf Create(ReadOnlySpan<nint> lengths, bool pinned = false);
        static TSelf Create(ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides, bool pinned = false);

        static TSelf CreateUninitialized(ReadOnlySpan<nint> lengths, bool pinned = false);
        static TSelf CreateUninitialized(ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides, bool pinned = false);

        bool IsReadOnly { get; }

        new T this[params ReadOnlySpan<nint> indexes] { get; set; }
        new T this[params ReadOnlySpan<NIndex> indexes] { get; set; }
        new TSelf this[params scoped ReadOnlySpan<NRange> ranges] { get; set; }

        TensorSpan<T> AsTensorSpan();
        TensorSpan<T> AsTensorSpan(params scoped ReadOnlySpan<nint> start);
        TensorSpan<T> AsTensorSpan(params scoped ReadOnlySpan<NIndex> startIndex);
        TensorSpan<T> AsTensorSpan(params scoped ReadOnlySpan<NRange> range);

        void Clear();
        void Fill(T value);
        new ref T GetPinnableReference();
    }

    public sealed class Tensor<T> : ITensor<Tensor<T>, T>
    {
        static TSelf ITensor<T>.Create(ReadOnlySpan<nint> lengths, bool pinned);
        static TSelf ITensor<T>.Create(ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides, bool pinned);

        static TSelf ITensor<T>.CreateUninitialized(ReadOnlySpan<nint> lengths, bool pinned);
        static TSelf ITensor<T>.CreateUninitialized(ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides, bool pinned);

        public static Tensor<T> Empty { get; }

        public bool IsEmpty { get; }
        public bool IsPinned { get; }
        public nint Length { get; }
        public int Rank { get; }
        public ReadOnlySpan<nint> Lengths { get; }
        public ReadOnlySpan<nint> Strides { get; }

        bool ITensor<T>.IsReadOnly { get; }

        public ref T this[params ReadOnlySpan<nint> indexes] { get; }
        public ref T this[params ReadOnlySpan<NIndex> indexes] { get; }
        public Tensor<T> this[params ReadOnlySpan<NRange> ranges] { get; set; }

        T IReadOnlyTensor<T>.this[params ReadOnlySpan<nint> indexes] { get; }
        T IReadOnlyTensor<T>.this[params ReadOnlySpan<NIndex> indexes] { get; }
        Tensor<T> IReadOnlyTensor<T>.this[params ReadOnlySpan<NRange> ranges] { get; set; }

        public static implicit operator TensorSpan<T>(Tensor<T> value);
        public static implicit operator ReadOnlyTensorSpan<T>(Tensor<T> value);

        public TensorSpan<T> AsTensorSpan();
        public TensorSpan<T> AsTensorSpan(params ReadOnlySpan<nint> start);
        public TensorSpan<T> AsTensorSpan(params ReadOnlySpan<NIndex> startIndex);
        public TensorSpan<T> AsTensorSpan(params ReadOnlySpan<NRange> ranges);

        public ReadOnlyTensorSpan<T> AsReadOnlyTensorSpan();
        public ReadOnlyTensorSpan<T> AsReadOnlyTensorSpan(params ReadOnlySpan<nint> start);
        public ReadOnlyTensorSpan<T> AsReadOnlyTensorSpan(params ReadOnlySpan<NIndex> startIndex);
        public ReadOnlyTensorSpan<T> AsReadOnlyTensorSpan(params ReadOnlySpan<NRange> ranges);

        public void Clear();
        public void CopyTo(TensorSpan<T> destination);
        public void FlattenTo(TensorSpan<T> destination);
        public void Fill(T value);
        public Enumerator GetEnumerator();
        public ref T GetPinnableReference();
        public Tensor<T> Slice(params ReadOnlySpan<nint> start);
        public Tensor<T> Slice(params ReadOnlySpan<NIndex> startIndexes);
        public Tensor<T> Slice(params ReadOnlySpan<NRange> ranges);
        public bool TryCopyTo(TensorSpan<T> destination);
        public bool TryFlattenTo(TensorSpan<T> destination);

        void IReadOnlyTensor<T>.GetLengths(Span<nint> destination);
        void IReadOnlyTensor<T>.GetStrides(Span<nint> destination);
        ref readonly T IReadOnlyTensor<T>.GetPinnableReference();

        // The behavior of Equals, GetHashCode, and ToString needs to be determined

        // For ToString, is the following sufficient to help mitigate potential issues:
        //   public string ToString(params ReadOnlySpan<nint> maximumLengths);

        public ref struct Enumerator
        {
            public ref readonly T Current { get; }
            public bool MoveNext();
        }
    }

    public static partial class Tensor
    {
        public static Tensor<T> Create<T>(ReadOnlySpan<nint> lengths, bool pinned = false);
        public static Tensor<T> Create<T>(ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides, bool pinned = false);

        public static Tensor<T> CreateUninitialized<T>(ReadOnlySpan<nint> lengths, bool pinned = false);
        public static Tensor<T> CreateUninitialized<T>(ReadOnlySpan<nint> lengths, ReadOnlySpan<nint> strides, bool pinned = false);
    }
}
```
