# API Review 02/08/2024

## Arm64: FEAT_SVE: loads pt 2

**Approved** | [#runtime/97831](https://github.com/dotnet/runtime/issues/97831#issuecomment-1934714477) | [Video](https://www.youtube.com/watch?v=HTgx7dPf80Q&t=0h0m0s)

* LoadVectorx2 (et al) is now Load2xVector (et al)
  * There are already-checked-in, but not released, LoadVector128x2 and friends that should be updated to match this new pattern
* `SvePrefetchType op` => `[ConstantExpected] SvePrefetchType prefetchType`

```c#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract class Sve : AdvSimd /// Feature: FEAT_SVE  Category: loads
{
    /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
    public static unsafe (Vector<T>, Vector<T>) Load2xVector(Vector<T> mask, const T *address); // LD2W or LD2D or LD2B or LD2H

    /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
    public static unsafe (Vector<T>, Vector<T>, Vector<T>) Load3xVector(Vector<T> mask, const T *address); // LD3W or LD3D or LD3B or LD3H

    /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
    public static unsafe (Vector<T>, Vector<T>, Vector<T>, Vector<T>) Load4xVector(Vector<T> mask, const T *address); // LD4W or LD4D or LD4B or LD4H

    /// T: byte, sbyte
    public static unsafe void Prefetch8Bit(Vector<T> mask, void *address, [ConstantExpected] SvePrefetchType prefetchType); // PRFB

    /// T: short, ushort
    public static unsafe void Prefetch16Bit(Vector<T> mask, void *address, [ConstantExpected] SvePrefetchType prefetchType); // PRFH

    /// T: int, uint
    public static unsafe void Prefetch32Bit(Vector<T> mask, void *address, [ConstantExpected] SvePrefetchType prefetchType); // PRFW

    /// T: long, ulong
    public static unsafe void Prefetch64Bit(Vector<T> mask, void *address, [ConstantExpected] SvePrefetchType prefetchType); // PRFD
}
```
## Arm64: FEAT_SVE: scatter stores

**Approved** | [#runtime/94014](https://github.com/dotnet/runtime/issues/94014#issuecomment-1934739714) | [Video](https://www.youtube.com/watch?v=HTgx7dPf80Q&t=0h31m6s)

* Because ScatterNarrowing(Vector,Vector,Vector) is an ambiguous overload across saving as 8/16/32 bits, all of the narrowings are renamed to ScatterNBitNarrowing(Vector,Vector,Vector)
  * But the non-narrowing ones can all stay as just `Scatter`


```c#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sve : AdvSimd /// Feature: FEAT_SVE  Category: scatterstores
{

  /// T: [float, uint], [int, uint], [uint, uint], [double, ulong], [long, ulong], [ulong, ulong]
  public static unsafe void Scatter(Vector<T> mask, Vector<T2> addresses, Vector<T> data); // ST1W or ST1D

  /// T: [float, int], [int, int], [uint, int], [float, uint], [int, uint], [uint, uint], [double, long], [long, long], [ulong, long], [double, ulong], [long, ulong], [ulong, ulong]
  public static unsafe void Scatter(Vector<T> mask, T* address, Vector<T2> indices, Vector<T> data); // ST1W or ST1D

  /// T: [float, int], [int, int], [uint, int], [float, uint], [int, uint], [uint, uint], [double, long], [long, long], [ulong, long], [double, ulong], [long, ulong], [ulong, ulong]
  public static unsafe void ScatterWithByteOffsets(Vector<T> mask, T* address, Vector<T2> offsets, Vector<T> data); // ST1W or ST1D

  public static unsafe void Scatter16BitNarrowing(Vector<int> mask, Vector<uint> addresses, Vector<int> data); // ST1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<int> mask, short* address, Vector<int> offsets, Vector<int> data); // ST1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<uint> mask, ushort* address, Vector<int> offsets, Vector<uint> data); // ST1H

  public static unsafe void Scatter16BitNarrowing(Vector<int> mask, short* address, Vector<int> indices, Vector<int> data); // ST1H

  public static unsafe void Scatter16BitNarrowing(Vector<uint> mask, ushort* address, Vector<int> indices, Vector<uint> data); // ST1H

  public static unsafe void Scatter8BitNarrowing(Vector<int> mask, Vector<uint> addresses, Vector<int> data); // ST1B

  public static unsafe void Scatter8BitWithByteOffsetsNarrowing(Vector<int> mask, sbyte* address, Vector<int> offsets, Vector<int> data); // ST1B

  public static unsafe void Scatter8BitWithByteOffsetsNarrowing(Vector<uint> mask, byte* address, Vector<int> offsets, Vector<uint> data); // ST1B

  public static unsafe void Scatter16BitNarrowing(Vector<long> mask, Vector<ulong> addresses, Vector<long> data); // ST1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<long> mask, short* address, Vector<long> offsets, Vector<long> data); // ST1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<ulong> mask, ushort* address, Vector<long> offsets, Vector<ulong> data); // ST1H

  public static unsafe void Scatter16BitNarrowing(Vector<long> mask, short* address, Vector<long> indices, Vector<long> data); // ST1H

  public static unsafe void Scatter16BitNarrowing(Vector<ulong> mask, ushort* address, Vector<long> indices, Vector<ulong> data); // ST1H

  public static unsafe void Scatter32BitNarrowing(Vector<long> mask, Vector<ulong> addresses, Vector<long> data); // ST1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowing(Vector<long> mask, int* address, Vector<long> offsets, Vector<long> data); // ST1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowing(Vector<ulong> mask, uint* address, Vector<long> offsets, Vector<ulong> data); // ST1W

  public static unsafe void Scatter32BitNarrowing(Vector<long> mask, int* address, Vector<long> indices, Vector<long> data); // ST1W

  public static unsafe void Scatter32BitNarrowing(Vector<ulong> mask, uint* address, Vector<long> indices, Vector<ulong> data); // ST1W

  public static unsafe void Scatter8BitNarrowing(Vector<long> mask, Vector<ulong> addresses, Vector<long> data); // ST1B

  public static unsafe void Scatter8BitWithByteOffsetsNarrowing(Vector<long> mask, sbyte* address, Vector<long> offsets, Vector<long> data); // ST1B

  public static unsafe void Scatter8BitWithByteOffsetsNarrowing(Vector<ulong> mask, byte* address, Vector<long> offsets, Vector<ulong> data); // ST1B

  public static unsafe void Scatter8BitNarrowing(Vector<uint> mask, Vector<uint> addresses, Vector<uint> data); // ST1B

  public static unsafe void Scatter8BitWithByteOffsetsNarrowing(Vector<int> mask, sbyte* address, Vector<uint> offsets, Vector<int> data); // ST1B

  public static unsafe void Scatter8BitWithByteOffsetsNarrowing(Vector<uint> mask, byte* address, Vector<uint> offsets, Vector<uint> data); // ST1B

  public static unsafe void Scatter16BitNarrowing(Vector<uint> mask, Vector<uint> addresses, Vector<uint> data); // ST1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<int> mask, short* address, Vector<uint> offsets, Vector<int> data); // ST1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<uint> mask, ushort* address, Vector<uint> offsets, Vector<uint> data); // ST1H

  public static unsafe void Scatter16BitNarrowing(Vector<int> mask, short* address, Vector<uint> indices, Vector<int> data); // ST1H

  public static unsafe void Scatter16BitNarrowing(Vector<uint> mask, ushort* address, Vector<uint> indices, Vector<uint> data); // ST1H

  public static unsafe void Scatter8BitNarrowing(Vector<ulong> mask, Vector<ulong> addresses, Vector<ulong> data); // ST1B

  public static unsafe void Scatter8BitWithByteOffsetsNarrowing(Vector<long> mask, sbyte* address, Vector<ulong> offsets, Vector<long> data); // ST1B

  public static unsafe void Scatter8BitWithByteOffsetsNarrowing(Vector<ulong> mask, byte* address, Vector<ulong> offsets, Vector<ulong> data); // ST1B

  public static unsafe void Scatter16BitNarrowing(Vector<ulong> mask, Vector<ulong> addresses, Vector<ulong> data); // ST1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<long> mask, short* address, Vector<ulong> offsets, Vector<long> data); // ST1H

  public static unsafe void Scatter16BitWithByteOffsetsNarrowing(Vector<ulong> mask, ushort* address, Vector<ulong> offsets, Vector<ulong> data); // ST1H

  public static unsafe void Scatter16BitNarrowing(Vector<long> mask, short* address, Vector<ulong> indices, Vector<long> data); // ST1H

  public static unsafe void Scatter16BitNarrowing(Vector<ulong> mask, ushort* address, Vector<ulong> indices, Vector<ulong> data); // ST1H

  public static unsafe void Scatter32BitNarrowing(Vector<ulong> mask, Vector<ulong> addresses, Vector<ulong> data); // ST1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowing(Vector<long> mask, int* address, Vector<ulong> offsets, Vector<long> data); // ST1W

  public static unsafe void Scatter32BitWithByteOffsetsNarrowing(Vector<ulong> mask, uint* address, Vector<ulong> offsets, Vector<ulong> data); // ST1W

  public static unsafe void Scatter32BitNarrowing(Vector<long> mask, int* address, Vector<ulong> indices, Vector<long> data); // ST1W

  public static unsafe void Scatter32BitNarrowing(Vector<ulong> mask, uint* address, Vector<ulong> indices, Vector<ulong> data); // ST1W

  /// total method signatures: 62
}
```
## Arm64: FEAT_SVE: stores

**Approved** | [#runtime/94011](https://github.com/dotnet/runtime/issues/94011#issuecomment-1934749679) | [Video](https://www.youtube.com/watch?v=HTgx7dPf80Q&t=0h48m3s)

* Storex{2,3,4} don't need the "xN" suffixes, removed.
* StoreNarrowing, unlike ScatterNarrowing, doesn't have ambiguity problems, so it doesn't need size infixes
* The data input tuples using ValueN is consistent with other intrinsics API.

```c#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sve : AdvSimd /// Feature: FEAT_SVE  Category: stores
{

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe void Store(Vector<T> mask, T* address, Vector<T> data); // ST1W or ST1D or ST1B or ST1H

  /// T: [short, sbyte], [int, short], [int, sbyte], [long, short], [long, int], [long, sbyte]
  /// T: [ushort, byte], [uint, ushort], [uint, byte], [ulong, ushort], [ulong, uint], [ulong, byte]
  public static unsafe void StoreNarrowing(Vector<T> mask, T2* address, Vector<T> data); // ST1B or ST1H or ST1W

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe void StoreNonTemporal(Vector<T> mask, T* address, Vector<T> data); // STNT1W or STNT1D or STNT1B or STNT1H

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe void Store(Vector<T> mask, T* address, (Vector<T> Value1, Vector<T> Value2) data); // ST2W or ST2D or ST2B or ST2H

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe void Store(Vector<T> mask, T* address, (Vector<T> Value1, Vector<T> Value2, Vector<T> Value3) data); // ST3W or ST3D or ST3B or ST3H

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe void Store(Vector<T> mask, T* address, (Vector<T> Value1, Vector<T> Value2, Vector<T> Value3, Vector<T> Value4) data); // ST4W or ST4D or ST4B or ST4H

  /// total method signatures: 17
}
```
## Arm64: FEAT_SVE2: bitwise

**Approved** | [#runtime/94015](https://github.com/dotnet/runtime/issues/94015#issuecomment-1934807250) | [Video](https://www.youtube.com/watch?v=HTgx7dPf80Q&t=0h53m0s)

* BitwiseClearXor should probably have been BitwiseClearAndXor, but since "And" could also be interpreted as "Bitwise AND", it's left out.
  * BitwiseClearXor(op1, op2, op3) => (xor, value, mask), as (value, mask) is already what op2/op3 are called in BitwiseClear.
* Since BitwiseSelectInverted(sel, left, right) is just BitwiseSelect(sel, right, left) it's cut to avoid the confusion of "Inverted", and "RightInverted" having a very high cognative difference.
* We talked for a while on the "Even" and "Odd" suffixes, and decided they're consistent how they're proposed.  There is known confusion in the future (...RoundToEvenOdd), but we'll address those as one-offs, since the suffix pattern is too well established at this point.
* Xor(op1, op2, op3) => Xor(value1, value2, value3)
* XorRotateRight(op1, op2, imm3) => `XorRotateRight(left, right, [ConstantExpected] byte count)`

```c#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract class Sve2 : Sve /// Feature: FEAT_SVE2  Category: bitwise
{
  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  /// op1 ^ (op2 & ~op3)
  public static unsafe Vector<T> BitwiseClearXor(Vector<T> xor, Vector<T> value, Vector<T> mask); // BCAX // MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> BitwiseSelect(Vector<T> select, Vector<T> left, Vector<T> right); // BSL // MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> BitwiseSelectLeftInverted(Vector<T> select, Vector<T> left, Vector<T> right); // BSL1N // MOVPRFX

    /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> BitwiseSelectRightInverted(Vector<T> select, Vector<T> left, Vector<T> right); // BSL2N // MOVPRFX

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> ShiftArithmeticRounded(Vector<T> value, Vector<T> count); // SRSHL or SRSHLR // predicated, MOVPRFX

  /// T: [byte, sbyte], [ushort, short], [uint, int], [ulong, long]
  public static unsafe Vector<T> ShiftLogicalRounded(Vector<T> value, Vector<T2> count); // URSHL or URSHLR // predicated, MOVPRFX

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> ShiftRightArithmeticRounded(Vector<T> value, [ConstantExpected] byte count); // SRSHR // predicated, MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> ShiftRightLogicalRounded(Vector<T> value, [ConstantExpected] byte count); // URSHR // predicated, MOVPRFX

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> ShiftRightArithmeticRoundedAdd(Vector<T> addend, Vector<T> value, [ConstantExpected] byte count); // SRSRA // MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> ShiftRightLogicalRoundedAdd(Vector<T> addend, Vector<T> value, [ConstantExpected] byte count); // URSRA // MOVPRFX

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> ShiftRightLogicalRoundedNarrowingEven(Vector<T2> value, [ConstantExpected] byte count); // RSHRNB

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> ShiftRightLogicalRoundedNarrowingOdd(Vector<T> even, Vector<T2> value, [ConstantExpected] byte count); // RSHRNT

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> ShiftArithmeticRoundedSaturate(Vector<T> value, Vector<T> count); // SQRSHL or SQRSHLR // predicated, MOVPRFX

  /// T: [byte, sbyte], [ushort, short], [uint, int], [ulong, long]
  public static unsafe Vector<T> ShiftLogicalRoundedSaturate(Vector<T> value, Vector<T2> count); // UQRSHL or UQRSHLR // predicated, MOVPRFX

  /// T: [sbyte, short], [short, int], [int, long]
  public static unsafe Vector<T> ShiftRightArithmeticRoundedNarrowingSaturateEven(Vector<T2> value, [ConstantExpected] byte count); // SQRSHRNB

  /// T: [sbyte, short], [short, int], [int, long]
  public static unsafe Vector<T> ShiftRightArithmeticRoundedNarrowingSaturateOdd(Vector<T> even, Vector<T2> value, [ConstantExpected] byte count); // SQRSHRNT 
  
  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> ShiftRightLogicalRoundedNarrowingSaturateEven(Vector<T2> value, [ConstantExpected] byte count); // UQRSHRNB

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> ShiftRightLogicalRoundedNarrowingSaturateOdd(Vector<T> even, Vector<T2> value, [ConstantExpected] byte count); // UQRSHRNT

  /// T: [byte, short], [ushort, int], [uint, long]
  public static unsafe Vector<T> ShiftRightArithmeticRoundedNarrowingSaturateUnsignedEven(Vector<T2> value, [ConstantExpected] byte count); // SQRSHRUNB

  /// T: [byte, short], [ushort, int], [uint, long]
  public static unsafe Vector<T> ShiftRightArithmeticRoundedNarrowingSaturateUnsignedOdd(Vector<T> even, Vector<T2> value, [ConstantExpected] byte count); // SQRSHRUNT

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> ShiftArithmeticSaturate(Vector<T> value, Vector<T> count); // SQSHL or SQSHLR // predicated, MOVPRFX

  /// T: [byte, sbyte], [ushort, short], [uint, int], [ulong, long]
  public static unsafe Vector<T> ShiftLeftLogicalSaturate(Vector<T> value, Vector<T2> count); // UQSHL or UQSHLR // predicated, MOVPRFX

  /// T: [byte, sbyte], [ushort, short], [uint, int], [ulong, long]
  public static unsafe Vector<T> ShiftLeftLogicalSaturateUnsigned(Vector<T2> value, [ConstantExpected] byte count); // SQSHLU // predicated, MOVPRFX

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T>  ShiftRightArithmeticNarrowingSaturateEven(Vector<T2> value, [ConstantExpected] byte count); // SQSHRNB or UQSHRNB

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T>  ShiftRightArithmeticNarrowingSaturateOdd(Vector<T> even, Vector<T2> value, [ConstantExpected] byte count); // SQSHRNT or UQSHRNT

  /// T: [byte, short], [ushort, int], [uint, long]
  public static unsafe Vector<T> ShiftRightArithmeticNarrowingSaturateUnsignedEven(Vector<T2> value, [ConstantExpected] byte count); // SQSHRUNB

  /// T: [byte, short], [ushort, int], [uint, long]
  public static unsafe Vector<T> ShiftRightArithmeticNarrowingSaturateUnsignedOdd(Vector<T> even, Vector<T2> value, [ConstantExpected] byte count); // SQSHRUNT

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> ShiftLeftAndInsert(Vector<T> left, Vector<T> right, [ConstantExpected] byte shift); // SLI

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> ShiftLeftLogicalWideningEven(Vector<T2> value, [ConstantExpected] byte count); // SSHLLB or USHLLB

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> ShiftLeftLogicalWideningOdd(Vector<T2> value, [ConstantExpected] byte count); // SSHLLT or USHLLT

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> ShiftRightArithmeticAdd(Vector<T> addend, Vector<T> value, [ConstantExpected] byte count); // SSRA // MOVPRFX
  
  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> ShiftRightLogicalAdd(Vector<T> addend, Vector<T> value, [ConstantExpected] byte count); // USRA // MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> ShiftRightAndInsert(Vector<T> left, Vector<T> right, [ConstantExpected] byte shift); // SRI

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> ShiftRightLogicalNarrowingEven(Vector<T2> value, [ConstantExpected] byte count); // SHRNB

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> ShiftRightLogicalNarrowingOdd(Vector<T> even, Vector<T2> value, [ConstantExpected] byte count); // SHRNT

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> Xor(Vector<T> value1, Vector<T> value2, Vector<T> value3); // EOR3 // MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> XorRotateRight(Vector<T> left, Vector<T> right, [ConstantExpected] byte count]); // XAR // MOVPRFX

  /// total method signatures: 33
}
```
## Arm64: FEAT_SVE: mask

**Approved** | [#runtime/93964](https://github.com/dotnet/runtime/issues/93964#issuecomment-1934860520) | [Video](https://www.youtube.com/watch?v=HTgx7dPf80Q&t=1h29m51s)

* ConditionalExtractAfterLastActiveElement is representing 3 different modes of an underlying instruction, where one is easy to pattern match, but the other two are hard to distinguish, so it's being broken into 3 variants.
* ConditionalExtractAfterLastActiveElement's `fallback` parameter should be `defaultValue` for the scalar input, `defaultValues` for AndReplicate, and `defaultScalar` for the vector return to indicate only lane 0 is preserved.
* The same two points apply to ConditionalExtractLastActiveElement.
* Review ran out of time on this issue, so the items after ConditionalExtractLastActiveElement need to be moved to a followup issue.

```c#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sve : AdvSimd /// Feature: FEAT_SVE  Category: mask
{
  /// T: float, double
  public static unsafe Vector<T> AbsoluteCompareGreaterThan(Vector<T> left, Vector<T> right); // FACGT // predicated

  /// T: float, double
  public static unsafe Vector<T> AbsoluteCompareGreaterThanOrEqual(Vector<T> left, Vector<T> right); // FACGE // predicated

  /// T: float, double
  public static unsafe Vector<T> AbsoluteCompareLessThan(Vector<T> left, Vector<T> right); // FACGT // predicated

  /// T: float, double
  public static unsafe Vector<T> AbsoluteCompareLessThanOrEqual(Vector<T> left, Vector<T> right); // FACGE // predicated

  /// T: float, double, int, long, uint, ulong
  public static unsafe Vector<T> Compact(Vector<T> mask, Vector<T> value); // COMPACT

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> CompareEqual(Vector<T> left, Vector<T> right); // FCMEQ or CMPEQ // predicated

  /// T: [sbyte, long], [short, long], [int, long]
  public static unsafe Vector<T> CompareEqual(Vector<T> left, Vector<T2> right); // CMPEQ // predicated

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> CompareGreaterThan(Vector<T> left, Vector<T> right); // FCMGT or CMPGT or CMPHI // predicated

  /// T: [sbyte, long], [short, long], [int, long], [byte, ulong], [ushort, ulong], [uint, ulong]
  public static unsafe Vector<T> CompareGreaterThan(Vector<T> left, Vector<T2> right); // CMPGT or CMPHI // predicated

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> CompareGreaterThanOrEqual(Vector<T> left, Vector<T> right); // FCMGE or CMPGE or CMPHS // predicated

  /// T: [sbyte, long], [short, long], [int, long], [byte, ulong], [ushort, ulong], [uint, ulong]
  public static unsafe Vector<T> CompareGreaterThanOrEqual(Vector<T> left, Vector<T2> right); // CMPGE or CMPHS // predicated

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> CompareLessThan(Vector<T> left, Vector<T> right); // FCMGT or CMPGT or CMPHI // predicated

  /// T: [sbyte, long], [short, long], [int, long], [byte, ulong], [ushort, ulong], [uint, ulong]
  public static unsafe Vector<T> CompareLessThan(Vector<T> left, Vector<T2> right); // CMPLT or CMPLO // predicated

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> CompareLessThanOrEqual(Vector<T> left, Vector<T> right); // FCMGE or CMPGE or CMPHS // predicated

  /// T: [sbyte, long], [short, long], [int, long], [byte, ulong], [ushort, ulong], [uint, ulong]
  public static unsafe Vector<T> CompareLessThanOrEqual(Vector<T> left, Vector<T2> right); // CMPLE or CMPLS // predicated

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> CompareNotEqualTo(Vector<T> left, Vector<T> right); // FCMNE or CMPNE // predicated

  /// T: [sbyte, long], [short, long], [int, long]
  public static unsafe Vector<T> CompareNotEqualTo(Vector<T> left, Vector<T2> right); // CMPNE // predicated

  /// T: float, double
  public static unsafe Vector<T> CompareUnordered(Vector<T> left, Vector<T> right); // FCMUO // predicated

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> ConditionalExtractAfterLastActiveElement(Vector<T> mask, Vector<T> defaultScalar, Vector<T> data); // CLASTA // MOVPRFX
  public static unsafe Vector<T> ConditionalExtractAfterLastActiveElementAndReplicate(Vector<T> mask, Vector<T> defaultValues, Vector<T> data); // CLASTA // MOVPRFX
  public static unsafe T ConditionalExtractAfterLastActiveElement(Vector<T> mask, T defaultValue, Vector<T> data); // CLASTA // MOVPRFX

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> ConditionalExtractLastActiveElement(Vector<T> mask, Vector<T> defaultScalar, Vector<T> data); // CLASTB // MOVPRFX
  public static unsafe Vector<T> ConditionalExtractLastActiveElementAndReplicate(Vector<T> mask, Vector<T> defaultValues, Vector<T> data); // CLASTB // MOVPRFX
  public static unsafe T ConditionalExtractLastActiveElement(Vector<T> mask, T defaultValue, Vector<T> data); // CLASTB // MOVPRFX
}
```
