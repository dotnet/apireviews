# 2020-02-18 ARM Intrnsics

## Arm64 Load & Store

**Approved** | [dotnet/runtime#24771](https://github.com/dotnet/runtime/issues/24771) | [Video](xxx)

```C#
namespace System.Runtime.Intrinsics.Arm
{
    public partial class AdvSimd 
    {
        public partial class Arm64 : ArmBase
        {
            /// <summary>
            /// Vector load
            ///
            /// Corresponds to vector form of ARM64 LDR
            /// </summary>
            public static unsafe Vector64<T>  LoadVector64<T>(void* address) where T : struct;
            public static unsafe Vector128<T> LoadVector128<T>(void* address) where T : struct;
        
            /// <summary>
            /// Vector store
            ///
            /// Corresponds to vector form of ARM64 STR
            /// </summary>
            public static unsafe void Store<T>(void* address, Vector64<T>  source) where T : struct;
            public static unsafe void Store<T>(void* address, Vector128<T> source) where T : struct;
        }
    }
}
```

## More SIMD HW Intrinsics

**Approved** | [dotnet/runtime#24794](https://github.com/dotnet/runtime/issues/24794) | [Video](xxx)

* Check whether `MaxPairwise`/`MinPairwise` should also include forms for `Vector128`
* Should the arguments for `ReciprocalStep` and `ReciprocalSquareRootStep` be something more specific than `left` and `right`?
* Looks like `ReverseElementBytes` needs more work, as well as clarity on support
* We skipped `Arm32` because it's not going to be implemented for .NET 5
* `ExtractVector` shouldn't have overloads for `float` and `double`, it could end up silently modifying/normalizing/corrupting the floating point types
* `MaxNumericPairwiseScalar` should be `MaxNumbercPairwiseScalar`. Some folks raised concerns around `PairwiseScalar` being confusing, but it matches the ISA name and we can't think of a better name
* We didn't review all the APIs (I commented the ones below). Tanner will see whether they are just applying a pattern or a net-new APIs, in which case we'll take another look.

```C#
amespace System.Runtime.Intrinsics.Arm
{
    public partial class AdvSimd
    {
        /// <summary>
        /// Vector CompareGreaterThanOrEqual
        /// For each element result[elem] = (|left[elem]| >= |right[elem]|) ? ~0 : 0
        /// Corresponds to vector forms of ARM64 FACGE
        /// </summary>
        public static Vector64<float>   AbsoluteCompareGreaterThanOrEqual(Vector64<float>   left, Vector64<float>   right);
        public static Vector128<float>  AbsoluteCompareGreaterThanOrEqual(Vector128<float>  left, Vector128<float>  right);

        /// <summary>
        /// Vector CompareGreaterThan
        ///
        /// For each element result[elem] = (|left[elem]| > |right[elem]|) ? ~0 : 0
        ///
        /// Corresponds to vector forms of ARM64 FACGT
        /// </summary>
        public static Vector64<float>   AbsoluteCompareGreaterThan(Vector64<float>   left, Vector64<float>   right);
        public static Vector128<float>  AbsoluteCompareGreaterThan(Vector128<float>  left, Vector128<float>  right);

        /// <summary>
        /// Vector absolute difference
        /// Corresponds to vector forms of ARM64 SABD, UABD, and FABD
        /// </summary>
        public static Vector64<byte>    AbsoluteDifference(Vector64<byte>    left, Vector64<byte>    right);
        public static Vector64<byte>    AbsoluteDifference(Vector64<sbyte>   left, Vector64<sbyte>   right);
        public static Vector64<ushort>  AbsoluteDifference(Vector64<ushort>  left, Vector64<ushort>  right);
        public static Vector64<ushort>  AbsoluteDifference(Vector64<short>   left, Vector64<short>   right);
        public static Vector64<uint>    AbsoluteDifference(Vector64<uint>    left, Vector64<uint>    right);
        public static Vector64<uint>    AbsoluteDifference(Vector64<int>     left, Vector64<int>     right);
        public static Vector64<float>   AbsoluteDifference(Vector64<float>   left, Vector64<float>   right);
        public static Vector128<byte>   AbsoluteDifference(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<byte>   AbsoluteDifference(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<ushort> AbsoluteDifference(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<ushort> AbsoluteDifference(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<uint>   AbsoluteDifference(Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<uint>   AbsoluteDifference(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<float>  AbsoluteDifference(Vector128<float>  left, Vector128<float>  right);

        /// <summary>
        /// Vector absolute difference add
        ///
        /// For each element result[elem] = acc[elem] + | left[elem] - right[elem] |
        ///
        /// Corresponds to vector forms of ARM64 SABA, UABA
        /// </summary>
        public static Vector64<byte>    AbsoluteDifferenceAdd(Vector64<byte>    addend, Vector64<byte>    left, Vector64<byte>    right);
        public static Vector64<byte>    AbsoluteDifferenceAdd(Vector64<sbyte>   addend, Vector64<sbyte>   left, Vector64<sbyte>   right);
        public static Vector64<ushort>  AbsoluteDifferenceAdd(Vector64<ushort>  addend, Vector64<ushort>  left, Vector64<ushort>  right);
        public static Vector64<ushort>  AbsoluteDifferenceAdd(Vector64<short>   addend, Vector64<short>   left, Vector64<short>   right);
        public static Vector64<uint>    AbsoluteDifferenceAdd(Vector64<uint>    addend, Vector64<uint>    left, Vector64<uint>    right);
        public static Vector64<uint>    AbsoluteDifferenceAdd(Vector64<int>     addend, Vector64<int>     left, Vector64<int>     right);
        public static Vector128<byte>   AbsoluteDifferenceAdd(Vector128<byte>   addend, Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<byte>   AbsoluteDifferenceAdd(Vector128<sbyte>  addend, Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<ushort> AbsoluteDifferenceAdd(Vector128<ushort> addend, Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<ushort> AbsoluteDifferenceAdd(Vector128<short>  addend, Vector128<short>  left, Vector128<short>  right);
        public static Vector128<uint>   AbsoluteDifferenceAdd(Vector128<uint>   addend, Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<uint>   AbsoluteDifferenceAdd(Vector128<int>    addend, Vector128<int>    left, Vector128<int>    right);

        /// <summary>
        /// Vector add pairwise
        /// For each byte result[byte] = 2*byte < result.Length ? (left[2*byte] + left[2*byte + 1]) : (right[2*byte - result.Length] + right[2*byte + 1 - result.Length])
        /// Corresponds to vector forms of ARM64 ADDP, and FADDP
        /// </summary>
        public static Vector64<byte>   AddPairwise(Vector64<byte>  left, Vector64<byte>  right) ;
        public static Vector64<sbyte>  AddPairwise(Vector64<sbyte>  left, Vector64<sbyte>  right) ;
        public static Vector64<ushort> AddPairwise(Vector64<ushort>  left, Vector64<ushort>  right) ;
        public static Vector64<short>  AddPairwise(Vector64<short>  left, Vector64<short>  right) ;
        public static Vector64<int>    AddPairwise(Vector64<int>  left, Vector64<int>  right) ;
        public static Vector64<uint>   AddPairwise(Vector64<uint>  left, Vector64<uint>  right) ;
        public static Vector64<float>  AddPairwise(Vector64<float>  left, Vector64<float>  right) ;

        /// <summary>
        /// Vector extract from pair of vectors
        /// For each byte result[byte] = byte + index < result.Length ? left[byte + index] : right[byte + index - result.Length]
        ///
        /// Note: index must be a JIT time const expression which can be used to populate the literal immediate field
        ///
        /// Corresponds to vector forms of ARM64 EXT
        /// </summary>
        public static Vector64<byte>   ExtractVector64(Vector64<byte>  upper, Vector64<byte>  lower, byte byteIndex);
        public static Vector64<sbyte>  ExtractVector64(Vector64<sbyte>  upper, Vector64<sbyte>  lower, byte byteIndex);
        public static Vector64<short>  ExtractVector64(Vector64<short>  upper, Vector64<short>  lower, byte byteIndex);
        public static Vector64<ushort> ExtractVector64(Vector64<ushort>  upper, Vector64<ushort>  lower, byte byteIndex);
        public static Vector64<int>    ExtractVector64(Vector64<int>  upper, Vector64<int>  lower, byte byteIndex);
        public static Vector64<uint>   ExtractVector64(Vector64<uint>  upper, Vector64<uint>  lower, byte byteIndex);

        public static Vector128<byte>   ExtractVector128(Vector128<byte> upper, Vector128<byte> lower, byte byteIndex);
        public static Vector128<sbyte>  ExtractVector128(Vector128<sbyte> upper, Vector128<sbyte> lower, byte byteIndex);
        public static Vector128<short>  ExtractVector128(Vector128<short> upper, Vector128<short> lower, byte byteIndex);
        public static Vector128<ushort> ExtractVector128(Vector128<ushort> upper, Vector128<ushort> lower, byte byteIndex);
        public static Vector128<int>    ExtractVector128(Vector128<int> upper, Vector128<int> lower, byte byteIndex);
        public static Vector128<uint>   ExtractVector128(Vector128<uint> upper, Vector128<uint> lower, byte byteIndex);
        public static Vector128<long>   ExtractVector128(Vector128<long> upper, Vector128<long> lower, byte byteIndex);
        public static Vector128<ulong>  ExtractVector128(Vector128<ulong> upper, Vector128<ulong> lower, byte byteIndex);
        public static Vector128<float>  ExtractVector128(Vector128<float> upper, Vector128<float> lower, byte byteIndex);
        public static Vector128<double> ExtractVector128(Vector128<double> upper, Vector128<double> lower, byte byteIndex);

        /// <summary>
        /// Vector max numeric
        /// Corresponds to vector forms of ARM64 FMAXNM
        /// </summary>
        public static Vector64<float>   MaxNumber(Vector64<float>   left, Vector64<float>   right);
        public static Vector128<float>  MaxNumber(Vector128<float>  left, Vector128<float>  right);

        /// <summary>
        /// Vector max pairwise
        ///
        /// For each element result[elem] = 2*elem < result.Length ? max(left[2*elem], left[2*byte + 1]) : max(right[2*byte - result.Length], right[2*byte + 1 - result.Length])
        ///
        /// Corresponds to vector forms of ARM64 SMAXP, UMAXP, and FMAXP
        /// </summary>
        public static Vector64<byte>    MaxPairwise(Vector64<byte>    left, Vector64<byte>    right);
        public static Vector64<sbyte>   MaxPairwise(Vector64<sbyte>   left, Vector64<sbyte>   right);
        public static Vector64<ushort>  MaxPairwise(Vector64<ushort>  left, Vector64<ushort>  right);
        public static Vector64<short>   MaxPairwise(Vector64<short>   left, Vector64<short>   right);
        public static Vector64<uint>    MaxPairwise(Vector64<uint>    left, Vector64<uint>    right);
        public static Vector64<int>     MaxPairwise(Vector64<int>     left, Vector64<int>     right);
        public static Vector64<float>   MaxPairwise(Vector64<float>   left, Vector64<float>   right);

        /// <summary>
        /// Vector min numeric
        /// Corresponds to vector forms of ARM64 FMINNM
        /// </summary>
        public static Vector64<float>   MinNumber(Vector64<float>   left, Vector64<float>   right);
        public static Vector128<float>  MinNumber(Vector128<float>  left, Vector128<float>  right);

        /// <summary>
        /// Vector min pairwise
        ///
        /// For each element result[elem] = 2*elem < result.Length ? min(left[2*elem], left[2*byte + 1]) : min(right[2*byte - result.Length], right[2*byte + 1 - result.Length])
        ///
        /// Corresponds to vector forms of ARM64 SMINP, UMINP, and FMINP
        /// </summary>
        public static Vector64<byte>    MinPairwise(Vector64<byte>    left, Vector64<byte>    right);
        public static Vector64<sbyte>   MinPairwise(Vector64<sbyte>   left, Vector64<sbyte>   right);
        public static Vector64<ushort>  MinPairwise(Vector64<ushort>  left, Vector64<ushort>  right);
        public static Vector64<short>   MinPairwise(Vector64<short>   left, Vector64<short>   right);
        public static Vector64<uint>    MinPairwise(Vector64<uint>    left, Vector64<uint>    right);
        public static Vector64<int>     MinPairwise(Vector64<int>     left, Vector64<int>     right);
        public static Vector64<float>   MinPairwise(Vector64<float>   left, Vector64<float>   right);

        /// <summary>
        /// Vector multiply add
        ///
        /// For each element result[elem] = acc[elem] + left[elem] * right[elem]
        ///
        /// Corresponds to vector forms of ARM64 MLA
        /// </summary>
        public static Vector64<byte>    MultiplyAdd(Vector64<byte>    addend, Vector64<byte>    left, Vector64<byte>    right);
        public static Vector64<sbyte>   MultiplyAdd(Vector64<sbyte>   addend, Vector64<sbyte>   left, Vector64<sbyte>   right);
        public static Vector64<ushort>  MultiplyAdd(Vector64<ushort>  addend, Vector64<ushort>  left, Vector64<ushort>  right);
        public static Vector64<short>   MultiplyAdd(Vector64<short>   addend, Vector64<short>   left, Vector64<short>   right);
        public static Vector64<uint>    MultiplyAdd(Vector64<uint>    addend, Vector64<uint>    left, Vector64<uint>    right);
        public static Vector64<int>     MultiplyAdd(Vector64<int>     addend, Vector64<int>     left, Vector64<int>     right);
        public static Vector128<byte>   MultiplyAdd(Vector128<byte>   addend, Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<sbyte>  MultiplyAdd(Vector128<sbyte>  addend, Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<ushort> MultiplyAdd(Vector128<ushort> addend, Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<short>  MultiplyAdd(Vector128<short>  addend, Vector128<short>  left, Vector128<short>  right);
        public static Vector128<uint>   MultiplyAdd(Vector128<uint>   addend, Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<int>    MultiplyAdd(Vector128<int>    addend, Vector128<int>    left, Vector128<int>    right);

        /// <summary>
        /// Vector multiply add by element
        ///
        /// For each element result[elem] = acc[elem] + left[elem] * right
        ///
        /// Corresponds to vector forms of ARM64 MLA
        /// </summary>
        public static Vector64<byte>    MultiplyAddBySelectedScalar(Vector64<byte>    addend, Vector64<byte>    left, Vector64<byte>    right, byte rightIndex);
        public static Vector64<sbyte>   MultiplyAddBySelectedScalar(Vector64<sbyte>   addend, Vector64<sbyte>   left, Vector64<sbyte>   right, byte rightIndex);
        public static Vector64<ushort>  MultiplyAddBySelectedScalar(Vector64<ushort>  addend, Vector64<ushort>  left, Vector64<ushort>  right, byte rightIndex);
        public static Vector64<short>   MultiplyAddBySelectedScalar(Vector64<short>   addend, Vector64<short>   left, Vector64<short>   right, byte rightIndex);
        public static Vector64<uint>    MultiplyAddBySelectedScalar(Vector64<uint>    addend, Vector64<uint>    left, Vector64<uint>    right, byte rightIndex);
        public static Vector64<int>     MultiplyAddBySelectedScalar(Vector64<int>     addend, Vector64<int>     left, Vector64<int>     right, byte rightIndex);
        public static Vector128<byte>   MultiplyAddBySelectedScalar(Vector128<byte>   addend, Vector128<byte>   left, Vector128<byte>   right, byte rightIndex);
        public static Vector128<sbyte>  MultiplyAddBySelectedScalar(Vector128<sbyte>  addend, Vector128<sbyte>  left, Vector128<sbyte>  right, byte rightIndex);
        public static Vector128<ushort> MultiplyAddBySelectedScalar(Vector128<ushort> addend, Vector128<ushort> left, Vector128<ushort> right, byte rightIndex);
        public static Vector128<short>  MultiplyAddBySelectedScalar(Vector128<short>  addend, Vector128<short>  left, Vector128<short>  right, byte rightIndex);
        public static Vector128<uint>   MultiplyAddBySelectedScalar(Vector128<uint>   addend, Vector128<uint>   left, Vector128<uint>   right, byte rightIndex);
        public static Vector128<int>    MultiplyAddBySelectedScalar(Vector128<int>    addend, Vector128<int>    left, Vector128<int>    right, byte rightIndex);

        /// <summary>
        /// Vector multiply subtract
        ///
        /// For each element result[elem] = acc[elem] - left[elem] * right[elem]
        ///
        /// Corresponds to vector forms of ARM64 MLS
        /// </summary>
        public static Vector64<byte>    MultiplySubtract(Vector64<byte>    minuend, Vector64<byte>    left, Vector64<byte>    right);
        public static Vector64<sbyte>   MultiplySubtract(Vector64<sbyte>   minuend, Vector64<sbyte>   left, Vector64<sbyte>   right);
        public static Vector64<ushort>  MultiplySubtract(Vector64<ushort>  minuend, Vector64<ushort>  left, Vector64<ushort>  right);
        public static Vector64<short>   MultiplySubtract(Vector64<short>   minuend, Vector64<short>   left, Vector64<short>   right);
        public static Vector64<uint>    MultiplySubtract(Vector64<uint>    minuend, Vector64<uint>    left, Vector64<uint>    right);
        public static Vector64<int>     MultiplySubtract(Vector64<int>     minuend, Vector64<int>     left, Vector64<int>     right);
        public static Vector128<byte>   MultiplySubtract(Vector128<byte>   minuend, Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<sbyte>  MultiplySubtract(Vector128<sbyte>  minuend, Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<ushort> MultiplySubtract(Vector128<ushort> minuend, Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<short>  MultiplySubtract(Vector128<short>  minuend, Vector128<short>  left, Vector128<short>  right);
        public static Vector128<uint>   MultiplySubtract(Vector128<uint>   minuend, Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<int>    MultiplySubtract(Vector128<int>    minuend, Vector128<int>    left, Vector128<int>    right);

        /// <summary>
        /// Vector multiply subtract by element
        ///
        /// For each element result[elem] = acc[elem] - left[elem] * right
        ///
        /// Corresponds to vector forms of ARM64 MLS
        /// </summary>
        public static Vector64<byte>    MultiplySubtractBySelectedScalar(Vector64<byte>    minuend, Vector64<byte>    left, Vector64<byte>    right, byte rightIndex);
        public static Vector64<sbyte>   MultiplySubtractBySelectedScalar(Vector64<sbyte>   minuend, Vector64<sbyte>   left, Vector64<sbyte>   right, byte rightIndex);
        public static Vector64<ushort>  MultiplySubtractBySelectedScalar(Vector64<ushort>  minuend, Vector64<ushort>  left, Vector64<ushort>  right, byte rightIndex);
        public static Vector64<short>   MultiplySubtractBySelectedScalar(Vector64<short>   minuend, Vector64<short>   left, Vector64<short>   right, byte rightIndex);
        public static Vector64<uint>    MultiplySubtractBySelectedScalar(Vector64<uint>    minuend, Vector64<uint>    left, Vector64<uint>    right, byte rightIndex);
        public static Vector64<int>     MultiplySubtractBySelectedScalar(Vector64<int>     minuend, Vector64<int>     left, Vector64<int>     right, byte rightIndex);
        public static Vector128<byte>   MultiplySubtractBySelectedScalar(Vector128<byte>   minuend, Vector128<byte>   left, Vector128<byte>   right, byte rightIndex);
        public static Vector128<sbyte>  MultiplySubtractBySelectedScalar(Vector128<sbyte>  minuend, Vector128<sbyte>  left, Vector128<sbyte>  right, byte rightIndex);
        public static Vector128<ushort> MultiplySubtractBySelectedScalar(Vector128<ushort> minuend, Vector128<ushort> left, Vector128<ushort> right, byte rightIndex);
        public static Vector128<short>  MultiplySubtractBySelectedScalar(Vector128<short>  minuend, Vector128<short>  left, Vector128<short>  right, byte rightIndex);
        public static Vector128<uint>   MultiplySubtractBySelectedScalar(Vector128<uint>   minuend, Vector128<uint>   left, Vector128<uint>   right, byte rightIndex);
        public static Vector128<int>    MultiplySubtractBySelectedScalar(Vector128<int>    minuend, Vector128<int>    left, Vector128<int>    right, byte rightIndex);

        /// <summary>
        /// Vector fused multiply add
        ///
        /// For each element result[elem] = acc[elem] + left[elem] * right[elem]
        ///
        /// Corresponds to vector forms of ARM64 FMLA
        /// </summary>
        public static Vector64<float>   FusedMultiplyAdd(Vector64<float>   addend, Vector64<float>   left, Vector64<float>   right);
        public static Vector128<float>  FusedMultiplyAdd(Vector128<float>  addend, Vector128<float>  left, Vector128<float>  right);

        /// <summary>
        /// Vector fused multiply subtract
        ///
        /// For each element result[elem] = acc[elem] - left[elem] * right[elem]
        ///
        /// Corresponds to vector forms of ARM64 FMLS
        /// </summary>
        public static Vector64<float>   FusedMultiplySubtract(Vector64<float>   minuend, Vector64<float>   left, Vector64<float>   right);
        public static Vector128<float>  FusedMultiplySubtract(Vector128<float>  minuend, Vector128<float>  left, Vector128<float>  right);


        /// <summary>
        /// Vector polynomial multiply
        /// Corresponds to vector forms of ARM64 PMUL
        /// </summary>
        public static Vector64<byte>    PolynomialMultiply(Vector64<byte>    left, Vector64<byte>    right);
        public static Vector64<sbyte>   PolynomialMultiply(Vector64<sbyte>   left, Vector64<sbyte>   right);
        public static Vector128<byte>   PolynomialMultiply(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<sbyte>  PolynomialMultiply(Vector128<sbyte>  left, Vector128<sbyte>  right);

        /// Vector reciprocal estimate
        ///
        /// See FRECPE docs
        ///
        /// Corresponds to vector forms of ARM64 FRECPE
        /// </summary>
        public static Vector64<float>   ReciprocalEstimate(Vector64<float>   value);
        public static Vector128<float>  ReciprocalEstimate(Vector128<float>  value);

        /// <summary>
        /// Vector reciprocal step
        ///
        /// See FRECPS docs
        ///
        /// Corresponds to vector forms of ARM64 FRECPS
        /// </summary>
        public static Vector64<float>   ReciprocalStep(Vector64<float>   left, Vector64<float>   right);
        public static Vector128<float>  ReciprocalStep(Vector128<float>  left, Vector128<float>  right);

        /// <summary>
        /// Vector reciprocal square root estimate
        ///
        /// See FRSQRTE docs
        ///
        /// Corresponds to vector forms of ARM64 FRSQRTE
        /// </summary>
        public static Vector64<float>   ReciprocalSquareRootEstimate(Vector64<float>   value);
        public static Vector128<float>  ReciprocalSquareRootEstimate(Vector128<float>  value);

        /// <summary>
        /// Vector reciprocal square root step
        ///
        /// See FRSQRTS docs
        ///
        /// Corresponds to vector forms of ARM64 FRSQRTS
        /// </summary>
        public static Vector64<float>   ReciprocalSquareRootStep(Vector64<float>   left, Vector64<float>   right);
        public static Vector128<float>  ReciprocalSquareRootStep(Vector128<float>  left, Vector128<float>  right);

        public partial class Arm64
        {
            /// <summary>
            /// Vector CompareGreaterThanOrEqual
            /// For each element result[elem] = (|left[elem]| >= |right[elem]|) ? ~0 : 0
            /// Corresponds to vector forms of ARM64 FACGE
            /// </summary>
            public static Vector128<double> AbsoluteCompareGreaterThanOrEqual(Vector128<double> left, Vector128<double> right);

            /// <summary>
            /// Vector CompareGreaterThan
            ///
            /// For each element result[elem] = (|left[elem]| > |right[elem]|) ? ~0 : 0
            ///
            /// Corresponds to vector forms of ARM64 FACGT
            /// </summary>
            public static Vector128<double> AbsoluteCompareGreaterThan(Vector128<double> left, Vector128<double> right);

            /// <summary>
            /// Vector absolute difference
            /// Corresponds to vector forms of ARM64 SABD, UABD, and FABD
            /// </summary>
            public static Vector128<double> AbsoluteDifference(Vector128<double> left, Vector128<double> right);

            /// <summary>
            /// Vector add pairwise
            /// For each byte result[byte] = 2*byte < result.Length ? (left[2*byte] + left[2*byte + 1]) : (right[2*byte - result.Length] + right[2*byte + 1 - result.Length])
            /// Corresponds to vector forms of ARM64 ADDP, and FADDP
            /// </summary>
            public static Vector128<byte>   AddPairwise(Vector128<byte>   left, Vector128<byte>   right);
            public static Vector128<sbyte>  AddPairwise(Vector128<sbyte>  left, Vector128<sbyte>  right);
            public static Vector128<ushort> AddPairwise(Vector128<ushort> left, Vector128<ushort> right);
            public static Vector128<short>  AddPairwise(Vector128<short>  left, Vector128<short>  right);
            public static Vector128<uint>   AddPairwise(Vector128<uint>   left, Vector128<uint>   right);
            public static Vector128<int>    AddPairwise(Vector128<int>    left, Vector128<int>    right);
            public static Vector128<long>   AddPairwise(Vector128<long>   left, Vector128<long>   right);
            public static Vector128<ulong>  AddPairwise(Vector128<ulong>  left, Vector128<ulong>  right);
            public static Vector128<float>  AddPairwise(Vector128<float>  left, Vector128<float>  right);
            public static Vector128<double> AddPairwise(Vector128<double> left, Vector128<double> right);

            /// <summary>
            /// Vector extract from pair of vectors
            /// For each byte result[byte] = byte + index < result.Length ? left[byte + index] : right[byte + index - result.Length]
            ///
            /// Note: index must be a JIT time const expression which can be used to populate the literal immediate field
            ///
            /// Corresponds to vector forms of ARM64 EXT
            /// </summary>
            public static Vector128<double> ExtractVector(Vector128<double> left, Vector128<double> right, byte index);

            /// <summary>
            /// Vector add across vector elements
            /// Corresponds to vector forms of ARM64 ADDV
            /// </summary>
            public static Vector64<byte>   AddAcross(Vector64<byte>    value);
            public static Vector64<sbyte>  AddAcross(Vector64<sbyte>   value);
            public static Vector64<ushort> AddAcross(Vector64<ushort>  value);
            public static Vector64<short>  AddAcross(Vector64<short>   value);
            public static Vector64<uint>   AddAcross(Vector64<uint>    value);
            public static Vector64<int>    AddAcross(Vector64<int>     value);
            public static Vector64<byte>   AddAcross(Vector128<byte>   value);
            public static Vector64<sbyte>  AddAcross(Vector128<sbyte>  value);
            public static Vector64<ushort> AddAcross(Vector128<ushort> value);
            public static Vector64<short>  AddAcross(Vector128<short>  value);
            public static Vector64<uint>   AddAcross(Vector128<uint>   value);
            public static Vector64<int>    AddAcross(Vector128<int>    value);

            /// <summary>
            /// Vector max numeric
            /// Corresponds to vector forms of ARM64 FMAXNM
            /// </summary>
            public static Vector128<double> MaxNumber(Vector128<double> left, Vector128<double> right);

            /// <summary>
            /// Vector max numeric pairwise
            ///
            /// For each element result[elem] = 2*elem < result.Length ? max(left[2*elem], left[2*byte + 1]) : max(right[2*byte - result.Length], right[2*byte + 1 - result.Length])
            ///
            /// Corresponds to vector forms of ARM64 FMAXNMP
            /// </summary>
            public static Vector64<float>   MaxNumberPairwise(Vector64<float>   left, Vector64<float>   right);
            public static Vector128<float>  MaxNumberPairwise(Vector128<float>  left, Vector128<float>  right);
            public static Vector128<double> MaxNumberPairwise(Vector128<double> left, Vector128<double> right);

            /// <summary>
            /// Vector max numeric across
            ///
            /// result = max(value[0], ... , value[length -1])
            ///
            /// Corresponds to vector forms of ARM64 FMAXNMV
            /// </summary>
            public static Vector64<float> MaxNumberAcross(Vector128<float>  value);

            /// <summary>
            /// Vector max pairwise
            ///
            /// For each element result[elem] = 2*elem < result.Length ? max(left[2*elem], left[2*byte + 1]) : max(right[2*byte - result.Length], right[2*byte + 1 - result.Length])
            ///
            /// Corresponds to vector forms of ARM64 SMAXP, UMAXP, and FMAXP
            /// </summary>
            public static Vector128<byte>   MaxPairwise(Vector128<byte>   left, Vector128<byte>   right);
            public static Vector128<sbyte>  MaxPairwise(Vector128<sbyte>  left, Vector128<sbyte>  right);
            public static Vector128<ushort> MaxPairwise(Vector128<ushort> left, Vector128<ushort> right);
            public static Vector128<short>  MaxPairwise(Vector128<short>  left, Vector128<short>  right);
            public static Vector128<uint>   MaxPairwise(Vector128<uint>   left, Vector128<uint>   right);
            public static Vector128<int>    MaxPairwise(Vector128<int>    left, Vector128<int>    right);
            public static Vector128<float>  MaxPairwise(Vector128<float>  left, Vector128<float>  right);
            public static Vector128<double> MaxPairwise(Vector128<double> left, Vector128<double> right);

            /// <summary>
            /// Vector max across
            ///
            /// result = max(value[0], ... , value[length -1])
            ///
            /// Corresponds to vector forms of ARM64 SMAXV, UMAXV, and FMAXV
            /// </summary>
            public static Vector64<byte>   MaxAcross(Vector64<byte>    value);
            public static Vector64<sbyte>  MaxAcross(Vector64<sbyte>   value);
            public static Vector64<ushort> MaxAcross(Vector64<ushort>  value);
            public static Vector64<short>  MaxAcross(Vector64<short>   value);
            public static Vector64<uint>   MaxAcross(Vector64<uint>    value);
            public static Vector64<int>    MaxAcross(Vector64<int>     value);
            public static Vector64<float>  MaxAcross(Vector64<float>   value);
            public static Vector64<byte>   MaxAcross(Vector128<byte>   value);
            public static Vector64<sbyte>  MaxAcross(Vector128<sbyte>  value);
            public static Vector64<ushort> MaxAcross(Vector128<ushort> value);
            public static Vector64<short>  MaxAcross(Vector128<short>  value);
            public static Vector64<uint>   MaxAcross(Vector128<uint>   value);
            public static Vector64<int>    MaxAcross(Vector128<int>    value);
            public static Vector64<ulong>  MaxAcross(Vector128<ulong>  value);
            public static Vector64<long>   MaxAcross(Vector128<long>   value);
            public static Vector64<float>  MaxAcross(Vector128<float>  value);
            public static Vector64<double> MaxAcross(Vector128<double> value);

// Not reviewed:
//
//            /// <summary>
//            /// Vector min numeric
//            /// Corresponds to vector forms of ARM64 FMINNM
//            /// </summary>
//            public static Vector128<double> MinNumber(Vector128<double> left, Vector128<double> right);
//
//            /// <summary>
//            /// Vector min numeric pairwise
//            ///
//            /// For each element result[elem] = 2*elem < result.Length ? min(left[2*elem], left[2*byte + 1]) : min(right[2*byte - result.Length], right[2*byte + 1 - result.Length])
//            ///
//            /// Corresponds to vector forms of ARM64 FMINNMP
//            /// </summary>
//            public static Vector64<float>   MinNumberPairwise(Vector64<float>   left, Vector64<float>   right);
//            public static Vector128<float>  MinNumberPairwise(Vector128<float>  left, Vector128<float>  right);
//            public static Vector128<double> MinNumberPairwise(Vector128<double> left, Vector128<double> right);
//
//            /// <summary>
//            /// Vector min numeric across
//            ///
//            /// result = min(value[0], ... , value[length -1])
//            ///
//            /// Corresponds to vector forms of ARM64 FMINNMV
//            /// </summary>
//            public static float  MinNumberAcross(Vector128<float>  value);
//
//            /// <summary>
//            /// Vector min pairwise
//            ///
//            /// For each element result[elem] = 2*elem < result.Length ? min(left[2*elem], left[2*byte + 1]) : min(right[2*byte - result.Length], right[2*byte + 1 - result.Length])
//            ///
//            /// Corresponds to vector forms of ARM64 SMINP, UMINP, and FMINP
//            /// </summary>
//            public static Vector128<byte>   MinPairwise(Vector128<byte>   left, Vector128<byte>   right);
//            public static Vector128<sbyte>  MinPairwise(Vector128<sbyte>  left, Vector128<sbyte>  right);
//            public static Vector128<ushort> MinPairwise(Vector128<ushort> left, Vector128<ushort> right);
//            public static Vector128<short>  MinPairwise(Vector128<short>  left, Vector128<short>  right);
//            public static Vector128<uint>   MinPairwise(Vector128<uint>   left, Vector128<uint>   right);
//            public static Vector128<int>    MinPairwise(Vector128<int>    left, Vector128<int>    right);
//            public static Vector128<float>  MinPairwise(Vector128<float>  left, Vector128<float>  right);
//            public static Vector128<double> MinPairwise(Vector128<double> left, Vector128<double> right);
//
//            /// <summary>
//            /// Vector min across
//            ///
//            /// result = max(value[0], ... , value[length -1])
//            ///
//            /// Corresponds to vector forms of ARM64 SMINV, UMINV, and FMINV
//            /// </summary>
//            public static byte   MinAcross(Vector64<byte>    value);
//            public static sbyte  MinAcross(Vector64<sbyte>   value);
//            public static ushort MinAcross(Vector64<ushort>  value);
//            public static short  MinAcross(Vector64<short>   value);
//            public static uint   MinAcross(Vector64<uint>    value);
//            public static int    MinAcross(Vector64<int>     value);
//            public static float  MinAcross(Vector64<float>   value);
//            public static byte   MinAcross(Vector128<byte>   value);
//            public static sbyte  MinAcross(Vector128<sbyte>  value);
//            public static ushort MinAcross(Vector128<ushort> value);
//            public static short  MinAcross(Vector128<short>  value);
//            public static uint   MinAcross(Vector128<uint>   value);
//            public static int    MinAcross(Vector128<int>    value);
//            public static float  MinAcross(Vector128<float>  value);
//            public static double MinAcross(Vector128<double> value);
//
//            /// <summary>
//            /// Vector fused multiply add
//            ///
//            /// For each element result[elem] = acc[elem] + left[elem] * right[elem]
//            ///
//            /// Corresponds to vector forms of ARM64 FMLA
//            /// </summary>
//            public static Vector128<double> FusedMultiplyAdd(Vector128<double> acc, Vector128<double> left, Vector128<double> right);
//
//            /// <summary>
//            /// Vector fused multiply add by element
//            ///
//            /// For each element result[elem] = acc[elem] + left[elem] * right
//            ///
//            /// Corresponds to vector forms of ARM64 FMLA
//            /// </summary>
//            public static Vector64<float>   FusedMultiplyAdd(Vector64<float>   acc, Vector64<float>   left, float   right);
//            public static Vector128<float>  FusedMultiplyAdd(Vector128<float>  acc, Vector128<float>  left, float   right);
//            public static Vector128<float>  FusedMultiplyAdd(Vector128<float>  acc, Vector128<float>  left, float   right);
//
//            /// <summary>
//            /// Vector fused multiply subtract
//            ///
//            /// For each element result[elem] = acc[elem] - left[elem] * right[elem]
//            ///
//            /// Corresponds to vector forms of ARM64 FMLS
//            /// </summary>
//            public static Vector128<double>  FusedMultiplySubtract(Vector128<double>  acc, Vector128<double>  left, Vector128<double>  right);
//
//            /// <summary>
//            /// Vector fused multiply subtract by element
//            ///
//            /// For each element result[elem] = acc[elem] - left[elem] * right
//            ///
//            /// Corresponds to vector forms of ARM64 FMLS
//            /// </summary>
//            public static Vector64<float>   FusedMultiplySubtract(Vector64<float>   acc, Vector64<float>   left, float   right);
//            public static Vector128<float>  FusedMultiplySubtract(Vector128<float>  acc, Vector128<float>  left, float   right);
//            public static Vector128<double> FusedMultiplySubtract(Vector128<double> acc, Vector128<double> left, float   right);
//
//            /// <summary>
//            /// Vector multiply extend
//            ///
//            /// For each element result[elem] = left[elem] * right[elem]
//            /// Handle extend special cases zero and infinite.  FMULX
//            ///
//            /// Corresponds to vector forms of ARM64 FMULX
//            /// </summary>
//            public static Vector64<float>   MultiplyExtend(Vector64<float>   left, Vector64<float>   right);
//            public static Vector128<float>  MultiplyExtend(Vector128<float>  left, Vector128<float>  right);
//            public static Vector128<double> MultiplyExtend(Vector128<double> left, Vector128<double> right);
//
//            /// <summary>
//            /// Vector multiply extend by element
//            ///
//            /// For each element result[elem] = left[elem] * right
//            /// Handle extend special cases zero and infinite.  FMULX
//            ///
//            /// Corresponds to vector forms of ARM64 FMULX
//            /// </summary>
//            public static Vector64<float>   MultiplyExtend(Vector64<float>   left, float  right);
//            public static Vector128<float>  MultiplyExtend(Vector128<float>  left, float  right);
//            public static Vector128<double> MultiplyExtend(Vector128<double> left, double right);
//
//            /// Vector reciprocal estimate
//            ///
//            /// See FRECPE docs
//            ///
//            /// Corresponds to vector forms of ARM64 FRECPE
//            /// </summary>
//            public static Vector128<double> ReciprocalEstimate(Vector128<double> value);
//
//            /// <summary>
//            /// Vector reciprocal step
//            ///
//            /// See FRECPS docs
//            ///
//            /// Corresponds to vector forms of ARM64 FRECPS
//            /// </summary>
//            public static Vector128<double> ReciprocalStep(Vector128<double> left, Vector128<double> right, byte index);
//
//            /// <summary>
//            /// Vector reciprocal square root estimate
//            ///
//            /// See FRSQRTE docs
//            ///
//            /// Corresponds to vector forms of ARM64 FRSQRTE
//            /// </summary>
//            public static Vector128<double> ReciprocalSquareRootEstimate(Vector128<double> value);
//
//            /// <summary>
//            /// Vector reciprocal square root step
//            ///
//            /// See FRSQRTS docs
//            ///
//            /// Corresponds to vector forms of ARM64 FRSQRTS
//            /// </summary>
//            public static Vector128<double> ReciprocalSquareRootEstimate(Vector128<double> left, Vector128<double> right, byte index);
//
//            /// <summary>
//            /// Vector reverse byte bits
//            /// Corresponds to vector forms of ARM64 RBIT
//            /// </summary>
//            public static Vector64<byte>    ReverseElementBits(Vector64<byte>    value);
//            public static Vector64<sbyte>   ReverseElementBits(Vector64<sbyte>   value);
//            public static Vector128<byte>   ReverseElementBits(Vector128<byte>   value);
//            public static Vector128<sbyte>  ReverseElementBits(Vector128<sbyte>  value);
        }
  }
}
```
