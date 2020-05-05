# Quick Reviews 5/5/2020

## Add empty X64/Arm64 nested classes to some System.Runtime.Intrinsics

**Approved** | [#runtime/34587](https://github.com/dotnet/runtime/issues/34587#issuecomment-624197774)

* The problem makes sense, but instead of giving `IsSupported` behavior, it might be better to hide the nested classes entirely and make calling `IsSupported` a compile-time error:

```C#
    partial class Sha1 : ArmBase
    {
        [EditorBrowsable(Never)]
        public abstract class Arm64            
        {
            // No inheritance, no IsSupported
        }
    }
```
## Consider adding Vector64<double>.CreateScalar, Vector64<ulong>CreateScalar and Vector64<long>.CreateScalar or renaming Create

**Approved** | [#runtime/34929](https://github.com/dotnet/runtime/issues/34929#issuecomment-624200505)

* Looks good as proposed

```C#
namespace System.Runtime.Intrinsics
{
    public partial class Vector64
    {
        public static unsafe Vector64<double> CreateScalar(double value);
        public static unsafe Vector64<long>   CreateScalar(long value);
        public static unsafe Vector64<ulong>  CreateScalar(ulong value);
    }
}
```
## [Arm64] "Move" Intrinsics

**Approved** | [#runtime/35037](https://github.com/dotnet/runtime/issues/35037#issuecomment-624211066)

* Looks good as proposed
* Should there be an `InsertSelectedScalar` that returns `Vector64`?

```C#
namespace System.Runtime.Intrinsics.Arm
{
    public abstract class AdvSimd
    {
        /// <summary>
        /// Duplicate general-purpose register to vector
        /// For each element result[elem] = value
        /// Corresponds to vector forms of DUP and VDUP
        /// </summary>
        Vector64<byte>    DuplicateToVector64(byte value);
        Vector64<short>   DuplicateToVector64(short value);
        Vector64<int>     DuplicateToVector64(int value);
        Vector64<sbyte>   DuplicateToVector64(sbyte value);
        Vector64<ushort>  DuplicateToVector64(ushort value);
        Vector64<uint>    DuplicateToVector64(uint value);

        Vector128<byte>   DuplicateToVector128(byte value);
        Vector128<short>  DuplicateToVector128(short value);
        Vector128<int>    DuplicateToVector128(int value);
        Vector128<sbyte>  DuplicateToVector128(sbyte value);
        Vector128<ushort> DuplicateToVector128(ushort value);
        Vector128<uint>   DuplicateToVector128(uint value);

        /// <summary>
        /// Duplicate vector element to vector
        /// For each element result[elem] = value[index]
        /// Corresponds to vector forms of DUP and VDUP
        /// </summary>
        Vector64<byte>    DuplicateSelectedScalarToVector64(Vector64<byte> value,     byte index);
        Vector64<short>   DuplicateSelectedScalarToVector64(Vector64<short> value,    byte index);
        Vector64<int>     DuplicateSelectedScalarToVector64(Vector64<int> value,      byte index);
        Vector64<float>   DuplicateSelectedScalarToVector64(Vector64<float> value,    byte index);
        Vector64<sbyte>   DuplicateSelectedScalarToVector64(Vector64<sbyte> value,    byte index);
        Vector64<ushort>  DuplicateSelectedScalarToVector64(Vector64<ushort> value,   byte index);
        Vector64<uint>    DuplicateSelectedScalarToVector64(Vector64<uint> value,     byte index);

        Vector64<byte>    DuplicateSelectedScalarToVector64(Vector128<byte> value,     byte index);
        Vector64<short>   DuplicateSelectedScalarToVector64(Vector128<short> value,    byte index);
        Vector64<int>     DuplicateSelectedScalarToVector64(Vector128<int> value,      byte index);
        Vector64<float>   DuplicateSelectedScalarToVector64(Vector128<float> value,    byte index);
        Vector64<sbyte>   DuplicateSelectedScalarToVector64(Vector128<sbyte> value,    byte index);
        Vector64<ushort>  DuplicateSelectedScalarToVector64(Vector128<ushort> value,   byte index);
        Vector64<uint>    DuplicateSelectedScalarToVector64(Vector128<uint> value,     byte index);

        Vector128<byte>   DuplicateSelectedScalarToVector128(Vector64<byte> value,    byte index);
        Vector128<short>  DuplicateSelectedScalarToVector128(Vector64<short> value,   byte index);
        Vector128<int>    DuplicateSelectedScalarToVector128(Vector64<int> value,     byte index);
        Vector128<float>  DuplicateSelectedScalarToVector128(Vector64<float> value,   byte index);
        Vector128<sbyte>  DuplicateSelectedScalarToVector128(Vector64<sbyte> value,   byte index);
        Vector128<ushort> DuplicateSelectedScalarToVector128(Vector64<ushort> value,  byte index);
        Vector128<uint>   DuplicateSelectedScalarToVector128(Vector64<uint> value,    byte index);

        Vector128<byte>   DuplicateSelectedScalarToVector128(Vector128<byte> value,   byte index);
        Vector128<double> DuplicateSelectedScalarToVector128(Vector128<double> value, byte index);
        Vector128<short>  DuplicateSelectedScalarToVector128(Vector128<short> value,  byte index);
        Vector128<int>    DuplicateSelectedScalarToVector128(Vector128<int> value,    byte index);
        Vector128<long>   DuplicateSelectedScalarToVector128(Vector128<long> value,   byte index);
        Vector128<float>  DuplicateSelectedScalarToVector128(Vector128<float> value,  byte index);
        Vector128<sbyte>  DuplicateSelectedScalarToVector128(Vector128<sbyte> value,  byte index);
        Vector128<ushort> DuplicateSelectedScalarToVector128(Vector128<ushort> value, byte index);
        Vector128<uint>   DuplicateSelectedScalarToVector128(Vector128<uint> value,   byte index);
        Vector128<ulong>  DuplicateSelectedScalarToVector128(Vector128<ulong> value,  byte index);

        public abstract class Arm64
        {
            /// <summary>
            /// Duplicate general-purpose register to vector
            /// For each element result[elem] = value
            /// Corresponds to vector forms of DUP
            /// </summary>
            Vector128<long>   DuplicateToVector64(long value);
            Vector128<ulong>  DuplicateToVector64(ulong value);

            /// <summary>
            /// Insert vector element from another vector element
            /// result[resultIndex] = value[valueIndex]
            /// Corresponds to vector forms of INS
            /// </summary>
            Vector128<byte>   InsertSelectedScalar(Vector128<byte> result,   byte resultIndex, Vector64<byte> value,    byte valueIndex);
            Vector128<short>  InsertSelectedScalar(Vector128<short> result,  byte resultIndex, Vector64<short> value,   byte valueIndex);
            Vector128<int>    InsertSelectedScalar(Vector128<int> result,    byte resultIndex, Vector64<int> value,     byte valueIndex);
            Vector128<float>  InsertSelectedScalar(Vector128<float> result,  byte resultIndex, Vector64<float> value,   byte valueIndex);
            Vector128<sbyte>  InsertSelectedScalar(Vector128<sbyte> result,  byte resultIndex, Vector64<sbyte> value,   byte valueIndex);
            Vector128<ushort> InsertSelectedScalar(Vector128<ushort> result, byte resultIndex, Vector64<ushort> value,  byte valueIndex);
            Vector128<uint>   InsertSelectedScalar(Vector128<uint> result,   byte resultIndex, Vector64<uint> value,    byte valueIndex);

            Vector128<byte>   InsertSelectedScalar(Vector128<byte> result,   byte resultIndex, Vector128<byte> value,   byte valueIndex);
            Vector128<double> InsertSelectedScalar(Vector128<double> result, byte resultIndex, Vector128<double> value, byte valueIndex);
            Vector128<short>  InsertSelectedScalar(Vector128<short> result,  byte resultIndex, Vector128<short> value,  byte valueIndex);
            Vector128<int>    InsertSelectedScalar(Vector128<int> result,    byte resultIndex, Vector128<int> value,    byte valueIndex);
            Vector128<long>   InsertSelectedScalar(Vector128<long> result,   byte resultIndex, Vector128<long> value,   byte valueIndex);
            Vector128<float>  InsertSelectedScalar(Vector128<float> result,  byte resultIndex, Vector128<float> value,  byte valueIndex);
            Vector128<sbyte>  InsertSelectedScalar(Vector128<sbyte> result,  byte resultIndex, Vector128<sbyte> value,  byte valueIndex);
            Vector128<ushort> InsertSelectedScalar(Vector128<ushort> result, byte resultIndex, Vector128<ushort> value, byte valueIndex);
            Vector128<uint>   InsertSelectedScalar(Vector128<uint> result,   byte resultIndex, Vector128<uint> value,   byte valueIndex);
            Vector128<ulong>  InsertSelectedScalar(Vector128<ulong> result,  byte resultIndex, Vector128<ulong> value,  byte valueIndex);
        }
    }
}
```
## ARM additional shifting intrinsics

**Approved** | [#runtime/33398](https://github.com/dotnet/runtime/issues/33398#issuecomment-624242885)

* We should rename the `shift` parameter to `count`
* What happens when the vector-based counts are too large?
* Some of the upper variants under `Arm64` seem to belong to `AdvSimd`
* Do we need `SignExtendAndWidenLower`, `ZeroExtendAndWidenLower`, `SignExtendAndWidenUpper`, SignExtendAndWidenUpper` at all?
    - If we expose them, we need to drop the `count` parameter
* Should we change the encoding of methods and instead of making up Markov-chain-like method names take arguments (e.g. `bool round` or `SomeFlags flags`)? The concern is metadata size.

```C#
namespace System.Runtime.Intrinsics.Arm
{
    public abstract class AdvSimd
    {
        /// <summary>
        /// Unsigned Shift Left
        /// For each element result[elem] = value[elem] << (shift[elem] & 0xFF)
        /// Corresponds to vector forms of USHL and VSHL
        /// </summary>
        Vector64<byte> ShiftLogical(Vector64<byte> value, Vector64<sbyte> count);
        Vector64<sbyte> ShiftLogical(Vector64<sbyte> value, Vector64<sbyte> count);
        Vector64<short> ShiftLogical(Vector64<short> value, Vector64<short> count);
        Vector64<ushort> ShiftLogical(Vector64<ushort> value, Vector64<short> count);
        Vector64<int> ShiftLogical(Vector64<int> value, Vector64<int> count);
        Vector64<uint> ShiftLogical(Vector64<uint> value, Vector64<int> count);
        Vector128<byte> ShiftLogical(Vector128<byte> value, Vector128<sbyte> count);
        Vector128<sbyte> ShiftLogical(Vector128<sbyte> value, Vector128<sbyte> count);
        Vector128<short> ShiftLogical(Vector128<short> value, Vector128<short> count);
        Vector128<ushort> ShiftLogical(Vector128<ushort> value, Vector128<short> count);
        Vector128<int> ShiftLogical(Vector128<int> value, Vector128<int> count);
        Vector128<uint> ShiftLogical(Vector128<uint> value, Vector128<int> count);
        Vector128<long> ShiftLogical(Vector128<long> value, Vector128<long> count);
        Vector128<ulong> ShiftLogical(Vector128<ulong> value, Vector128<long> count);

        Vector64<long> ShiftLogicalScalar(Vector64<long> value, Vector64<long> count);
        Vector64<ulong> ShiftLogicalScalar(Vector64<ulong> value, Vector64<long> count);

        /// <summary>
        /// Unsigned Rounding Shift Left
        /// For each element result[elem] = (value[elem] + (1 << (-(shift[elem] & 0xFF) - 1))) << (shift[elem] & 0xFF)
        /// Corresponds to vector forms of URSHL and VRSHL
        /// </summary>
        Vector64<byte> ShiftLogicalRounded(Vector64<byte> value, Vector64<sbyte> count);
        Vector64<sbyte> ShiftLogicalRounded(Vector64<sbyte> value, Vector64<sbyte> count);
        Vector64<short> ShiftLogicalRounded(Vector64<short> value, Vector64<short> count);
        Vector64<ushort> ShiftLogicalRounded(Vector64<ushort> value, Vector64<short> count);
        Vector64<int> ShiftLogicalRounded(Vector64<int> value, Vector64<int> count);
        Vector64<uint> ShiftLogicalRounded(Vector64<uint> value, Vector64<int> count);
        Vector128<byte> ShiftLogicalRounded(Vector128<byte> value, Vector128<sbyte> count);
        Vector128<sbyte> ShiftLogicalRounded(Vector128<sbyte> value, Vector128<sbyte> count);
        Vector128<short> ShiftLogicalRounded(Vector128<short> value, Vector128<short> count);
        Vector128<ushort> ShiftLogicalRounded(Vector128<ushort> value, Vector128<short> count);
        Vector128<int> ShiftLogicalRounded(Vector128<int> value, Vector128<int> count);
        Vector128<uint> ShiftLogicalRounded(Vector128<uint> value, Vector128<int> count);
        Vector128<long> ShiftLogicalRounded(Vector128<long> value, Vector128<long> count);
        Vector128<ulong> ShiftLogicalRounded(Vector128<ulong> value, Vector128<long> count);

        Vector64<long> ShiftLogicalRoundedScalar(Vector64<long> value, Vector64<long> count);
        Vector64<ulong> ShiftLogicalRoundedScalar(Vector64<ulong> value, Vector64<long> count);

        /// <summary>
        /// Signed Shift Left
        /// For each element result[elem] = value[elem] << (shift[elem] & 0xFF)
        /// Corresponds to vector forms of SSHL and VSHL
        /// </summary>
        Vector64<sbyte> ShiftArithmetic(Vector64<sbyte> value, Vector64<sbyte> count);
        Vector64<short> ShiftArithmetic(Vector64<short> value, Vector64<short> count);
        Vector64<int> ShiftArithmetic(Vector64<int> value, Vector64<int> count);
        Vector128<sbyte> ShiftArithmetic(Vector128<sbyte> value, Vector128<sbyte> count);
        Vector128<short> ShiftArithmetic(Vector128<short> value, Vector128<short> count);
        Vector128<int> ShiftArithmetic(Vector128<int> value, Vector128<int> count);
        Vector128<long> ShiftArithmetic(Vector128<long> value, Vector128<long> count);

        Vector64<long> ShiftArithmeticScalar(Vector64<long> value, Vector64<long> count);

        /// <summary>
        /// Signed Rounding Shift Left
        /// For each element result[elem] = (value[elem] + (1 << (-(shift[elem] & 0xFF) - 1))) << (shift[elem] & 0xFF)
        /// Corresponds to vector forms of SRSHL and VRSHL
        /// </summary>
        Vector64<sbyte> ShiftArithmeticRounded(Vector64<sbyte> value, Vector64<sbyte> count);
        Vector64<short> ShiftArithmeticRounded(Vector64<short> value, Vector64<short> count);
        Vector64<int> ShiftArithmeticRounded(Vector64<int> value, Vector64<int> count);
        Vector128<sbyte> ShiftArithmeticRounded(Vector128<sbyte> value, Vector128<sbyte> count);
        Vector128<short> ShiftArithmeticRounded(Vector128<short> value, Vector128<short> count);
        Vector128<int> ShiftArithmeticRounded(Vector128<int> value, Vector128<int> count);
        Vector128<long> ShiftArithmeticRounded(Vector128<long> value, Vector128<long> count);

        Vector64<long> ShiftArithmeticRoundedScalar(Vector64<long> value, Vector64<long> count);

        /// <summary>
        /// Unsigned Saturating Shift Left
        /// For each element result[elem] = value[elem] << (ShiftLogical[elem] & 0xFF)
        /// Corresponds to vector forms of UQSHL and VQSHL
        /// </summary>
        Vector64<byte> ShiftLogicalSaturate(Vector64<byte> value, Vector64<sbyte> count);
        Vector64<sbyte> ShiftLogicalSaturate(Vector64<sbyte> value, Vector64<sbyte> count);
        Vector64<short> ShiftLogicalSaturate(Vector64<short> value, Vector64<short> count);
        Vector64<ushort> ShiftLogicalSaturate(Vector64<ushort> value, Vector64<short> count);
        Vector64<int> ShiftLogicalSaturate(Vector64<int> value, Vector64<int> count);
        Vector64<uint> ShiftLogicalSaturate(Vector64<uint> value, Vector64<int> count);
        Vector128<byte> ShiftLogicalSaturate(Vector128<byte> value, Vector128<sbyte> count);
        Vector128<sbyte> ShiftLogicalSaturate(Vector128<sbyte> value, Vector128<sbyte> count);
        Vector128<short> ShiftLogicalSaturate(Vector128<short> value, Vector128<short> count);
        Vector128<ushort> ShiftLogicalSaturate(Vector128<ushort> value, Vector128<short> count);
        Vector128<int> ShiftLogicalSaturate(Vector128<int> value, Vector128<int> count);
        Vector128<uint> ShiftLogicalSaturate(Vector128<uint> value, Vector128<int> count);
        Vector128<long> ShiftLogicalSaturate(Vector128<long> value, Vector128<long> count);
        Vector128<ulong> ShiftLogicalSaturate(Vector128<ulong> value, Vector128<long> count);

        Vector64<long> ShiftLogicalSaturateScalar(Vector64<long> value, Vector64<long> count);
        Vector64<ulong> ShiftLogicalSaturateScalar(Vector64<ulong> value, Vector64<long> count);

        /// <summary>
        /// Unsigned Saturating Rounding Shift Left
        /// For each element result[elem] = (value[elem] + (1 << (-(ShiftLogical[elem] & 0xFF) - 1))) << (ShiftLogical[elem] & 0xFF)
        /// Corresponds to vector forms of UQRSHL and VQRSHL
        /// </summary>
        Vector64<byte> ShiftLogicalRoundedSaturate(Vector64<byte> value, Vector64<sbyte> count);
        Vector64<sbyte> ShiftLogicalRoundedSaturate(Vector64<sbyte> value, Vector64<sbyte> count);
        Vector64<short> ShiftLogicalRoundedSaturate(Vector64<short> value, Vector64<short> count);
        Vector64<ushort> ShiftLogicalRoundedSaturate(Vector64<ushort> value, Vector64<short> count);
        Vector64<int> ShiftLogicalRoundedSaturate(Vector64<int> value, Vector64<int> count);
        Vector64<uint> ShiftLogicalRoundedSaturate(Vector64<uint> value, Vector64<int> count);
        Vector128<byte> ShiftLogicalRoundedSaturate(Vector128<byte> value, Vector128<sbyte> count);
        Vector128<sbyte> ShiftLogicalRoundedSaturate(Vector128<sbyte> value, Vector128<sbyte> count);
        Vector128<short> ShiftLogicalRoundedSaturate(Vector128<short> value, Vector128<short> count);
        Vector128<ushort> ShiftLogicalRoundedSaturate(Vector128<ushort> value, Vector128<short> count);
        Vector128<int> ShiftLogicalRoundedSaturate(Vector128<int> value, Vector128<int> count);
        Vector128<uint> ShiftLogicalRoundedSaturate(Vector128<uint> value, Vector128<int> count);
        Vector128<long> ShiftLogicalRoundedSaturate(Vector128<long> value, Vector128<long> count);
        Vector128<ulong> ShiftLogicalRoundedSaturate(Vector128<ulong> value, Vector128<long> count);

        Vector64<long> ShiftLogicalRoundedSaturateScalar(Vector64<long> value, Vector64<long> count);
        Vector64<ulong> ShiftLogicalRoundedSaturateScalar(Vector64<ulong> value, Vector64<long> count);

        /// <summary>
        /// Signed Saturating Shift Left
        /// For each element result[elem] = value[elem] << (ShiftLogical[elem] & 0xFF)
        /// Corresponds to vector forms of SQSHL and VQSHL
        /// </summary>
        Vector64<sbyte> ShiftArithmeticSaturate(Vector64<sbyte> value, Vector64<sbyte> count);
        Vector64<short> ShiftArithmeticSaturate(Vector64<short> value, Vector64<short> count);
        Vector64<int> ShiftArithmeticSaturate(Vector64<int> value, Vector64<int> count);
        Vector128<sbyte> ShiftArithmeticSaturate(Vector128<sbyte> value, Vector128<sbyte> count);
        Vector128<short> ShiftArithmeticSaturate(Vector128<short> value, Vector128<short> count);
        Vector128<int> ShiftArithmeticSaturate(Vector128<int> value, Vector128<int> count);
        Vector128<long> ShiftArithmeticSaturate(Vector128<long> value, Vector128<long> count);

        Vector64<long> ShiftArithmeticSaturateScalar(Vector64<long> value, Vector64<long> count);

        /// <summary>
        /// Signed Saturating Rounding Shift Left
        /// For each element result[elem] = (value[elem] + (1 << (-(ShiftLogical[elem] & 0xFF) - 1))) << (ShiftLogical[elem] & 0xFF)
        /// Corresponds to vector forms of SQRSHL and VQRSHL
        /// </summary>
        Vector64<sbyte> ShiftArithmeticRoundedSaturate(Vector64<sbyte> value, Vector64<sbyte> count);
        Vector64<short> ShiftArithmeticRoundedSaturate(Vector64<short> value, Vector64<short> count);
        Vector64<int> ShiftArithmeticRoundedSaturate(Vector64<int> value, Vector64<int> count);
        Vector128<sbyte> ShiftArithmeticRoundedSaturate(Vector128<sbyte> value, Vector128<sbyte> count);
        Vector128<short> ShiftArithmeticRoundedSaturate(Vector128<short> value, Vector128<short> count);
        Vector128<int> ShiftArithmeticRoundedSaturate(Vector128<int> value, Vector128<int> count);
        Vector128<long> ShiftArithmeticRoundedSaturate(Vector128<long> value, Vector128<long> count);

        Vector64<long> ShiftArithmeticRoundedSaturateScalar(Vector64<long> value, Vector64<long> count);

        /// <summary>
        /// Shift Left Immediate
        /// For each element result[elem] = value[elem] << shift
        /// Corresponds to vector forms of SHL and VSHL
        /// </summary>
        Vector64<byte> ShiftLeftLogical(Vector64<byte> value, byte count);
        Vector64<sbyte> ShiftLeftLogical(Vector64<sbyte> value, byte count);
        Vector64<short> ShiftLeftLogical(Vector64<short> value, byte count);
        Vector64<ushort> ShiftLeftLogical(Vector64<ushort> value, byte count);
        Vector64<int> ShiftLeftLogical(Vector64<int> value, byte count);
        Vector64<uint> ShiftLeftLogical(Vector64<uint> value, byte count);
        Vector128<byte> ShiftLeftLogical(Vector128<byte> value, byte count);
        Vector128<sbyte> ShiftLeftLogical(Vector128<sbyte> value, byte count);
        Vector128<short> ShiftLeftLogical(Vector128<short> value, byte count);
        Vector128<ushort> ShiftLeftLogical(Vector128<ushort> value, byte count);
        Vector128<int> ShiftLeftLogical(Vector128<int> value, byte count);
        Vector128<uint> ShiftLeftLogical(Vector128<uint> value, byte count);
        Vector128<long> ShiftLeftLogical(Vector128<long> value, byte count);
        Vector128<ulong> ShiftLeftLogical(Vector128<ulong> value, byte count);

        Vector64<long> ShiftLeftLogicalScalar(Vector64<long> value, byte count);
        Vector64<ulong> ShiftLeftLogicalScalar(Vector64<ulong> value, byte count);

        /// <summary>
        /// Unsigned Shift Right Immediate
        /// For each element result[elem] = value[elem] >> shift
        /// Corresponds to vector forms of USHR and VSHR
        /// </summary>
        Vector64<byte> ShiftRightLogical(Vector64<byte> value, byte count);
        Vector64<sbyte> ShiftRightLogical(Vector64<sbyte> value, byte count);
        Vector64<short> ShiftRightLogical(Vector64<short> value, byte count);
        Vector64<ushort> ShiftRightLogical(Vector64<ushort> value, byte count);
        Vector64<int> ShiftRightLogical(Vector64<int> value, byte count);
        Vector64<uint> ShiftRightLogical(Vector64<uint> value, byte count);
        Vector128<byte> ShiftRightLogical(Vector128<byte> value, byte count);
        Vector128<sbyte> ShiftRightLogical(Vector128<sbyte> value, byte count);
        Vector128<short> ShiftRightLogical(Vector128<short> value, byte count);
        Vector128<ushort> ShiftRightLogical(Vector128<ushort> value, byte count);
        Vector128<int> ShiftRightLogical(Vector128<int> value, byte count);
        Vector128<uint> ShiftRightLogical(Vector128<uint> value, byte count);
        Vector128<long> ShiftRightLogical(Vector128<long> value, byte count);
        Vector128<ulong> ShiftRightLogical(Vector128<ulong> value, byte count);

        Vector64<long> ShiftRightLogicalScalar(Vector64<long> value, byte count);
        Vector64<ulong> ShiftRightLogicalScalar(Vector64<ulong> value, byte count);

        /// <summary>
        /// Unsigned Rounding Shift Right Immediate
        /// For each element result[elem] = (value[elem] + (1 << (shift - 1))) >> shift
        /// Corresponds to vector forms of URSHR and VRSHR
        /// </summary>
        Vector64<byte> ShiftRightLogicalRounded(Vector64<byte> value, byte count);
        Vector64<sbyte> ShiftRightLogicalRounded(Vector64<sbyte> value, byte count);
        Vector64<short> ShiftRightLogicalRounded(Vector64<short> value, byte count);
        Vector64<ushort> ShiftRightLogicalRounded(Vector64<ushort> value, byte count);
        Vector64<int> ShiftRightLogicalRounded(Vector64<int> value, byte count);
        Vector64<uint> ShiftRightLogicalRounded(Vector64<uint> value, byte count);
        Vector128<byte> ShiftRightLogicalRounded(Vector128<byte> value, byte count);
        Vector128<sbyte> ShiftRightLogicalRounded(Vector128<sbyte> value, byte count);
        Vector128<short> ShiftRightLogicalRounded(Vector128<short> value, byte count);
        Vector128<ushort> ShiftRightLogicalRounded(Vector128<ushort> value, byte count);
        Vector128<int> ShiftRightLogicalRounded(Vector128<int> value, byte count);
        Vector128<uint> ShiftRightLogicalRounded(Vector128<uint> value, byte count);
        Vector128<long> ShiftRightLogicalRounded(Vector128<long> value, byte count);
        Vector128<ulong> ShiftRightLogicalRounded(Vector128<ulong> value, byte count);

        Vector64<long> ShiftRightLogicalRoundedScalar(Vector64<long> value, byte count);
        Vector64<ulong> ShiftRightLogicalRoundedScalar(Vector64<ulong> value, byte count);

        /// <summary>
        /// Signed Shift Right Immediate
        /// For each element result[elem] = value[elem] >> shift
        /// Corresponds to vector forms of SSHR and VSHR
        /// </summary>
        Vector64<sbyte> ShiftRightArithmetic(Vector64<sbyte> value, byte count);
        Vector64<short> ShiftRightArithmetic(Vector64<short> value, byte count);
        Vector64<int> ShiftRightArithmetic(Vector64<int> value, byte count);
        Vector128<sbyte> ShiftRightArithmetic(Vector128<sbyte> value, byte count);
        Vector128<short> ShiftRightArithmetic(Vector128<short> value, byte count);
        Vector128<int> ShiftRightArithmetic(Vector128<int> value, byte count);
        Vector128<long> ShiftRightArithmetic(Vector128<long> value, byte count);

        Vector64<long> ShiftRightArithmeticScalar(Vector64<long> value, byte count);

        /// <summary>
        /// Signed Rounding Shift Right Immediate
        /// For each element result[elem] = (value[elem] + (1 << (shift - 1))) >> shift
        /// Corresponds to vector forms of SRSHR and VRSHR
        /// </summary>
        Vector64<sbyte> ShiftRightArithmeticRounded(Vector64<sbyte> value, byte count);
        Vector64<short> ShiftRightArithmeticRounded(Vector64<short> value, byte count);
        Vector64<int> ShiftRightArithmeticRounded(Vector64<int> value, byte count);
        Vector128<sbyte> ShiftRightArithmeticRounded(Vector128<sbyte> value, byte count);
        Vector128<short> ShiftRightArithmeticRounded(Vector128<short> value, byte count);
        Vector128<int> ShiftRightArithmeticRounded(Vector128<int> value, byte count);
        Vector128<long> ShiftRightArithmeticRounded(Vector128<long> value, byte count);

        Vector64<long> ShiftRightArithmeticRoundedScalar(Vector64<long> value, byte count);

        /// <summary>
        /// Unsigned Shift Right and Accumulate
        /// For each element result[elem] = addend[elem] + (value[elem] >> shift)
        /// Corresponds to vector forms of USRA and VSRA
        /// </summary>
        Vector64<byte> ShiftRightLogicalAdd(Vector64<byte> addend, Vector64<byte> value, byte count);
        Vector64<sbyte> ShiftRightLogicalAdd(Vector64<sbyte> addend, Vector64<sbyte> value, byte count);
        Vector64<short> ShiftRightLogicalAdd(Vector64<short> addend, Vector64<short> value, byte count);
        Vector64<ushort> ShiftRightLogicalAdd(Vector64<ushort> addend, Vector64<ushort> value, byte count);
        Vector64<int> ShiftRightLogicalAdd(Vector64<int> addend, Vector64<int> value, byte count);
        Vector64<uint> ShiftRightLogicalAdd(Vector64<uint> addend, Vector64<uint> value, byte count);
        Vector128<byte> ShiftRightLogicalAdd(Vector128<byte> addend, Vector128<byte> value, byte count);
        Vector128<sbyte> ShiftRightLogicalAdd(Vector128<sbyte> addend, Vector128<sbyte> value, byte count);
        Vector128<short> ShiftRightLogicalAdd(Vector128<short> addend, Vector128<short> value, byte count);
        Vector128<ushort> ShiftRightLogicalAdd(Vector128<ushort> addend, Vector128<ushort> value, byte count);
        Vector128<int> ShiftRightLogicalAdd(Vector128<int> addend, Vector128<int> value, byte count);
        Vector128<uint> ShiftRightLogicalAdd(Vector128<uint> addend, Vector128<uint> value, byte count);
        Vector128<long> ShiftRightLogicalAdd(Vector128<long> addend, Vector128<long> value, byte count);
        Vector128<ulong> ShiftRightLogicalAdd(Vector128<ulong> addend, Vector128<ulong> value, byte count);

        Vector64<long> ShiftRightLogicalAddScalar(Vector64<long> addend, Vector64<long> value, byte count);
        Vector64<ulong> ShiftRightLogicalAddScalar(Vector64<ulong> addend, Vector64<ulong> value, byte count);

        /// <summary>
        /// Signed Shift Right and Accumulate
        /// For each element result[elem] = addend[elem] + (value[elem] >> shift)
        /// Corresponds to vector forms of SSRA and VSRA
        /// </summary>
        Vector64<sbyte> ShiftRightArithmeticAdd(Vector64<sbyte> addend, Vector64<sbyte> value, byte count);
        Vector64<short> ShiftRightArithmeticAdd(Vector64<short> addend, Vector64<short> value, byte count);
        Vector64<int> ShiftRightArithmeticAdd(Vector64<int> addend, Vector64<int> value, byte count);
        Vector128<sbyte> ShiftRightArithmeticAdd(Vector128<sbyte> addend, Vector128<sbyte> value, byte count);
        Vector128<short> ShiftRightArithmeticAdd(Vector128<short> addend, Vector128<short> value, byte count);
        Vector128<int> ShiftRightArithmeticAdd(Vector128<int> addend, Vector128<int> value, byte count);
        Vector128<long> ShiftRightArithmeticAdd(Vector128<long> addend, Vector128<long> value, byte count);

        Vector64<long> ShiftRightArithmeticAddScalar(Vector64<long> addend, Vector64<long> value, byte count);

        /// <summary>
        /// Unsigned Rounding Shift Right and Accumulate
        /// For each element result[elem] = addend[elem] + (value[elem] >> RoundedShift)
        /// Corresponds to vector forms of URSRA and VRSRA
        /// </summary>
        Vector64<byte> ShiftRightLogicalAddRounded(Vector64<byte> addend, Vector64<byte> value, byte count);
        Vector64<sbyte> ShiftRightLogicalAddRounded(Vector64<sbyte> addend, Vector64<sbyte> value, byte count);
        Vector64<short> ShiftRightLogicalAddRounded(Vector64<short> addend, Vector64<short> value, byte count);
        Vector64<ushort> ShiftRightLogicalAddRounded(Vector64<ushort> addend, Vector64<ushort> value, byte count);
        Vector64<int> ShiftRightLogicalAddRounded(Vector64<int> addend, Vector64<int> value, byte count);
        Vector64<uint> ShiftRightLogicalAddRounded(Vector64<uint> addend, Vector64<uint> value, byte count);
        Vector128<byte> ShiftRightLogicalAddRounded(Vector128<byte> addend, Vector128<byte> value, byte count);
        Vector128<sbyte> ShiftRightLogicalAddRounded(Vector128<sbyte> addend, Vector128<sbyte> value, byte count);
        Vector128<short> ShiftRightLogicalAddRounded(Vector128<short> addend, Vector128<short> value, byte count);
        Vector128<ushort> ShiftRightLogicalAddRounded(Vector128<ushort> addend, Vector128<ushort> value, byte count);
        Vector128<int> ShiftRightLogicalAddRounded(Vector128<int> addend, Vector128<int> value, byte count);
        Vector128<uint> ShiftRightLogicalAddRounded(Vector128<uint> addend, Vector128<uint> value, byte count);
        Vector128<long> ShiftRightLogicalAddRounded(Vector128<long> addend, Vector128<long> value, byte count);
        Vector128<ulong> ShiftRightLogicalAddRounded(Vector128<ulong> addend, Vector128<ulong> value, byte count);

        Vector64<long> ShiftRightLogicalAddRoundedScalar(Vector64<long> addend, Vector64<long> value, byte count);
        Vector64<ulong> ShiftRightLogicalAddRoundedScalar(Vector64<ulong> addend, Vector64<ulong> value, byte count);

        /// <summary>
        /// Signed Rounding Shift Right and Accumulate
        /// For each element result[elem] = addend[elem] + (value[elem] >> RoundedShift)
        /// Corresponds to vector forms of SRSRA and VRSRA
        /// </summary>
        Vector64<sbyte> ShiftRightArithmeticAddRounded(Vector64<sbyte> addend, Vector64<sbyte> value, byte count);
        Vector64<short> ShiftRightArithmeticAddRounded(Vector64<short> addend, Vector64<short> value, byte count);
        Vector64<int> ShiftRightArithmeticAddRounded(Vector64<int> addend, Vector64<int> value, byte count);
        Vector128<sbyte> ShiftRightArithmeticAddRounded(Vector128<sbyte> addend, Vector128<sbyte> value, byte count);
        Vector128<short> ShiftRightArithmeticAddRounded(Vector128<short> addend, Vector128<short> value, byte count);
        Vector128<int> ShiftRightArithmeticAddRounded(Vector128<int> addend, Vector128<int> value, byte count);
        Vector128<long> ShiftRightArithmeticAddRounded(Vector128<long> addend, Vector128<long> value, byte count);

        Vector64<long> ShiftRightArithmeticAddRoundedScalar(Vector64<long> addend, Vector64<long> value, byte count);

        /// <summary>
        /// Signed Saturating Shift Left and Unsigned Saturating Shift Left
        /// For each element result[elem] = value[elem] << shift
        /// Corresponds to vector forms of SQSHL, UQSHL, and VQSHL
        /// </summary>
        Vector64<byte> ShiftLeftLogicalSaturate(Vector64<byte> value, byte count);
        Vector64<sbyte> ShiftLeftLogicalSaturate(Vector64<sbyte> value, byte count);
        Vector64<short> ShiftLeftLogicalSaturate(Vector64<short> value, byte count);
        Vector64<ushort> ShiftLeftLogicalSaturate(Vector64<ushort> value, byte count);
        Vector64<int> ShiftLeftLogicalSaturate(Vector64<int> value, byte count);
        Vector64<uint> ShiftLeftLogicalSaturate(Vector64<uint> value, byte count);
        Vector128<byte> ShiftLeftLogicalSaturate(Vector128<byte> value, byte count);
        Vector128<sbyte> ShiftLeftLogicalSaturate(Vector128<sbyte> value, byte count);
        Vector128<short> ShiftLeftLogicalSaturate(Vector128<short> value, byte count);
        Vector128<ushort> ShiftLeftLogicalSaturate(Vector128<ushort> value, byte count);
        Vector128<int> ShiftLeftLogicalSaturate(Vector128<int> value, byte count);
        Vector128<uint> ShiftLeftLogicalSaturate(Vector128<uint> value, byte count);
        Vector128<long> ShiftLeftLogicalSaturate(Vector128<long> value, byte count);
        Vector128<ulong> ShiftLeftLogicalSaturate(Vector128<ulong> value, byte count);

        Vector64<long> ShiftLeftLogicalSaturateScalar(Vector64<long> value, byte count);
        Vector64<ulong> ShiftLeftLogicalSaturateScalar(Vector64<ulong> value, byte count);

        /// <summary>
        /// Signed Saturating Shift Left Unsigned
        /// For each element result[elem] = value[elem] << shift
        /// Corresponds to vector forms of SQSHLU and VQSHLU
        /// </summary>
        Vector64<byte> ShiftLeftLogicalSaturateUnsigned(Vector64<sbyte> value, byte count);
        Vector64<ushort> ShiftLeftLogicalSaturateUnsigned(Vector64<short> value, byte count);
        Vector64<uint> ShiftLeftLogicalSaturateUnsigned(Vector64<int> value, byte count);
        Vector128<byte> ShiftLeftLogicalSaturateUnsigned(Vector128<sbyte> value, byte count);
        Vector128<ushort> ShiftLeftLogicalSaturateUnsigned(Vector128<short> value, byte count);
        Vector128<uint> ShiftLeftLogicalSaturateUnsigned(Vector128<int> value, byte count);
        Vector128<ulong> ShiftLeftLogicalSaturateUnsigned(Vector128<long> value, byte count);

        Vector64<ulong> ShiftLeftLogicalSaturateUnsignedScalar(Vector64<long> value, byte count);

        /// <summary>
        /// Shift Right Narrow Immediate
        /// For each element result[elem] = value[elem] >> shift
        /// Corresponds to vector forms of SHRN and VSHRN
        /// </summary>
        Vector64<sbyte> ShiftRightLogicalAndNarrowLower(Vector128<short> value, byte count);
        Vector64<byte> ShiftRightLogicalAndNarrowLower(Vector128<ushort> value, byte count);
        Vector64<short> ShiftRightLogicalAndNarrowLower(Vector128<int> value, byte count);
        Vector64<ushort> ShiftRightLogicalAndNarrowLower(Vector128<uint> value, byte count);
        Vector64<int> ShiftRightLogicalAndNarrowLower(Vector128<long> value, byte count);
        Vector64<uint> ShiftRightLogicalAndNarrowLower(Vector128<ulong> value, byte count);

        /// <summary>
        /// Rounding Shift Right Narrow Immediate
        /// For each element result[elem] = (value[elem] + (1 << (shift - 1))) >> shift
        /// Corresponds to vector forms of RSHRN and VRSHRN
        /// </summary>
        Vector64<sbyte> ShiftRightLogicalAndNarrowRoundedLower(Vector128<short> value, byte count);
        Vector64<byte> ShiftRightLogicalAndNarrowRoundedLower(Vector128<ushort> value, byte count);
        Vector64<short> ShiftRightLogicalAndNarrowRoundedLower(Vector128<int> value, byte count);
        Vector64<ushort> ShiftRightLogicalAndNarrowRoundedLower(Vector128<uint> value, byte count);
        Vector64<int> ShiftRightLogicalAndNarrowRoundedLower(Vector128<long> value, byte count);
        Vector64<uint> ShiftRightLogicalAndNarrowRoundedLower(Vector128<ulong> value, byte count);

        /// <summary>
        /// Shift Left Long
        /// For each element result[elem] = value[elem] << shift
        /// Corresponds to vector forms of SHLL and VSHLL
        /// </summary>
        Vector128<short> ShiftLeftLogicalAndWidenLower(Vector64<sbyte> value, byte count);
        Vector128<ushort> ShiftLeftLogicalAndWidenLower(Vector64<byte> value, byte count);
        Vector128<int> ShiftLeftLogicalAndWidenLower(Vector64<short> value, byte count);
        Vector128<uint> ShiftLeftLogicalAndWidenLower(Vector64<ushort> value, byte count);
        Vector128<long> ShiftLeftLogicalAndWidenLower(Vector64<int> value, byte count);
        Vector128<ulong> ShiftLeftLogicalAndWidenLower(Vector64<uint> value, byte count);

        /// <summary>
        /// Unsigned Saturating Shift Right Narrow Immediate
        /// For each element result[elem] = value[elem] >> shift
        /// Corresponds to vector forms of UQSHRN and VQSHRUN
        /// </summary>
        Vector64<sbyte> ShiftRightLogicalAndNarrowSaturateLower(Vector128<short> value, byte count);
        Vector64<byte> ShiftRightLogicalAndNarrowSaturateLower(Vector128<ushort> value, byte count);
        Vector64<short> ShiftRightLogicalAndNarrowSaturateLower(Vector128<int> value, byte count);
        Vector64<ushort> ShiftRightLogicalAndNarrowSaturateLower(Vector128<uint> value, byte count);
        Vector64<int> ShiftRightLogicalAndNarrowSaturateLower(Vector128<long> value, byte count);
        Vector64<uint> ShiftRightLogicalAndNarrowSaturateLower(Vector128<ulong> value, byte count);

        /// <summary>
        /// Unsigned Saturating Rounded Shift Right Narrow Immediate
        /// For each element result[elem] = value[elem] >> shift
        /// Corresponds to vector forms of UQRSHRN and VQRSHRUN
        /// </summary>
        Vector64<sbyte> ShiftRightLogicalAndNarrowRoundedSaturateLower(Vector128<short> value, byte count);
        Vector64<byte> ShiftRightLogicalAndNarrowRoundedSaturateLower(Vector128<ushort> value, byte count);
        Vector64<short> ShiftRightLogicalAndNarrowRoundedSaturateLower(Vector128<int> value, byte count);
        Vector64<ushort> ShiftRightLogicalAndNarrowRoundedSaturateLower(Vector128<uint> value, byte count);
        Vector64<int> ShiftRightLogicalAndNarrowRoundedSaturateLower(Vector128<long> value, byte count);
        Vector64<uint> ShiftRightLogicalAndNarrowRoundedSaturateLower(Vector128<ulong> value, byte count);

        /// <summary>
        /// Signed Saturating Shift Right Narrow Immediate
        /// For each element result[elem] = value[elem] >> shift
        /// Corresponds to vector forms of SQSHRN and VQSHRN
        /// </summary>
        Vector64<sbyte> ShiftRightArithmeticAndNarrowSaturateLower(Vector128<short> value, byte count);
        Vector64<short> ShiftRightArithmeticAndNarrowSaturateLower(Vector128<int> value, byte count);
        Vector64<int> ShiftRightArithmeticAndNarrowSaturateLower(Vector128<long> value, byte count);

        /// <summary>
        /// Signed Saturating Rounded Shift Right Narrow Immediate
        /// For each element result[elem] = value[elem] >> shift
        /// Corresponds to vector forms of SQRSHRN and VQRSHRN
        /// </summary>
        Vector64<sbyte> ShiftRightArithmeticAndNarrowRoundedSaturateLower(Vector128<short> value, byte count);
        Vector64<short> ShiftRightArithmeticAndNarrowRoundedSaturateLower(Vector128<int> value, byte count);
        Vector64<int> ShiftRightArithmeticAndNarrowRoundedSaturateLower(Vector128<long> value, byte count);

        public abstract class Arm64
        {
            /// <summary>
            /// Signed Saturating Rounding Shift Left
            /// For each element result[elem] = (value[elem] + (1 << (-(ShiftLogical[elem] & 0xFF) - 1))) << (ShiftLogical[elem] & 0xFF)
            /// Corresponds to vector forms of SQRSHL
            /// </summary>
            Vector64<sbyte> ShiftArithmeticRoundedSaturateScalar(Vector64<sbyte> value, Vector64<sbyte> count);
            Vector64<short> ShiftArithmeticRoundedSaturateScalar(Vector64<short> value, Vector64<short> count);
            Vector64<int> ShiftArithmeticRoundedSaturateScalar(Vector64<int> value, Vector64<int> count);

            /// <summary>
            /// Signed Saturating Shift Left
            /// For each element result[elem] = value[elem] << (ShiftLogical[elem] & 0xFF)
            /// Corresponds to vector forms of SQSHL
            /// </summary>
            Vector64<sbyte> ShiftArithmeticSaturateScalar(Vector64<sbyte> value, Vector64<sbyte> count);
            Vector64<short> ShiftArithmeticSaturateScalar(Vector64<short> value, Vector64<short> count);
            Vector64<int> ShiftArithmeticSaturateScalar(Vector64<int> value, Vector64<int> count);

            /// <summary>
            /// Signed Rounding Shift Left
            /// For each element result[elem] = (value[elem] + (1 << (-(shift[elem] & 0xFF) - 1))) << (shift[elem] & 0xFF)
            /// Corresponds to vector forms of SRSHL
            /// </summary>
            Vector64<sbyte> ShiftArithmeticRoundedScalar(Vector64<sbyte> value, Vector64<sbyte> count);
            Vector64<short> ShiftArithmeticRoundedScalar(Vector64<short> value, Vector64<short> count);
            Vector64<int> ShiftArithmeticRoundedScalar(Vector64<int> value, Vector64<int> count);

            /// <summary>
            /// Signed Shift Left
            /// For each element result[elem] = value[elem] << (shift[elem] & 0xFF)
            /// Corresponds to vector forms of SSHL
            /// </summary>
            Vector64<sbyte> ShiftArithmeticScalar(Vector64<sbyte> value, Vector64<sbyte> count);
            Vector64<short> ShiftArithmeticScalar(Vector64<short> value, Vector64<short> count);
            Vector64<int> ShiftArithmeticScalar(Vector64<int> value, Vector64<int> count);

            /// <summary>
            /// Unsigned Saturating Rounding Shift Left
            /// For each element result[elem] = (value[elem] + (1 << (-(ShiftLogical[elem] & 0xFF) - 1))) << (ShiftLogical[elem] & 0xFF)
            /// Corresponds to vector forms of UQRSHL and VQRSHL
            /// </summary>
            Vector64<byte> ShiftLogicalRoundedSaturateScalar(Vector64<byte> value, Vector64<sbyte> count);
            Vector64<sbyte> ShiftLogicalRoundedSaturateScalar(Vector64<sbyte> value, Vector64<sbyte> count);
            Vector64<short> ShiftLogicalRoundedSaturateScalar(Vector64<short> value, Vector64<short> count);
            Vector64<ushort> ShiftLogicalRoundedSaturateScalar(Vector64<ushort> value, Vector64<short> count);
            Vector64<int> ShiftLogicalRoundedSaturateScalar(Vector64<int> value, Vector64<int> count);
            Vector64<uint> ShiftLogicalRoundedSaturateScalar(Vector64<uint> value, Vector64<int> count);

            /// <summary>
            /// Unsigned Saturating Shift Left
            /// For each element result[elem] = value[elem] << (ShiftLogical[elem] & 0xFF)
            /// Corresponds to vector forms of UQSHL and VQSHL
            /// </summary>
            Vector64<byte> ShiftLogicalSaturateScalar(Vector64<byte> value, Vector64<sbyte> count);
            Vector64<sbyte> ShiftLogicalSaturateScalar(Vector64<sbyte> value, Vector64<sbyte> count);
            Vector64<short> ShiftLogicalSaturateScalar(Vector64<short> value, Vector64<short> count);
            Vector64<ushort> ShiftLogicalSaturateScalar(Vector64<ushort> value, Vector64<short> count);
            Vector64<int> ShiftLogicalSaturateScalar(Vector64<int> value, Vector64<int> count);
            Vector64<uint> ShiftLogicalSaturateScalar(Vector64<uint> value, Vector64<int> count);

            /// <summary>
            /// Unsigned Rounding Shift Left
            /// For each element result[elem] = (value[elem] + (1 << (-(shift[elem] & 0xFF) - 1))) << (shift[elem] & 0xFF)
            /// Corresponds to vector forms of URSHL and VRSHL
            /// </summary>
            Vector64<byte> ShiftLogicalRoundedScalar(Vector64<byte> value, Vector64<sbyte> count);
            Vector64<sbyte> ShiftLogicalRoundedScalar(Vector64<sbyte> value, Vector64<sbyte> count);
            Vector64<short> ShiftLogicalRoundedScalar(Vector64<short> value, Vector64<short> count);
            Vector64<ushort> ShiftLogicalRoundedScalar(Vector64<ushort> value, Vector64<short> count);
            Vector64<int> ShiftLogicalRoundedScalar(Vector64<int> value, Vector64<int> count);
            Vector64<uint> ShiftLogicalRoundedScalar(Vector64<uint> value, Vector64<int> count);


            /// <summary>
            /// Unsigned Shift Left
            /// For each element result[elem] = value[elem] << (shift[elem] & 0xFF)
            /// Corresponds to vector forms of USHL and VSHL
            /// </summary>
            Vector64<byte> ShiftLogicalScalar(Vector64<byte> value, Vector64<sbyte> count);
            Vector64<sbyte> ShiftLogicalScalar(Vector64<sbyte> value, Vector64<sbyte> count);
            Vector64<short> ShiftLogicalScalar(Vector64<short> value, Vector64<short> count);
            Vector64<ushort> ShiftLogicalScalar(Vector64<ushort> value, Vector64<short> count);
            Vector64<int> ShiftLogicalScalar(Vector64<int> value, Vector64<int> count);
            Vector64<uint> ShiftLogicalScalar(Vector64<uint> value, Vector64<int> count);

            /// <summary>
            /// Signed Extend Long
            /// For each element result[elem] = value[elem] << shift
            /// Corresponds to vector forms of SXTL
            /// </summary>
            Vector128<short> SignExtendAndWidenLower(Vector64<sbyte> value, byte count);
            Vector128<int> SignExtendAndWidenLower(Vector64<short> value, byte count);
            Vector128<long> SignExtendAndWidenLower(Vector64<int> value, byte count);

            /// <summary>
            /// Unsigned Extend Long
            /// For each element result[elem] = value[elem] << shift
            /// Corresponds to vector forms of UXTL
            /// </summary>
            Vector128<short> ZeroExtendAndWidenLower(Vector64<sbyte> value, byte count);
            Vector128<ushort> ZeroExtendAndWidenLower(Vector64<byte> value, byte count);
            Vector128<int> ZeroExtendAndWidenLower(Vector64<short> value, byte count);
            Vector128<uint> ZeroExtendAndWidenLower(Vector64<ushort> value, byte count);
            Vector128<long> ZeroExtendAndWidenLower(Vector64<int> value, byte count);
            Vector128<ulong> ZeroExtendAndWidenLower(Vector64<uint> value, byte count);

            /// <summary>
            /// Signed Saturating Shift Right Unsigned Narrow Immediate
            /// For each element result[elem] = value[elem] >> shift
            /// Corresponds to vector forms of SQSHRUN
            /// </summary>
            Vector64<byte> ShiftRightArithmeticUnsignedAndNarrowSaturateLower(Vector128<short> value, byte count);
            Vector64<ushort> ShiftRightArithmeticUnsignedAndNarrowSaturateLower(Vector128<int> value, byte count);
            Vector64<uint> ShiftRightArithmeticUnsignedAndNarrowSaturateLower(Vector128<long> value, byte count);

            /// <summary>
            /// Signed Saturating Rounded Shift Right Unsigned Narrow Immediate
            /// For each element result[elem] = value[elem] >> shift
            /// Corresponds to vector forms of SQRSHRUN
            /// </summary>
            Vector64<byte> ShiftRightArithmeticUnsignedAndNarrowRoundedSaturateLower(Vector128<short> value, byte count);
            Vector64<ushort> ShiftRightArithmeticUnsignedAndNarrowRoundedSaturateLower(Vector128<int> value, byte count);
            Vector64<uint> ShiftRightArithmeticUnsignedAndNarrowRoundedSaturateLower(Vector128<long> value, byte count);

            /// <summary>
            /// Shift Right Narrow Immediate
            /// For each element result[elem] = value[elem] >> shift
            /// Corresponds to vector forms of SHRN2
            /// </summary>
            Vector128<sbyte> ShiftRightLogicalAndNarrowUpper(Vector64<sbyte> lower, Vector128<short> value, byte count);
            Vector128<byte> ShiftRightLogicalAndNarrowUpper(Vector64<byte> lower, Vector128<ushort> value, byte count);
            Vector128<short> ShiftRightLogicalAndNarrowUpper(Vector64<short> lower, Vector128<int> value, byte count);
            Vector128<ushort> ShiftRightLogicalAndNarrowUpper(Vector64<ushort> lower, Vector128<uint> value, byte count);
            Vector128<int> ShiftRightLogicalAndNarrowUpper(Vector64<int> lower, Vector128<long> value, byte count);
            Vector128<uint> ShiftRightLogicalAndNarrowUpper(Vector64<uint> lower, Vector128<ulong> value, byte count);

            /// <summary>
            /// Rounding Shift Right Narrow Immediate
            /// For each element result[elem] = (value[elem] + (1 << (shift - 1))) >> shift
            /// Corresponds to vector forms of RSHRN2
            /// </summary>
            Vector128<sbyte> ShiftRightLogicalAndNarrowRoundedUpper(Vector64<sbyte> lower, Vector128<short> value, byte count);
            Vector128<byte> ShiftRightLogicalAndNarrowRoundedUpper(Vector64<byte> lower, Vector128<ushort> value, byte count);
            Vector128<short> ShiftRightLogicalAndNarrowRoundedUpper(Vector64<short> lower, Vector128<int> value, byte count);
            Vector128<ushort> ShiftRightLogicalAndNarrowRoundedUpper(Vector64<ushort> lower, Vector128<uint> value, byte count);
            Vector128<int> ShiftRightLogicalAndNarrowRoundedUpper(Vector64<int> lower, Vector128<long> value, byte count);
            Vector128<uint> ShiftRightLogicalAndNarrowRoundedUpper(Vector64<uint> lower, Vector128<ulong> value, byte count);

            /// <summary>
            /// Shift Left Long
            /// For each element result[elem] = value[elem] << shift
            /// Corresponds to vector forms of SHLL2
            /// </summary>
            Vector128<short> ShiftLeftLogicalAndWidenUpper(Vector128<sbyte> value, byte count);
            Vector128<ushort> ShiftLeftLogicalAndWidenUpper(Vector128<byte> value, byte count);
            Vector128<int> ShiftLeftLogicalAndWidenUpper(Vector128<short> value, byte count);
            Vector128<uint> ShiftLeftLogicalAndWidenUpper(Vector128<ushort> value, byte count);
            Vector128<long> ShiftLeftLogicalAndWidenUpper(Vector128<int> value, byte count);
            Vector128<ulong> ShiftLeftLogicalAndWidenUpper(Vector128<uint> value, byte count);

            /// <summary>
            /// Signed Extend Long
            /// For each element result[elem] = value[elem] << shift
            /// Corresponds to vector forms of SXTL2
            /// </summary>
            Vector128<short> SignExtendAndWidenUpper(Vector128<sbyte> value, byte count);
            Vector128<int> SignExtendAndWidenUpper(Vector128<short> value, byte count);
            Vector128<long> SignExtendAndWidenUpper(Vector128<int> value, byte count);

            /// <summary>
            /// Unsigned Extend Long
            /// For each element result[elem] = value[elem] << shift
            /// Corresponds to vector forms of UXTL2
            /// </summary>
            Vector128<short> ZeroExtendAndWidenUpper(Vector128<sbyte> value, byte count);
            Vector128<ushort> ZeroExtendAndWidenUpper(Vector128<byte> value, byte count);
            Vector128<int> ZeroExtendAndWidenUpper(Vector128<short> value, byte count);
            Vector128<uint> ZeroExtendAndWidenUpper(Vector128<ushort> value, byte count);
            Vector128<long> ZeroExtendAndWidenUpper(Vector128<int> value, byte count);
            Vector128<ulong> ZeroExtendAndWidenUpper(Vector128<uint> value, byte count);

            /// <summary>
            /// Unsigned Saturating Shift Right Narrow Immediate
            /// For each element result[elem] = value[elem] >> shift
            /// Corresponds to vector forms of UQSHRN2
            /// </summary>
            Vector128<sbyte> ShiftRightLogicalAndNarrowSaturateUpper(Vector64<sbyte> lower, Vector128<short> value, byte count);
            Vector128<byte> ShiftRightLogicalAndNarrowSaturateUpper(Vector64<byte> lower, Vector128<ushort> value, byte count);
            Vector128<short> ShiftRightLogicalAndNarrowSaturateUpper(Vector64<short> lower, Vector128<int> value, byte count);
            Vector128<ushort> ShiftRightLogicalAndNarrowSaturateUpper(Vector64<ushort> lower, Vector128<uint> value, byte count);
            Vector128<int> ShiftRightLogicalAndNarrowSaturateUpper(Vector64<int> lower, Vector128<long> value, byte count);
            Vector128<uint> ShiftRightLogicalAndNarrowSaturateUpper(Vector64<uint> lower, Vector128<ulong> value, byte count);

            /// <summary>
            /// Unsigned Saturating Rounded Shift Right Narrow Immediate
            /// For each element result[elem] = value[elem] >> shift
            /// Corresponds to vector forms of UQRSHRN2
            /// </summary>
            Vector128<sbyte> ShiftRightLogicalAndNarrowRoundedSaturateUpper(Vector64<sbyte> lower, Vector128<short> value, byte count);
            Vector128<byte> ShiftRightLogicalAndNarrowRoundedSaturateUpper(Vector64<byte> lower, Vector128<ushort> value, byte count);
            Vector128<short> ShiftRightLogicalAndNarrowRoundedSaturateUpper(Vector64<short> lower, Vector128<int> value, byte count);
            Vector128<ushort> ShiftRightLogicalAndNarrowRoundedSaturateUpper(Vector64<ushort> lower, Vector128<uint> value, byte count);
            Vector128<int> ShiftRightLogicalAndNarrowRoundedSaturateUpper(Vector64<int> lower, Vector128<long> value, byte count);
            Vector128<uint> ShiftRightLogicalAndNarrowRoundedSaturateUpper(Vector64<uint> lower, Vector128<ulong> value, byte count);

            /// <summary>
            /// Signed Saturating Shift Right Narrow Immediate
            /// For each element result[elem] = value[elem] >> shift
            /// Corresponds to vector forms of SQSHRN2
            /// </summary>
            Vector128<sbyte> ShiftRightArithmeticAndNarrowSaturateUpper(Vector64<sbyte> lower, Vector128<short> value, byte count);
            Vector128<short> ShiftRightArithmeticAndNarrowSaturateUpper(Vector64<short> lower, Vector128<int> value, byte count);
            Vector128<int> ShiftRightArithmeticAndNarrowSaturateUpper(Vector64<int> lower, Vector128<long> value, byte count);

            /// <summary>
            /// Signed Saturating Rounded Shift Right Narrow Immediate
            /// For each element result[elem] = value[elem] >> shift
            /// Corresponds to vector forms of SQRSHRN2
            /// </summary>
            Vector128<sbyte> ShiftRightArithmeticAndNarrowRoundedSaturateUpper(Vector64<sbyte> lower, Vector128<short> value, byte count);
            Vector128<short> ShiftRightArithmeticAndNarrowRoundedSaturateUpper(Vector64<short> lower, Vector128<int> value, byte count);
            Vector128<int> ShiftRightArithmeticAndNarrowRoundedSaturateUpper(Vector64<int> lower, Vector128<long> value, byte count);

            /// <summary>
            /// Signed Saturating Shift Right Unsigned Narrow Immediate
            /// For each element result[elem] = value[elem] >> shift
            /// Corresponds to vector forms of SQSHRUN2
            /// </summary>
            Vector128<byte> ShiftRightArithmeticUnsignedAndNarrowSaturateUpper(Vector64<sbyte> lower, Vector128<short> value, byte count);
            Vector128<ushort> ShiftRightArithmeticUnsignedAndNarrowSaturateUpper(Vector64<short> lower, Vector128<int> value, byte count);
            Vector128<uint> ShiftRightArithmeticUnsignedAndNarrowSaturateUpper(Vector64<int> lower, Vector128<long> value, byte count);

            /// <summary>
            /// Signed Saturating Rounded Shift Right Unsigned Narrow Immediate
            /// For each element result[elem] = value[elem] >> shift
            /// Corresponds to vector forms of SQRSHRUN2
            /// </summary>
            Vector128<byte> ShiftRightArithmeticUnsignedAndNarrowRoundedSaturateUpper(Vector64<sbyte> lower, Vector128<short> value, byte count);
            Vector128<ushort> ShiftRightArithmeticUnsignedAndNarrowRoundedSaturateUpper(Vector64<short> lower, Vector128<int> value, byte count);
            Vector128<uint> ShiftRightArithmeticUnsignedAndNarrowRoundedSaturateUpper(Vector64<int> lower, Vector128<long> value, byte count);
        }
    }
}
```
