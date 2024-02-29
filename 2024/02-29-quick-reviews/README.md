# API Review 02/29/2024

## Expose `AVX10` converged vector ISA

**Approved** | [#runtime/98069](https://github.com/dotnet/runtime/issues/98069#issuecomment-1971768501) | [Video](https://www.youtube.com/watch?v=o1GpyXImDjc&t=0h0m0s)

* Looks good as proposed
* We decided that `Avx10v1.V512` should have the full `Avx512*` API surface
    - Easiest to make it extend `Avx512BW` and copy in the others

```C#
namespace System.Runtime.Intrinsics.X86;

public abstract class Avx10v1 : Avx2
{
    public static new bool IsSupported { get; }
    
    /// From AVX512F VL
    public static Vector128<ulong> Abs(Vector128<long> value);
    public static Vector128<int> AlignRight32(Vector128<int> left, Vector128<int> right, [ConstantExpected] byte mask);
    public static Vector128<uint> AlignRight32(Vector128<uint> left, Vector128<uint> right, [ConstantExpected] byte mask);
    public static Vector128<long> AlignRight64(Vector128<long> left, Vector128<long> right, [ConstantExpected] byte mask);
    public static Vector128<ulong> AlignRight64(Vector128<ulong> left, Vector128<ulong> right, [ConstantExpected] byte mask);
    public static Vector128<int> CompareGreaterThanOrEqual(Vector128<int> left, Vector128<int> right);
    public static Vector128<int> CompareLessThan(Vector128<int> left, Vector128<int> right);
    public static Vector128<int> CompareLessThanOrEqual(Vector128<int> left, Vector128<int> right);
    public static Vector128<int> CompareNotEqual(Vector128<int> left, Vector128<int> right);
    public static Vector128<long> CompareGreaterThanOrEqual(Vector128<long> left, Vector128<long> right);
    public static Vector128<long> CompareLessThan(Vector128<long> left, Vector128<long> right);
    public static Vector128<long> CompareLessThanOrEqual(Vector128<long> left, Vector128<long> right);
    public static Vector128<long> CompareNotEqual(Vector128<long> left, Vector128<long> right);
    public static Vector128<uint> CompareGreaterThan(Vector128<uint> left, Vector128<uint> right);
    public static Vector128<uint> CompareGreaterThanOrEqual(Vector128<uint> left, Vector128<uint> right);
    public static Vector128<uint> CompareLessThan(Vector128<uint> left, Vector128<uint> right);
    public static Vector128<uint> CompareLessThanOrEqual(Vector128<uint> left, Vector128<uint> right);
    public static Vector128<uint> CompareNotEqual(Vector128<uint> left, Vector128<uint> right);
    public static Vector128<ulong> CompareGreaterThan(Vector128<ulong> left, Vector128<ulong> right);
    public static Vector128<ulong> CompareGreaterThanOrEqual(Vector128<ulong> left, Vector128<ulong> right);
    public static Vector128<ulong> CompareLessThan(Vector128<ulong> left, Vector128<ulong> right);
    public static Vector128<ulong> CompareLessThanOrEqual(Vector128<ulong> left, Vector128<ulong> right);
    public static Vector128<ulong> CompareNotEqual(Vector128<ulong> left, Vector128<ulong> right);
    public static Vector128<byte> ConvertToVector128Byte(Vector128<int> value);
    public static Vector128<byte> ConvertToVector128Byte(Vector128<long> value);
    public static Vector128<byte> ConvertToVector128Byte(Vector128<uint> value);
    public static Vector128<byte> ConvertToVector128Byte(Vector128<ulong> value);
    public static Vector128<byte> ConvertToVector128ByteWithSaturation(Vector128<uint> value);
    public static Vector128<byte> ConvertToVector128ByteWithSaturation(Vector128<ulong> value);
    public static Vector128<double> ConvertToVector128Double(Vector128<uint> value);
    public static Vector128<short> ConvertToVector128Int16(Vector128<int> value);
    public static Vector128<short> ConvertToVector128Int16(Vector128<long> value);
    public static Vector128<short> ConvertToVector128Int16(Vector128<uint> value);
    public static Vector128<short> ConvertToVector128Int16(Vector128<ulong> value);
    public static Vector128<short> ConvertToVector128Int16WithSaturation(Vector128<int> value);
    public static Vector128<short> ConvertToVector128Int16WithSaturation(Vector128<long> value);
    public static Vector128<int> ConvertToVector128Int32(Vector128<long> value);
    public static Vector128<int> ConvertToVector128Int32(Vector128<ulong> value);
    public static Vector128<int> ConvertToVector128Int32WithSaturation(Vector128<long> value);
    public static Vector128<sbyte> ConvertToVector128SByte(Vector128<int> value);
    public static Vector128<sbyte> ConvertToVector128SByte(Vector128<long> value);
    public static Vector128<sbyte> ConvertToVector128SByte(Vector128<uint> value);
    public static Vector128<sbyte> ConvertToVector128SByte(Vector128<ulong> value);
    public static Vector128<sbyte> ConvertToVector128SByteWithSaturation(Vector128<int> value);
    public static Vector128<sbyte> ConvertToVector128SByteWithSaturation(Vector128<long> value);
    public static Vector128<float> ConvertToVector128Single(Vector128<uint> value);
    public static Vector128<ushort> ConvertToVector128UInt16(Vector128<int> value);
    public static Vector128<ushort> ConvertToVector128UInt16(Vector128<long> value);
    public static Vector128<ushort> ConvertToVector128UInt16(Vector128<uint> value);
    public static Vector128<ushort> ConvertToVector128UInt16(Vector128<ulong> value);
    public static Vector128<ushort> ConvertToVector128UInt16WithSaturation(Vector128<uint> value);
    public static Vector128<ushort> ConvertToVector128UInt16WithSaturation(Vector128<ulong> value);
    public static Vector128<uint> ConvertToVector128UInt32(Vector128<long> value);
    public static Vector128<uint> ConvertToVector128UInt32(Vector128<ulong> value);
    public static Vector128<uint> ConvertToVector128UInt32(Vector128<float> value);
    public static Vector128<uint> ConvertToVector128UInt32(Vector128<double> value);
    public static Vector128<uint> ConvertToVector128UInt32WithSaturation(Vector128<ulong> value);
    public static Vector128<uint> ConvertToVector128UInt32WithTruncation(Vector128<float> value);
    public static Vector128<uint> ConvertToVector128UInt32WithTruncation(Vector128<double> value);
    public static Vector128<float> Fixup(Vector128<float> left, Vector128<float> right, Vector128<int> table, [ConstantExpected] byte control);
    public static Vector128<double> Fixup(Vector128<double> left, Vector128<double> right, Vector128<long> table, [ConstantExpected] byte control);
    public static Vector128<float> GetExponent(Vector128<float> value);
    public static Vector128<double> GetExponent(Vector128<double> value);
    public static Vector128<float> GetMantissa(Vector128<float> value, [ConstantExpected(Max = (byte)(0x0F))] byte control);
    public static Vector128<double> GetMantissa(Vector128<double> value, [ConstantExpected(Max = (byte)(0x0F))] byte control);
    public static Vector128<long> Max(Vector128<long> left, Vector128<long> right);
    public static Vector128<ulong> Max(Vector128<ulong> left, Vector128<ulong> right);
    public static Vector128<long> Min(Vector128<long> left, Vector128<long> right);
    public static Vector128<ulong> Min(Vector128<ulong> left, Vector128<ulong> right);
    public static Vector128<long> PermuteVar2x64x2(Vector128<long> lower, Vector128<long> indices, Vector128<long> upper);
    public static Vector128<ulong> PermuteVar2x64x2(Vector128<ulong> lower, Vector128<ulong> indices, Vector128<ulong> upper);
    public static Vector128<double> PermuteVar2x64x2(Vector128<double> lower, Vector128<long> indices, Vector128<double> upper);
    public static Vector128<int> PermuteVar4x32x2(Vector128<int> lower, Vector128<int> indices, Vector128<int> upper);
    public static Vector128<uint> PermuteVar4x32x2(Vector128<uint> lower, Vector128<uint> indices, Vector128<uint> upper);
    public static Vector128<float> PermuteVar4x32x2(Vector128<float> lower, Vector128<int> indices, Vector128<float> upper);
    public static Vector128<float> Reciprocal14(Vector128<float> value);
    public static Vector128<double> Reciprocal14(Vector128<double> value);
    public static Vector128<float> ReciprocalSqrt14(Vector128<float> value);
    public static Vector128<double> ReciprocalSqrt14(Vector128<double> value);
    public static Vector128<int> RotateLeft(Vector128<int> value, [ConstantExpected] byte count);
    public static Vector128<uint> RotateLeft(Vector128<uint> value, [ConstantExpected] byte count);
    public static Vector128<long> RotateLeft(Vector128<long> value, [ConstantExpected] byte count);
    public static Vector128<ulong> RotateLeft(Vector128<ulong> value, [ConstantExpected] byte count);
    public static Vector128<int> RotateLeftVariable(Vector128<int> value, Vector128<uint> count);
    public static Vector128<uint> RotateLeftVariable(Vector128<uint> value, Vector128<uint> count);
    public static Vector128<long> RotateLeftVariable(Vector128<long> value, Vector128<ulong> count);
    public static Vector128<ulong> RotateLeftVariable(Vector128<ulong> value, Vector128<ulong> count);
    public static Vector128<int> RotateRight(Vector128<int> value, [ConstantExpected] byte count);
    public static Vector128<uint> RotateRight(Vector128<uint> value, [ConstantExpected] byte count);
    public static Vector128<long> RotateRight(Vector128<long> value, [ConstantExpected] byte count);
    public static Vector128<ulong> RotateRight(Vector128<ulong> value, [ConstantExpected] byte count);
    public static Vector128<int> RotateRightVariable(Vector128<int> value, Vector128<uint> count);
    public static Vector128<uint> RotateRightVariable(Vector128<uint> value, Vector128<uint> count);
    public static Vector128<long> RotateRightVariable(Vector128<long> value, Vector128<ulong> count);
    public static Vector128<ulong> RotateRightVariable(Vector128<ulong> value, Vector128<ulong> count);
    public static Vector128<float> RoundScale(Vector128<float> value, [ConstantExpected] byte control);
    public static Vector128<double> RoundScale(Vector128<double> value, [ConstantExpected] byte control);
    public static Vector128<float> Scale(Vector128<float> left, Vector128<float> right);
    public static Vector128<double> Scale(Vector128<double> left, Vector128<double> right);
    public static Vector128<long> ShiftRightArithmetic(Vector128<long> value, Vector128<long> count);
    public static Vector128<long> ShiftRightArithmetic(Vector128<long> value, [ConstantExpected] byte count);
    public static Vector128<long> ShiftRightArithmeticVariable(Vector128<long> value, Vector128<ulong> count);
    public static Vector128<sbyte> TernaryLogic(Vector128<sbyte> a, Vector128<sbyte> b, Vector128<sbyte> c, [ConstantExpected] byte control);
    public static Vector128<byte> TernaryLogic(Vector128<byte> a, Vector128<byte> b, Vector128<byte> c, [ConstantExpected] byte control);
    public static Vector128<short> TernaryLogic(Vector128<short> a, Vector128<short> b, Vector128<short> c, [ConstantExpected] byte control);
    public static Vector128<ushort> TernaryLogic(Vector128<ushort> a, Vector128<ushort> b, Vector128<ushort> c, [ConstantExpected] byte control);
    public static Vector128<int> TernaryLogic(Vector128<int> a, Vector128<int> b, Vector128<int> c, [ConstantExpected] byte control);
    public static Vector128<uint> TernaryLogic(Vector128<uint> a, Vector128<uint> b, Vector128<uint> c, [ConstantExpected] byte control);
    public static Vector128<long> TernaryLogic(Vector128<long> a, Vector128<long> b, Vector128<long> c, [ConstantExpected] byte control);
    public static Vector128<ulong> TernaryLogic(Vector128<ulong> a, Vector128<ulong> b, Vector128<ulong> c, [ConstantExpected] byte control);
    public static Vector128<float> TernaryLogic(Vector128<float> a, Vector128<float> b, Vector128<float> c, [ConstantExpected] byte control);
    public static Vector128<double> TernaryLogic(Vector128<double> a, Vector128<double> b, Vector128<double> c, [ConstantExpected] byte control);

    /// From AVX512BW VL
    public static Vector128<byte> CompareGreaterThan(Vector128<byte> left, Vector128<byte> right);
    public static Vector128<byte> CompareGreaterThanOrEqual(Vector128<byte> left, Vector128<byte> right);
    public static Vector128<byte> CompareLessThan(Vector128<byte> left, Vector128<byte> right);
    public static Vector128<byte> CompareLessThanOrEqual(Vector128<byte> left, Vector128<byte> right);
    public static Vector128<byte> CompareNotEqual(Vector128<byte> left, Vector128<byte> right);
    public static Vector128<short> CompareGreaterThanOrEqual(Vector128<short> left, Vector128<short> right);
    public static Vector128<short> CompareLessThan(Vector128<short> left, Vector128<short> right);
    public static Vector128<short> CompareLessThanOrEqual(Vector128<short> left, Vector128<short> right);
    public static Vector128<short> CompareNotEqual(Vector128<short> left, Vector128<short> right);
    public static Vector128<sbyte> CompareGreaterThanOrEqual(Vector128<sbyte> left, Vector128<sbyte> right);
    public static Vector128<sbyte> CompareLessThan(Vector128<sbyte> left, Vector128<sbyte> right);
    public static Vector128<sbyte> CompareLessThanOrEqual(Vector128<sbyte> left, Vector128<sbyte> right);
    public static Vector128<sbyte> CompareNotEqual(Vector128<sbyte> left, Vector128<sbyte> right);
    public static Vector128<ushort> CompareGreaterThan(Vector128<ushort> left, Vector128<ushort> right);
    public static Vector128<ushort> CompareGreaterThanOrEqual(Vector128<ushort> left, Vector128<ushort> right);
    public static Vector128<ushort> CompareLessThan(Vector128<ushort> left, Vector128<ushort> right);
    public static Vector128<ushort> CompareLessThanOrEqual(Vector128<ushort> left, Vector128<ushort> right);
    public static Vector128<ushort> CompareNotEqual(Vector128<ushort> left, Vector128<ushort> right);
    public static Vector128<byte> ConvertToVector128Byte(Vector128<short> value);
    public static Vector128<byte> ConvertToVector128Byte(Vector128<ushort> value);
    public static Vector128<byte> ConvertToVector128ByteWithSaturation(Vector128<ushort> value);
    public static Vector128<sbyte> ConvertToVector128SByte(Vector128<short> value);
    public static Vector128<sbyte> ConvertToVector128SByte(Vector128<ushort> value);
    public static Vector128<sbyte> ConvertToVector128SByteWithSaturation(Vector128<short> value);
    public static Vector128<short> PermuteVar8x16(Vector128<short> left, Vector128<short> control);
    public static Vector128<ushort> PermuteVar8x16(Vector128<ushort> left, Vector128<ushort> control);
    public static Vector128<short> PermuteVar8x16x2(Vector128<short> lower, Vector128<short> indices, Vector128<short> upper);
    public static Vector128<ushort> PermuteVar8x16x2(Vector128<ushort> lower, Vector128<ushort> indices, Vector128<ushort> upper);
    public static Vector128<short> ShiftLeftLogicalVariable(Vector128<short> value, Vector128<ushort> count);
    public static Vector128<ushort> ShiftLeftLogicalVariable(Vector128<ushort> value, Vector128<ushort> count);
    public static Vector128<short> ShiftRightArithmeticVariable(Vector128<short> value, Vector128<ushort> count);
    public static Vector128<short> ShiftRightLogicalVariable(Vector128<short> value, Vector128<ushort> count);
    public static Vector128<ushort> ShiftRightLogicalVariable(Vector128<ushort> value, Vector128<ushort> count);
    public static Vector128<ushort> SumAbsoluteDifferencesInBlock32(Vector128<byte> left, Vector128<byte> right, [ConstantExpected] byte control);

    /// From AVX512CD VL
    public static Vector128<int> DetectConflicts(Vector128<int> value);
    public static Vector128<uint> DetectConflicts(Vector128<uint> value);
    public static Vector128<long> DetectConflicts(Vector128<long> value);
    public static Vector128<ulong> DetectConflicts(Vector128<ulong> value);
    public static Vector128<int> LeadingZeroCount(Vector128<int> value);
    public static Vector128<uint> LeadingZeroCount(Vector128<uint> value);
    public static Vector128<long> LeadingZeroCount(Vector128<long> value);
    public static Vector128<ulong> LeadingZeroCount(Vector128<ulong> value);

    /// From AVX512DQ VL
    public static Vector128<int> BroadcastPairScalarToVector128(Vector128<int> value);
    public static Vector128<uint> BroadcastPairScalarToVector128(Vector128<uint> value);
    public static Vector128<double> ConvertToVector128Double(Vector128<long> value);
    public static Vector128<double> ConvertToVector128Double(Vector128<ulong> value);
    public static Vector128<long> ConvertToVector128Int64(Vector128<float> value);
    public static Vector128<long> ConvertToVector128Int64(Vector128<double> value);
    public static Vector128<long> ConvertToVector128Int64WithTruncation(Vector128<float> value);
    public static Vector128<long> ConvertToVector128Int64WithTruncation(Vector128<double> value);
    public static Vector128<float> ConvertToVector128Single(Vector128<long> value);
    public static Vector128<float> ConvertToVector128Single(Vector128<ulong> value);
    public static Vector128<ulong> ConvertToVector128UInt64(Vector128<float> value);
    public static Vector128<ulong> ConvertToVector128UInt64(Vector128<double> value);
    public static Vector128<ulong> ConvertToVector128UInt64WithTruncation(Vector128<float> value);
    public static Vector128<ulong> ConvertToVector128UInt64WithTruncation(Vector128<double> value);
    public static Vector128<long> MultiplyLow(Vector128<long> left, Vector128<long> right);
    public static Vector128<ulong> MultiplyLow(Vector128<ulong> left, Vector128<ulong> right);
    public static Vector128<float> Range(Vector128<float> left, Vector128<float> right, [ConstantExpected(Max = (byte)(0x0F))] byte control);
    public static Vector128<double> Range(Vector128<double> left, Vector128<double> right, [ConstantExpected(Max = (byte)(0x0F))] byte control);
    public static Vector128<float> Reduce(Vector128<float> value, [ConstantExpected] byte control);
    public static Vector128<double> Reduce(Vector128<double> value, [ConstantExpected] byte control);

    /// From AVX512_Vbmi_VL
    public static Vector128<sbyte> PermuteVar16x8(Vector128<sbyte> left, Vector128<sbyte> control);
    public static Vector128<byte> PermuteVar16x8(Vector128<byte> left, Vector128<byte> control);
    public static Vector128<byte> PermuteVar16x8x2(Vector128<byte> lower, Vector128<byte> indices, Vector128<byte> upper);
    public static Vector128<sbyte> PermuteVar16x8x2(Vector128<sbyte> lower, Vector128<sbyte> indices, Vector128<sbyte> upper);
       
    public abstract class V256 : Avx2
    {
        public static new bool IsSupported { get; }
        
        /// From AVX512F VL
        public static Vector256<ulong> Abs(Vector256<long> value);
        public static Vector256<int> AlignRight32(Vector256<int> left, Vector256<int> right, [ConstantExpected] byte mask);
        public static Vector256<uint> AlignRight32(Vector256<uint> left, Vector256<uint> right, [ConstantExpected] byte mask);
        public static Vector256<long> AlignRight64(Vector256<long> left, Vector256<long> right, [ConstantExpected] byte mask);
        public static Vector256<ulong> AlignRight64(Vector256<ulong> left, Vector256<ulong> right, [ConstantExpected] byte mask);
        public static Vector256<int> CompareGreaterThanOrEqual(Vector256<int> left, Vector256<int> right);
        public static Vector256<int> CompareLessThan(Vector256<int> left, Vector256<int> right);
        public static Vector256<int> CompareLessThanOrEqual(Vector256<int> left, Vector256<int> right);
        public static Vector256<int> CompareNotEqual(Vector256<int> left, Vector256<int> right);
        public static Vector256<long> CompareGreaterThanOrEqual(Vector256<long> left, Vector256<long> right);
        public static Vector256<long> CompareLessThan(Vector256<long> left, Vector256<long> right);
        public static Vector256<long> CompareLessThanOrEqual(Vector256<long> left, Vector256<long> right);
        public static Vector256<long> CompareNotEqual(Vector256<long> left, Vector256<long> right);
        public static Vector256<uint> CompareGreaterThan(Vector256<uint> left, Vector256<uint> right);
        public static Vector256<uint> CompareGreaterThanOrEqual(Vector256<uint> left, Vector256<uint> right);
        public static Vector256<uint> CompareLessThan(Vector256<uint> left, Vector256<uint> right);
        public static Vector256<uint> CompareLessThanOrEqual(Vector256<uint> left, Vector256<uint> right);
        public static Vector256<uint> CompareNotEqual(Vector256<uint> left, Vector256<uint> right);
        public static Vector256<ulong> CompareGreaterThan(Vector256<ulong> left, Vector256<ulong> right);
        public static Vector256<ulong> CompareGreaterThanOrEqual(Vector256<ulong> left, Vector256<ulong> right);
        public static Vector256<ulong> CompareLessThan(Vector256<ulong> left, Vector256<ulong> right);
        public static Vector256<ulong> CompareLessThanOrEqual(Vector256<ulong> left, Vector256<ulong> right);
        public static Vector256<ulong> CompareNotEqual(Vector256<ulong> left, Vector256<ulong> right);
        public static Vector128<byte> ConvertToVector128Byte(Vector256<int> value);
        public static Vector128<byte> ConvertToVector128Byte(Vector256<long> value);
        public static Vector128<byte> ConvertToVector128Byte(Vector256<uint> value);
        public static Vector128<byte> ConvertToVector128Byte(Vector256<ulong> value);
        public static Vector128<byte> ConvertToVector128ByteWithSaturation(Vector256<uint> value);
        public static Vector128<byte> ConvertToVector128ByteWithSaturation(Vector256<ulong> value);
        public static Vector128<short> ConvertToVector128Int16(Vector256<int> value);
        public static Vector128<short> ConvertToVector128Int16(Vector256<long> value);
        public static Vector128<short> ConvertToVector128Int16(Vector256<uint> value);
        public static Vector128<short> ConvertToVector128Int16(Vector256<ulong> value);
        public static Vector128<short> ConvertToVector128Int16WithSaturation(Vector256<int> value);
        public static Vector128<short> ConvertToVector128Int16WithSaturation(Vector256<long> value);
        public static Vector128<int> ConvertToVector128Int32(Vector256<long> value);
        public static Vector128<int> ConvertToVector128Int32(Vector256<ulong> value);
        public static Vector128<int> ConvertToVector128Int32WithSaturation(Vector256<long> value);
        public static Vector128<sbyte> ConvertToVector128SByte(Vector256<int> value);
        public static Vector128<sbyte> ConvertToVector128SByte(Vector256<long> value);
        public static Vector128<sbyte> ConvertToVector128SByte(Vector256<uint> value);
        public static Vector128<sbyte> ConvertToVector128SByte(Vector256<ulong> value);
        public static Vector128<sbyte> ConvertToVector128SByteWithSaturation(Vector256<int> value);
        public static Vector128<sbyte> ConvertToVector128SByteWithSaturation(Vector256<long> value);
        public static Vector128<ushort> ConvertToVector128UInt16(Vector256<int> value);
        public static Vector128<ushort> ConvertToVector128UInt16(Vector256<long> value);
        public static Vector128<ushort> ConvertToVector128UInt16(Vector256<uint> value);
        public static Vector128<ushort> ConvertToVector128UInt16(Vector256<ulong> value);
        public static Vector128<ushort> ConvertToVector128UInt16WithSaturation(Vector256<uint> value);
        public static Vector128<ushort> ConvertToVector128UInt16WithSaturation(Vector256<ulong> value);
        public static Vector128<uint> ConvertToVector128UInt32(Vector256<long> value);
        public static Vector128<uint> ConvertToVector128UInt32(Vector256<ulong> value);
        public static Vector128<uint> ConvertToVector128UInt32(Vector256<double> value);
        public static Vector128<uint> ConvertToVector128UInt32WithSaturation(Vector256<ulong> value);
        public static Vector128<uint> ConvertToVector128UInt32WithTruncation(Vector256<double> value);
        public static Vector256<double> ConvertToVector256Double(Vector128<uint> value);
        public static Vector256<float> ConvertToVector256Single(Vector256<uint> value);
        public static Vector256<uint> ConvertToVector256UInt32(Vector256<float> value);
        public static Vector256<uint> ConvertToVector256UInt32WithTruncation(Vector256<float> value);
        public static Vector256<float> Fixup(Vector256<float> left, Vector256<float> right, Vector256<int> table, [ConstantExpected] byte control);
        public static Vector256<double> Fixup(Vector256<double> left, Vector256<double> right, Vector256<long> table, [ConstantExpected] byte control);
        public static Vector256<float> GetExponent(Vector256<float> value);
        public static Vector256<double> GetExponent(Vector256<double> value);
        public static Vector256<float> GetMantissa(Vector256<float> value, [ConstantExpected(Max = (byte)(0x0F))] byte control);
        public static Vector256<double> GetMantissa(Vector256<double> value, [ConstantExpected(Max = (byte)(0x0F))] byte control);
        public static Vector256<long> Max(Vector256<long> left, Vector256<long> right);
        public static Vector256<ulong> Max(Vector256<ulong> left, Vector256<ulong> right);
        public static Vector256<long> Min(Vector256<long> left, Vector256<long> right);
        public static Vector256<ulong> Min(Vector256<ulong> left, Vector256<ulong> right);
        public static Vector256<long> PermuteVar4x64(Vector256<long> value, Vector256<long> control);
        public static Vector256<ulong> PermuteVar4x64(Vector256<ulong> value, Vector256<ulong> control);
        public static Vector256<double> PermuteVar4x64(Vector256<double> value, Vector256<long> control);
        public static Vector256<long> PermuteVar4x64x2(Vector256<long> lower, Vector256<long> indices, Vector256<long> upper);
        public static Vector256<ulong> PermuteVar4x64x2(Vector256<ulong> lower, Vector256<ulong> indices, Vector256<ulong> upper);
        public static Vector256<double> PermuteVar4x64x2(Vector256<double> lower, Vector256<long> indices, Vector256<double> upper);
        public static Vector256<int> PermuteVar8x32x2(Vector256<int> lower, Vector256<int> indices, Vector256<int> upper);
        public static Vector256<uint> PermuteVar8x32x2(Vector256<uint> lower, Vector256<uint> indices, Vector256<uint> upper);
        public static Vector256<float> PermuteVar8x32x2(Vector256<float> lower, Vector256<int> indices, Vector256<float> upper);
        public static Vector256<float> Reciprocal14(Vector256<float> value);
        public static Vector256<double> Reciprocal14(Vector256<double> value);
        public static Vector256<float> ReciprocalSqrt14(Vector256<float> value);
        public static Vector256<double> ReciprocalSqrt14(Vector256<double> value);
        public static Vector256<int> RotateLeft(Vector256<int> value, [ConstantExpected] byte count);
        public static Vector256<uint> RotateLeft(Vector256<uint> value, [ConstantExpected] byte count);
        public static Vector256<long> RotateLeft(Vector256<long> value, [ConstantExpected] byte count);
        public static Vector256<ulong> RotateLeft(Vector256<ulong> value, [ConstantExpected] byte count);
        public static Vector256<int> RotateLeftVariable(Vector256<int> value, Vector256<uint> count);
        public static Vector256<uint> RotateLeftVariable(Vector256<uint> value, Vector256<uint> count);
        public static Vector256<long> RotateLeftVariable(Vector256<long> value, Vector256<ulong> count);
        public static Vector256<ulong> RotateLeftVariable(Vector256<ulong> value, Vector256<ulong> count);
        public static Vector256<int> RotateRight(Vector256<int> value, [ConstantExpected] byte count);
        public static Vector256<uint> RotateRight(Vector256<uint> value, [ConstantExpected] byte count);
        public static Vector256<long> RotateRight(Vector256<long> value, [ConstantExpected] byte count);
        public static Vector256<ulong> RotateRight(Vector256<ulong> value, [ConstantExpected] byte count);
        public static Vector256<int> RotateRightVariable(Vector256<int> value, Vector256<uint> count);
        public static Vector256<uint> RotateRightVariable(Vector256<uint> value, Vector256<uint> count);
        public static Vector256<long> RotateRightVariable(Vector256<long> value, Vector256<ulong> count);
        public static Vector256<ulong> RotateRightVariable(Vector256<ulong> value, Vector256<ulong> count);
        public static Vector256<float> RoundScale(Vector256<float> value, [ConstantExpected] byte control);
        public static Vector256<double> RoundScale(Vector256<double> value, [ConstantExpected] byte control);
        public static Vector256<float> Scale(Vector256<float> left, Vector256<float> right);
        public static Vector256<double> Scale(Vector256<double> left, Vector256<double> right);
        public static Vector256<long> ShiftRightArithmetic(Vector256<long> value, Vector128<long> count);
        public static Vector256<long> ShiftRightArithmetic(Vector256<long> value, [ConstantExpected] byte count);
        public static Vector256<long> ShiftRightArithmeticVariable(Vector256<long> value, Vector256<ulong> count);
        public static Vector256<double> Shuffle2x128(Vector256<double> left, Vector256<double> right, [ConstantExpected] byte control);
        public static Vector256<int> Shuffle2x128(Vector256<int> left, Vector256<int> right, [ConstantExpected] byte control);
        public static Vector256<long> Shuffle2x128(Vector256<long> left, Vector256<long> right, [ConstantExpected] byte control);
        public static Vector256<float> Shuffle2x128(Vector256<float> left, Vector256<float> right, [ConstantExpected] byte control);
        public static Vector256<uint> Shuffle2x128(Vector256<uint> left, Vector256<uint> right, [ConstantExpected] byte control);
        public static Vector256<ulong> Shuffle2x128(Vector256<ulong> left, Vector256<ulong> right, [ConstantExpected] byte control);
        public static Vector256<sbyte> TernaryLogic(Vector256<sbyte> a, Vector256<sbyte> b, Vector256<sbyte> c, [ConstantExpected] byte control);
        public static Vector256<byte> TernaryLogic(Vector256<byte> a, Vector256<byte> b, Vector256<byte> c, [ConstantExpected] byte control);
        public static Vector256<short> TernaryLogic(Vector256<short> a, Vector256<short> b, Vector256<short> c, [ConstantExpected] byte control);
        public static Vector256<ushort> TernaryLogic(Vector256<ushort> a, Vector256<ushort> b, Vector256<ushort> c, [ConstantExpected] byte control);
        public static Vector256<int> TernaryLogic(Vector256<int> a, Vector256<int> b, Vector256<int> c, [ConstantExpected] byte control);
        public static Vector256<uint> TernaryLogic(Vector256<uint> a, Vector256<uint> b, Vector256<uint> c, [ConstantExpected] byte control);
        public static Vector256<long> TernaryLogic(Vector256<long> a, Vector256<long> b, Vector256<long> c, [ConstantExpected] byte control);
        public static Vector256<ulong> TernaryLogic(Vector256<ulong> a, Vector256<ulong> b, Vector256<ulong> c, [ConstantExpected] byte control);
        public static Vector256<float> TernaryLogic(Vector256<float> a, Vector256<float> b, Vector256<float> c, [ConstantExpected] byte control);
        public static Vector256<double> TernaryLogic(Vector256<double> a, Vector256<double> b, Vector256<double> c, [ConstantExpected] byte control);

        /// From AVX512BW VL
        public static Vector256<byte> CompareGreaterThan(Vector256<byte> left, Vector256<byte> right);
        public static Vector256<byte> CompareGreaterThanOrEqual(Vector256<byte> left, Vector256<byte> right);
        public static Vector256<byte> CompareLessThan(Vector256<byte> left, Vector256<byte> right);
        public static Vector256<byte> CompareLessThanOrEqual(Vector256<byte> left, Vector256<byte> right);
        public static Vector256<byte> CompareNotEqual(Vector256<byte> left, Vector256<byte> right);
        public static Vector256<short> CompareGreaterThanOrEqual(Vector256<short> left, Vector256<short> right);
        public static Vector256<short> CompareLessThan(Vector256<short> left, Vector256<short> right);
        public static Vector256<short> CompareLessThanOrEqual(Vector256<short> left, Vector256<short> right);
        public static Vector256<short> CompareNotEqual(Vector256<short> left, Vector256<short> right);
        public static Vector256<sbyte> CompareGreaterThanOrEqual(Vector256<sbyte> left, Vector256<sbyte> right);
        public static Vector256<sbyte> CompareLessThan(Vector256<sbyte> left, Vector256<sbyte> right);
        public static Vector256<sbyte> CompareLessThanOrEqual(Vector256<sbyte> left, Vector256<sbyte> right);
        public static Vector256<sbyte> CompareNotEqual(Vector256<sbyte> left, Vector256<sbyte> right);
        public static Vector256<ushort> CompareGreaterThan(Vector256<ushort> left, Vector256<ushort> right);
        public static Vector256<ushort> CompareGreaterThanOrEqual(Vector256<ushort> left, Vector256<ushort> right);
        public static Vector256<ushort> CompareLessThan(Vector256<ushort> left, Vector256<ushort> right);
        public static Vector256<ushort> CompareLessThanOrEqual(Vector256<ushort> left, Vector256<ushort> right);
        public static Vector256<ushort> CompareNotEqual(Vector256<ushort> left, Vector256<ushort> right);
        public static Vector128<byte> ConvertToVector128Byte(Vector256<short> value);
        public static Vector128<byte> ConvertToVector128Byte(Vector256<ushort> value);
        public static Vector128<byte> ConvertToVector128ByteWithSaturation(Vector256<ushort> value);
        public static Vector128<sbyte> ConvertToVector128SByte(Vector256<short> value);
        public static Vector128<sbyte> ConvertToVector128SByte(Vector256<ushort> value);
        public static Vector128<sbyte> ConvertToVector128SByteWithSaturation(Vector256<short> value);
        public static Vector256<short> PermuteVar16x16(Vector256<short> left, Vector256<short> control);
        public static Vector256<ushort> PermuteVar16x16(Vector256<ushort> left, Vector256<ushort> control);
        public static Vector256<short> PermuteVar16x16x2(Vector256<short> lower, Vector256<short> indices, Vector256<short> upper);
        public static Vector256<ushort> PermuteVar16x16x2(Vector256<ushort> lower, Vector256<ushort> indices, Vector256<ushort> upper);
        public static Vector256<short> ShiftLeftLogicalVariable(Vector256<short> value, Vector256<ushort> count);
        public static Vector256<ushort> ShiftLeftLogicalVariable(Vector256<ushort> value, Vector256<ushort> count);
        public static Vector256<short> ShiftRightArithmeticVariable(Vector256<short> value, Vector256<ushort> count);
        public static Vector256<short> ShiftRightLogicalVariable(Vector256<short> value, Vector256<ushort> count);
        public static Vector256<ushort> ShiftRightLogicalVariable(Vector256<ushort> value, Vector256<ushort> count);
        public static Vector256<ushort> SumAbsoluteDifferencesInBlock32(Vector256<byte> left, Vector256<byte> right, [ConstantExpected] byte control);

        /// FROM AVX512CD VL
        public static Vector256<int> DetectConflicts(Vector256<int> value);
        public static Vector256<uint> DetectConflicts(Vector256<uint> value);
        public static Vector256<long> DetectConflicts(Vector256<long> value);
        public static Vector256<ulong> DetectConflicts(Vector256<ulong> value);
        public static Vector256<int> LeadingZeroCount(Vector256<int> value);
        public static Vector256<uint> LeadingZeroCount(Vector256<uint> value);
        public static Vector256<long> LeadingZeroCount(Vector256<long> value);
        public static Vector256<ulong> LeadingZeroCount(Vector256<ulong> value);
        
        /// From AVX512DQ VL
        public static Vector256<int> BroadcastPairScalarToVector256(Vector128<int> value);
        public static Vector256<uint> BroadcastPairScalarToVector256(Vector128<uint> value);
        public static Vector256<float> BroadcastPairScalarToVector256(Vector128<float> value);
        public static Vector128<float> ConvertToVector128Single(Vector256<long> value);
        public static Vector128<float> ConvertToVector128Single(Vector256<ulong> value);
        public static Vector256<double> ConvertToVector256Double(Vector256<long> value);
        public static Vector256<double> ConvertToVector256Double(Vector256<ulong> value);
        public static Vector256<long> ConvertToVector256Int64(Vector128<float> value);
        public static Vector256<long> ConvertToVector256Int64(Vector256<double> value);
        public static Vector256<long> ConvertToVector256Int64WithTruncation(Vector128<float> value);
        public static Vector256<long> ConvertToVector256Int64WithTruncation(Vector256<double> value);
        public static Vector256<ulong> ConvertToVector256UInt64(Vector128<float> value);
        public static Vector256<ulong> ConvertToVector256UInt64(Vector256<double> value);
        public static Vector256<ulong> ConvertToVector256UInt64WithTruncation(Vector128<float> value);
        public static Vector256<ulong> ConvertToVector256UInt64WithTruncation(Vector256<double> value);
        public static Vector256<long> MultiplyLow(Vector256<long> left, Vector256<long> right);
        public static Vector256<ulong> MultiplyLow(Vector256<ulong> left, Vector256<ulong> right);
        public static Vector256<float> Range(Vector256<float> left, Vector256<float> right, [ConstantExpected(Max = (byte)(0x0F))] byte control);
        public static Vector256<double> Range(Vector256<double> left, Vector256<double> right, [ConstantExpected(Max = (byte)(0x0F))] byte control);
        public static Vector256<float> Reduce(Vector256<float> value, [ConstantExpected] byte control);
        public static Vector256<double> Reduce(Vector256<double> value, [ConstantExpected] byte control);

        /// From AVX512_Vbmi_VL
        public static Vector256<sbyte> PermuteVar32x8(Vector256<sbyte> left, Vector256<sbyte> control);
        public static Vector256<byte> PermuteVar32x8(Vector256<byte> left, Vector256<byte> control);
        public static Vector256<byte> PermuteVar32x8x2(Vector256<byte> lower, Vector256<byte> indices, Vector256<byte> upper);
        public static Vector256<sbyte> PermuteVar32x8x2(Vector256<sbyte> lower, Vector256<sbyte> indices, Vector256<sbyte> upper);
    }

    public abstract class V512 : Avx512BW
    {
        public static new bool IsSupported { get; }
        
        /// From AVX512CD
        public static Vector512<int> DetectConflicts(Vector512<int> value);
        public static Vector512<uint> DetectConflicts(Vector512<uint> value);
        public static Vector512<long> DetectConflicts(Vector512<long> value);
        public static Vector512<ulong> DetectConflicts(Vector512<ulong> value);
        public static Vector512<int> LeadingZeroCount(Vector512<int> value);
        public static Vector512<uint> LeadingZeroCount(Vector512<uint> value);
        public static Vector512<long> LeadingZeroCount(Vector512<long> value);
        public static Vector512<ulong> LeadingZeroCount(Vector512<ulong> value);

        /// From AVX512DQ
        public static Vector512<float> And(Vector512<float> left, Vector512<float> right);
        public static Vector512<double> And(Vector512<double> left, Vector512<double> right);
        public static Vector512<float> AndNot(Vector512<float> left, Vector512<float> right);
        public static Vector512<double> AndNot(Vector512<double> left, Vector512<double> right);
        public static Vector512<int> BroadcastPairScalarToVector512(Vector128<int> value);
        public static Vector512<uint> BroadcastPairScalarToVector512(Vector128<uint> value);
        public static Vector512<float> BroadcastPairScalarToVector512(Vector128<float> value);
        public static unsafe Vector512<long> BroadcastVector128ToVector512(long* address);
        public static unsafe Vector512<ulong> BroadcastVector128ToVector512(ulong* address);
        public static unsafe Vector512<double> BroadcastVector128ToVector512(double* address);
        public static unsafe Vector512<int> BroadcastVector256ToVector512(int* address);
        public static unsafe Vector512<uint> BroadcastVector256ToVector512(uint* address);
        public static unsafe Vector512<float> BroadcastVector256ToVector512(float* address);
        public static Vector256<float> ConvertToVector256Single(Vector512<long> value);
        public static Vector256<float> ConvertToVector256Single(Vector512<ulong> value);
        public static Vector512<double> ConvertToVector512Double(Vector512<long> value);
        public static Vector512<double> ConvertToVector512Double(Vector512<ulong> value);
        public static Vector512<long> ConvertToVector512Int64(Vector256<float> value);
        public static Vector512<long> ConvertToVector512Int64(Vector512<double> value);
        public static Vector512<long> ConvertToVector512Int64WithTruncation(Vector256<float> value);
        public static Vector512<long> ConvertToVector512Int64WithTruncation(Vector512<double> value);
        public static Vector512<ulong> ConvertToVector512UInt64(Vector256<float> value);
        public static Vector512<ulong> ConvertToVector512UInt64(Vector512<double> value);
        public static Vector512<ulong> ConvertToVector512UInt64WithTruncation(Vector256<float> value);
        public static Vector512<ulong> ConvertToVector512UInt64WithTruncation(Vector512<double> value);
        public static new Vector128<long> ExtractVector128(Vector512<long> value, [ConstantExpected] byte index);
        public static new Vector128<ulong> ExtractVector128(Vector512<ulong> value, [ConstantExpected] byte index);
        public static new Vector128<double> ExtractVector128(Vector512<double> value, [ConstantExpected] byte index);
        public static new Vector256<int> ExtractVector256(Vector512<int> value, [ConstantExpected] byte index);
        public static new Vector256<uint> ExtractVector256(Vector512<uint> value, [ConstantExpected] byte index);
        public static new Vector256<float> ExtractVector256(Vector512<float> value, [ConstantExpected] byte index);
        public static new Vector512<long> InsertVector128(Vector512<long> value, Vector128<long> data, [ConstantExpected] byte index);
        public static new Vector512<ulong> InsertVector128(Vector512<ulong> value, Vector128<ulong> data, [ConstantExpected] byte index);
        public static new Vector512<double> InsertVector128(Vector512<double> value, Vector128<double> data, [ConstantExpected] byte index);
        public static new Vector512<int> InsertVector256(Vector512<int> value, Vector256<int> data, [ConstantExpected] byte index);
        public static new Vector512<uint> InsertVector256(Vector512<uint> value, Vector256<uint> data, [ConstantExpected] byte index);
        public static new Vector512<float> InsertVector256(Vector512<float> value, Vector256<float> data, [ConstantExpected] byte index);
        public static Vector512<long> MultiplyLow(Vector512<long> left, Vector512<long> right);
        public static Vector512<ulong> MultiplyLow(Vector512<ulong> left, Vector512<ulong> right);
        public static Vector512<float> Or(Vector512<float> left, Vector512<float> right);
        public static Vector512<double> Or(Vector512<double> left, Vector512<double> right);
        public static Vector512<float> Range(Vector512<float> left, Vector512<float> right, [ConstantExpected(Max = (byte)(0x0F))] byte control);
        public static Vector512<double> Range(Vector512<double> left, Vector512<double> right, [ConstantExpected(Max = (byte)(0x0F))] byte control);
        public static Vector512<float> Reduce(Vector512<float> value, [ConstantExpected] byte control);
        public static Vector512<double> Reduce(Vector512<double> value, [ConstantExpected] byte control);
        public static Vector512<float> Xor(Vector512<float> left, Vector512<float> right);
        public static Vector512<double> Xor(Vector512<double> left, Vector512<double> right);

        /// From AVX512Vbmi
        public static Vector512<sbyte> PermuteVar64x8(Vector512<sbyte> left, Vector512<sbyte> control);
        public static Vector512<byte> PermuteVar64x8(Vector512<byte> left, Vector512<byte> control);
        public static Vector512<byte> PermuteVar64x8x2(Vector512<byte> lower, Vector512<byte> indices, Vector512<byte> upper);
        public static Vector512<sbyte> PermuteVar64x8x2(Vector512<sbyte> lower, Vector512<sbyte> indices, Vector512<sbyte> upper);   
    }
}
```
## : AVX-IFMA Intrinsics

**Approved** | [#runtime/98833](https://github.com/dotnet/runtime/issues/98833#issuecomment-1971776760) | [Video](https://www.youtube.com/watch?v=o1GpyXImDjc&t=0h51m11s)

* We should rename the parameters to `addend`, `left`, and `right`

```C#
namespace System.Runtime.Intrinsics.X86
{
    public abstract class AvxIfma : Avx2
    {
        public static new bool IsSupported { get; }

        public static Vector128<ulong> MultiplyAdd52Low(Vector128<ulong> addend, Vector128<ulong> left, Vector128<ulong> right);
        public static Vector128<ulong> MultiplyAdd52High(Vector128<ulong> addend, Vector128<ulong> left, Vector128<ulong> right);
        public static Vector256<ulong> MultiplyAdd52Low(Vector256<ulong> addend, Vector256<ulong> left, Vector256<ulong> right);
        public static Vector256<ulong> MultiplyAdd52High(Vector256<ulong> addend, Vector256<ulong> left, Vector256<ulong> right);

        public new abstract class X64 : Avx2.X64
        {
            public static new bool IsSupported { get; }
        }
    }
}
```
## AVX-512 IFMA Intrinsics

**Approved** | [#runtime/96476](https://github.com/dotnet/runtime/issues/96476#issuecomment-1971778627) | [Video](https://www.youtube.com/watch?v=o1GpyXImDjc&t=0h55m8s)

* We should rename the parameters to `addend`, `left`, and `right`

```C#
namespace System.Runtime.Intrinsics.X86
{
    public abstract class Avx512Ifma : Avx512F
    {
        public static bool IsSupported { get; }

        public static Vector512<ulong> MultiplyAdd52Low(Vector512<ulong> addend, Vector512<ulong> left, Vector512<ulong> right);
        public static Vector512<ulong> MultiplyAdd52High(Vector512<ulong> addend, Vector512<ulong> left, Vector512<ulong> right);

        public abstract class VL : Avx512F.VL
        {
            public static new bool IsSupported { get; }
            public static Vector256<ulong> MultiplyAdd52Low(Vector256<ulong> addend, Vector256<ulong> left, Vector256<ulong> right);
            public static Vector256<ulong> MultiplyAdd52High(Vector256<ulong> addend, Vector256<ulong> left, Vector256<ulong> right);
            public static Vector128<ulong> MultiplyAdd52Low(Vector128<ulong> addend, Vector128<ulong> left, Vector128<ulong> right);
            public static Vector128<ulong> MultiplyAdd52High(Vector128<ulong> addend, Vector128<ulong> left, Vector128<ulong> right);
        }
    }
}
```
## Expose `AVX512 FP16` and `AVX` `F16C` ISA

**Approved** | [#runtime/98820](https://github.com/dotnet/runtime/issues/98820#issuecomment-1971798608) | [Video](https://www.youtube.com/watch?v=o1GpyXImDjc&t=0h56m22s)

* Looks good as proposed
* Renamed the `right` parameter in conjugate methods to `rightConjugate`

```C#
namespace System.Runtime.Intrinsics.X86;

public sealed class F16c : Avx2
{
    public static bool IsSupported { get; }

    public static Vector128<float> ConvertToVector128Single(Vector128<Half> value);
    public static Vector256<float> ConvertToVector256Single(Vector128<Half> value);

    public static Vector128<Half> ConvertToVector128Half(Vector128<float> value, byte control);
    public static Vector128<Half> ConvertToVector128Half(Vector256<float> value, byte control);
}

public sealed class Avx512Fp16 : Avx512F
{
    public static bool IsSupported { get; }

    public static Vector512<Half> Add(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> Add(Vector512<Half> left, Vector512<Half> right, FloatRoundingMode mode);
    public static Vector512<Half> Divide(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> Divide(Vector512<Half> left, Vector512<Half> right, FloatRoundingMode mode);
    public static Vector512<Half> GetExponent(Vector512<Half> value);
    public static Vector512<Half> GetMantissa(Vector512<Half> value, byte control);
    public static Vector512<Half> Max(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> Min(Vector512<Half> value, Vector512<Half> right);
    public static Vector512<Half> Multiply(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> Multiply(Vector512<Half> left, Vector512<Half> right, FloatRoundingMode mode);
    public static Vector512<Half> Reciprocal(Vector512<Half> value);
    public static Half Reduce(Vector512<Half> left, byte control);
    public static Vector512<Half> RoundScale(Vector512<Half> left, byte control);
    public static Vector512<Half> ReciprocalSqrt(Vector512<Half> value);
    public static Vector512<Half> ReciprocalSqrt(Vector512<Half> value, FloatRoundingMode mode);
    public static Vector512<Half> Scale(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> Scale(Vector512<Half> left, Vector512<Half> right, FloatRoundingMode mode);
    public static Vector512<Half> Sqrt(Vector512<Half> value);
    public static Vector512<Half> Sqrt(Vector512<Half> value, FloatRoundingMode mode);
    public static Vector512<Half> Subtract(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> Subtract(Vector512<Half> left, Vector512<Half> right, FloatRoundingMode mode);
    public static Vector512<Half> FusedComplexMultiplyAdd(Vector512<Half> addend, Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> FusedComplexMultiplyAddConjugate(Vector512<Half> addend, Vector512<Half> left, Vector512<Half> rightConjugate);
    public static Vector512<Half> FusedComplexMultiplyAddConjugate(Vector512<Half> addend, Vector512<Half> left, Vector512<Half> rightConjugate, FloatRoundingMode mode);
    public static Vector512<Half> ComplexMultiply(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> ComplexMultiply(Vector512<Half> left, Vector512<Half> right, FloatRoundingMode mode);
    public static Vector512<Half> ComplexMultiplyConjugate(Vector512<Half> left, Vector512<Half> rightConjugate);
    public static Vector512<Half> ComplexMultiplyConjugate(Vector512<Half> left, Vector512<Half> rightConjugate, FloatRoundingMode mode);
    public static Vector512<Half> FusedMultiplyAddSubtract(Vector512<Half> a, Vector512<Half> b, Vector512<Half> c);
    public static Vector512<Half> FusedMultiplyAddSubtract(Vector512<Half> a, Vector512<Half> b, Vector512<Half> c, FloatRoundingMode mode);
    public static Vector512<Half> FusedMultiplySubtractAdd(Vector512<Half> a, Vector512<Half> b, Vector512<Half> c);
    public static Vector512<Half> FusedMultiplySubtractAdd(Vector512<Half> a, Vector512<Half> b, Vector512<Half> c, FloatRoundingMode mode);
    public static Vector512<Half> FusedMultiplyAdd(Vector512<Half> a, Vector512<Half> b, Vector512<Half> c);
    public static Vector512<Half> FusedMultiplyAdd(Vector512<Half> a, Vector512<Half> b, Vector512<Half> c, FloatRoundingMode mode);
    public static Vector512<Half> FusedMultiplySubtract(Vector512<Half> a, Vector512<Half> b, Vector512<Half> c);
    public static Vector512<Half> FusedMultiplySubtract(Vector512<Half> a, Vector512<Half> b, Vector512<Half> c, FloatRoundingMode mode);
    public static Vector512<Half> FusedMultiplyAddNegated(Vector512<Half> a, Vector512<Half> b, Vector512<Half> c);
    public static Vector512<Half> FusedMultiplyAddNegated(Vector512<Half> a, Vector512<Half> b, Vector512<Half> c, FloatRoundingMode mode);
    public static Vector512<Half> FusedMultiplySubtractNegated(Vector512<Half> a, Vector512<Half> b, Vector512<Half> c);
    public static Vector512<Half> FusedMultiplySubtractNegated(Vector512<Half> a, Vector512<Half> b, Vector512<Half> c, FloatRoundingMode mode);
    public static Vector512<Half> Compare(Vector512<Half> left, Vector512<Half> right, FloatComparisonMode mode);
    public static Vector512<Half> CompareEqual(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> CompareGreaterThan(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> CompareGreaterThanOrEqual(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> CompareLessThan(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> CompareLessThanOrEqual(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> CompareNotEqual(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> CompareNotGreaterThan(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> CompareNotGreaterThanOrEqual(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> CompareNotLessThan(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> CompareNotLessThanOrEqual(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> CompareOrdered(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> CompareUnordered(Vector512<Half> left, Vector512<Half> right);
    public static Vector512<Half> Classify(Vector512<Half> value, byte control);
    public static Vector512<Half> ConvertToVector512Half(Vector512<short> value);
    public static Vector512<Half> ConvertToVector512Half(Vector512<short> value, FloatRoundingMode mode);
    public static Vector256<Half> ConvertToVector256Half(Vector512<int> value);
    public static Vector256<Half> ConvertToVector256Half(Vector512<int> value, FloatRoundingMode mode);
    public static Vector128<Half> ConvertToVector128Half(Vector512<long> value);
    public static Vector128<Half> ConvertToVector128Half(Vector512<long> value, FloatRoundingMode mode);
    public static Vector512<Half> ConvertToVector512Half(Vector512<ushort> value);
    public static Vector512<Half> ConvertToVector512Half(Vector512<ushort> value, FloatRoundingMode mode);
    public static Vector256<Half> ConvertToVector256Half(Vector512<uint> value);
    public static Vector256<Half> ConvertToVector256Half(Vector512<uint> value, FloatRoundingMode mode);
    public static Vector128<Half> ConvertToVector128Half(Vector512<ulong> value);
    public static Vector128<Half> ConvertToVector128Half(Vector512<ulong> value, FloatRoundingMode mode);
    public static Vector256<Half> ConvertToVector256Half(Vector512<float> value);
    public static Vector256<Half> ConvertToVector256Half(Vector512<float> value, FloatRoundingMode mode);
    public static Vector128<Half> ConvertToVector128Half(Vector512<double> value);
    public static Vector128<Half> ConvertToVector128Half(Vector512<double> value, FloatRoundingMode mode);
    public static Vector512<short> ConvertToVector512Int16(Vector512<Half> value);
    public static Vector512<short> ConvertToVector512Int16(Vector512<Half> value, FloatRoundingMode mode);
    public static Vector512<short> ConvertToVector512Int16WithTruncation(Vector512<Half> value);
    public static Vector512<int> ConvertToVector512Int32(Vector256<Half> value);
    public static Vector512<int> ConvertToVector512Int32(Vector256<Half> value, FloatRoundingMode mode);
    public static Vector512<int> ConvertToVector512Int32WithTruncation(Vector256<Half> value);
    public static Vector512<long> ConvertToVector512Int64(Vector128<Half> value);
    public static Vector512<long> ConvertToVector512Int64(Vector128<Half> value, FloatRoundingMode mode);
    public static Vector512<long> ConvertToVector512Int64WithTruncation(Vector128<Half> value);
    public static Vector512<ushort> ConvertToVector512UInt16(Vector512<Half> value);
    public static Vector512<ushort> ConvertToVector512UInt16(Vector512<Half> value, FloatRoundingMode mode);
    public static Vector512<ushort> ConvertToVector512UInt16WithTruncation(Vector512<Half> value);
    public static Vector512<uint> ConvertToVector512UInt32(Vector256<Half> value);
    public static Vector512<uint> ConvertToVector512UInt32(Vector256<Half> value, FloatRoundingMode mode);
    public static Vector512<uint> ConvertToVector512UInt32WithTruncation(Vector256<Half> value);
    public static Vector512<ulong> ConvertToVector512UInt64(Vector128<Half> value);
    public static Vector512<ulong> ConvertToVector512UInt64(Vector128<Half> value, FloatRoundingMode mode);
    public static Vector512<ulong> ConvertToVector512UInt64WithTruncation(Vector128<Half> value);
    public static Vector512<float> ConvertToVector512Single(Vector256<Half> value);
    public static Vector512<double> ConvertToVector512Double(Vector128<Half> value);
    public static Vector512<double> ConvertToVector512Double(Vector128<Half> value, FloatRoundingMode mode);
    public static Vector128<Half> AddScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> AddScalar(Vector128<Half> left, Vector128<Half> right, FloatRoundingMode mode);
    public static Vector128<Half> DivideScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> DivideScalar(Vector128<Half> left, Vector128<Half> right, FloatRoundingMode mode);
    public static Vector128<Half> GetExponentScalar(Vector128<Half> value);
    public static Vector128<Half> GetMantissaScalar(Vector128<Half> value, byte control);
    public static Vector128<Half> MaxScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> MinScalar(Vector128<Half> value, Vector128<Half> right);
    public static Vector128<Half> MultiplyScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> MultiplyScalar(Vector128<Half> left, Vector128<Half> right, FloatRoundingMode mode);
    public static Vector128<Half> ReciprocalScalar(Vector128<Half> value);
    public static Half ReduceScalar(Vector128<Half> left, byte control);
    public static Vector128<Half> RoundScaleScalar(Vector128<Half> left, byte control);
    public static Vector128<Half> ReciprocalSqrtScalar(Vector128<Half> value);
    public static Vector128<Half> ScaleScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> ScaleScalar(Vector128<Half> left, Vector128<Half> right, FloatRoundingMode mode);
    public static Vector128<Half> SqrtScalar(Vector128<Half> value);
    public static Vector128<Half> SqrtScalar(Vector128<Half> value, FloatRoundingMode mode);
    public static Vector128<Half> SubtractScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> SubtractScalar(Vector128<Half> left, Vector128<Half> right, FloatRoundingMode mode);
    public static Vector128<Half> FusedComplexMultiplyAddScalar(Vector128<Half> addend, Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> FusedComplexMultiplyAddScalar(Vector128<Half> addend, Vector128<Half> left, Vector128<Half> right, FloatRoundingMode mode);
    public static Vector128<Half> FusedComplexMultiplyAddConjugateScalar(Vector128<Half> addend, Vector128<Half> left, Vector128<Half> rightConjugate);
    public static Vector128<Half> FusedComplexMultiplyAddConjugateScalar(Vector128<Half> addend, Vector128<Half> left, Vector128<Half> rightConjugate, FloatRoundingMode mode);
    public static Vector128<Half> ComplexMultiplyScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> ComplexMultiplyScalar(Vector128<Half> left, Vector128<Half> right, FloatRoundingMode mode);
    public static Vector128<Half> ComplexMultiplyConjugateScalar(Vector128<Half> left, Vector128<Half> rightConjugate);
    public static Vector128<Half> ComplexMultiplyConjugateScalar(Vector128<Half> left, Vector128<Half> rightConjugate, FloatRoundingMode mode);
    public static Vector128<Half> FusedMultiplyAddSubtractScalar(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c);
    public static Vector128<Half> FusedMultiplyAddSubtractScalar(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c, FloatRoundingMode mode);
    public static Vector128<Half> FusedMultiplySubtractAddScalar(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c);
    public static Vector128<Half> FusedMultiplySubtractAddScalar(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c, FloatRoundingMode mode);
    public static Vector128<Half> FusedMultiplyAddScalar(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c);
    public static Vector128<Half> FusedMultiplyAddScalar(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c, FloatRoundingMode mode);
    public static Vector128<Half> FusedMultiplySubtractScalar(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c);
    public static Vector128<Half> FusedMultiplySubtractScalar(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c, FloatRoundingMode mode);
    public static Vector128<Half> FusedMultiplyAddNegatedScalar(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c);
    public static Vector128<Half> FusedMultiplyAddNegatedScalar(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c, FloatRoundingMode mode);
    public static Vector128<Half> FusedMultiplySubtractNegatedScalar(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c);
    public static Vector128<Half> FusedMultiplySubtractNegatedScalar(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c, FloatRoundingMode mode);
    public static Vector128<Half> CompareScalar(Vector128<Half> left, Vector128<Half> right, FloatComparisonMode mode);
    public static Vector128<Half> CompareEqualScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> CompareGreaterThanScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> CompareGreaterThanOrEqualScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> CompareLessThanScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> CompareLessThanOrEqualScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> CompareNotEqualScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> CompareNotGreaterThanScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> CompareNotGreaterThanOrEqualScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> CompareNotLessThanScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> CompareNotLessThanOrEqualScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> CompareOrderedScalar(Vector128<Half> left, Vector128<Half> right);
    public static Vector128<Half> CompareUnorderedScalar(Vector128<Half> left, Vector128<Half> right);
    public static bool CompareScalarOrderedEqual(Vector128<Half> left, Vector128<Half> right);
    public static bool CompareScalarOrderedGreaterThan(Vector128<Half> left, Vector128<Half> right);
    public static bool CompareScalarOrderedGreaterThanOrEqual(Vector128<Half> left, Vector128<Half> right);
    public static bool CompareScalarOrderedLessThan(Vector128<Half> left, Vector128<Half> right);
    public static bool CompareScalarOrderedLessThanOrEqual(Vector128<Half> left, Vector128<Half> right);
    public static bool CompareScalarOrderedNotEqual(Vector128<Half> left, Vector128<Half> right);
    public static bool CompareScalarUnorderedEqual(Vector128<float> left, Vector128<float> right);
    public static bool CompareScalarUnorderedGreaterThan(Vector128<float> left, Vector128<float> right);
    public static bool CompareScalarUnorderedGreaterThanOrEqual(Vector128<float> left, Vector128<float> right);
    public static bool CompareScalarUnorderedLessThan(Vector128<float> left, Vector128<float> right);
    public static bool CompareScalarUnorderedLessThanOrEqual(Vector128<float> left, Vector128<float> right);
    public static bool CompareScalarUnorderedNotEqual(Vector128<float> left, Vector128<float> right);
    public static Vector128<Half> ClassifyScalar(Vector128<Half> value, byte control);
    public static Vector128<Half> ConvertScalarToVector128Half(Vector128<Half> upper, int value);
    public static Vector128<Half> ConvertScalarToVector128Half(Vector128<Half> upper, int value, FloatRoundingMode mode);
    public static Vector128<Half> ConvertScalarToVector128Half(Vector128<Half> upper, uint value);
    public static Vector128<Half> ConvertScalarToVector128Half(Vector128<Half> upper, uint value, FloatRoundingMode mode);
    public static Vector128<Half> ConvertScalarToVector128Half(Vector128<Half> upper, Vector128<float> value);
    public static Vector128<Half> ConvertScalarToVector128Half(Vector128<Half> upper, Vector128<float> value, FloatRoundingMode mode);
    public static Vector128<Half> ConvertScalarToVector128Half(Vector128<Half> upper, Vector128<double> value);
    public static Vector128<Half> ConvertScalarToVector128Half(Vector128<Half> upper, Vector128<double> value, FloatRoundingMode mode);
    public static int ConvertToInt32(Vector128<Half> value);
    public static int ConvertToInt32(Vector128<Half> value, FloatRoundingMode mode);
    public static int ConvertToInt32WithTruncation(Vector128<Half> value);
    public static uint ConvertToUInt32(Vector128<Half> value);
    public static uint ConvertToUInt32(Vector128<Half> value, FloatRoundingMode mode);
    public static uint ConvertToUInt32WithTruncation(Vector128<Half> value);
    public static Vector128<float> ConvertScalarToVector128Single(Vector128<float> upper, Vector128<Half> value);
    public static Vector128<float> ConvertScalarToVector128Single(Vector128<float> upper, Vector128<Half> value, FloatRoundingMode mode);
    public static Vector128<double> ConvertScalarToVector128Double(Vector128<double> upper, Vector128<Half> value);
    public static Vector128<double> ConvertScalarToVector128Double(Vector128<double> upper, Vector128<Half> value, FloatRoundingMode mode);

    public sealed class X64 : Avx512F.X64
    {
        public static bool IsSupported { get; }

        public static Vector128<Half> ConvertScalarToVector128Half(Vector128<Half> upper, long value);
        public static Vector128<Half> ConvertScalarToVector128Half(Vector128<Half> upper, ulong value);
        public static long ConvertToInt64(Vector128<Half> value);
        public static long ConvertToInt64WithTruncation(Vector128<Half> value);
        public static ulong ConvertToUInt64(Vector128<Half> value);
        public static ulong ConvertToUInt64WithTruncation(Vector128<Half> value);
    }

    public sealed class VL : Avx512F.VL
    {
        public static bool IsSupported { get; }

        public static Vector128<Half> Add(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> Divide(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> GetExponent(Vector128<Half> value);
        public static Vector128<Half> GetMantissa(Vector128<Half> value, byte control);
        public static Vector128<Half> Max(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> Min(Vector128<Half> value, Vector128<Half> right);
        public static Vector128<Half> Multiply(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> Reciprocal(Vector128<Half> value);
        public static Half Reduce(Vector128<Half> left, byte control);
        public static Vector128<Half> RoundScale(Vector128<Half> left, byte control);
        public static Vector128<Half> ReciprocalSqrt(Vector128<Half> value);
        public static Vector128<Half> Scale(Vector128<Half> left, Vector128<Half> right, byte control);
        public static Vector128<Half> Sqrt(Vector128<Half> value);
        public static Vector128<Half> Subtract(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> FusedComplexMultiplyAdd(Vector128<Half> addend, Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> FusedComplexMultiplyAddConjugate(Vector128<Half> addend, Vector128<Half> left, Vector128<Half> rightConjugate);
        public static Vector128<Half> ComplexMultiply(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> ComplexMultiplyConjugate(Vector128<Half> left, Vector128<Half> rightConjugate);
        public static Vector128<Half> FusedMultiplyAddSubtract(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c);
        public static Vector128<Half> FusedMultiplySubtractAdd(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c);
        public static Vector128<Half> FusedMultiplyAdd(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c);
        public static Vector128<Half> FusedMultiplySubtract(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c);
        public static Vector128<Half> FusedMultiplyAddNegated(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c);
        public static Vector128<Half> FusedMultiplySubtractNegated(Vector128<Half> a, Vector128<Half> b, Vector128<Half> c);
        public static Vector128<Half> Compare(Vector128<Half> left, Vector128<Half> right, FloatComparisonMode mode);
        public static Vector128<Half> CompareEqual(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> CompareGreaterThan(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> CompareGreaterThanOrEqual(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> CompareLessThan(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> CompareLessThanOrEqual(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> CompareNotEqual(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> CompareNotGreaterThan(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> CompareNotGreaterThanOrEqual(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> CompareNotLessThan(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> CompareNotLessThanOrEqual(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> CompareOrdered(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> CompareUnordered(Vector128<Half> left, Vector128<Half> right);
        public static Vector128<Half> Classify(Vector128<Half> value, byte control);
        public static Vector128<Half> ConvertToVector128Half(Vector128<short> value);
        public static Vector128<Half> ConvertToVector128Half(Vector128<int> value);
        public static Vector128<Half> ConvertToVector128Half(Vector128<long> value);
        public static Vector128<Half> ConvertToVector128Half(Vector128<ushort> value);
        public static Vector128<Half> ConvertToVector128Half(Vector128<uint> value);
        public static Vector128<Half> ConvertToVector128Half(Vector128<ulong> value);
        public static Vector128<Half> ConvertToVector128Half(Vector128<float> value);
        public static Vector128<Half> ConvertToVector128Half(Vector128<double> value);
        public static Vector128<short> ConvertToVector128Int16(Vector128<Half> value);
        public static Vector128<short> ConvertToVector128Int16WithTruncation(Vector128<Half> value);
        public static Vector128<int> ConvertToVector128Int32(Vector128<Half> value);
        public static Vector128<int> ConvertToVector128Int32WithTruncation(Vector128<Half> value);
        public static Vector128<long> ConvertToVector128Int64(Vector128<Half> value);
        public static Vector128<long> ConvertToVector128Int64WithTruncation(Vector128<Half> value);
        public static Vector128<ushort> ConvertToVector128UInt16(Vector128<Half> value);
        public static Vector128<ushort> ConvertToVector128UInt16WithTruncation(Vector128<Half> value);
        public static Vector128<uint> ConvertToVector128UInt32(Vector128<Half> value);
        public static Vector128<uint> ConvertToVector128UInt32WithTruncation(Vector128<Half> value);
        public static Vector128<ulong> ConvertToVector128UInt64(Vector128<Half> value);
        public static Vector128<ulong> ConvertToVector128UInt64WithTruncation(Vector128<Half> value);
        public static Vector128<float> ConvertToVector128Single(Vector128<Half> value);
        public static Vector128<double> ConvertToVector128Double(Vector128<Half> value);
        public static Vector256<Half> Add(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> Divide(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> GetExponent(Vector256<Half> value);
        public static Vector256<Half> GetMantissa(Vector256<Half> value, byte control);
        public static Vector256<Half> Max(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> Min(Vector256<Half> value, Vector256<Half> right);
        public static Vector256<Half> Multiply(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> Reciprocal(Vector256<Half> value);
        public static Half Reduce(Vector256<Half> left, byte control);
        public static Vector256<Half> RoundScale(Vector256<Half> left, byte control);
        public static Vector256<Half> ReciprocalSqrt(Vector256<Half> value);
        public static Vector256<Half> Scale(Vector256<Half> left, Vector256<Half> right, byte control);
        public static Vector256<Half> Sqrt(Vector256<Half> value);
        public static Vector256<Half> Subtract(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> FusedComplexMultiplyAdd(Vector256<Half> addend, Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> FusedComplexMultiplyAddConjugate(Vector256<Half> addend, Vector256<Half> left, Vector256<Half> rightConjugate);
        public static Vector256<Half> ComplexMultiply(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> ComplexMultiplyConjugate(Vector256<Half> left, Vector256<Half> rightConjugate);
        public static Vector256<Half> FusedMultiplyAddSubtract(Vector256<Half> a, Vector256<Half> b, Vector256<Half> c);
        public static Vector256<Half> FusedMultiplySubtractAdd(Vector256<Half> a, Vector256<Half> b, Vector256<Half> c);
        public static Vector256<Half> FusedMultiplyAdd(Vector256<Half> a, Vector256<Half> b, Vector256<Half> c);
        public static Vector256<Half> FusedMultiplySubtract(Vector256<Half> a, Vector256<Half> b, Vector256<Half> c);
        public static Vector256<Half> FusedMultiplyAddNegated(Vector256<Half> a, Vector256<Half> b, Vector256<Half> c);
        public static Vector256<Half> FusedMultiplySubtractNegated(Vector256<Half> a, Vector256<Half> b, Vector256<Half> c);
        public static Vector256<Half> Compare(Vector256<Half> left, Vector256<Half> right, FloatComparisonMode mode);
        public static Vector256<Half> CompareEqual(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> CompareGreaterThan(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> CompareGreaterThanOrEqual(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> CompareLessThan(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> CompareLessThanOrEqual(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> CompareNotEqual(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> CompareNotGreaterThan(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> CompareNotGreaterThanOrEqual(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> CompareNotLessThan(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> CompareNotLessThanOrEqual(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> CompareOrdered(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> CompareUnordered(Vector256<Half> left, Vector256<Half> right);
        public static Vector256<Half> Classify(Vector256<Half> value, byte control);
        public static Vector256<Half> ConvertToVector256Half(Vector256<short> value);
        public static Vector128<Half> ConvertToVector128Half(Vector256<int> value);
        public static Vector128<Half> ConvertToVector128Half(Vector256<long> value);
        public static Vector256<Half> ConvertToVector256Half(Vector256<ushort> value);
        public static Vector128<Half> ConvertToVector128Half(Vector256<uint> value);
        public static Vector128<Half> ConvertToVector128Half(Vector256<ulong> value);
        public static Vector128<Half> ConvertToVector128Half(Vector256<float> value);
        public static Vector128<Half> ConvertToVector128Half(Vector256<double> value);
        public static Vector256<short> ConvertToVector256Int16(Vector256<Half> value);
        public static Vector256<short> ConvertToVector256Int16WithTruncation(Vector256<Half> value);
        public static Vector256<int> ConvertToVector256Int32(Vector128<Half> value);
        public static Vector256<int> ConvertToVector256Int32WithTruncation(Vector128<Half> value);
        public static Vector256<long> ConvertToVector256Int64(Vector128<Half> value);
        public static Vector256<long> ConvertToVector256Int64WithTruncation(Vector128<Half> value);
        public static Vector256<ushort> ConvertToVector256UInt16(Vector256<Half> value);
        public static Vector256<ushort> ConvertToVector256UInt16WithTruncation(Vector256<Half> value);
        public static Vector256<uint> ConvertToVector256UInt32(Vector128<Half> value);
        public static Vector256<uint> ConvertToVector256UInt32WithTruncation(Vector128<Half> value);
        public static Vector256<ulong> ConvertToVector256UInt64(Vector128<Half> value);
        public static Vector256<ulong> ConvertToVector256UInt64WithTruncation(Vector128<Half> value);
        public static Vector256<float> ConvertToVector256Single(Vector128<Half> value);
        public static Vector256<double> ConvertToVector256Double(Vector128<Half> value);
    }
}
```
## GFNI Intrinsics

**Approved** | [#runtime/96170](https://github.com/dotnet/runtime/issues/96170#issuecomment-1971805592) | [Video](https://www.youtube.com/watch?v=o1GpyXImDjc&t=1h10m41s)

* Looks good as proposed

```C#
namespace System.Runtime.Intrinsics.X86;

public abstract class Avx512Gfni : Avx512F
{
    public static bool IsSupported { get; }

    public static Vector512<byte> GaloisFieldAffineTransformInverse(Vector512<byte> x, Vector512<byte> a, [ConstantExpected] byte b);
    public static Vector512<byte> GaloisFieldAffineTransform(Vector512<byte> x, Vector512<byte> a, [ConstantExpected] byte b);
    public static Vector512<byte> GaloisFieldMultiply(Vector512<byte> left, Vector512<byte> right);

    public abstract class VL : Avx512F.VL
    {
        public static new bool IsSupported { get; }

        public static Vector256<byte> GaloisFieldAffineTransformInverse(Vector256<byte> x, Vector256<byte> a, [ConstantExpected] byte b);
        public static Vector128<byte> GaloisFieldAffineTransformInverse(Vector128<byte> x, Vector128<byte> a, [ConstantExpected] byte b);
        public static Vector256<byte> GaloisFieldAffineTransform(Vector256<byte> x, Vector256<byte> a, [ConstantExpected] byte b);
        public static Vector128<byte> GaloisFieldAffineTransform(Vector128<byte> x, Vector128<byte> a, [ConstantExpected] byte b);
        public static Vector256<byte> GaloisFieldMultiply(Vector256<byte> left, Vector256<byte> right);
        public static Vector128<byte> GaloisFieldMultiply(Vector128<byte> left, Vector128<byte> right);
    }
}

public abstract class AvxGfni : Avx
{
    public static bool IsSupported { get; }

    public static Vector256<byte> GaloisFieldAffineTransformInverse(Vector256<byte> x, Vector256<byte> a, [ConstantExpected] byte b);
    public static Vector256<byte> GaloisFieldAffineTransform(Vector256<byte> x, Vector256<byte> a, [ConstantExpected] byte b);
    public static Vector256<byte> GaloisFieldMultiply(Vector256<byte> left, Vector256<byte> right);
}

public abstract class Gfni : Sse41
{
    public static bool IsSupported { get; }

    public static Vector128<byte> GaloisFieldAffineTransformInverse(Vector128<byte> x, Vector128<byte> a, [ConstantExpected] byte b);
    public static Vector128<byte> GaloisFieldAffineTransform(Vector128<byte> x, Vector128<byte> a, [ConstantExpected] byte b);
    public static Vector128<byte> GaloisFieldMultiply(Vector128<byte> left, Vector128<byte> right);
}
```
## AVX-512 `VPOPCNTDQ` and `BITALG` Intrinsics

**Approved** | [#runtime/96162](https://github.com/dotnet/runtime/issues/96162#issuecomment-1971819684) | [Video](https://www.youtube.com/watch?v=o1GpyXImDjc&t=1h15m42s)

* Looks good as proposed

```C#
namespace System.Runtime.Intrinsics.X86;

[Intrinsic]
[CLSCompliant(false)]
public abstract class Avx512VPopcntDQ : Avx512DQ
{
    public static new bool IsSupported { get; }

    [Intrinsic]
    public new abstract class X64 : Avx512DQ.X64
    {
        public static new bool IsSupported { get; }
    }

    public static Vector512<int> PopCount(Vector512<int> value);
    public static Vector512<uint> PopCount(Vector512<uint> value);
    public static Vector512<long> PopCount(Vector512<long> value);
    public static Vector512<ulong> PopCount(Vector512<ulong> value);

    public abstract class VL : Avx512DQ.VL
    {
        public static new bool IsSupported { get; }

        public static Vector256<int> PopCount(Vector256<int> value);
        public static Vector256<uint> PopCount(Vector256<uint> value);
        public static Vector256<long> PopCount(Vector256<long> value);
        public static Vector256<ulong> PopCount(Vector256<ulong> value);
        public static Vector128<int> PopCount(Vector128<int> value);
        public static Vector128<uint> PopCount(Vector128<uint> value);
        public static Vector128<long> PopCount(Vector128<long> value);
        public static Vector128<ulong> PopCount(Vector128<ulong> value);
    }
}

[Intrinsic]
[CLSCompliant(false)]
public abstract class Avx512BitAlg : Avx512BW
{
    public static new bool IsSupported { get; }

    [Intrinsic]
    public new abstract class X64 : Avx512BW.X64
    {
        public static new bool IsSupported { get; }
    }

    public static Vector512<short> PopCount(Vector512<short> value);
    public static Vector512<ushort> PopCount(Vector512<ushort> value);
    public static Vector512<byte> PopCount(Vector512<byte> value);
    public static Vector512<sbyte> PopCount(Vector512<sbyte> value);
    public static Vector512<byte> ShuffleBits(Vector512<ulong> value, Vector512<byte> control);
    public static Vector512<sbyte> ShuffleBits(Vector512<long> value, Vector512<sbyte> control);
    public static Vector512<byte> MaskShuffleBits(Vector512<byte> mask, Vector512<ulong> value, Vector512<byte> control);
    public static Vector512<sbyte> MaskShuffleBits(Vector512<sbyte> mask, Vector512<long> value, Vector512<sbyte> control);

    public abstract class VL : Avx512BW.VL
    {
        public static new bool IsSupported { get; }

        public static Vector256<short> PopCount(Vector256<short> value);
        public static Vector256<ushort> PopCount(Vector256<ushort> value);
        public static Vector256<byte> PopCount(Vector256<byte> value);
        public static Vector256<sbyte> PopCount(Vector256<sbyte> value);
        public static Vector128<short> PopCount(Vector128<short> value);
        public static Vector128<ushort> PopCount(Vector128<ushort> value);
        public static Vector128<byte> PopCount(Vector128<byte> value);
        public static Vector128<sbyte> PopCount(Vector128<sbyte> value);       
        public static Vector256<byte> ShuffleBits(Vector256<ulong> value, Vector256<byte> control);
        public static Vector256<sbyte> ShuffleBits(Vector256<long> value, Vector256<sbyte> control);
        public static Vector256<byte> MaskShuffleBits(Vector256<byte> mask, Vector256<ulong> value, Vector256<byte> control);
        public static Vector256<sbyte> MaskShuffleBits(Vector256<sbyte> mask, Vector256<long> value, Vector256<sbyte> control);
        public static Vector128<byte> ShuffleBits(Vector128<ulong> value, Vector128<byte> control);
        public static Vector128<sbyte> ShuffleBits(Vector128<long> value, Vector128<sbyte> control);
        public static Vector128<byte> MaskShuffleBits(Vector128<byte> mask, Vector128<ulong> value, Vector128<byte> control);
        public static Vector128<sbyte> MaskShuffleBits(Vector128<sbyte> mask, Vector128<long> value, Vector128<sbyte> control);
    }
}
```
## Expose System.Runtime.Intrinsics.X86.Aes256 and Aes512

**Approved** | [#runtime/86952](https://github.com/dotnet/runtime/issues/86952#issuecomment-1971832725) | [Video](https://www.youtube.com/watch?v=o1GpyXImDjc&t=1h25m25s)

* Looks good as proposed, we opted to change the surface area slightly to match the approach approved for Avx10 where we have a nested V256 and V512 class

```C#
namespace System.Runtime.Intrinsics.X86;

public abstract class Aes
{
    public abstract class V256
    {
        public static new bool IsSupported { get; }

        public static Vector256<byte> Decrypt(Vector256<byte> value, Vector256<byte> roundKey);
        public static Vector256<byte> DecryptLast(Vector256<byte> value, Vector256<byte> roundKey);
        public static Vector256<byte> Encrypt(Vector256<byte> value, Vector256<byte> roundKey);   
        public static Vector256<byte> EncryptLast(Vector256<byte> value, Vector256<byte> roundKey);
    }

    public abstract class V512
    {
        public static Vector512<byte> Decrypt(Vector512<byte> value, Vector512<byte> roundKey);   
        public static Vector512<byte> DecryptLast(Vector512<byte> value, Vector512<byte> roundKey);
        public static Vector512<byte> Encrypt(Vector512<byte> value, Vector512<byte> roundKey);
        public static Vector512<byte> EncryptLast(Vector512<byte> value, Vector512<byte> roundKey);
    }
}
```
