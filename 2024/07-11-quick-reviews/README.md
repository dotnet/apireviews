# API Review 07/11/2024

## Tensor<T> and supporting types, part 3

**Approved** | [#runtime/104117](https://github.com/dotnet/runtime/issues/104117#issuecomment-2223499466) | [Video](https://www.youtube.com/watch?v=w-rkltxMw-w&t=0h0m0s)


* As with the previous review, "RO" is used in lieu of "ReadOnly" because of GH size limits.
* Mean => Average, for consistency with System.Linq.Enumerable
* We agreed to the pattern of just sticking "Any" and "All" as suffixes on the existing methods, even though "IsFooAll" or "IsFooAny" reads a bit strangely.
  * `public static bool [T::]IsFoo();`
  * `public static VectorLike<bool-like> IsFoo(VectorLike<T> foos);`
  * `public static bool IsFooAny(VectorLike<T> foos);`
  * `public static bool IsFooAll(VectorLike<T> foos);`
* `pinned` vs `isPinned`: `pinned`, to match GC.AllocateArray
* Tensor.Split, `numSplits` `nint` => `int`. (Then got renamed)
* In the tensor class, most of the first parameters got renamed from x to tensor, except for Broadcast, where they're named source.

```C#
namespace System.Numerics.Tensors;

public static partial class Tensor
{
    public static T Average<T>(in ROTensorSpan<T> x) where T : IFloatingPoint<T>;

    public static Tensor<bool> IsPow2(in ROTensorSpan<T> x) where T : IBinaryNumber;
    public static TensorSpan<bool> IsPow2(scoped in ROTensorSpan<T> x, in TensorSpan<bool> destination) where T : IBinaryNumber;
    public static bool IsPow2All(in ROTensorSpan<T> x) where T : IBinaryNumber;
    public static bool IsPow2Any(in ROTensorSpan<T> x) where T : IBinaryNumber;

    public static Tensor<bool> IsCanonical(in ROTensorSpan<T> x) where T : INumberBase;
    public static TensorSpan<bool> IsCanonical(scoped in ROTensorSpan<T> x, in TensorSpan<bool> destination) where T : INumberBase;
    public static bool IsCanonicalAll(in ROTensorSpan<T> x) where T : INumberBase;
    public static bool IsCanonicalAny(in ROTensorSpan<T> x) where T : INumberBase;
    // repeat for:
    // * IsComplexNumber
    // * IsEvenInteger
    // * IsFinite
    // * IsImaginaryNumber
    // * IsInfinity
    // * IsInteger
    // * IsNaN
    // * IsNegative
    // * IsNegativeInfinity
    // * IsNormal
    // * IsOddInteger
    // * IsPositive
    // * IsPositiveInfinity
    // * IsRealNumber
    // * IsSubnormal
    // * IsZero
}

public static partial class TensorPrimitives
{
    public static T Average<T>(ROSpan<T> x) where T : IFloatingPoint<T>;

    public static void IsPow2(scoped in ROSpan<T> x, in Span<bool> destination) where T : IBinaryNumber;
    public static bool IsPow2All(in ROSpan<T> x) where T : IBinaryNumber;
    public static bool IsPow2Any(in ROSpan<T> x) where T : IBinaryNumber;

    public static void IsCanonical(scoped in ROSpan<T> x, in Span<bool> destination) where T : INumberBase;
    public static bool IsCanonicalAll(in ROSpan<T> x) where T : INumberBase;
    public static bool IsCanonicalAny(in ROSpan<T> x) where T : INumberBase;
    // repeat for
    // * IsComplexNumber
    // * IsEvenInteger
    // * IsFinite
    // * IsImaginaryNumber
    // * IsInfinity
    // * IsInteger
    // * IsNaN
    // * IsNegative
    // * IsNegativeInfinity
    // * IsNormal
    // * IsOddInteger
    // * IsPositive
    // * IsPositiveInfinity
    // * IsRealNumber
    // * IsSubnormal
    // * IsZero
}
```

```C#
namespace System.Numerics.Tensors;

public static partial class Tensor
{
    public static ROTensorSpan<T> AsReadOnlyTensorSpan<T>(this T[]? array, params scoped ROSpan<nint> lengths);
    public static TensorSpan<T> AsTensorSpan<T>(this T[]? array, params scoped ROSpan<nint> lengths);

    public static Tensor<T> Broadcast<T>(in ROTensorSpan<T> source, ROSpan<nint> lengths);

    // This gets the lengths from lengthsSource
    public static Tensor<T> Broadcast<T>(in ROTensorSpan<T> source, in ROTensorSpan<T> lengthsSource);

    public static void BroadcastTo<T>(this Tensor<T> source, in TensorSpan<T> destination);
    public static void BroadcastTo<T>(in this TensorSpan<T> source, in TensorSpan<T> destination);
    public static void BroadcastTo<T>(in this ROTensorSpan<T> source, in TensorSpan<T> destination);

    public static Tensor<T> Concatenate<T>(params ROSpan<Tensor<T>> tensors);
    public static Tensor<T> ConcatenateOnDimension<T>(int dimension, params ROSpan<Tensor<T>> tensors);
    public static TensorSpan<T> Concatenate<T>(scoped ROSpan<Tensor<T>> tensors, in TensorSpan<T> destination);
    public static TensorSpan<T> ConcatenateOnDimension<T>(int dimension, scoped ROSpan<Tensor<T>> tensors, in TensorSpan<T> destination);

    public static Tensor<T> Create<T>(ROSpan<nint> lengths, bool pinned = false);
    public static Tensor<T> Create<T>(ROSpan<nint> lengths, ROSpan<nint> strides, bool pinned = false);
    public static Tensor<T> Create<T>(T[] values, ROSpan<nint> lengths, bool pinned = false);
    public static Tensor<T> Create<T>(T[] values, ROSpan<nint> lengths, ROSpan<nint> strides, bool pinned = false);
    public static Tensor<T> Create<T>(IEnumerable<T> values, ROSpan<nint> lengths, bool pinned = false);
    public static Tensor<T> Create<T>(IEnumerable<T> values, ROSpan<nint> lengths, ROSpan<nint> strides, bool pinned = false);

    public static Tensor<T> CreateAndFillGaussianNormalDistribution<T>(params ROSpan<nint> lengths) where T : IFloatingPoint<T>;
    public static Tensor<T> CreateAndFillGaussianNormalDistribution<T>(Random random, params ROSpan<nint> lengths) where T : IFloatingPoint<T>;
    public static Tensor<T> CreateAndFillUniformDistribution<T>(params ROSpan<nint> lengths) where T : IFloatingPoint<T>;
    public static Tensor<T> CreateAndFillUniformDistribution<T>(Random random, params ROSpan<nint> lengths) where T : IFloatingPoint<T>;

    public static Tensor<T> CreateUninitialized<T>(ROSpan<nint> lengths, bool pinned = false);
    public static Tensor<T> CreateUninitialized<T>(ROSpan<nint> lengths, ROSpan<nint> strides, bool pinned = false);

    public static TensorSpan<T> FillGaussianNormalDistribution<T>(in TensorSpan<T> destination, Random? random = null) where T : IFloatingPoint<T>;
    public static TensorSpan<T> FillUniformDistribution<T>(in TensorSpan<T> destination, Random? random = null) where T : IFloatingPoint<T>;

    public static TensorSpan<T> FilteredUpdate<T>(in this TensorSpan<T> tensor, scoped in ROTensorSpan<bool> filter, scoped in ROTensorSpan<T> values);
    public static TensorSpan<T> FilteredUpdate<T>(in this TensorSpan<T> tensor, scoped in ROTensorSpan<bool> filter, T value);

    public static Tensor<T> PermuteDimensions<T>(this Tensor<T> tensor, params ROSpan<int> dimensions);
    public static TensorSpan<T> PermuteDimensions<T>(in this TensorSpan<T> tensor, params scoped ROSpan<int> dimensions);
    public static ROTensorSpan<T> PermuteDimensions<T>(in this ROTensorSpan<T> tensor, params scoped ROSpan<int> dimensions);

    public static Tensor<T> Reshape<T>(this Tensor<T> tensor, params ROSpan<nint> lengths);
    public static TensorSpan<T> Reshape<T>(in this TensorSpan<T> tensor, params scoped ROSpan<nint> lengths);
    public static ROTensorSpan<T> Reshape<T>(in this ROTensorSpan<T> tensor, params scoped ROSpan<nint> lengths);

    public static Tensor<T> Resize<T>(Tensor<T> tensor, ROSpan<nint> lengths);

    public static void ResizeTo<T>(this Tensor<T> tensor, in TensorSpan<T> destination);
    public static void ResizeTo<T>(in this TensorSpan<T> tensor, in TensorSpan<T> destination);
    public static void ResizeTo<T>(in this ROTensorSpan<T> tensor, in TensorSpan<T> destination);

    public static Tensor<T> Reverse<T>(in ROTensorSpan<T> tensor);
    public static Tensor<T> ReverseDimension<T>(in ROTensorSpan<T> tensor, int dimension);
    public static TensorSpan<T> Reverse<T>(scoped in ROTensorSpan<T> tensor, in TensorSpan<T> destination);
    public static TensorSpan<T> ReverseDimension<T>(scoped in ROTensorSpan<T> tensor, int dimension, in TensorSpan<T> destination);

    public static bool SequenceEqual<T>(this in ROTensorSpan<T> tensor, in ROTensorSpan<T> other) where T : IEquatable<T>?;
    public static bool SequenceEqual<T>(this in TensorSpan<T> tensor, in ROTensorSpan<T> other) where T : IEquatable<T>?;

    public static Tensor<T> SetSlice<T>(this Tensor<T> tensor, in ROTensorSpan<T> values, params ROSpan<NRange> ranges);
    public static TensorSpan<T> SetSlice<T>(this TensorSpan<T> tensor, scoped in ROTensorSpan<T> values, params scoped ROSpan<NRange> ranges);

    public static Tensor<T>[] Split<T>(in ROTensorSpan<T> tensor, int splitCount, int dimension);

    public static Tensor<T> Squeeze<T>(this Tensor<T> tensor);
    public static Tensor<T> SqueezeDimension<T>(this Tensor<T> tensor, int dimension);
    public static TensorSpan<T> Squeeze<T>(in this TensorSpan<T> tensor);
    public static ROTensorSpan<T> SqueezeDimension<T>(in this ROTensorSpan<T> tensor, int dimension);

    public static Tensor<T> Stack<T>(params ROSpan<Tensor<T>> tensors);
    public static Tensor<T> StackAlongDimension<T>(int dimension, params ROSpan<Tensor<T>> tensors);
    public static TensorSpan<T> Stack<T>(scoped ROSpan<Tensor<T>> tensors, in TensorSpan<T> destination);
    public static TensorSpan<T> StackAlongDimension<T>(scoped ROSpan<Tensor<T>> tensors, in TensorSpan<T> destination, int dimension);

    public static string ToString<T>(in this Tensor<T> tensor, params ROSpan<nint> maximumLengths);
    public static string ToString<T>(in this ROTensorSpan<T> tensor, params ROSpan<nint> maximumLengths);
    public static string ToString<T>(in this TensorSpan<T> tensor, params ROSpan<nint> maximumLengths);

    public static Tensor<T> Transpose<T>(in ROTensorSpan<T> tensor);
    public static TensorSpan<T> Transpose<T>(scoped in ROTensorSpan<T> tensor, in TensorSpan<T> destination);

    public static bool TryBroadcastTo<T>(this Tensor<T> source, in TensorSpan<T> destination);
    public static bool TryBroadcastTo<T>(in this TensorSpan<T> source, in TensorSpan<T> destination);
    public static bool TryBroadcastTo<T>(in this ROTensorSpan<T> source, in TensorSpan<T> destination);

    public static Tensor<T> Unsqueeze<T>(this Tensor<T> tensor, int dimension);
    public static TensorSpan<T> Unsqueeze<T>(in this TensorSpan<T> tensor, int dimension);
    public static ROTensorSpan<T> Unsqueeze<T>(in this ROTensorSpan<T> tensor, int dimension);
}
```

