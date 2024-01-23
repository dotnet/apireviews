# API Review 01/23/2024

## Arm64: FEAT_SVE: counting

**Approved** | [#runtime/94003](https://github.com/dotnet/runtime/issues/94003#issuecomment-1906729515) | [Video](https://www.youtube.com/watch?v=pP36q4j_SXE&t=0h0m0s)

* There's a `static bool IsSupported { get; }`, but described in a different partial/issue.
* The parameterless overloads of Count{N}BitElements were removed.
* The mask pattern of Count{N}BitElements were marked `[ConstantExpected]`
* GetActiveElementCount should be for all integral types (add sbyte, short, int, long) and also float/double
* All parameters named `op` are now named `value`
* All `imm_factor` parameters were renamed `scale` and marked `[ConstantExpected]`, and changed from ulong to byte.
* SaturatingDecrementByteElementCount got renamed to SaturatingDecrementBy8BitElementCount (insert "By", rename "Byte" to "8Bit"), and similarly for bigger types.
* SaturatingDecrementBy8BitElementCount (and friends), we removed the 2 argument overloads, swapped the 2nd and 3rd parameter, and defaulted the SveMaskPattern parameter
* In SveMaskPattern:
  * `SV_ALL` => `All`
  * `SV_MUL{N}` => `LargestMultipleOf{N}`
  * `SV_VL{N}` => `VectorCount{N}`
  * `SV_POW2` => `LargestPowerOf2`
* SveMaskPattern was moved from being nested in Sve to being directly in Intrinsics.Arm

```c#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sve : AdvSimd /// Feature: FEAT_SVE  Category: counting
{
  public static unsafe ulong Count16BitElements([ConstantExpected] SveMaskPattern pattern); // CNTH
  public static unsafe ulong Count32BitElements([ConstantExpected] SveMaskPattern pattern); // CNTW
  public static unsafe ulong Count64BitElements([ConstantExpected] SveMaskPattern pattern); // CNTD
  public static unsafe ulong Count8BitElements([ConstantExpected] SveMaskPattern pattern); // CNTB

  /// T: byte, ushort, uint, ulong
  public static unsafe ulong GetActiveElementCount(Vector<T> mask, Vector<T> from); // CNTP

  /// T: [byte, sbyte], [ushort, short], [uint, int], [ulong, long]
  public static unsafe Vector<T> LeadingSignCount(Vector<T2> value); // CLS // predicated, MOVPRFX

  /// T: [byte, sbyte], [ushort, short], [uint, int], [ulong, long]
  public static unsafe Vector<T> LeadingZeroCount(Vector<T2> value); // CLZ // predicated, MOVPRFX

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> LeadingZeroCount(Vector<T> value); // CLZ // predicated, MOVPRFX

  /// T: [uint, float], [ulong, double], [byte, sbyte], [ushort, short], [uint, int], [ulong, long]
  public static unsafe Vector<T> PopCount(Vector<T2> value); // CNT // predicated, MOVPRFX

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> PopCount(Vector<T> value); // CNT // predicated, MOVPRFX

  /// T: byte, ushort, uint, ulong
  public static unsafe int SaturatingDecrementByActiveElementCount(int value, Vector<T> from); // SQDECP

  /// T: byte, ushort, uint, ulong
  public static unsafe long SaturatingDecrementByActiveElementCount(long value, Vector<T> from); // SQDECP

  /// T: byte, ushort, uint, ulong
  public static unsafe uint SaturatingDecrementByActiveElementCount(uint value, Vector<T> from); // UQDECP

  /// T: byte, ushort, uint, ulong
  public static unsafe ulong SaturatingDecrementByActiveElementCount(ulong value, Vector<T> from); // UQDECP

  /// T: short, int, long, ushort, uint, ulong
  public static unsafe Vector<T> SaturatingDecrementByActiveElementCount(Vector<T> value, Vector<T> from); // SQDECP or UQDECP // MOVPRFX

  public static unsafe int SaturatingDecrementBy8BitElementCount(int value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQDECB

  public static unsafe long SaturatingDecrementBy8BitElementCount(long value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQDECB

  public static unsafe uint SaturatingDecrementBy8BitElementCount(uint value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQDECB

  public static unsafe ulong SaturatingDecrementBy8BitElementCount(ulong value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQDECB

  public static unsafe int SaturatingDecrementBy16BitElementCount(int value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQDECH

  public static unsafe long SaturatingDecrementBy16BitElementCount(long value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQDECH

  public static unsafe uint SaturatingDecrementBy16BitElementCount(uint value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQDECH

  public static unsafe ulong SaturatingDecrementBy16BitElementCount(ulong value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQDECH

  /// T: short, ushort
  public static unsafe Vector<T> SaturatingDecrementBy16BitElementCount(Vector<T> value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQDECH or UQDECH // MOVPRFX

  public static unsafe int SaturatingDecrementBy32BitElementCount(int value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQDECW

  public static unsafe long SaturatingDecrementBy32BitElementCount(long value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQDECW

  public static unsafe uint SaturatingDecrementBy32BitElementCount(uint value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQDECW

  public static unsafe ulong SaturatingDecrementBy32BitElementCount(ulong value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQDECW

  /// T: int, uint
  public static unsafe Vector<T> SaturatingDecrementBy32BitElementCount(Vector<T> value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQDECW or UQDECW // MOVPRFX

  public static unsafe int SaturatingDecrementBy64BitElementCount(int value, [ConstantExpected] byte scale); // SQDECD

  public static unsafe long SaturatingDecrementBy64BitElementCount(long value, [ConstantExpected] byte scale); // SQDECD

  public static unsafe uint SaturatingDecrementBy64BitElementCount(uint value, [ConstantExpected] byte scale); // UQDECD

  public static unsafe ulong SaturatingDecrementBy64BitElementCount(ulong value, [ConstantExpected] byte scale); // UQDECD

  public static unsafe int SaturatingDecrementBy64BitElementCount(int value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQDECD

  public static unsafe long SaturatingDecrementBy64BitElementCount(long value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQDECD

  public static unsafe uint SaturatingDecrementBy64BitElementCount(uint value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQDECD

  public static unsafe ulong SaturatingDecrementBy64BitElementCount(ulong value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQDECD

  /// T: long, ulong
  public static unsafe Vector<T> SaturatingDecrementBy64BitElementCount(Vector<T> value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQDECD or UQDECD // MOVPRFX

  /// T: byte, ushort, uint, ulong
  public static unsafe int SaturatingIncrementByActiveElementCount(int value, Vector<T> from); // SQINCP

  /// T: byte, ushort, uint, ulong
  public static unsafe long SaturatingIncrementByActiveElementCount(long value, Vector<T> from); // SQINCP

  /// T: byte, ushort, uint, ulong
  public static unsafe uint SaturatingIncrementByActiveElementCount(uint value, Vector<T> from); // UQINCP

  /// T: byte, ushort, uint, ulong
  public static unsafe ulong SaturatingIncrementByActiveElementCount(ulong value, Vector<T> from); // UQINCP

  /// T: short, int, long, ushort, uint, ulong
  public static unsafe Vector<T> SaturatingIncrementByActiveElementCount(Vector<T> value, Vector<T> from); // SQINCP or UQINCP // MOVPRFX

  public static unsafe int SaturatingIncrementBy8BitElementCount(int value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQINCB

  public static unsafe long SaturatingIncrementBy8BitElementCount(long value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQINCB

  public static unsafe uint SaturatingIncrementBy8BitElementCount(uint value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQINCB

  public static unsafe ulong SaturatingIncrementBy8BitElementCount(ulong value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQINCB

  public static unsafe int SaturatingIncrementBy16BitElementCount(int value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQINCH

  public static unsafe long SaturatingIncrementBy16BitElementCount(long value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQINCH

  public static unsafe uint SaturatingIncrementBy16BitElementCount(uint value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQINCH

  public static unsafe ulong SaturatingIncrementBy16BitElementCount(ulong value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQINCH

  /// T: short, ushort
  public static unsafe Vector<T> SaturatingIncrementBy16BitElementCount(Vector<T> value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQINCH or UQINCH // MOVPRFX

  public static unsafe int SaturatingIncrementBy32BitElementCount(int value, [ConstantExpected] byte scale); // SQINCW

  public static unsafe long SaturatingIncrementBy32BitElementCount(long value, [ConstantExpected] byte scale); // SQINCW

  public static unsafe uint SaturatingIncrementBy32BitElementCount(uint value, [ConstantExpected] byte scale); // UQINCW

  public static unsafe ulong SaturatingIncrementBy32BitElementCount(ulong value, [ConstantExpected] byte scale); // UQINCW

  public static unsafe int SaturatingIncrementBy32BitElementCount(int value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQINCW

  public static unsafe long SaturatingIncrementBy32BitElementCount(long value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQINCW

  public static unsafe uint SaturatingIncrementBy32BitElementCount(uint value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQINCW

  public static unsafe ulong SaturatingIncrementBy32BitElementCount(ulong value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQINCW

  /// T: int, uint
  public static unsafe Vector<T> SaturatingIncrementBy32BitElementCount(Vector<T> value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQINCW or UQINCW // MOVPRFX

  public static unsafe int SaturatingIncrementBy64BitElementCount(int value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQINCD

  public static unsafe long SaturatingIncrementBy64BitElementCount(long value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQINCD

  public static unsafe uint SaturatingIncrementBy64BitElementCount(uint value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQINCD

  public static unsafe ulong SaturatingIncrementBy64BitElementCount(ulong value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // UQINCD

  /// T: long, ulong
  public static unsafe Vector<T> SaturatingIncrementBy64BitElementCount(Vector<T> value, [ConstantExpected] byte scale); // SQINCD or UQINCD // MOVPRFX

  /// T: long, ulong
  public static unsafe Vector<T> SaturatingIncrementBy64BitElementCount(Vector<T> value, [ConstantExpected] byte scale, [ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // SQINCD or UQINCD // MOVPRFX


}

  // All patterns used by PTRUE.
  public enum SveMaskPattern : byte
  {
    LargestPowerOf2 = 0,   // The largest power of 2.
    VectorCount1 = 1,    // 1 element.
    VectorCount2 = 2,    // 2 elements.
    VectorCount3 = 3,    // 3 elements.
    VectorCount4 = 4,    // 4 elements.
    VectorCount5 = 5,    // 5 elements.
    VectorCount6 = 6,    // 6 elements.
    VectorCount7 = 7,    // 7 elements.
    VectorCount8 = 8,    // 8 elements.
    VectorCount16 = 9,   // 16 elements.
    VectorCount32 = 10,  // 32 elements.
    VectorCount64 = 11,  // 64 elements.
    VectorCount128 = 12, // 128 elements.
    VectorCount256 = 13, // 256 elements.
    LargestMultipleOf3 = 29,  // The largest multiple of 3.
    LargestMultipleOf4 = 30,  // The largest multiple of 4.
    All  = 31    // All available (implicitly a multiple of two).
  };
```
## Arm64: FEAT_SVE: bitwise

**Approved** | [#runtime/93887](https://github.com/dotnet/runtime/issues/93887#issuecomment-1906751301) | [Video](https://www.youtube.com/watch?v=pP36q4j_SXE&t=0h51m18s)

* Cnot => BooleanNot => BooleanNot
* ShiftRightArithmeticDivide => ShiftRightArithmeticForDivide; and `(op1, ulong imm2)` => `(value, [ConstantExpected] byte control)`
* AndAcross/OrAcross should be returning `Vector<T>`, not `T`
* Everything under "Optional" was cut because the JIT can optimize those patterns already, without the overloads.

```c#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract class Sve : AdvSimd /// Feature: FEAT_SVE  Category: bitwise
{
  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> And(Vector<T> left, Vector<T> right); // AND // predicated, MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> AndAcross(Vector<T> value); // ANDV // predicated

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> AndNot(Vector<T> left, Vector<T> right); // NAND // predicated

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> BitwiseClear(Vector<T> left, Vector<T> right); // BIC // predicated, MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> BooleanNot(Vector<T> value); // CNOT // predicated, MOVPRFX

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> InsertIntoShiftedVector(Vector<T> left, T right); // INSR

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> Not(Vector<T> value); // NOT or EOR // predicated, MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> Or(Vector<T> left, Vector<T> right); // ORR // predicated, MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> OrAcross(Vector<T> value); // ORV // predicated

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> OrNot(Vector<T> left, Vector<T> right); // NOR or ORN // predicated

  /// T: [sbyte, byte], [short, ushort], [int, uint], [long, ulong], [sbyte, ulong], [short, ulong], [int, ulong], [byte, ulong], [ushort, ulong], [uint, ulong]
  public static unsafe Vector<T> ShiftLeftLogical(Vector<T> left, Vector<T2> right); // LSL or LSLR // predicated, MOVPRFX

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> ShiftLeftLogical(Vector<T> left, Vector<T> right); // LSL or LSLR // predicated, MOVPRFX

  /// T: [sbyte, byte], [short, ushort], [int, uint], [long, ulong], [sbyte, ulong], [short, ulong], [int, ulong]
  public static unsafe Vector<T> ShiftRightArithmetic(Vector<T> left, Vector<T2> right); // ASR or ASRR // predicated, MOVPRFX

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> ShiftRightArithmeticForDivide(Vector<T> value, [ConstantExpected] byte control); // ASRD // predicated, MOVPRFX

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> ShiftRightLogical(Vector<T> left, Vector<T> right); // LSR or LSRR // predicated, MOVPRFX

  /// T: [byte, ulong], [ushort, ulong], [uint, ulong]
  public static unsafe Vector<T> ShiftRightLogical(Vector<T> left, Vector<T2> right); // LSR // predicated, MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> Xor(Vector<T> left, Vector<T> right); // EOR // predicated, MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> XorAcross(Vector<T> value); // EORV // predicated

}
```
## Arm64: FEAT_SVE: fp

**Approved** | [#runtime/94005](https://github.com/dotnet/runtime/issues/94005#issuecomment-1906796880) | [Video](https://www.youtube.com/watch?v=pP36q4j_SXE&t=1h5m15s)

* AddRotateComplex parameters `(Vector<T> op1, Vector<T> op2, ulong imm_rotation)` => `(Vector<T> left, Vector<T> right, [ConstantExpected] byte rotation)`
* AddSequentialAcross should return `Vector<T>`
* AddSequentialAcross `(T initial, Vector<T> op)` => `(Vector<T> initial, Vector<T> value)`
* MultiplyAddRotateComplex `(Vector<T> op1, Vector<T> op2, Vector<T> op3, ulong imm_rotation)` => `(Vector<T> addend, Vector<T> left, Vector<T> right, [ConstantExpected] byte rotation)`
* `MultiplyAddRotateComplex` with two immediates was renamed to `MultiplyAddRotateComplexBySelectedScalar` for consistency
  * And `ulong imm_index, ulong imm_rotation` => `[ConstantExpected] rightIndex, [ConstantExpected] rotation`
* TrigonometricMultiplyAddCoefficient  `(Vector<T> op1, Vector<T> op2, ulong imm3)` => `(Vector<T> left, Vector<T> right, [ConstantExpected] byte control)`
* TrigonometricSelectCoefficient `(left, right)` => `(value, selector)`
* TrigonometricStartingValue `(left, right)` => `(value, sign)`
* The optional overloads of Scale were cut as the JIT can handle it without these overloads.

```c#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract class Sve : AdvSimd /// Feature: FEAT_SVE  Category: fp
{
  /// T: float, double
  public static unsafe Vector<T> AddRotateComplex(Vector<T> left, Vector<T> right, [ConstantExpected] byte rotation); // FCADD // predicated, MOVPRFX

  /// T: float, double
  public static unsafe Vector<T> AddSequentialAcross(Vector<T> initial, Vector<T> value); // FADDA // predicated

  /// T: [double, float], [double, int], [double, long], [double, uint], [double, ulong]
  public static unsafe Vector<T> ConvertToDouble(Vector<T2> value); // FCVT or SCVTF or UCVTF // predicated, MOVPRFX

  /// T: [int, float], [int, double]
  public static unsafe Vector<T> ConvertToInt32(Vector<T2> value); // FCVTZS // predicated, MOVPRFX

  /// T: [long, float], [long, double]
  public static unsafe Vector<T> ConvertToInt64(Vector<T2> value); // FCVTZS // predicated, MOVPRFX

  /// T: [float, double], [float, int], [float, long], [float, uint], [float, ulong]
  public static unsafe Vector<T> ConvertToSingle(Vector<T2> value); // FCVT or SCVTF or UCVTF // predicated, MOVPRFX

  /// T: [uint, float], [uint, double]
  public static unsafe Vector<T> ConvertToUInt32(Vector<T2> value); // FCVTZU // predicated, MOVPRFX

  /// T: [ulong, float], [ulong, double]
  public static unsafe Vector<T> ConvertToUInt64(Vector<T2> value); // FCVTZU // predicated, MOVPRFX

  /// T: [float, uint], [double, ulong]
  public static unsafe Vector<T> FloatingPointExponentialAccelerator(Vector<T2> value); // FEXPA

  /// T: float, double
  public static unsafe Vector<T> MultiplyAddRotateComplex(Vector<T> addend, Vector<T> left, Vector<T> right, [ConstantExpected] byte rotation); // FCMLA // predicated, MOVPRFX

  public static unsafe Vector<float> MultiplyAddRotateComplexBySelectedScalar(Vector<float> addend, Vector<float> left, Vector<float> right, [ConstantExpected] byte rightIndex, [ConstantExpected] byte rotation); // FCMLA // MOVPRFX

  /// T: float, double
  public static unsafe Vector<T> ReciprocalEstimate(Vector<T> value); // FRECPE

  /// T: float, double
  public static unsafe Vector<T> ReciprocalExponent(Vector<T> value); // FRECPX // predicated, MOVPRFX

  /// T: float, double
  public static unsafe Vector<T> ReciprocalSqrtEstimate(Vector<T> value); // FRSQRTE

  /// T: float, double
  public static unsafe Vector<T> ReciprocalSqrtStep(Vector<T> left, Vector<T> right); // FRSQRTS

  /// T: float, double
  public static unsafe Vector<T> ReciprocalStep(Vector<T> left, Vector<T> right); // FRECPS

  /// T: float, double
  public static unsafe Vector<T> RoundAwayFromZero(Vector<T> value); // FRINTA // predicated, MOVPRFX

  /// T: float, double
  public static unsafe Vector<T> RoundToNearest(Vector<T> value); // FRINTN // predicated, MOVPRFX

  /// T: float, double
  public static unsafe Vector<T> RoundToNegativeInfinity(Vector<T> value); // FRINTM // predicated, MOVPRFX

  /// T: float, double
  public static unsafe Vector<T> RoundToPositiveInfinity(Vector<T> value); // FRINTP // predicated, MOVPRFX

  /// T: float, double
  public static unsafe Vector<T> RoundToZero(Vector<T> value); // FRINTZ // predicated, MOVPRFX

  /// T: [float, int], [double, long]
  public static unsafe Vector<T> Scale(Vector<T> left, Vector<T2> right); // FSCALE // predicated, MOVPRFX

  /// T: float, double
  public static unsafe Vector<T> Sqrt(Vector<T> value); // FSQRT // predicated, MOVPRFX

  /// T: float, double
  public static unsafe Vector<T> TrigonometricMultiplyAddCoefficient(Vector<T> left, Vector<T> right, [ConstantExpected] byte control); // FTMAD // MOVPRFX

  /// T: [float, uint], [double, ulong]
  public static unsafe Vector<T> TrigonometricSelectCoefficient(Vector<T> value, Vector<T2> selector); // FTSSEL

  /// T: [float, uint], [double, ulong]
  public static unsafe Vector<T> TrigonometricStartingValue(Vector<T> value, Vector<T2> sign); // FTSMUL
}
```
## Arm64: FEAT_SVE: loads

**Approved** | [#runtime/94006](https://github.com/dotnet/runtime/issues/94006#issuecomment-1906854361) | [Video](https://www.youtube.com/watch?v=pP36q4j_SXE&t=1h31m11s)

* Compute{Byte}Addresses are now Compute{8}BitAddresses
* Renamed 8-bit ComputeAddresses second parameters to `indices` to be consistent with the other sizes.
* All of the Load methods taking a `T* base` are `T* address` for consistency with other Load methods
* All of the methods without "NonFaulting" have had the masks removed
* LoadVectorByteSignExtendNonFaultingToInt16 => LoadVectorSByteSignExtendNonFaultingToInt16, because they are operating on sbyte
* LoadVectorSByteSignExtendNonFaultingToInt16 => LoadVectorSByteNonFaultingSignExtendToInt16 (move NonFaulting left)
* LoadVector{Type}ZeroExtendNonFaulting{Etc} => NonFaultingZeroExtend
* All optionals are cut, consistent with the others we reviwed today.

* LoadVector x{2,3,4} are not approved, they need a review with more people present.  (x => x? x => X? x => Bulk? x => "Load3Vectors"?)
* Prefetch and SvePrefetchType are also not approved, delayed to the same further review as LoadVector x

```c#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract class Sve : AdvSimd /// Feature: FEAT_SVE  Category: loads
{

  /// T: [uint, int], [ulong, long]
  public static unsafe Vector<T> Compute8BitAddresses(Vector<T> bases, Vector<T2> indices); // ADR

  /// T: uint, ulong
  public static unsafe Vector<T> Compute8BitAddresses(Vector<T> bases, Vector<T> indices); // ADR

  /// T: [uint, int], [ulong, long]
  public static unsafe Vector<T> Compute16BitAddresses(Vector<T> bases, Vector<T2> indices); // ADR

  /// T: uint, ulong
  public static unsafe Vector<T> Compute16BitAddresses(Vector<T> bases, Vector<T> indices); // ADR

  /// T: [uint, int], [ulong, long]
  public static unsafe Vector<T> Compute32BitAddresses(Vector<T> bases, Vector<T2> indices); // ADR

  /// T: uint, ulong
  public static unsafe Vector<T> Compute32BitAddresses(Vector<T> bases, Vector<T> indices); // ADR

  /// T: [uint, int], [ulong, long]
  public static unsafe Vector<T> Compute64BitAddresses(Vector<T> bases, Vector<T2> indices); // ADR

  /// T: uint, ulong
  public static unsafe Vector<T> Compute64BitAddresses(Vector<T> bases, Vector<T> indices); // ADR

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> LoadVector(T* address); // LD1W or LD1D or LD1B or LD1H

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> LoadVector128AndReplicateToVector(Vector<T> mask, T* address); // LD1RQW or LD1RQD or LD1RQB or LD1RQH

  public static unsafe Vector<short> LoadVectorSByteNonFaultingSignExtendToInt16(Vector<short> mask, sbyte* address); // LDNF1SB

  public static unsafe Vector<int> LoadVectorSByteNonFaultingSignExtendToInt32(Vector<int> mask, sbyte* address); // LDNF1SB

  public static unsafe Vector<long> LoadVectorSByteNonFaultingSignExtendToInt64(Vector<long> mask, sbyte* address); // LDNF1SB

  public static unsafe Vector<ushort> LoadVectorSByteNonFaultingSignExtendToUInt16(Vector<ushort> mask, sbyte* address); // LDNF1SB

  public static unsafe Vector<uint> LoadVectorSByteNonFaultingSignExtendToUInt32(Vector<uint> mask, sbyte* address); // LDNF1SB

  public static unsafe Vector<ulong> LoadVectorSByteNonFaultingSignExtendToUInt64(Vector<ulong> mask, sbyte* address); // LDNF1SB

  public static unsafe Vector<short> LoadVectorSByteSignExtendToInt16(sbyte* address); // LD1SB

  public static unsafe Vector<int> LoadVectorSByteSignExtendToInt32(sbyte* address); // LD1SB

  public static unsafe Vector<long> LoadVectorByteSignExtendToInt64(sbyte* address); // LD1SB

  public static unsafe Vector<ushort> LoadVectorByteSignExtendToUInt16(sbyte* address); // LD1SB

  public static unsafe Vector<uint> LoadVectorByteSignExtendToUInt32(sbyte* address); // LD1SB

  public static unsafe Vector<ulong> LoadVectorByteSignExtendToUInt64(sbyte* address); // LD1SB

  public static unsafe Vector<short> LoadVectorByteNonFaultingZeroExtendToInt16(Vector<short> mask, byte* address); // LDNF1B

  public static unsafe Vector<int> LoadVectorByteNonFaultingZeroExtendToInt32(Vector<int> mask, byte* address); // LDNF1B

  public static unsafe Vector<long> LoadVectorByteNonFaultingZeroExtendToInt64(Vector<long> mask, byte* address); // LDNF1B

  public static unsafe Vector<ushort> LoadVectorByteNonFaultingZeroExtendToUInt16(Vector<ushort> mask, byte* address); // LDNF1B

  public static unsafe Vector<uint> LoadVectorByteNonFaultingZeroExtendToUInt32(Vector<uint> mask, byte* address); // LDNF1B

  public static unsafe Vector<ulong> LoadVectorByteNonFaultingZeroExtendToUInt64(Vector<ulong> mask, byte* address); // LDNF1B

  public static unsafe Vector<short> LoadVectorByteZeroExtendToInt16(byte* address); // LD1B

  public static unsafe Vector<int> LoadVectorByteZeroExtendToInt32(byte* address); // LD1B

  public static unsafe Vector<long> LoadVectorByteZeroExtendToInt64(byte* address); // LD1B

  public static unsafe Vector<ushort> LoadVectorByteZeroExtendToUInt16(byte* address); // LD1B

  public static unsafe Vector<uint> LoadVectorByteZeroExtendToUInt32(byte* address); // LD1B

  public static unsafe Vector<ulong> LoadVectorByteZeroExtendToUInt64(byte* address); // LD1B

  public static unsafe Vector<int> LoadVectorInt16NonFaultingSignExtendToInt32(Vector<int> mask, short* address); // LDNF1SH

  public static unsafe Vector<long> LoadVectorInt16NonFaultingSignExtendToInt64(Vector<long> mask, short* address); // LDNF1SH

  public static unsafe Vector<uint> LoadVectorInt16NonFaultingSignExtendToUInt32(Vector<uint> mask, short* address); // LDNF1SH

  public static unsafe Vector<ulong> LoadVectorInt16NonFaultingSignExtendToUInt64(Vector<ulong> mask, short* address); // LDNF1SH

  public static unsafe Vector<int> LoadVectorInt16SignExtendToInt32(short* address); // LD1SH

  public static unsafe Vector<long> LoadVectorInt16SignExtendToInt64(short* address); // LD1SH

  public static unsafe Vector<uint> LoadVectorInt16SignExtendToUInt32(short* address); // LD1SH

  public static unsafe Vector<ulong> LoadVectorInt16SignExtendToUInt64(short* address); // LD1SH

  public static unsafe Vector<int> LoadVectorInt16NonFaultingZeroExtendToInt32(Vector<int> mask, ushort* address); // LDNF1H

  public static unsafe Vector<long> LoadVectorInt16NonFaultingZeroExtendToInt64(Vector<long> mask, ushort* address); // LDNF1H

  public static unsafe Vector<uint> LoadVectorInt16NonFaultingZeroExtendToUInt32(Vector<uint> mask, ushort* address); // LDNF1H

  public static unsafe Vector<ulong> LoadVectorInt16NonFaultingZeroExtendToUInt64(Vector<ulong> mask, ushort* address); // LDNF1H

  public static unsafe Vector<int> LoadVectorInt16ZeroExtendToInt32(ushort* address); // LD1H

  public static unsafe Vector<long> LoadVectorInt16ZeroExtendToInt64(ushort* address); // LD1H

  public static unsafe Vector<uint> LoadVectorInt16ZeroExtendToUInt32(ushort* address); // LD1H

  public static unsafe Vector<ulong> LoadVectorInt16ZeroExtendToUInt64(ushort* address); // LD1H

  public static unsafe Vector<long> LoadVectorInt32NonFaultingSignExtendToInt64(Vector<long> mask, int* address); // LDNF1SW

  public static unsafe Vector<ulong> LoadVectorInt32NonFaultingSignExtendToUInt64(Vector<ulong> mask, int* address); // LDNF1SW

  public static unsafe Vector<long> LoadVectorInt32SignExtendToInt64(int* address); // LD1SW

  public static unsafe Vector<ulong> LoadVectorInt32SignExtendToUInt64(int* address); // LD1SW

  public static unsafe Vector<long> LoadVectorInt32NonFaultingZeroExtendToInt64(Vector<long> mask, uint* address); // LDNF1W

  public static unsafe Vector<ulong> LoadVectorInt32NonFaultingZeroExtendToUInt64(Vector<ulong> mask, uint* address); // LDNF1W

  public static unsafe Vector<long> LoadVectorInt32ZeroExtendToInt64(uint* address); // LD1W

  public static unsafe Vector<ulong> LoadVectorInt32ZeroExtendToUInt64(uint* address); // LD1W

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> LoadVectorNonFaulting(Vector<T> mask, T* address); // LDNF1W or LDNF1D or LDNF1B or LDNF1H

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> LoadVectorNonTemporal(T* address); // LDNT1W or LDNT1D or LDNT1B or LDNT1H
}
```
