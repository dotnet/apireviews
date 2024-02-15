# API Review 02/15/2024

## Arm64: FEAT_SVE: mask part 2

**Approved** | [#runtime/98184](https://github.com/dotnet/runtime/issues/98184#issuecomment-1946957820) | [Video](https://www.youtube.com/watch?v=_30Ee_VrCsw&t=0h0m0s)

* CreateWhileLessThanMask depends on return overloading, which isn't legal; so it is changing to CreateWhileLessThanMaskNBit, i.e. CreateWhileLessThanMask8Bit, CreateWhileLessThanMask16Bit, CreateWhileLessThanMask32Bit, CreateWhileLessThanMask64Bit
* ExtractAfterLast is expanded to ExtractAfterLastScalar and ExtractAfterLastVector
* ExtractVector's final argument is `[ConstExpected] byte index`, not `ulong index`
* FalseMask() => CreateFalseMask() so it has a verb
    * Then further to CreateFalseMaskByte, CreateFalseMaskSByte, etc, because it needs to vary the return type.
    * And similar to TrueMask
* MaskGetFirstSet => CreateMaskForNextActiveElement
  * (mask, from) => (totalMask, fromMask)
* MaskSetFirst => CreateMaskForFirstActiveElement
  * (mask, from) => (totalMask, fromMask)
* MaskTestAnyTrue => TestAnyTrue
  * (mask, from) => (leftMask, rightMask)
* PropagateBreak => CreateBreakPropagateMask

```c#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sve : AdvSimd /// Feature: FEAT_SVE  Category: mask
{
  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> ConditionalSelect(Vector<T> mask, Vector<T> left, Vector<T> right); // SEL

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateBreakAfterMask(Vector<T> totalMask, Vector<T> fromMask); // BRKA // predicated

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateBreakAfterPropagateMask(Vector<T> mask, Vector<T> left, Vector<T> right); // BRKPA

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateBreakBeforeMask(Vector<T> totalMask, Vector<T> fromMask); // BRKB // predicated

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateBreakBeforePropagateMask(Vector<T> mask, Vector<T> left, Vector<T> right); // BRKPB

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileLessThanMask{8,16,32,64}Bit(int left, int right); // WHILELT

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileLessThanMask{8,16,32,64}Bit(long left, long right); // WHILELT

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileLessThanMask{8,16,32,64}Bit(uint left, uint right); // WHILELO

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileLessThanMask{8,16,32,64}Bit(ulong left, ulong right); // WHILELO

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileLessThanOrEqualMask{8,16,32,64}Bit(int left, int right); // WHILELE

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileLessThanOrEqualMask{8,16,32,64}Bit(long left, long right); // WHILELE

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileLessThanOrEqualMask{8,16,32,64}Bit(uint left, uint right); // WHILELS

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateWhileLessThanOrEqualMask{8,16,32,64}Bit(ulong left, ulong right); // WHILELS

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe T ExtractAfterLastScalar(Vector<T> value); // LASTA // predicated
  public static unsafe Vector<T> ExtractAfterLastVector(Vector<T> value); // LASTA // predicated

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe T ExtractLastScalar(Vector<T> value); // LASTB // predicated
  public static unsafe Vector<T> ExtractLastVector(Vector<T> value); // LASTB // predicated

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> ExtractVector(Vector<T> upper, Vector<T> lower, [ConstExpected] byte index); // EXT // MOVPRFX

  public static unsafe Vector<byte> CreateFalseMaskByte(); // PFALSE
  public static unsafe Vector<sbyte> CreateFalseMaskSByte(); // PFALSE
  // repeat for Int16, UInt16, Int32, UInt32, Int64, UInt64, Single, Double

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateMaskForNextActiveElement(Vector<T> totalMask, Vector<T> fromMask); // PNEXT

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateMaskForFirstActiveElement(Vector<T> totalMask, Vector<T> fromMask); // PFIRST

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe bool TestAnyTrue(Vector<T> leftMask, Vector<T> rightMask); // PTEST

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe bool TestFirstTrue(Vector<T> leftMask, Vector<T> rightMask); // PTEST

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe bool TestLastTrue(Vector<T> leftMask, Vector<T> rightMask); // PTEST

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> CreateBreakPropagateMask(Vector<T> totalMask, Vector<T> fromMask); // BRKN // predicated

  public static unsafe Vector<byte> CreateTrueMaskByte([ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // PTRUE
  public static unsafe Vector<sbyte> CreateTrueMaskSByte([ConstantExpected] SveMaskPattern pattern = SveMaskPattern.All); // PTRUE
  // repeat for Int16, UInt16, Int32, UInt32, Int64, UInt64, Single, Double
}
```
## Arm64: FEAT_SVE2: bit manipulate

**Approved** | [#runtime/94020](https://github.com/dotnet/runtime/issues/94020#issuecomment-1947004562) | [Video](https://www.youtube.com/watch?v=_30Ee_VrCsw&t=0h44m29s)

* InterleavingXorEvenOdd(odd, left, right) => (odd, leftEven, rightOdd)
* ShiftLeft*'s `count` was renamed `shiftAmount` for consistency with the cross-platform APIs.
* The "optional" entries have been cut because the JIT can pattern-match it.

```c#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract class Sve2 : Sve /// Feature: FEAT_SVE2  Category: bitmanipulate
{

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> InterleavingXorEvenOdd(Vector<T> odd, Vector<T> leftEven, Vector<T> rightOdd); // EORBT // MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> InterleavingXorOddEven(Vector<T> even, Vector<T> leftOdd, Vector<T> rightEven); // EORTB // MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> ShiftLeftLogicalWideningEven(Vector<T2> value, [ConstantExpected] byte shiftAmount); // SSHLLB or USHLLB

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> ShiftLeftLogicalWideningOdd(Vector<T2> value, [ConstantExpected] byte shiftAmount); // SSHLLT or USHLLT

  /// T: [float, uint], [double, ulong], [sbyte, byte], [short, ushort], [int, uint], [long, ulong]
  public static unsafe Vector<T> VectorTableLookup((Vector<T>, Vector<T>) table, Vector<T2> indices); // TBL

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> VectorTableLookup((Vector<T>, Vector<T>) table, Vector<T> indices); // TBL

  /// T: [float, uint], [double, ulong], [sbyte, byte], [short, ushort], [int, uint], [long, ulong]
  public static unsafe Vector<T> VectorTableLookupExtension(Vector<T> defaultValues, Vector<T> data, Vector<T2> indices); // TBX

  /// T: byte, ushort, uint, ulong
  public static unsafe Vector<T> VectorTableLookupExtension(Vector<T> defaultValues, Vector<T> data, Vector<T> indices); // TBX
}
```
## Arm64: FEAT_SVE2: maths

**Approved** | [#runtime/94022](https://github.com/dotnet/runtime/issues/94022#issuecomment-1947173250) | [Video](https://www.youtube.com/watch?v=_30Ee_VrCsw&t=0h58m56s)

* AddPairwiseWidening => AddPairwiseWideningAndAdd.
  * (left, right) => (addend, value)
* AddWideningEvenOdd(left, right) => AddWideningEvenOdd(leftEven, rightOdd)
* Multiply => MultiplyBySelectedScalar
* `MultiplyWideningOdd(Vector<T2> op1, Vector<T2> op2, [ConstantExpected] byte rightIndex)` => `MultiplyBySelectedScalarWideningOdd(Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex);`
* PolynomialMultiply also should have an sbyte variant
* MultiplyDoublingWideningLowerAndSubtractSaturateEven (and the other MDW-LowerAndSubtract) should not have had the word Lower in it.
* SubtractBorrowWidening(op1, op2, op3) => (left, right, borrow)

```c#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract class Sve2 : Sve /// Feature: FEAT_SVE2  Category: maths
{

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> AbsoluteDifferenceAdd(Vector<T> addend, Vector<T> left, Vector<T> right); // SABA or UABA // MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> AbsoluteDifferenceWideningLowerAndAddEven(Vector<T> addend, Vector<T2> left, Vector<T2> right); // SABALB or UABALB // MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> AbsoluteDifferenceWideningLowerAndAddOdd(Vector<T> addend, Vector<T2> left, Vector<T2> right); // SABALT or UABALT // MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> AbsoluteDifferenceWideningEven(Vector<T2> left, Vector<T2> right); // SABDLB or UABDLB

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> AbsoluteDifferenceWideningOdd(Vector<T2> left, Vector<T2> right); // SABDLT or UABDLT

  /// T: uint, ulong
  public static unsafe Vector<T> AddCarryWideningEven(Vector<T> left, Vector<T> right, Vector<T> carry); // ADCLB // MOVPRFX

  /// T: uint, ulong
  public static unsafe Vector<T> AddCarryWideningOdd(Vector<T> left, Vector<T> right, Vector<T> carry); // ADCLT // MOVPRFX

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> AddHighNarrowingEven(Vector<T2> left, Vector<T2> right); // ADDHNB

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> AddHighNarrowingOdd(Vector<T> even, Vector<T2> left, Vector<T2> right); // ADDHNT

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> AddPairwise(Vector<T> left, Vector<T> right); // FADDP or ADDP // predicated, MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> AddPairwiseWideningAndAdd(Vector<T> addend, Vector<T> value); // SADALP or UADALP // predicated, MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> AddSaturate(Vector<T> left, Vector<T> right); // SQADD or UQADD // predicated, MOVPRFX

  /// T: [byte, sbyte], [ushort, short], [uint, int], [ulong, long]
  public static unsafe Vector<T> AddSaturate(Vector<T> left, Vector<T2> right); // USQADD // predicated, MOVPRFX

  /// T: [sbyte, byte], [short, ushort], [int, uint], [long, ulong]
  public static unsafe Vector<T> AddSaturate(Vector<T> left, Vector<T2> right); // SUQADD // predicated, MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> AddWideningEven(Vector<T> left, Vector<T2> right); // SADDWB or UADDWB

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> AddWideningOdd(Vector<T> left, Vector<T2> right); // SADDWT or UADDWT

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> AddWideningEven(Vector<T2> left, Vector<T2> right); // SADDLB or UADDLB

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> AddWideningOdd(Vector<T2> left, Vector<T2> right); // SADDLT or UADDLT

  /// T: [short, sbyte], [int, short], [long, int]
  public static unsafe Vector<T> AddWideningEvenOdd(Vector<T2> leftEven, Vector<T2> rightOdd); // SADDLBT

  /// T: [int, sbyte], [long, short]
  public static unsafe Vector<T> DotProductRotateComplex(Vector<T> addend, Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rotation); // CDOT // MOVPRFX

  /// T: [int, sbyte], [long, short]
  public static unsafe Vector<T> DotProductRotateComplexBySelectedIndex(Vector<T> addend, Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex, [ConstantExpected] byte rotation); // CDOT // MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> FusedAddHalving(Vector<T> left, Vector<T> right); // SHADD or UHADD // predicated, MOVPRFX

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> FusedSubtractHalving(Vector<T> left, Vector<T> right); // SHSUB or UHSUB or SHSUBR or UHSUBR // predicated, MOVPRFX

  /// T: float, double
  public static unsafe Vector<T> MaxNumberPairwise(Vector<T> left, Vector<T> right); // FMAXNMP // predicated, MOVPRFX

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> MaxPairwise(Vector<T> left, Vector<T> right); // FMAXP or SMAXP or UMAXP // predicated, MOVPRFX

  /// T: float, double
  public static unsafe Vector<T> MinNumberPairwise(Vector<T> left, Vector<T> right); // FMINNMP // predicated, MOVPRFX

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> MinPairwise(Vector<T> left, Vector<T> right); // FMINP or SMINP or UMINP // predicated, MOVPRFX

  /// T: short, int, long, ushort, uint, ulong
  public static unsafe Vector<T> MultiplyBySelectedScalar(Vector<T> left, Vector<T> right, [ConstantExpected] byte rightTndex); // MUL

  /// T: short, int, long, ushort, uint, ulong
  public static unsafe Vector<T> MultiplyAddBySelectedScalar(Vector<T> addend, Vector<T> left, Vector<T> right, [ConstantExpected] byte rightIndex); // MLA // MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> MultiplyWideningEvenAndAdd(Vector<T> addend, Vector<T2> left, Vector<T2> right); // SMLALB or UMLALB // MOVPRFX

  /// T: [int, short], [long, int], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> MultiplyBySelectedScalarWideningEvenAndAdd(Vector<T> addend, Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex); // SMLALB or UMLALB // MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> MultiplyWideningOddAndAdd(Vector<T> addend, Vector<T2> left, Vector<T2> right); // SMLALT or UMLALT // MOVPRFX

  /// T: [int, short], [long, int], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> MultiplyBySelectedScalarWideningOddAndAdd(Vector<T> addend, Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex); // SMLALT or UMLALT // MOVPRFX

  /// T: short, int, long, ushort, uint, ulong
  public static unsafe Vector<T> MultiplySubtractBySelectedScalar(Vector<T> minuend, Vector<T> left, Vector<T> right, [ConstantExpected] byte rightIndex); // MLS // MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> MultiplyWideningEvenAndSubtract(Vector<T> minuend, Vector<T2> left, Vector<T2> right); // SMLSLB or UMLSLB // MOVPRFX

  /// T: [int, short], [long, int], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> MultiplyBySelectedScalarWideningEvenAndSubtract(Vector<T> minuend, Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex); // SMLSLB or UMLSLB // MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> MultiplyWideningOddAndSubtract(Vector<T> minuend, Vector<T2> left, Vector<T2> right); // SMLSLT or UMLSLT // MOVPRFX

  /// T: [int, short], [long, int], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> MultiplyBySelectedScalarWideningOddAndSubtract(Vector<T> minuend, Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex); // SMLSLT or UMLSLT // MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> MultiplyWideningEven(Vector<T2> left, Vector<T2> right); // SMULLB or UMULLB

  /// T: [int, short], [long, int], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> MultiplyBySelectedScalarWideningEven(Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex); // SMULLB or UMULLB

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> MultiplyWideningOdd(Vector<T2> left, Vector<T2> right); // SMULLT or UMULLT

  /// T: [int, short], [long, int], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> MultiplyBySelectedScalarWideningOdd(Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex); // SMULLT or UMULLT

  public static unsafe Vector<byte> PolynomialMultiply(Vector<byte> left, Vector<byte> right); // PMUL
  public static unsafe Vector<sbyte> PolynomialMultiply(Vector<sbyte> left, Vector<sbyte> right); // PMUL

  /// T: [ushort, byte], [ulong, uint]
  public static unsafe Vector<T> PolynomialMultiplyWideningEven(Vector<T2> left, Vector<T2> right); // PMULLB

  /// T: [ushort, byte], [ulong, uint]
  public static unsafe Vector<T> PolynomialMultiplyWideningOdd(Vector<T2> left, Vector<T2> right); // PMULLT

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> AddRoundedHighNarrowingEven(Vector<T2> left, Vector<T2> right); // RADDHNB

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> AddRoundedHighNarrowingOdd(Vector<T> even, Vector<T2> left, Vector<T2> right); // RADDHNT

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> FusedAddRoundedHalving(Vector<T> left, Vector<T> right); // SRHADD or URHADD // predicated, MOVPRFX

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> SubtractRoundedHighNarrowingEven(Vector<T2> left, Vector<T2> right); // RSUBHNB

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> SubtractRoundedHighNarrowingOdd(Vector<T> even, Vector<T2> left, Vector<T2> right); // RSUBHNT

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> AbsSaturate(Vector<T> value); // SQABS // predicated, MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int]
  public static unsafe Vector<T> MultiplyDoublingWideningAndAddSaturateEven(Vector<T> addend, Vector<T2> left, Vector<T2> right); // SQDMLALB // MOVPRFX

  /// T: [int, short], [long, int]
  public static unsafe Vector<T> MultiplyDoublingWideningBySelectedScalarAndAddSaturateEven(Vector<T> addend, Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex); // SQDMLALB // MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int]
  public static unsafe Vector<T> MultiplyDoublingWideningAndAddSaturateEvenOdd(Vector<T> addend, Vector<T2> leftEven, Vector<T2> rightOdd); // SQDMLALBT // MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int]
  public static unsafe Vector<T> MultiplyDoublingWideningAndAddSaturateOdd(Vector<T> addend, Vector<T2> left, Vector<T2> right); // SQDMLALT // MOVPRFX

  /// T: [int, short], [long, int]
  public static unsafe Vector<T> MultiplyDoublingWideningBySelectedScalarAndAddSaturateOdd(Vector<T> addend, Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex); // SQDMLALT // MOVPRFX

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> MultiplyDoublingSaturateHigh(Vector<T> left, Vector<T> right); // SQDMULH

  /// T: short, int, long
  public static unsafe Vector<T> MultiplyDoublingBySelectedScalarSaturateHigh(Vector<T> left, Vector<T> right, [ConstantExpected] byte rightIndex); // SQDMULH

  /// T: [short, sbyte], [int, short], [long, int]
  public static unsafe Vector<T> MultiplyDoublingWideningAndSubtractSaturateEven(Vector<T> minuend, Vector<T2> left, Vector<T2> right); // SQDMLSLB // MOVPRFX

  /// T: [int, short], [long, int]
  public static unsafe Vector<T> MultiplyDoublingWideningBySelectedScalarAndSubtractSaturateEven(Vector<T> minuend, Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex); // SQDMLSLB // MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int]
  public static unsafe Vector<T> MultiplyDoublingWideningAndSubtractSaturateEvenOdd(Vector<T> minuend, Vector<T2> leftEven, Vector<T2> rightOdd); // SQDMLSLBT // MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int]
  public static unsafe Vector<T> MultiplyDoublingWideningAndSubtractSaturateOdd(Vector<T> minuend, Vector<T2> left, Vector<T2> right); // SQDMLSLT // MOVPRFX

  /// T: [int, short], [long, int]
  public static unsafe Vector<T> MultiplyDoublingWideningAndSubtractSaturateOdd(Vector<T> minuend, Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex); // SQDMLSLT // MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int]
  public static unsafe Vector<T> MultiplyDoublingWideningSaturateEven(Vector<T2> left, Vector<T2> right); // SQDMULLB

  /// T: [int, short], [long, int]
  public static unsafe Vector<T> MultiplyDoublingWideningSaturateEvenBySelectedScalar(Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex); // SQDMULLB

  /// T: [short, sbyte], [int, short], [long, int]
  public static unsafe Vector<T> MultiplyDoublingWideningSaturateOdd(Vector<T2> left, Vector<T2> right); // SQDMULLT

  /// T: [int, short], [long, int]
  public static unsafe Vector<T> MultiplyDoublingWideningSaturateOddBySelectedScalar(Vector<T2> left, Vector<T2> right, [ConstantExpected] byte rightIndex); // SQDMULLT

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> NegateSaturate(Vector<T> value); // SQNEG // predicated, MOVPRFX

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> MultiplyRoundedDoublingSaturateAndAddHigh(Vector<T> addend, Vector<T> left, Vector<T> right); // SQRDMLAH // MOVPRFX

  /// T: short, int, long
  public static unsafe Vector<T> MultiplyRoundedDoublingSaturateBySelectedScalarAndAddHigh(Vector<T> addend, Vector<T> left, Vector<T> right, [ConstantExpected] byte rightIndex); // SQRDMLAH // MOVPRFX

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> MultiplyRoundedDoublingSaturateHigh(Vector<T> left, Vector<T> right); // SQRDMULH

  /// T: short, int, long
  public static unsafe Vector<T> MultiplyRoundedDoublingBySelectedScalarSaturateHigh(Vector<T> left, Vector<T> right, [ConstantExpected] byte rightIndex); // SQRDMULH

  /// T: sbyte, short, int, long
  public static unsafe Vector<T> MultiplyRoundedDoublingSaturateAndSubtractHigh(Vector<T> minuend, Vector<T> left, Vector<T> right); // SQRDMLSH // MOVPRFX

  /// T: short, int, long
  public static unsafe Vector<T> MultiplyRoundedDoublingSaturateBySelectedScalarAndSubtractHigh(Vector<T> minuend, Vector<T> left, Vector<T> right, [ConstantExpected] byte rightIndex); // SQRDMLSH // MOVPRFX

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> SubtractHighNarrowingEven(Vector<T2> left, Vector<T2> right); // SUBHNB

  /// T: [sbyte, short], [short, int], [int, long], [byte, ushort], [ushort, uint], [uint, ulong]
  public static unsafe Vector<T> SubtractHighNarrowingOdd(Vector<T> even, Vector<T2> left, Vector<T2> right); // SUBHNT

  /// T: sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> SubtractSaturate(Vector<T> left, Vector<T> right); // SQSUB or UQSUB or SQSUBR or UQSUBR // predicated, MOVPRFX

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> SubtractWideningEven(Vector<T> left, Vector<T2> right); // SSUBWB or USUBWB

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> SubtractWideningOdd(Vector<T> left, Vector<T2> right); // SSUBWT or USUBWT

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> SubtractWideningEven(Vector<T2> left, Vector<T2> right); // SSUBLB or USUBLB

  /// T: [short, sbyte], [int, short], [long, int]
  public static unsafe Vector<T> SubtractWideningEvenOdd(Vector<T2> leftEven, Vector<T2> rightOdd); // SSUBLBT

  /// T: [short, sbyte], [int, short], [long, int], [ushort, byte], [uint, ushort], [ulong, uint]
  public static unsafe Vector<T> SubtractWideningOdd(Vector<T2> left, Vector<T2> right); // SSUBLT or USUBLT

  /// T: [short, sbyte], [int, short], [long, int]
  public static unsafe Vector<T> SubtractWideningOddEven(Vector<T2> leftOdd, Vector<T2> rightEven); // SSUBLTB

  /// T: uint, ulong
  public static unsafe Vector<T> SubtractBorrowWideningEven(Vector<T> left, Vector<T> right, Vector<T> borrow); // SBCLB // MOVPRFX

  /// T: uint, ulong
  public static unsafe Vector<T> SubtractBorrowWideningOdd(Vector<T> left, Vector<T> right, Vector<T> borrow); // SBCLT // MOVPRFX
}
```
