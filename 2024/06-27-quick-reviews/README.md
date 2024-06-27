# API Review 06/27/2024

## Tensor<T> and supporting types, part 2

**Approved** | [#runtime/103611](https://github.com/dotnet/runtime/issues/103611#issuecomment-2195486391) | [Video](https://www.youtube.com/watch?v=LK16qDS6q1A&t=0h0m0s)

* `ref readonly TensorSpan<T> Method<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination)` got changed to `TensorSpan<T> Method<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination)` because the `ref readonly` can't be guaranteed.
* Equals/EqualsAll/EqualsAny should have been IEqualityOperators, not IComparisonOperators
* MultiplyAddEstimate should be INumber, not IFloatingPointIeee754
* The shift operations could allow per-element variance, but it's not currently in demand, so it's being omitted for now.
* Tensor/TensorPrimitives Mean has been removed from this proposal, deferred to a later discussion
* While we already have some `StandardDeviation` functions, and no `StdDev`, we feel like `StdDev` is the better name on these types.
* The pattern for IsPow2 and other Is-* is deferred.  The top contenders seemed to be `Tensor<bool> IsPow2(x)` paired with either `bool AllPow2(x)` or `bool IsPow2All(x)`

Note that the abbreviation "RO" is being used in lieu of "ReadOnly" to reduce total message size (GitHub has per message size limits, who knew?)

```diff
namespace System.Numerics.Tensors
{
    public partial interface IReadOnlyTensor<TSelf, T>
    {
+       [UnscopedRef]
+       ReadOnlySpan<nint> Lengths { get;}

+       [UnscopedRef]
+       ReadOnlySpan<nint> Strides { get;}

-       void GetLengths(scoped Span<nint> destination);
-       void GetStrides(scoped Span<nint> destination);
    }
}
```

```C#
namespace System.Numerics.Tensors;

public static partial class Tensor
{
    // Other

    public static Tensor<T> CosineSimilarity<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IRootFunctions<T>;
    public static Tensor<T> CosineSimilarity<T>(in ROTensorSpan<T> x, T y) where T : IRootFunctions<T>;

    public static TensorSpan<T> CosineSimilarity<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IRootFunctions<T>;
    public static TensorSpan<T> CosineSimilarity<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : IRootFunctions<T>;

    public static T Distance<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IRootFunctions<T>;
    public static T Distance<T>(in ROTensorSpan<T> x, T y) where T : IRootFunctions<T>;
    public static T Distance<T>(T x, in ROTensorSpan<T> y) where T : IRootFunctions<T>;

    public static T Dot<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IAdditionOperators<T, T, T>, IAdditiveIdentity<T, T>, IMultiplicativeIdentity<T, T>, IMultiplyOperators<T, T, T>;
    public static T Dot<T>(in ROTensorSpan<T> x, T y) where T : IAdditionOperators<T, T, T>, IAdditiveIdentity<T, T>, IMultiplicativeIdentity<T, T>, IMultiplyOperators<T, T, T>;

    public static T Norm<T>(in ROTensorSpan<T> x) where T : IRootFunctions<T>;

    public static T Product<T>(in ROTensorSpan<T> x) where T : IMultiplicativeIdentity<T, T>, IMultiplyOperators<T, T, T>;

    public static Tensor<T> Sigmoid<T>(in ROTensorSpan<T> x) where T : IExponentialFunctions<T>;
    public static TensorSpan<T> Sigmoid<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IExponentialFunctions<T>;

    public static Tensor<T> SoftMax<T>(in ROTensorSpan<T> x) where T : IExponentialFunctions<T>;
    public static TensorSpan<T> SoftMax<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IExponentialFunctions<T>;

    public static T Sum<T>(in ROTensorSpan<T> x) where T : IAdditionOperators<T, T, T>, IAdditiveIdentity<T, T>;

    // IAdditionOperators

    public static Tensor<T> Add<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IAdditionOperators<T, T, T>, IAdditiveIdentity<T, T>;
    public static Tensor<T> Add<T>(in ROTensorSpan<T> x, T y) where T : IAdditionOperators<T, T, T>, IAdditiveIdentity<T, T>;

    public static TensorSpan<T> Add<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IAdditionOperators<T, T, T>, IAdditiveIdentity<T, T>;
    public static TensorSpan<T> Add<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : IAdditionOperators<T, T, T>, IAdditiveIdentity<T, T>;

    // IBinaryInteger

    public static Tensor<T> LeadingZeroCount<T>(in ROTensorSpan<T> x) where T : IBinaryInteger<T>;
    public static TensorSpan<T> LeadingZeroCount<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IBinaryInteger<T>;

    public static Tensor<T> PopCount<T>(in ROTensorSpan<T> x) where T : IBinaryInteger<T>;
    public static TensorSpan<T> PopCount<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IBinaryInteger<T>;

    public static Tensor<T> RotateLeft<T>(in ROTensorSpan<T> x, int rotateAmount) where T : IBinaryInteger<T>;
    public static TensorSpan<T> RotateLeft<T>(scoped in ROTensorSpan<T> x, int rotateAmount, in TensorSpan<T> destination) where T : IBinaryInteger<T>;

    public static Tensor<T> RotateRight<T>(in ROTensorSpan<T> x, int rotateAmount) where T : IBinaryInteger<T>;
    public static TensorSpan<T> RotateRight<T>(scoped in ROTensorSpan<T> x, int rotateAmount, in TensorSpan<T> destination) where T : IBinaryInteger<T>;

    public static Tensor<T> TrailingZeroCount<T>(in ROTensorSpan<T> x) where T : IBinaryInteger<T>;
    public static TensorSpan<T> TrailingZeroCount<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IBinaryInteger<T>;

    // IBitwiseOperators

    public static Tensor<T> BitwiseAnd<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IBitwiseOperators<T, T, T>;
    public static Tensor<T> BitwiseAnd<T>(in ROTensorSpan<T> x, T y) where T : IBitwiseOperators<T, T, T>;

    public static TensorSpan<T> BitwiseAnd<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IBitwiseOperators<T, T, T>;
    public static TensorSpan<T> BitwiseAnd<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : IBitwiseOperators<T, T, T>;

    public static Tensor<T> BitwiseOr<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IBitwiseOperators<T, T, T>;
    public static Tensor<T> BitwiseOr<T>(in ROTensorSpan<T> x, T y) where T : IBitwiseOperators<T, T, T>;

    public static TensorSpan<T> BitwiseOr<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IBitwiseOperators<T, T, T>;
    public static TensorSpan<T> BitwiseOr<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : IBitwiseOperators<T, T, T>;

    public static Tensor<T> OnesComplement<T>(in ROTensorSpan<T> x) where T : IBitwiseOperators<T, T, T>;
    public static TensorSpan<T> OnesComplement<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IBitwiseOperators<T, T, T>;

    public static Tensor<T> Xor<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IBitwiseOperators<T, T, T>;
    public static Tensor<T> Xor<T>(in ROTensorSpan<T> x, T y) where T : IBitwiseOperators<T, T, T>;

    public static TensorSpan<T> Xor<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IBitwiseOperators<T, T, T>;
    public static TensorSpan<T> Xor<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : IBitwiseOperators<T, T, T>;

    // IComparisonOperators

    public static Tensor<bool> GreaterThan<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;
    public static Tensor<bool> GreaterThan<T>(in ROTensorSpan<T> x, T y) where T : IComparisonOperators<T, T, bool>;
    public static Tensor<bool> GreaterThan<T>(T x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;

    public static TensorSpan<bool> GreaterThan<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<bool> destination) where T : IComparisonOperators<T, T, bool>;
    public static TensorSpan<bool> GreaterThan<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<bool> destination) where T : IComparisonOperators<T, T, bool>;
    public static TensorSpan<bool> GreaterThan<T>(T x, scoped in ROTensorSpan<T> y, in TensorSpan<bool> destination) where T : IComparisonOperators<T, T, bool>;

    public static bool GreaterThanAll<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;
    public static bool GreaterThanAll<T>(in ROTensorSpan<T> x, T y) where T : IComparisonOperators<T, T, bool>;
    public static bool GreaterThanAll<T>(T x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;

    public static bool GreaterThanAny<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;
    public static bool GreaterThanAny<T>(in ROTensorSpan<T> x, T y) where T : IComparisonOperators<T, T, bool>;
    public static bool GreaterThanAny<T>(T x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;

    public static Tensor<bool> GreaterThanOrEqual<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;
    public static Tensor<bool> GreaterThanOrEqual<T>(in ROTensorSpan<T> x, T y) where T : IComparisonOperators<T, T, bool>;
    public static Tensor<bool> GreaterThanOrEqual<T>(T x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;

    public static TensorSpan<bool> GreaterThanOrEqual<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<bool> destination) where T : IComparisonOperators<T, T, bool>;
    public static TensorSpan<bool> GreaterThanOrEqual<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<bool> destination) where T : IComparisonOperators<T, T, bool>;
    public static TensorSpan<bool> GreaterThanOrEqual<T>(T x, scoped in ROTensorSpan<T> y, in TensorSpan<bool> destination) where T : IComparisonOperators<T, T, bool>;

    public static bool GreaterThanOrEqualAll<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;
    public static bool GreaterThanOrEqualAll<T>(in ROTensorSpan<T> x, T y) where T : IComparisonOperators<T, T, bool>;
    public static bool GreaterThanOrEqualAll<T>(T x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;

    public static bool GreaterThanOrEqualAny<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;
    public static bool GreaterThanOrEqualAny<T>(in ROTensorSpan<T> x, T y) where T : IComparisonOperators<T, T, bool>;
    public static bool GreaterThanOrEqualAny<T>(T x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;

    public static Tensor<bool> LessThan<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;
    public static Tensor<bool> LessThan<T>(in ROTensorSpan<T> x, T y) where T : IComparisonOperators<T, T, bool>;
    public static Tensor<bool> LessThan<T>(T x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;

    public static TensorSpan<bool> LessThan<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<bool> destination) where T : IComparisonOperators<T, T, bool>;
    public static TensorSpan<bool> LessThan<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<bool> destination) where T : IComparisonOperators<T, T, bool>;
    public static TensorSpan<bool> LessThan<T>(T x, scoped in ROTensorSpan<T> y, in TensorSpan<bool> destination) where T : IComparisonOperators<T, T, bool>;

    public static bool LessThanAll<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;
    public static bool LessThanAll<T>(in ROTensorSpan<T> x, T y) where T : IComparisonOperators<T, T, bool>;
    public static bool LessThanAll<T>(T x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;

    public static bool LessThanAny<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;
    public static bool LessThanAny<T>(in ROTensorSpan<T> x, T y) where T : IComparisonOperators<T, T, bool>;
    public static bool LessThanAny<T>(T x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;

    public static Tensor<bool> LessThanOrEqual<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;
    public static Tensor<bool> LessThanOrEqual<T>(in ROTensorSpan<T> x, T y) where T : IComparisonOperators<T, T, bool>;
    public static Tensor<bool> LessThanOrEqual<T>(T x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;

    public static TensorSpan<bool> LessThanOrEqual<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<bool> destination) where T : IComparisonOperators<T, T, bool>;
    public static TensorSpan<bool> LessThanOrEqual<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<bool> destination) where T : IComparisonOperators<T, T, bool>;
    public static TensorSpan<bool> LessThanOrEqual<T>(T x, scoped in ROTensorSpan<T> y, in TensorSpan<bool> destination) where T : IComparisonOperators<T, T, bool>;

    public static bool LessThanOrEqualAll<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;
    public static bool LessThanOrEqualAll<T>(in ROTensorSpan<T> x, T y) where T : IComparisonOperators<T, T, bool>;
    public static bool LessThanOrEqualAll<T>(T x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;

    public static bool LessThanOrEqualAny<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;
    public static bool LessThanOrEqualAny<T>(in ROTensorSpan<T> x, T y) where T : IComparisonOperators<T, T, bool>;
    public static bool LessThanOrEqualAny<T>(T x, in ROTensorSpan<T> y) where T : IComparisonOperators<T, T, bool>;

    // IDivisionOperators

    public static Tensor<T> Divide<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IDivisionOperators<T, T, T>;
    public static Tensor<T> Divide<T>(in ROTensorSpan<T> x, T y) where T : IDivisionOperators<T, T, T>;
    public static Tensor<T> Divide<T>(T x, in ROTensorSpan<T> y) where T : IDivisionOperators<T, T, T>;

    public static TensorSpan<T> Divide<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IDivisionOperators<T, T, T>;
    public static TensorSpan<T> Divide<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : IDivisionOperators<T, T, T>;
    public static TensorSpan<T> Divide<T>(T x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IDivisionOperators<T, T, T>;

    // IEqualityOperators

    public static Tensor<bool> Equals<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IEqualityOperators<T, bool>;
    public static Tensor<bool> Equals<T>(in ROTensorSpan<T> x, T y) where T : IEqualityOperators<T, bool>;

    public static TensorSpan<bool> Equals<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<bool> destination) where T : IEqualityOperators<T, bool>;
    public static TensorSpan<bool> Equals<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<bool> destination) where T : IEqualityOperators<T, bool>;

    public static bool EqualsAll<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IEqualityOperators<T, bool>;
    public static bool EqualsAll<T>(in ROTensorSpan<T> x, T y) where T : IEqualityOperators<T, bool>;

    public static bool EqualsAny<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IEqualityOperators<T, bool>;
    public static bool EqualsAny<T>(in ROTensorSpan<T> x, T y) where T : IEqualityOperators<T, bool>;

    // IExponentialFunctions

    public static Tensor<T> Exp<T>(in ROTensorSpan<T> x) where T : IExponentialFunctions<T>;
    public static TensorSpan<T> Exp<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IExponentialFunctions<T>;

    public static Tensor<T> ExpM1<T>(in ROTensorSpan<T> x) where T : IExponentialFunctions<T>;
    public static TensorSpan<T> ExpM1<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IExponentialFunctions<T>;

    public static Tensor<T> Exp2<T>(in ROTensorSpan<T> x) where T : IExponentialFunctions<T>;
    public static TensorSpan<T> Exp2<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IExponentialFunctions<T>;

    public static Tensor<T> Exp2M1<T>(in ROTensorSpan<T> x) where T : IExponentialFunctions<T>;
    public static TensorSpan<T> Exp2M1<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IExponentialFunctions<T>;

    public static Tensor<T> Exp10<T>(in ROTensorSpan<T> x) where T : IExponentialFunctions<T>;
    public static TensorSpan<T> Exp10<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IExponentialFunctions<T>;

    public static Tensor<T> Exp10M1<T>(in ROTensorSpan<T> x) where T : IExponentialFunctions<T>;
    public static TensorSpan<T> Exp10M1<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IExponentialFunctions<T>;

    // IFloatingPoint

    public static Tensor<T> Ceiling<T>(in ROTensorSpan<T> x) where T : IFloatingPoint<T>;
    public static TensorSpan<T> Ceiling<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IFloatingPoint<T>;

    public static Tensor<T> Floor<T>(in ROTensorSpan<T> x) where T : IFloatingPoint<T>;
    public static TensorSpan<T> Floor<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IFloatingPoint<T>;

    public static Tensor<T> Reciprocal<T>(in ROTensorSpan<T> x) where T : IFloatingPoint<T>;
    public static TensorSpan<T> Reciprocal<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IFloatingPoint<T>;

    public static Tensor<T> Round<T>(in ROTensorSpan<T> x) where T : IFloatingPoint<T>;
    public static TensorSpan<T> Round<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IFloatingPoint<T>;

    public static Tensor<T> Round<T>(in ROTensorSpan<T> x, int digits, MidpointRounding mode) where T : IFloatingPoint<T>;
    public static TensorSpan<T> Round<T>(scoped in ROTensorSpan<T> x, int digits, MidpointRounding mode, in TensorSpan<T> destination) where T : IFloatingPoint<T>;

    public static Tensor<T> Round<T>(in ROTensorSpan<T> x, int digits) where T : IFloatingPoint<T>;
    public static TensorSpan<T> Round<T>(scoped in ROTensorSpan<T> x, int digits, in TensorSpan<T> destination) where T : IFloatingPoint<T>;

    public static Tensor<T> Round<T>(in ROTensorSpan<T> x, MidpointRounding mode) where T : IFloatingPoint<T>;
    public static TensorSpan<T> Round<T>(scoped in ROTensorSpan<T> x, MidpointRounding mode, in TensorSpan<T> destination) where T : IFloatingPoint<T>;

    public static Tensor<T> Truncate<T>(in ROTensorSpan<T> x) where T : IFloatingPoint<T>;
    public static TensorSpan<T> Truncate<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IFloatingPoint<T>;

    // IFloatingPointIeee754

    public static Tensor<T> Atan2<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> Atan2<T>(in ROTensorSpan<T> x, T y) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> Atan2<T>(T x, in ROTensorSpan<T> y) where T : IFloatingPointIeee754<T>;

    public static TensorSpan<T> Atan2<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> Atan2<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> Atan2<T>(T x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;

    public static Tensor<T> Atan2Pi<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> Atan2Pi<T>(in ROTensorSpan<T> x, T y) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> Atan2Pi<T>(T x, in ROTensorSpan<T> y) where T : IFloatingPointIeee754<T>;

    public static TensorSpan<T> Atan2Pi<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> Atan2Pi<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> Atan2Pi<T>(T x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;

    public static Tensor<T> FusedMultiplyAdd<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y, in ROTensorSpan<T> addend) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> FusedMultiplyAdd<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y, T addend) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> FusedMultiplyAdd<T>(in ROTensorSpan<T> x, T y, in ROTensorSpan<T> addend) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> FusedMultiplyAdd<T>(in ROTensorSpan<T> x, T y, T addend) where T : IFloatingPointIeee754<T>;

    public static TensorSpan<T> FusedMultiplyAdd<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, scoped in ROTensorSpan<T> addend, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> FusedMultiplyAdd<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, T addend, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> FusedMultiplyAdd<T>(scoped in ROTensorSpan<T> x, T y, scoped in ROTensorSpan<T> addend, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> FusedMultiplyAdd<T>(scoped in ROTensorSpan<T> x, T y, T addend, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;

    public static Tensor<T> Ieee754Remainder<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> Ieee754Remainder<T>(in ROTensorSpan<T> x, T y) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> Ieee754Remainder<T>(T x, in ROTensorSpan<T> y) where T : IFloatingPointIeee754<T>;

    public static TensorSpan<T> Ieee754Remainder<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> Ieee754Remainder<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> Ieee754Remainder<T>(T x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;

    public static Tensor<int> ILogB<T>(in ROTensorSpan<T> x) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<int> ILogB<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;

    public static Tensor<T> Lerp<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y, in ROTensorSpan<T> amount) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> Lerp<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y, T amount) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> Lerp<T>(in ROTensorSpan<T> x, T y, in ROTensorSpan<T> amount) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> Lerp<T>(in ROTensorSpan<T> x, T y, T amount) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> Lerp<T>(T x, in ROTensorSpan<T> y, in ROTensorSpan<T> amount) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> Lerp<T>(T x, in ROTensorSpan<T> y, T amount) where T : IFloatingPointIeee754<T>;
    public static Tensor<T> Lerp<T>(T x, T y, in ROTensorSpan<T> amount) where T : IFloatingPointIeee754<T>;

    public static TensorSpan<T> Lerp<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, scoped in ROTensorSpan<T> amount, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> Lerp<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, T amount, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> Lerp<T>(scoped in ROTensorSpan<T> x, T y, scoped in ROTensorSpan<T> amount, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> Lerp<T>(scoped in ROTensorSpan<T> x, T y, T amount, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> Lerp<T>(T x, scoped in ROTensorSpan<T> y, scoped in ROTensorSpan<T> amount, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> Lerp<T>(T x, scoped in ROTensorSpan<T> y, T amount, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> Lerp<T>(T x, T y, scoped in ROTensorSpan<T> amount, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;

    public static Tensor<T> ReciprocalEstimate<T>(in ROTensorSpan<T> x) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> ReciprocalEstimate<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;

    public static Tensor<T> ReciprocalSqrtEstimate<T>(in ROTensorSpan<T> x) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<T> ReciprocalSqrtEstimate<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;

    public static Tensor<int> ScaleB<T>(in ROTensorSpan<T> x, int n) where T : IFloatingPointIeee754<T>;
    public static TensorSpan<int> ScaleB<T>(scoped in ROTensorSpan<T> x, int n, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;

    // IHyperbolicFunctions

    public static Tensor<T> Acosh<T>(in ROTensorSpan<T> x) where T : IHyperbolicFunctions<T>;
    public static TensorSpan<T> Acosh<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IHyperbolicFunctions<T>;

    public static Tensor<T> Asinh<T>(in ROTensorSpan<T> x) where T : IHyperbolicFunctions<T>;
    public static TensorSpan<T> Asinh<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IHyperbolicFunctions<T>;

    public static Tensor<T> Atanh<T>(in ROTensorSpan<T> x) where T : IHyperbolicFunctions<T>;
    public static TensorSpan<T> Atanh<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IHyperbolicFunctions<T>;

    public static Tensor<T> Cosh<T>(in ROTensorSpan<T> x) where T : IHyperbolicFunctions<T>;
    public static TensorSpan<T> Cosh<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IHyperbolicFunctions<T>;

    public static Tensor<T> Sinh<T>(in ROTensorSpan<T> x) where T : IHyperbolicFunctions<T>;
    public static TensorSpan<T> Sinh<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IHyperbolicFunctions<T>;

    public static Tensor<T> Tanh<T>(in ROTensorSpan<T> x) where T : IHyperbolicFunctions<T>;
    public static TensorSpan<T> Tanh<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IHyperbolicFunctions<T>;

    // ILogarithmicFunctions

    public static Tensor<T> Log<T>(in ROTensorSpan<T> x) where T : ILogarithmicFunctions<T>;
    public static TensorSpan<T> Log<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ILogarithmicFunctions<T>;

    public static Tensor<T> Log<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : ILogarithmicFunctions<T>;
    public static Tensor<T> Log<T>(in ROTensorSpan<T> x, T y) where T : ILogarithmicFunctions<T>;
    public static Tensor<T> Log<T>(T x, in ROTensorSpan<T> y) where T : ILogarithmicFunctions<T>;
    
    public static TensorSpan<T> Log<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : ILogarithmicFunctions<T>;
    public static TensorSpan<T> Log<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : ILogarithmicFunctions<T>;
    public static TensorSpan<T> Log<T>(T x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : ILogarithmicFunctions<T>;

    public static Tensor<T> LogP1<T>(in ROTensorSpan<T> x) where T : ILogarithmicFunctions<T>;
    public static TensorSpan<T> LogP1<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ILogarithmicFunctions<T>;

    public static Tensor<T> Log2<T>(in ROTensorSpan<T> x) where T : ILogarithmicFunctions<T>;
    public static TensorSpan<T> Log2<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ILogarithmicFunctions<T>;

    public static Tensor<T> Log2P1<T>(in ROTensorSpan<T> x) where T : ILogarithmicFunctions<T>;
    public static TensorSpan<T> Log2P1<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ILogarithmicFunctions<T>;

    public static Tensor<T> Log10<T>(in ROTensorSpan<T> x) where T : ILogarithmicFunctions<T>;
    public static TensorSpan<T> Log10<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ILogarithmicFunctions<T>;

    public static Tensor<T> Log10P1<T>(in ROTensorSpan<T> x) where T : ILogarithmicFunctions<T>;
    public static TensorSpan<T> Log10P1<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ILogarithmicFunctions<T>;

    // IMultiplyOperators

    public static Tensor<T> Multiply<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IMultiplyOperators<T, T, T>, IMultiplicativeIdentity<T, T>;
    public static Tensor<T> Multiply<T>(in ROTensorSpan<T> x, T y) where T : IMultiplyOperators<T, T, T>, IMultiplicativeIdentity<T, T>;

    public static TensorSpan<T> Multiply<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IMultiplyOperators<T, T, T>, IMultiplicativeIdentity<T, T>;
    public static TensorSpan<T> Multiply<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : IMultiplyOperators<T, T, T>, IMultiplicativeIdentity<T, T>;

    // INumber

    public static Tensor<T> CopySign<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> sign) where T : INumber<T>;
    public static Tensor<T> CopySign<T>(in ROTensorSpan<T> x, T sign) where T : INumber<T>;
    public static Tensor<T> CopySign<T>(T x, in ROTensorSpan<T> sign) where T : INumber<T>;

    public static TensorSpan<T> CopySign<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> sign, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> CopySign<T>(scoped in ROTensorSpan<T> x, T sign, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> CopySign<T>(T x, scoped in ROTensorSpan<T> sign, in TensorSpan<T> destination) where T : INumber<T>;

    public static int IndexOfMax<T>(in ROTensorSpan<T> x) where T : INumber<T>;
    public static int IndexOfMaxNumber<T>(in ROTensorSpan<T> x) where T : INumber<T>;

    public static int IndexOfMin<T>(in ROTensorSpan<T> x) where T : INumber<T>;
    public static int IndexOfMinNumber<T>(in ROTensorSpan<T> x) where T : INumber<T>;

    public static T Max<T>(in ROTensorSpan<T> x) where T : INumber<T>;

    public static Tensor<T> Max<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : INumber<T>;
    public static Tensor<T> Max<T>(in ROTensorSpan<T> x, T y) where T : INumber<T>;

    public static TensorSpan<T> Max<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> Max<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : INumber<T>;

    public static T MaxNumber<T>(in ROTensorSpan<T> x) where T : INumber<T>;

    public static Tensor<T> MaxNumber<T>(in ROTensorSpan<T> x, T y) where T : INumber<T>;
    public static Tensor<T> MaxNumber<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : INumber<T>;

    public static TensorSpan<T> MaxNumber<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> MaxNumber<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : INumber<T>;

    public static T Min<T>(in ROTensorSpan<T> x) where T : INumber<T>;

    public static Tensor<T> Min<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : INumber<T>;
    public static Tensor<T> Min<T>(in ROTensorSpan<T> x, T y) where T : INumber<T>;

    public static TensorSpan<T> Min<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> Min<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : INumber<T>;

    public static T MinNumber<T>(in ROTensorSpan<T> x) where T : INumber<T>;

    public static Tensor<T> MinNumber<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : INumber<T>;
    public static Tensor<T> MinNumber<T>(in ROTensorSpan<T> x, T y) where T : INumber<T>;

    public static TensorSpan<T> MinNumber<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> MinNumber<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : INumber<T>;

    // INumberBase

    public static Tensor<T> Abs<T>(in ROTensorSpan<T> x) where T : INumberBase<T>;
    public static TensorSpan<T> Abs<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : INumberBase<T>;

    public static Tensor<TTo> ConvertChecked<TFrom, TTo>(in ROTensorSpan<TFrom> source) where TFrom : INumberBase<TFrom> where TTo : INumberBase<TTo>;
    public static TensorSpan<TTo> ConvertChecked<TFrom, TTo>(scoped in ROTensorSpan<TFrom> source, in TensorSpan<TTo> destination) where TFrom : INumberBase<TFrom> where TTo : INumberBase<TTo>;

    public static Tensor<TTo> ConvertSaturating<TFrom, TTo>(in ROTensorSpan<TFrom> source) where TFrom : INumberBase<TFrom> where TTo : INumberBase<TTo>;
    public static TensorSpan<TTo> ConvertSaturating<TFrom, TTo>(scoped in ROTensorSpan<TFrom> source, in TensorSpan<TTo> destination) where TFrom : INumberBase<TFrom> where TTo : INumberBase<TTo>;

    public static Tensor<TTo> ConvertTruncating<TFrom, TTo>(in ROTensorSpan<TFrom> source) where TFrom : INumberBase<TFrom> where TTo : INumberBase<TTo>;
    public static TensorSpan<TTo> ConvertTruncating<TFrom, TTo>(scoped in ROTensorSpan<TFrom> source, in TensorSpan<TTo> destination) where TFrom : INumberBase<TFrom> where TTo : INumberBase<TTo>;

    public static int IndexOfMaxMagnitude<T>(in ROTensorSpan<T> x) where T : INumberBase<T>;
    public static int IndexOfMaxMagnitudeNumber<T>(in ROTensorSpan<T> x) where T : INumberBase<T>;

    public static int IndexOfMinMagnitude<T>(in ROTensorSpan<T> x) where T : INumberBase<T>;
    public static int IndexOfMinMagnitudeNumber<T>(in ROTensorSpan<T> x) where T : INumberBase<T>;

    public static T MaxMagnitude<T>(in ROTensorSpan<T> x) where T : INumberBase<T>;

    public static Tensor<T> MaxMagnitude<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : INumber<T>;
    public static Tensor<T> MaxMagnitude<T>(in ROTensorSpan<T> x, T y) where T : INumber<T>;

    public static TensorSpan<T> MaxMagnitude<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> MaxMagnitude<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : INumber<T>;

    public static T MaxMagnitudeNumber<T>(in ROTensorSpan<T> x) where T : INumberBase<T>;

    public static Tensor<T> MaxMagnitudeNumber<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : INumber<T>;
    public static Tensor<T> MaxMagnitudeNumber<T>(in ROTensorSpan<T> x, T y) where T : INumber<T>;

    public static TensorSpan<T> MaxMagnitudeNumber<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> MaxMagnitudeNumber<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : INumber<T>;

    public static T MinMagnitude<T>(in ROTensorSpan<T> x) where T : INumberBase<T>;

    public static Tensor<T> MinMagnitude<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : INumber<T>;
    public static Tensor<T> MinMagnitude<T>(in ROTensorSpan<T> x, T y) where T : INumber<T>;

    public static TensorSpan<T> MinMagnitude<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> MinMagnitude<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : INumber<T>;

    public static T MinMagnitudeNumber<T>(in ROTensorSpan<T> x) where T : INumberBase<T>;

    public static Tensor<T> MinMagnitudeNumber<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : INumber<T>;
    public static Tensor<T> MinMagnitudeNumber<T>(in ROTensorSpan<T> x, T y) where T : INumber<T>;

    public static TensorSpan<T> MinMagnitudeNumber<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> MinMagnitudeNumber<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : INumber<T>;

    public static Tensor<T> MultiplyAddEstimate<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y, in ROTensorSpan<T> addend) where T : INumber<T>;
    public static Tensor<T> MultiplyAddEstimate<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y, T addend) where T : INumber<T>;
    public static Tensor<T> MultiplyAddEstimate<T>(in ROTensorSpan<T> x, T y, in ROTensorSpan<T> addend) where T : INumber<T>;
    public static Tensor<T> MultiplyAddEstimate<T>(in ROTensorSpan<T> x, T y, T addend) where T : INumber<T>;

    public static TensorSpan<T> MultiplyAddEstimate<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, scoped in ROTensorSpan<T> addend, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> MultiplyAddEstimate<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, T addend, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> MultiplyAddEstimate<T>(scoped in ROTensorSpan<T> x, T y, scoped in ROTensorSpan<T> addend, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> MultiplyAddEstimate<T>(scoped in ROTensorSpan<T> x, T y, T addend, in TensorSpan<T> destination) where T : INumber<T>;

    // IPowerFunctions

    public static Tensor<T> Pow<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IPowerFunctions<T>;
    public static Tensor<T> Pow<T>(in ROTensorSpan<T> x, T y) where T : IPowerFunctions<T>;
    public static Tensor<T> Pow<T>(T x, in ROTensorSpan<T> y) where T : IPowerFunctions<T>;

    public static TensorSpan<T> Pow<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IPowerFunctions<T>;
    public static TensorSpan<T> Pow<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : IPowerFunctions<T>;
    public static TensorSpan<T> Pow<T>(T x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IPowerFunctions<T>;

    // IRootFunctions

    public static Tensor<T> Cbrt<T>(in ROTensorSpan<T> x) where T : IRootFunctions<T>;
    public static TensorSpan<T> Cbrt<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IRootFunctions<T>;

    public static Tensor<T> Hypot<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IRootFunctions<T>;
    public static Tensor<T> Hypot<T>(in ROTensorSpan<T> x, T y) where T : IRootFunctions<T>;

    public static TensorSpan<T> Hypot<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IRootFunctions<T>;
    public static TensorSpan<T> Hypot<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : IRootFunctions<T>;

    public static Tensor<T> RootN<T>(in ROTensorSpan<T> x, int n) where T : IRootFunctions<T>;
    public static TensorSpan<T> RootN<T>(scoped in ROTensorSpan<T> x, int n, in TensorSpan<T> destination) where T : IRootFunctions<T>;

    public static Tensor<T> Sqrt<T>(in ROTensorSpan<T> x) where T : IRootFunctions<T>;
    public static TensorSpan<T> Sqrt<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IRootFunctions<T>;

    // IShiftOperators

    public static Tensor<T> ShiftLeft<T>(in ROTensorSpan<T> x, int shiftAmount) where T : IBinaryInteger<T>;
    public static TensorSpan<T> ShiftLeft<T>(scoped in ROTensorSpan<T> x, int shiftAmount, in TensorSpan<T> destination) where T : IBinaryInteger<T>;

    public static Tensor<T> ShiftRightArithmetic<T>(in ROTensorSpan<T> x, int shiftAmount) where T : IBinaryInteger<T>;
    public static TensorSpan<T> ShiftRightArithmetic<T>(scoped in ROTensorSpan<T> x, int shiftAmount, in TensorSpan<T> destination) where T : IBinaryInteger<T>;

    public static Tensor<T> ShiftRightLogical<T>(in ROTensorSpan<T> x, int shiftAmount) where T : IBinaryInteger<T>;
    public static TensorSpan<T> ShiftRightLogical<T>(scoped in ROTensorSpan<T> x, int shiftAmount, in TensorSpan<T> destination) where T : IBinaryInteger<T>;

    // ISubtractionOperators

    public static Tensor<T> Subtract<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : ISubtractionOperators<T, T, T>;
    public static Tensor<T> Subtract<T>(in ROTensorSpan<T> x, T y) where T : ISubtractionOperators<T, T, T>;
    public static Tensor<T> Subtract<T>(T x, in ROTensorSpan<T> y) where T : ISubtractionOperators<T, T, T>;

    public static TensorSpan<T> Subtract<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : ISubtractionOperators<T, T, T>;
    public static TensorSpan<T> Subtract<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : ISubtractionOperators<T, T, T>;
    public static TensorSpan<T> Subtract<T>(T x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : ISubtractionOperators<T, T, T>;

    // ITrigonometricFunctions

    public static Tensor<T> Acos<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static TensorSpan<T> Acos<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ITrigonometricFunctions<T>;

    public static Tensor<T> AcosPi<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static TensorSpan<T> AcosPi<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ITrigonometricFunctions<T>;

    public static Tensor<T> Asin<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static TensorSpan<T> Asin<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ITrigonometricFunctions<T>;

    public static Tensor<T> AsinPi<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static TensorSpan<T> AsinPi<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ITrigonometricFunctions<T>;

    public static Tensor<T> Atan<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static TensorSpan<T> Atan<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ITrigonometricFunctions<T>;

    public static Tensor<T> AtanPi<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static TensorSpan<T> AtanPi<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ITrigonometricFunctions<T>;

    public static Tensor<T> Cos<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static TensorSpan<T> Cos<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ITrigonometricFunctions<T>;

    public static Tensor<T> CosPi<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static TensorSpan<T> CosPi<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ITrigonometricFunctions<T>;

    public static Tensor<T> DegreesToRadians<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static TensorSpan<T> DegreesToRadians<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ITrigonometricFunctions<T>;

    public static Tensor<T> RadiansToDegrees<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static TensorSpan<T> RadiansToDegrees<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ITrigonometricFunctions<T>;

    public static Tensor<T> Sin<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static TensorSpan<T> Sin<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ITrigonometricFunctions<T>;

    public static (Tensor<T> Sin, Tensor<T> Cos) SinCos<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static (TensorSpan<T> Sin, TensorSpan<T> Cos) SinCos<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> sinDestination, in TensorSpan<T> cosDestination) where T : ITrigonometricFunctions<T>;

    public static (Tensor<T> SinPi, Tensor<T> CosPi) SinCosPi<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static (TensorSpan<T> Sin, TensorSpan<T> Cos) SinCosPi<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> sinPiDestination, in TensorSpan<T> cosPiDestination) where T : ITrigonometricFunctions<T>;

    public static Tensor<T> SinPi<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static TensorSpan<T> SinPi<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ITrigonometricFunctions<T>;

    public static Tensor<T> Tan<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static TensorSpan<T> Tan<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ITrigonometricFunctions<T>;

    public static Tensor<T> TanPi<T>(in ROTensorSpan<T> x) where T : ITrigonometricFunctions<T>;
    public static TensorSpan<T> TanPi<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : ITrigonometricFunctions<T>;

    // IUnaryNegationOperators

    public static Tensor<T> Negate<T>(in ROTensorSpan<T> x) where T : IUnaryNegationOperators<T, T>;
    public static TensorSpan<T> Negate<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IUnaryNegationOperators<T, T>;
}
```

```C#
namespace System.Numerics.Tensors;

public static partial class Tensor
{
    public static Tensor<T> BitDecrement<T>(in ROTensorSpan<T> x) where T : IFloatingPointIeee754<T, T>;
    public static TensorSpan<T> BitDecrement<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;

    public static Tensor<T> BitIncrement<T>(in ROTensorSpan<T> x) where T : IFloatingPointIeee754<T, T>;
    public static TensorSpan<T> BitIncrement<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IFloatingPointIeee754<T>;

    public static Tensor<T> Clamp<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> min, in ROTensorSpan<T> max) where T : INumber<T>;
    public static Tensor<T> Clamp<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> min, T max) where T : INumber<T>;
    public static Tensor<T> Clamp<T>(in ROTensorSpan<T> x, T min, in ROTensorSpan<T> max) where T : INumber<T>;
    public static Tensor<T> Clamp<T>(in ROTensorSpan<T> x, T min, T max) where T : INumber<T>;
    public static Tensor<T> Clamp<T>(T x, in ROTensorSpan<T> min, in ROTensorSpan<T> max) where T : INumber<T>;
    public static Tensor<T> Clamp<T>(T x, in ROTensorSpan<T> min, T max) where T : INumber<T>;
    public static Tensor<T> Clamp<T>(T x, T min, scoped in ROTensorSpan<T> max) where T : INumber<T>;

    public static TensorSpan<T> Clamp<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> min, scoped in ROTensorSpan<T> max, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> Clamp<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> min, T max, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> Clamp<T>(scoped in ROTensorSpan<T> x, T min, scoped in ROTensorSpan<T> max, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> Clamp<T>(scoped in ROTensorSpan<T> x, T min, T max, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> Clamp<T>(T x, scoped in ROTensorSpan<T> min, scoped in ROTensorSpan<T> max, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> Clamp<T>(T x, scoped in ROTensorSpan<T> min, T max, in TensorSpan<T> destination) where T : INumber<T>;
    public static TensorSpan<T> Clamp<T>(T x, T min, scoped in ROTensorSpan<T> max, in TensorSpan<T> destination) where T : INumber<T>;

    public static Tensor<TTo> ConvertToInteger<TFrom, TTo>(in ROTensorSpan<TFrom> source) where TFrom : IFloatingPoint<TFrom> where TTo : IBinaryInteger<TTo>;
    public static TensorSpan<TTo> ConvertToInteger<TFrom, TTo>(scoped in ROTensorSpan<TFrom> source, in TensorSpan<TTo> destination) where TFrom : IFloatingPoint<TFrom> where TTo : IBinaryInteger<TTo>;

    public static Tensor<TTo> ConvertToIntegerNative<TFrom, TTo>(in ROTensorSpan<TFrom> source) where TFrom : IFloatingPoint<TFrom> where TTo : IBinaryInteger<TTo>;
    public static TensorSpan<TTo> ConvertToIntegerNative<TFrom, TTo>(scoped in ROTensorSpan<TFrom> source, in TensorSpan<TTo> destination) where TFrom : IFloatingPoint<TFrom> where TTo : IBinaryInteger<TTo>;

    public static (Tensor<T> Quotient, Tensor<T> Remainder) DivRem<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IBinaryInteger<T>;
    public static (Tensor<T> Quotient, Tensor<T> Remainder) DivRem<T>(in ROTensorSpan<T> x, T y) where T : IBinaryInteger<T>;
    public static (Tensor<T> Quotient, Tensor<T> Remainder) DivRem<T>(T x, in ROTensorSpan<T> y) where T : IBinaryInteger<T>;

    public static (TensorSpan<T> Quotient, TensorSpan<T> Remainder) DivRem<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y, in TensorSpan<T> quotientDestination, in TensorSpan<T> remainderDestination) where T : IBinaryInteger<T>;
    public static (TensorSpan<T> Quotient, TensorSpan<T> Remainder) DivRem<T>(in ROTensorSpan<T> x, T y, in TensorSpan<T> quotientDestination, in TensorSpan<T> remainderDestination) where T : IBinaryInteger<T>;
    public static (TensorSpan<T> Quotient, TensorSpan<T> Remainder) DivRem<T>(T x, in ROTensorSpan<T> y, in TensorSpan<T> quotientDestination, in TensorSpan<T> remainderDestination) where T : IBinaryInteger<T>;

    public static Tensor<T> Decrement<T>(in ROTensorSpan<T> x) where T : IDecrementOperators<T, T>;
    public static TensorSpan<T> Decrement<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IDecrementOperators<T, T>;

    public static Tensor<T> Increment<T>(in ROTensorSpan<T> x) where T : IIncrementOperators<T, T>;
    public static TensorSpan<T> Increment<T>(scoped in ROTensorSpan<T> x, in TensorSpan<T> destination) where T : IIncrementOperators<T, T>;

    public static T Mean<T>(in ROTensorSpan<T> x) where T : IFloatingPoint<T>;

    public static Tensor<T> Remainder<T>(in ROTensorSpan<T> x, in ROTensorSpan<T> y) where T : IModulusOperators<T, T, T>;
    public static Tensor<T> Remainder<T>(in ROTensorSpan<T> x, T y) where T : IModulusOperators<T, T, T>;
    public static Tensor<T> Remainder<T>(T x, in ROTensorSpan<T> y) where T : IModulusOperators<T, T, T>;

    public static TensorSpan<T> Remainder<T>(scoped in ROTensorSpan<T> x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IModulusOperators<T, T, T>;
    public static TensorSpan<T> Remainder<T>(scoped in ROTensorSpan<T> x, T y, in TensorSpan<T> destination) where T : IModulusOperators<T, T, T>;
    public static TensorSpan<T> Remainder<T>(T x, scoped in ROTensorSpan<T> y, in TensorSpan<T> destination) where T : IModulusOperators<T, T, T>;

    public static Tensor<int> Sign<T>(in ROTensorSpan<T> x) where T : INumber<T>;
    public static TensorSpan<int> Sign<T>(scoped in ROTensorSpan<T> x, in TensoreSpan<T> destination) where T : INumber<T>;

    public static T StdDev<T>(in ROTensorSpan<T> x) where T : IPowerFunctions<T>;
}

public static partial class TensorPrimitives
{
    public static void BitDecrement<T>(ROSpan<T> x, Span<T> destination) where T : IFloatingPointIeee754<T>;

    public static void BitIncrement<T>(ROSpan<T> x, Span<T> destination) where T : IFloatingPointIeee754<T>;

    public static void Clamp<T>(ROSpan<T> x, ROSpan<T> min, ROSpan<T> max, Span<T> destination) where T : INumber<T>;
    public static void Clamp<T>(ROSpan<T> x, ROSpan<T> min, T max, Span<T> destination) where T : INumber<T>;
    public static void Clamp<T>(ROSpan<T> x, T min, ROSpan<T> max, Span<T> destination) where T : INumber<T>;
    public static void Clamp<T>(ROSpan<T> x, T min, T max, Span<T> destination) where T : INumber<T>;
    public static void Clamp<T>(T x, ROSpan<T> min, ROSpan<T> max, Span<T> destination) where T : INumber<T>;
    public static void Clamp<T>(T x, ROSpan<T> min, T max, Span<T> destination) where T : INumber<T>;
    public static void Clamp<T>(T x, T min, ROSpan<T> max, Span<T> destination) where T : INumber<T>;

    public static Span<TTo> ConvertToInteger<TFrom, TTo>(ROSpan<TFrom> source, Span<TTo> destination) where TFrom : IFloatingPoint<TFrom> where TTo : IBinaryInteger<TTo>;
    public static Span<TTo> ConvertToIntegerNative<TFrom, TTo>(ROSpan<TFrom> source, Span<TTo> destination) where TFrom : IFloatingPoint<TFrom> where TTo : IBinaryInteger<TTo>;

    public static void DivRem<T>(ROSpan<T> x, ROSpan<T> y, Span<T> quotientDestination, Span<T> remainderDestination) where T : IBinaryInteger<T>;
    public static void DivRem<T>(ROSpan<T> x, T y, Span<T> quotientDestination, Span<T> remainderDestination) where T : IBinaryInteger<T>;
    public static void DivRem<T>(T x, ROSpan<T> y, Span<T> quotientDestination, Span<T> remainderDestination) where T : IBinaryInteger<T>;

    public static void Decrement<T>(ROSpan<T> x, Span<T> destination) where T : IDecrementOperators<T, T>;

    public static void Increment<T>(ROSpan<T> x, Span<T> destination) where T : IIncrementOperators<T, T>;

    public static void Remainder<T>(ROSpan<T> x, ROSpan<T> y, Span<T> destination) where T : IModulusOperators<T, T, T>;
    public static void Remainder<T>(ROSpan<T> x, T y, Span<T> destination) where T : IModulusOperators<T, T, T>;
    public static void Remainder<T>(T x, ROSpan<T> y, Span<T> destination) where T : IModulusOperators<T, T, T>;

    public static void Sign<T>(ROSpan<T> x, in Span<int> destination) where T : INumber<T>;

    public static T StdDev<T>(ROSpan<T> x) where T : IPowerFunctions<T>;
}
```
