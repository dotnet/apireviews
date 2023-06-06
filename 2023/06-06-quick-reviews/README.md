# API Review 06/06/2023

## AVX512 masking support

**Approved** | [#runtime/87097](https://github.com/dotnet/runtime/issues/87097#issuecomment-1579233288) | [Video](https://www.youtube.com/watch?v=qHcX5_hlMRU&t=0h0m0s)

* Let's remove `IntComparisonMode` as it doesn't provide any value over the named methods and it's more consistent what we have done before.

```C#
namespace System.Runtime.Intrinsics.X86;

public static partial class Avx512F
{
    public static Vector512<double> BlendVariable(Vector512<double> left, Vector512<double> right, Vector512<double> mask);
    public static Vector512<int>    BlendVariable(Vector512<int>    left, Vector512<int>    right, Vector512<int>    mask);
    public static Vector512<long>   BlendVariable(Vector512<long>   left, Vector512<long>   right, Vector512<long>   mask);
    public static Vector512<float>  BlendVariable(Vector512<float>  left, Vector512<float>  right, Vector512<float>  mask);
    public static Vector512<uint>   BlendVariable(Vector512<uint>   left, Vector512<uint>   right, Vector512<uint>   mask);
    public static Vector512<ulong>  BlendVariable(Vector512<ulong>  left, Vector512<ulong>  right, Vector512<ulong>  mask);

    public static Vector512<double> Compare                     (Vector512<double> left, Vector512<double> right, [ConstantExpected(Max = FloatComparisonMode.UnorderedTrueSignaling)] FloatComparisonMode mode);
    public static Vector512<double> CompareEqual                (Vector512<double> left, Vector512<double> right);
    public static Vector512<double> CompareGreaterThan          (Vector512<double> left, Vector512<double> right);
    public static Vector512<double> CompareGreaterThanOrEqual   (Vector512<double> left, Vector512<double> right);
    public static Vector512<double> CompareLessThan             (Vector512<double> left, Vector512<double> right);
    public static Vector512<double> CompareLessThanOrEqual      (Vector512<double> left, Vector512<double> right);
    public static Vector512<double> CompareNotEqual             (Vector512<double> left, Vector512<double> right);
    public static Vector512<double> CompareNotGreaterThan       (Vector512<double> left, Vector512<double> right);
    public static Vector512<double> CompareNotGreaterThanOrEqual(Vector512<double> left, Vector512<double> right);
    public static Vector512<double> CompareNotLessThan          (Vector512<double> left, Vector512<double> right);
    public static Vector512<double> CompareNotLessThanOrEqual   (Vector512<double> left, Vector512<double> right);
    public static Vector512<double> CompareOrdered              (Vector512<double> left, Vector512<double> right);
    public static Vector512<double> CompareUnordered            (Vector512<double> left, Vector512<double> right);

    public static Vector512<float> Compare                     (Vector512<float> left, Vector512<float> right, [ConstantExpected(Max = FloatComparisonMode.UnorderedTrueSignaling)] FloatComparisonMode mode);
    public static Vector512<float> CompareEqual                (Vector512<float> left, Vector512<float> right);
    public static Vector512<float> CompareGreaterThan          (Vector512<float> left, Vector512<float> right);
    public static Vector512<float> CompareGreaterThanOrEqual   (Vector512<float> left, Vector512<float> right);
    public static Vector512<float> CompareLessThan             (Vector512<float> left, Vector512<float> right);
    public static Vector512<float> CompareLessThanOrEqual      (Vector512<float> left, Vector512<float> right);
    public static Vector512<float> CompareNotEqual             (Vector512<float> left, Vector512<float> right);
    public static Vector512<float> CompareNotGreaterThan       (Vector512<float> left, Vector512<float> right);
    public static Vector512<float> CompareNotGreaterThanOrEqual(Vector512<float> left, Vector512<float> right);
    public static Vector512<float> CompareNotLessThan          (Vector512<float> left, Vector512<float> right);
    public static Vector512<float> CompareNotLessThanOrEqual   (Vector512<float> left, Vector512<float> right);
    public static Vector512<float> CompareOrdered              (Vector512<float> left, Vector512<float> right);
    public static Vector512<float> CompareUnordered            (Vector512<float> left, Vector512<float> right);

    public static Vector512<int> CompareEqual             (Vector512<int> left, Vector512<int> right);
    public static Vector512<int> CompareGreaterThan       (Vector512<int> left, Vector512<int> right);
    public static Vector512<int> CompareGreaterThanOrEqual(Vector512<int> left, Vector512<int> right);
    public static Vector512<int> CompareLessThan          (Vector512<int> left, Vector512<int> right);
    public static Vector512<int> CompareLessThanOrEqual   (Vector512<int> left, Vector512<int> right);
    public static Vector512<int> CompareNotEqual          (Vector512<int> left, Vector512<int> right);

    public static Vector512<long> CompareEqual             (Vector512<long> left, Vector512<long> right);
    public static Vector512<long> CompareGreaterThan       (Vector512<long> left, Vector512<long> right);
    public static Vector512<long> CompareGreaterThanOrEqual(Vector512<long> left, Vector512<long> right);
    public static Vector512<long> CompareLessThan          (Vector512<long> left, Vector512<long> right);
    public static Vector512<long> CompareLessThanOrEqual   (Vector512<long> left, Vector512<long> right);
    public static Vector512<long> CompareNotEqual          (Vector512<long> left, Vector512<long> right);

    public static Vector512<uint> CompareEqual             (Vector512<uint> left, Vector512<uint> right);
    public static Vector512<uint> CompareGreaterThan       (Vector512<uint> left, Vector512<uint> right);
    public static Vector512<uint> CompareGreaterThanOrEqual(Vector512<uint> left, Vector512<uint> right);
    public static Vector512<uint> CompareLessThan          (Vector512<uint> left, Vector512<uint> right);
    public static Vector512<uint> CompareLessThanOrEqual   (Vector512<uint> left, Vector512<uint> right);
    public static Vector512<uint> CompareNotEqual          (Vector512<uint> left, Vector512<uint> right);

    public static Vector512<ulong> CompareEqual             (Vector512<ulong> left, Vector512<ulong> right);
    public static Vector512<ulong> CompareGreaterThan       (Vector512<ulong> left, Vector512<ulong> right);
    public static Vector512<ulong> CompareGreaterThanOrEqual(Vector512<ulong> left, Vector512<ulong> right);
    public static Vector512<ulong> CompareLessThan          (Vector512<ulong> left, Vector512<ulong> right);
    public static Vector512<ulong> CompareLessThanOrEqual   (Vector512<ulong> left, Vector512<ulong> right);
    public static Vector512<ulong> CompareNotEqual          (Vector512<ulong> left, Vector512<ulong> right);

    public static Vector512<double> Compress(Vector512<double> value, Vector512<double> mask);
    public static Vector512<int>    Compress(Vector512<int>    value, Vector512<int>    mask);
    public static Vector512<long>   Compress(Vector512<long>   value, Vector512<long>   mask);
    public static Vector512<float>  Compress(Vector512<float>  value, Vector512<float>  mask);
    public static Vector512<uint>   Compress(Vector512<uint>   value, Vector512<uint>   mask);
    public static Vector512<ulong>  Compress(Vector512<ulong>  value, Vector512<ulong>  mask);

    public static Vector512<double> Expand(Vector512<double> value, Vector512<double> mask);
    public static Vector512<int>    Expand(Vector512<int>    value, Vector512<int>    mask);
    public static Vector512<long>   Expand(Vector512<long>   value, Vector512<long>   mask);
    public static Vector512<float>  Expand(Vector512<float>  value, Vector512<float>  mask);
    public static Vector512<uint>   Expand(Vector512<uint>   value, Vector512<uint>   mask);
    public static Vector512<ulong>  Expand(Vector512<ulong>  value, Vector512<ulong>  mask);

    public static unsafe Vector512<double> GatherMaskVector512(Vector512<double> source, double* baseAddress, Vector512<int> index, Vector512<double> mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<int>    GatherMaskVector512(Vector512<int>    source, int*    baseAddress, Vector512<int> index, Vector512<int>    mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<long>   GatherMaskVector512(Vector512<long>   source, long*   baseAddress, Vector512<int> index, Vector512<long>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<float>  GatherMaskVector512(Vector512<float>  source, float*  baseAddress, Vector512<int> index, Vector512<float>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<uint>   GatherMaskVector512(Vector512<uint>   source, uint*   baseAddress, Vector512<int> index, Vector512<uint>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<ulong>  GatherMaskVector512(Vector512<ulong>  source, ulong*  baseAddress, Vector512<int> index, Vector512<ulong>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);

    public static unsafe Vector512<double> GatherMaskVector512(Vector512<double> source, double* baseAddress, Vector512<long> index, Vector512<double> mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<int>    GatherMaskVector512(Vector512<int>    source, int*    baseAddress, Vector512<long> index, Vector512<int>    mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<long>   GatherMaskVector512(Vector512<long>   source, long*   baseAddress, Vector512<long> index, Vector512<long>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<uint>   GatherMaskVector512(Vector512<uint>   source, uint*   baseAddress, Vector512<long> index, Vector512<uint>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<float>  GatherMaskVector512(Vector512<float>  source, float*  baseAddress, Vector512<long> index, Vector512<float>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<ulong>  GatherMaskVector512(Vector512<ulong>  source, ulong*  baseAddress, Vector512<long> index, Vector512<ulong>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);

    public static unsafe Vector512<double> GatherVector512(double* baseAddress, Vector512<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<int>    GatherVector512(int*    baseAddress, Vector512<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<long>   GatherVector512(long*   baseAddress, Vector512<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<float>  GatherVector512(float*  baseAddress, Vector512<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<uint>   GatherVector512(uint*   baseAddress, Vector512<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<ulong>  GatherVector512(ulong*  baseAddress, Vector512<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);

    public static unsafe Vector512<double> GatherVector512(double* baseAddress, Vector512<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<int>    GatherVector512(int*    baseAddress, Vector512<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<long>   GatherVector512(long*   baseAddress, Vector512<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<float>  GatherVector512(float*  baseAddress, Vector512<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<uint>   GatherVector512(uint*   baseAddress, Vector512<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe Vector512<ulong>  GatherVector512(ulong*  baseAddress, Vector512<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);

    public static unsafe Vector512<double> MaskLoad(double* address, Vector512<double> mask);
    public static unsafe Vector512<int>    MaskLoad(int*    address, Vector512<int>    mask);
    public static unsafe Vector512<long>   MaskLoad(long*   address, Vector512<long>   mask);
    public static unsafe Vector512<float>  MaskLoad(float*  address, Vector512<float>  mask);
    public static unsafe Vector512<uint>   MaskLoad(uint*   address, Vector512<uint>   mask);
    public static unsafe Vector512<ulong>  MaskLoad(ulong*  address, Vector512<ulong>  mask);

    public static unsafe void MaskStore(double* address, Vector512<double> mask, Vector512<double> source);
    public static unsafe void MaskStore(int*    address, Vector512<int>    mask, Vector512<int>    source);
    public static unsafe void MaskStore(long*   address, Vector512<long>   mask, Vector512<long>   source);
    public static unsafe void MaskStore(float*  address, Vector512<float>  mask, Vector512<float>  source);
    public static unsafe void MaskStore(uint*   address, Vector512<uint>   mask, Vector512<uint>   source);
    public static unsafe void MaskStore(ulong*  address, Vector512<ulong>  mask, Vector512<ulong>  source);

    public static int MoveMask(Vector256<short>  value);
    public static int MoveMask(Vector256<ushort> value);
    public static int MoveMask(Vector512<int>    value);
    public static int MoveMask(Vector512<float>  value);
    public static int MoveMask(Vector512<uint>   value);

    public static unsafe void ScatterMaskVector512(Vector512<double> value, double* baseAddress, Vector512<int> index, Vector512<double> mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterMaskVector512(Vector512<int>    value, int*    baseAddress, Vector512<int> index, Vector512<int>    mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterMaskVector512(Vector512<long>   value, long*   baseAddress, Vector512<int> index, Vector512<long>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterMaskVector512(Vector512<float>  value, float*  baseAddress, Vector512<int> index, Vector512<float>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterMaskVector512(Vector512<uint>   value, uint*   baseAddress, Vector512<int> index, Vector512<uint>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterMaskVector512(Vector512<ulong>  value, ulong*  baseAddress, Vector512<int> index, Vector512<ulong>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);

    public static unsafe void ScatterMaskVector512(Vector512<double> value, double* baseAddress, Vector512<long> index, Vector512<double> mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterMaskVector512(Vector512<int>    value, int*    baseAddress, Vector512<long> index, Vector512<int>    mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterMaskVector512(Vector512<long>   value, long*   baseAddress, Vector512<long> index, Vector512<long>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterMaskVector512(Vector512<float>  value, uint*   baseAddress, Vector512<long> index, Vector512<uint>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterMaskVector512(Vector512<uint>   value, float*  baseAddress, Vector512<long> index, Vector512<float>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterMaskVector512(Vector512<ulong>  value, ulong*  baseAddress, Vector512<long> index, Vector512<ulong>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);

    public static unsafe void ScatterVector512(Vector512<double> value, double* baseAddress, Vector512<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterVector512(Vector512<int>    value, int*    baseAddress, Vector512<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterVector512(Vector512<long>   value, long*   baseAddress, Vector512<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterVector512(Vector512<float>  value, float*  baseAddress, Vector512<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterVector512(Vector512<uint>   value, uint*   baseAddress, Vector512<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterVector512(Vector512<ulong>  value, ulong*  baseAddress, Vector512<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);

    public static unsafe void ScatterVector512(Vector512<double> value, double* baseAddress, Vector512<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterVector512(Vector512<int>    value, int*    baseAddress, Vector512<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterVector512(Vector512<long>   value, long*   baseAddress, Vector512<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterVector512(Vector512<float>  value, float*  baseAddress, Vector512<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterVector512(Vector512<uint>   value, uint*   baseAddress, Vector512<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    public static unsafe void ScatterVector512(Vector512<ulong>  value, ulong*  baseAddress, Vector512<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);

    public static bool TestC(Vector512<double> left, Vector512<double> right);
    public static bool TestC(Vector512<int>    left, Vector512<int>    right);
    public static bool TestC(Vector512<long>   left, Vector512<long>   right);
    public static bool TestC(Vector512<float>  left, Vector512<float>  right);
    public static bool TestC(Vector512<uint>   left, Vector512<uint>   right);
    public static bool TestC(Vector512<ulong>  left, Vector512<ulong>  right);

    public static bool TestNotZAndNotC(Vector512<double> left, Vector512<double> right);
    public static bool TestNotZAndNotC(Vector512<int>    left, Vector512<int>    right);
    public static bool TestNotZAndNotC(Vector512<long>   left, Vector512<long>   right);
    public static bool TestNotZAndNotC(Vector512<float>  left, Vector512<float>  right);
    public static bool TestNotZAndNotC(Vector512<uint>   left, Vector512<uint>   right);
    public static bool TestNotZAndNotC(Vector512<ulong>  left, Vector512<ulong>  right);

    public static bool TestZ(Vector512<double> left, Vector512<double> right);
    public static bool TestZ(Vector512<int>    left, Vector512<int>    right);
    public static bool TestZ(Vector512<long>   left, Vector512<long>   right);
    public static bool TestZ(Vector512<float>  left, Vector512<float>  right);
    public static bool TestZ(Vector512<uint>   left, Vector512<uint>   right);
    public static bool TestZ(Vector512<ulong>  left, Vector512<ulong>  right);

    public static partial class VL
    {
        public static Vector128<int> CompareGreaterThanOrEqual(Vector128<int> left, Vector128<int> right);
        public static Vector128<int> CompareLessThan          (Vector128<int> left, Vector128<int> right);
        public static Vector128<int> CompareLessThanOrEqual   (Vector128<int> left, Vector128<int> right);
        public static Vector128<int> CompareNotEqual          (Vector128<int> left, Vector128<int> right);

        public static Vector256<int> CompareGreaterThanOrEqual(Vector256<int> left, Vector256<int> right);
        public static Vector256<int> CompareLessThan          (Vector256<int> left, Vector256<int> right);
        public static Vector256<int> CompareLessThanOrEqual   (Vector256<int> left, Vector256<int> right);
        public static Vector256<int> CompareNotEqual          (Vector256<int> left, Vector256<int> right);

        public static Vector128<long> CompareGreaterThanOrEqual(Vector128<long> left, Vector128<long> right);
        public static Vector128<long> CompareLessThan          (Vector128<long> left, Vector128<long> right);
        public static Vector128<long> CompareLessThanOrEqual   (Vector128<long> left, Vector128<long> right);
        public static Vector128<long> CompareNotEqual          (Vector128<long> left, Vector128<long> right);
        public static Vector256<long> CompareGreaterThanOrEqual(Vector256<long> left, Vector256<long> right);
        public static Vector256<long> CompareLessThan          (Vector256<long> left, Vector256<long> right);
        public static Vector256<long> CompareLessThanOrEqual   (Vector256<long> left, Vector256<long> right);
        public static Vector256<long> CompareNotEqual          (Vector256<long> left, Vector256<long> right);

        public static Vector128<uint> CompareGreaterThan       (Vector128<uint> left, Vector128<uint> right);
        public static Vector128<uint> CompareGreaterThanOrEqual(Vector128<uint> left, Vector128<uint> right);
        public static Vector128<uint> CompareLessThan          (Vector128<uint> left, Vector128<uint> right);
        public static Vector128<uint> CompareLessThanOrEqual   (Vector128<uint> left, Vector128<uint> right);
        public static Vector128<uint> CompareNotEqual          (Vector128<uint> left, Vector128<uint> right);
        public static Vector256<uint> CompareGreaterThan       (Vector256<uint> left, Vector256<uint> right);
        public static Vector256<uint> CompareGreaterThanOrEqual(Vector256<uint> left, Vector256<uint> right);
        public static Vector256<uint> CompareLessThan          (Vector256<uint> left, Vector256<uint> right);
        public static Vector256<uint> CompareLessThanOrEqual   (Vector256<uint> left, Vector256<uint> right);
        public static Vector256<uint> CompareNotEqual          (Vector256<uint> left, Vector256<uint> right);

        public static Vector128<ulong> CompareGreaterThan       (Vector128<ulong> left, Vector128<ulong> right);
        public static Vector128<ulong> CompareGreaterThanOrEqual(Vector128<ulong> left, Vector128<ulong> right);
        public static Vector128<ulong> CompareLessThan          (Vector128<ulong> left, Vector128<ulong> right);
        public static Vector128<ulong> CompareLessThanOrEqual   (Vector128<ulong> left, Vector128<ulong> right);
        public static Vector128<ulong> CompareNotEqual          (Vector128<ulong> left, Vector128<ulong> right);
        public static Vector256<ulong> CompareGreaterThan       (Vector256<ulong> left, Vector256<ulong> right);
        public static Vector256<ulong> CompareGreaterThanOrEqual(Vector256<ulong> left, Vector256<ulong> right);
        public static Vector256<ulong> CompareLessThan          (Vector256<ulong> left, Vector256<ulong> right);
        public static Vector256<ulong> CompareLessThanOrEqual   (Vector256<ulong> left, Vector256<ulong> right);
        public static Vector256<ulong> CompareNotEqual          (Vector256<ulong> left, Vector256<ulong> right);

        public static Vector128<double> Compress(Vector128<double> value, Vector128<double> mask);
        public static Vector128<int>    Compress(Vector128<int>    value, Vector128<int>    mask);
        public static Vector128<long>   Compress(Vector128<long>   value, Vector128<long>   mask);
        public static Vector128<float>  Compress(Vector128<float>  value, Vector128<float>  mask);
        public static Vector128<uint>   Compress(Vector128<uint>   value, Vector128<uint>   mask);
        public static Vector128<ulong>  Compress(Vector128<ulong>  value, Vector128<ulong>  mask);
        public static Vector256<double> Compress(Vector256<double> value, Vector256<double> mask);
        public static Vector256<int>    Compress(Vector256<int>    value, Vector256<int>    mask);
        public static Vector256<long>   Compress(Vector256<long>   value, Vector256<long>   mask);
        public static Vector256<float>  Compress(Vector256<float>  value, Vector256<float>  mask);
        public static Vector256<uint>   Compress(Vector256<uint>   value, Vector256<uint>   mask);
        public static Vector256<ulong>  Compress(Vector256<ulong>  value, Vector256<ulong>  mask);

        public static Vector128<double> Expand(Vector128<double> value, Vector128<double> mask);
        public static Vector128<int>    Expand(Vector128<int>    value, Vector128<int>    mask);
        public static Vector128<long>   Expand(Vector128<long>   value, Vector128<long>   mask);
        public static Vector128<float>  Expand(Vector128<float>  value, Vector128<float>  mask);
        public static Vector128<uint>   Expand(Vector128<uint>   value, Vector128<uint>   mask);
        public static Vector128<ulong>  Expand(Vector128<ulong>  value, Vector128<ulong>  mask);
        public static Vector256<double> Expand(Vector256<double> value, Vector256<double> mask);
        public static Vector256<int>    Expand(Vector256<int>    value, Vector256<int>    mask);
        public static Vector256<long>   Expand(Vector256<long>   value, Vector256<long>   mask);
        public static Vector256<float>  Expand(Vector256<float>  value, Vector256<float>  mask);
        public static Vector256<uint>   Expand(Vector256<uint>   value, Vector256<uint>   mask);
        public static Vector256<ulong>  Expand(Vector256<ulong>  value, Vector256<ulong>  mask);

        public static unsafe void ScatterMaskVector128(Vector128<double> value, double* baseAddress, Vector128<int> index, Vector128<double> mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector128(Vector128<int>    value, int*    baseAddress, Vector128<int> index, Vector128<int>    mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector128(Vector128<long>   value, long*   baseAddress, Vector128<int> index, Vector128<long>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector128(Vector128<float>  value, float*  baseAddress, Vector128<int> index, Vector128<float>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector128(Vector128<uint>   value, uint*   baseAddress, Vector128<int> index, Vector128<uint>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector128(Vector128<ulong>  value, ulong*  baseAddress, Vector128<int> index, Vector128<ulong>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector256(Vector256<double> value, double* baseAddress, Vector256<int> index, Vector256<double> mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector256(Vector256<int>    value, int*    baseAddress, Vector256<int> index, Vector256<int>    mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector256(Vector256<long>   value, long*   baseAddress, Vector256<int> index, Vector256<long>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector256(Vector256<float>  value, float*  baseAddress, Vector256<int> index, Vector256<float>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector256(Vector256<uint>   value, uint*   baseAddress, Vector256<int> index, Vector256<uint>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector256(Vector256<ulong>  value, ulong*  baseAddress, Vector256<int> index, Vector256<ulong>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);

        public static unsafe void ScatterMaskVector128(Vector128<double> value, double* baseAddress, Vector128<long> index, Vector128<double> mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector128(Vector128<int>    value, int*    baseAddress, Vector128<long> index, Vector128<int>    mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector128(Vector128<long>   value, long*   baseAddress, Vector128<long> index, Vector128<long>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector128(Vector128<float>  value, uint*   baseAddress, Vector128<long> index, Vector128<uint>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector128(Vector128<uint>   value, float*  baseAddress, Vector128<long> index, Vector128<float>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector128(Vector128<ulong>  value, ulong*  baseAddress, Vector128<long> index, Vector128<ulong>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector256(Vector256<double> value, double* baseAddress, Vector256<long> index, Vector256<double> mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector256(Vector256<int>    value, int*    baseAddress, Vector256<long> index, Vector256<int>    mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector256(Vector256<long>   value, long*   baseAddress, Vector256<long> index, Vector256<long>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector256(Vector256<float>  value, uint*   baseAddress, Vector256<long> index, Vector256<uint>   mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector256(Vector256<uint>   value, float*  baseAddress, Vector256<long> index, Vector256<float>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterMaskVector256(Vector256<ulong>  value, ulong*  baseAddress, Vector256<long> index, Vector256<ulong>  mask, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);

        public static unsafe void ScatterVector128(Vector128<double> value, double* baseAddress, Vector128<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector128(Vector128<int>    value, int*    baseAddress, Vector128<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector128(Vector128<long>   value, long*   baseAddress, Vector128<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector128(Vector128<float>  value, float*  baseAddress, Vector128<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector128(Vector128<uint>   value, uint*   baseAddress, Vector128<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector128(Vector128<ulong>  value, ulong*  baseAddress, Vector128<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector256(Vector256<double> value, double* baseAddress, Vector256<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector256(Vector256<int>    value, int*    baseAddress, Vector256<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector256(Vector256<long>   value, long*   baseAddress, Vector256<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector256(Vector256<float>  value, float*  baseAddress, Vector256<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector256(Vector256<uint>   value, uint*   baseAddress, Vector256<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector256(Vector256<ulong>  value, ulong*  baseAddress, Vector256<int> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);

        public static unsafe void ScatterVector128(Vector128<double> value, double* baseAddress, Vector128<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector128(Vector128<int>    value, int*    baseAddress, Vector128<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector128(Vector128<long>   value, long*   baseAddress, Vector128<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector128(Vector128<float>  value, float*  baseAddress, Vector128<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector128(Vector128<uint>   value, uint*   baseAddress, Vector128<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector128(Vector128<ulong>  value, ulong*  baseAddress, Vector128<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector256(Vector256<double> value, double* baseAddress, Vector256<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector256(Vector256<int>    value, int*    baseAddress, Vector256<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector256(Vector256<long>   value, long*   baseAddress, Vector256<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector256(Vector256<float>  value, float*  baseAddress, Vector256<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector256(Vector256<uint>   value, uint*   baseAddress, Vector256<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
        public static unsafe void ScatterVector256(Vector256<ulong>  value, ulong*  baseAddress, Vector256<long> index, [ConstantExpected(Min = (byte)(1), Max = (byte)(8))] byte scale);
    }
}

public static partial class Avx512BW
{
    public static Vector512<byte>   BlendVariable(Vector512<byte>   left, Vector512<byte>   right, Vector512<byte>   mask);
    public static Vector512<short>  BlendVariable(Vector512<short>  left, Vector512<short>  right, Vector512<short>  mask);
    public static Vector512<sbyte>  BlendVariable(Vector512<sbyte>  left, Vector512<sbyte>  right, Vector512<sbyte>  mask);
    public static Vector512<ushort> BlendVariable(Vector512<ushort> left, Vector512<ushort> right, Vector512<ushort> mask);

    public static Vector512<byte> CompareEqual             (Vector512<byte> left, Vector512<byte> right);
    public static Vector512<byte> CompareGreaterThan       (Vector512<byte> left, Vector512<byte> right);
    public static Vector512<byte> CompareGreaterThanOrEqual(Vector512<byte> left, Vector512<byte> right);
    public static Vector512<byte> CompareLessThan          (Vector512<byte> left, Vector512<byte> right);
    public static Vector512<byte> CompareLessThanOrEqual   (Vector512<byte> left, Vector512<byte> right);
    public static Vector512<byte> CompareNotEqual          (Vector512<byte> left, Vector512<byte> right);

    public static Vector512<short> CompareEqual             (Vector512<short> left, Vector512<short> right);
    public static Vector512<short> CompareGreaterThan       (Vector512<short> left, Vector512<short> right);
    public static Vector512<short> CompareGreaterThanOrEqual(Vector512<short> left, Vector512<short> right);
    public static Vector512<short> CompareLessThan          (Vector512<short> left, Vector512<short> right);
    public static Vector512<short> CompareLessThanOrEqual   (Vector512<short> left, Vector512<short> right);
    public static Vector512<short> CompareNotEqual          (Vector512<short> left, Vector512<short> right);

    public static Vector512<sbyte> CompareEqual             (Vector512<sbyte> left, Vector512<sbyte> right);
    public static Vector512<sbyte> CompareGreaterThan       (Vector512<sbyte> left, Vector512<sbyte> right);
    public static Vector512<sbyte> CompareGreaterThanOrEqual(Vector512<sbyte> left, Vector512<sbyte> right);
    public static Vector512<sbyte> CompareLessThan          (Vector512<sbyte> left, Vector512<sbyte> right);
    public static Vector512<sbyte> CompareLessThanOrEqual   (Vector512<sbyte> left, Vector512<sbyte> right);
    public static Vector512<sbyte> CompareNotEqual          (Vector512<sbyte> left, Vector512<sbyte> right);

    public static Vector512<ushort> CompareEqual             (Vector512<ushort> left, Vector512<ushort> right);
    public static Vector512<ushort> CompareGreaterThan       (Vector512<ushort> left, Vector512<ushort> right);
    public static Vector512<ushort> CompareGreaterThanOrEqual(Vector512<ushort> left, Vector512<ushort> right);
    public static Vector512<ushort> CompareLessThan          (Vector512<ushort> left, Vector512<ushort> right);
    public static Vector512<ushort> CompareLessThanOrEqual   (Vector512<ushort> left, Vector512<ushort> right);
    public static Vector512<ushort> CompareNotEqual          (Vector512<ushort> left, Vector512<ushort> right);

    public static int MoveMask(Vector512<short>  value);
    public static int MoveMask(Vector512<ushort> value);

    public static long MoveMask(Vector512<byte>  value);
    public static long MoveMask(Vector512<sbyte> value);

    public static bool TestC(Vector512<byte>   left, Vector512<byte>   right);
    public static bool TestC(Vector512<short>  left, Vector512<short>  right);
    public static bool TestC(Vector512<sbyte>  left, Vector512<sbyte>  right);
    public static bool TestC(Vector512<ushort> left, Vector512<ushort> right);

    public static bool TestNotZAndNotC(Vector512<byte>   left, Vector512<byte>   right);
    public static bool TestNotZAndNotC(Vector512<short>  left, Vector512<short>  right);
    public static bool TestNotZAndNotC(Vector512<sbyte>  left, Vector512<sbyte>  right);
    public static bool TestNotZAndNotC(Vector512<ushort> left, Vector512<ushort> right);

    public static bool TestZ(Vector512<byte>   left, Vector512<byte>   right);
    public static bool TestZ(Vector512<short>  left, Vector512<short>  right);
    public static bool TestZ(Vector512<sbyte>  left, Vector512<sbyte>  right);
    public static bool TestZ(Vector512<ushort> left, Vector512<ushort> right);

    public static partial class VL
    {
        public static Vector128<byte> CompareGreaterThan       (Vector128<byte> left, Vector128<byte> right);
        public static Vector128<byte> CompareGreaterThanOrEqual(Vector128<byte> left, Vector128<byte> right);
        public static Vector128<byte> CompareLessThan          (Vector128<byte> left, Vector128<byte> right);
        public static Vector128<byte> CompareLessThanOrEqual   (Vector128<byte> left, Vector128<byte> right);
        public static Vector128<byte> CompareNotEqual          (Vector128<byte> left, Vector128<byte> right);
        public static Vector256<byte> CompareGreaterThan       (Vector256<byte> left, Vector256<byte> right);
        public static Vector256<byte> CompareGreaterThanOrEqual(Vector256<byte> left, Vector256<byte> right);
        public static Vector256<byte> CompareLessThan          (Vector256<byte> left, Vector256<byte> right);
        public static Vector256<byte> CompareLessThanOrEqual   (Vector256<byte> left, Vector256<byte> right);
        public static Vector256<byte> CompareNotEqual          (Vector256<byte> left, Vector256<byte> right);

        public static Vector128<short> CompareGreaterThanOrEqual(Vector128<short> left, Vector128<short> right);
        public static Vector128<short> CompareLessThan          (Vector128<short> left, Vector128<short> right);
        public static Vector128<short> CompareLessThanOrEqual   (Vector128<short> left, Vector128<short> right);
        public static Vector128<short> CompareNotEqual          (Vector128<short> left, Vector128<short> right);
        public static Vector256<short> CompareGreaterThanOrEqual(Vector256<short> left, Vector256<short> right);
        public static Vector256<short> CompareLessThan          (Vector256<short> left, Vector256<short> right);
        public static Vector256<short> CompareLessThanOrEqual   (Vector256<short> left, Vector256<short> right);
        public static Vector256<short> CompareNotEqual          (Vector256<short> left, Vector256<short> right);

        public static Vector128<sbyte> CompareGreaterThanOrEqual(Vector128<sbyte> left, Vector128<sbyte> right);
        public static Vector128<sbyte> CompareLessThan          (Vector128<sbyte> left, Vector128<sbyte> right);
        public static Vector128<sbyte> CompareLessThanOrEqual   (Vector128<sbyte> left, Vector128<sbyte> right);
        public static Vector128<sbyte> CompareNotEqual          (Vector128<sbyte> left, Vector128<sbyte> right);
        public static Vector256<sbyte> CompareGreaterThanOrEqual(Vector256<sbyte> left, Vector256<sbyte> right);
        public static Vector256<sbyte> CompareLessThan          (Vector256<sbyte> left, Vector256<sbyte> right);
        public static Vector256<sbyte> CompareLessThanOrEqual   (Vector256<sbyte> left, Vector256<sbyte> right);
        public static Vector256<sbyte> CompareNotEqual          (Vector256<sbyte> left, Vector256<sbyte> right);

        public static Vector128<ushort> CompareGreaterThan       (Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<ushort> CompareGreaterThanOrEqual(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<ushort> CompareLessThan          (Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<ushort> CompareLessThanOrEqual   (Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<ushort> CompareNotEqual          (Vector128<ushort> left, Vector128<ushort> right);
        public static Vector256<ushort> CompareGreaterThan       (Vector256<ushort> left, Vector256<ushort> right);
        public static Vector256<ushort> CompareGreaterThanOrEqual(Vector256<ushort> left, Vector256<ushort> right);
        public static Vector256<ushort> CompareLessThan          (Vector256<ushort> left, Vector256<ushort> right);
        public static Vector256<ushort> CompareLessThanOrEqual   (Vector256<ushort> left, Vector256<ushort> right);
        public static Vector256<ushort> CompareNotEqual          (Vector256<ushort> left, Vector256<ushort> right);
    }
}

public static partial class Avx512DQ
{
    public static Vector512<double> Classify(Vector512<double> value, [ConstantExpected] byte control);
    public static Vector512<float>  Classify(Vector512<float>  value, [ConstantExpected] byte control);

    public static Vector128<double> ClassifyScalar(Vector128<double> value, [ConstantExpected] byte control);
    public static Vector128<float>  ClassifyScalar(Vector128<float>  value, [ConstantExpected] byte control);

    public static int MoveMask(Vector128<short>  value);
    public static int MoveMask(Vector128<ushort> value);
    public static int MoveMask(Vector256<int>    value);
    public static int MoveMask(Vector256<uint>   value);
    public static int MoveMask(Vector512<double> value);
    public static int MoveMask(Vector512<long>   value);
    public static int MoveMask(Vector512<ulong>  value);

    public static partial class VL
    {
        public static Vector128<double> Classify(Vector128<double> value, [ConstantExpected] byte control);
        public static Vector128<float>  Classify(Vector128<float>  value, [ConstantExpected] byte control);
        public static Vector256<double> Classify(Vector256<double> value, [ConstantExpected] byte control);
        public static Vector256<float>  Classify(Vector256<float>  value, [ConstantExpected] byte control);
    }
}

public abstract class Avx512Vbmi2 : Avx512BW
{
    public static new bool IsSupported { get; }

    public static Vector512<byte>   Compress(Vector512<byte>   value, Vector512<byte>   mask);
    public static Vector512<short>  Compress(Vector512<short>  value, Vector512<short>  mask);
    public static Vector512<sbyte>  Compress(Vector512<sbyte>  value, Vector512<sbyte>  mask);
    public static Vector512<ushort> Compress(Vector512<ushort> value, Vector512<ushort> mask);

    public static Vector512<byte>   Expand(Vector512<byte>   value, Vector512<byte>   mask);
    public static Vector512<short>  Expand(Vector512<short>  value, Vector512<short>  mask);
    public static Vector512<sbyte>  Expand(Vector512<sbyte>  value, Vector512<sbyte>  mask);
    public static Vector512<ushort> Expand(Vector512<ushort> value, Vector512<ushort> mask);

    public abstract class VL : Avx512BW.VL
    {
        public static new bool IsSupported { get; }

        public static Vector128<byte>   Compress(Vector128<byte>   value, Vector128<byte>   mask);
        public static Vector128<short>  Compress(Vector128<short>  value, Vector128<short>  mask);
        public static Vector128<sbyte>  Compress(Vector128<sbyte>  value, Vector128<sbyte>  mask);
        public static Vector128<ushort> Compress(Vector128<ushort> value, Vector128<ushort> mask);
        public static Vector256<byte>   Compress(Vector256<byte>   value, Vector256<byte>   mask);
        public static Vector256<short>  Compress(Vector256<short>  value, Vector256<short>  mask);
        public static Vector256<sbyte>  Compress(Vector256<sbyte>  value, Vector256<sbyte>  mask);
        public static Vector256<ushort> Compress(Vector256<ushort> value, Vector256<ushort> mask);

        public static Vector128<byte>   Expand(Vector128<byte>   value, Vector128<byte>   mask);
        public static Vector128<short>  Expand(Vector128<short>  value, Vector128<short>  mask);
        public static Vector128<sbyte>  Expand(Vector128<sbyte>  value, Vector128<sbyte>  mask);
        public static Vector128<ushort> Expand(Vector128<ushort> value, Vector128<ushort> mask);
        public static Vector256<byte>   Expand(Vector256<byte>   value, Vector256<byte>   mask);
        public static Vector256<short>  Expand(Vector256<short>  value, Vector256<short>  mask);
        public static Vector256<sbyte>  Expand(Vector256<sbyte>  value, Vector256<sbyte>  mask);
        public static Vector256<ushort> Expand(Vector256<ushort> value, Vector256<ushort> mask);
    }

    public abstract class X64 : Avx512BW.X64
    {
        public static new bool IsSupported { get; }
    }
}
```
## Arm64 LoadVector64 and LoadVector128 for 2,3 and 4 variants

**Approved** | [#runtime/84510](https://github.com/dotnet/runtime/issues/84510#issuecomment-1579262376) | [Video](https://www.youtube.com/watch?v=qHcX5_hlMRU&t=1h7m36s)

* Looks good as proposed -- `Store..AndUnzip` was fixed to be `Store...AndZip`

```C#
namespace System.Runtime.Intrinsics.Arm;

public abstract partial class AdvSimd
{
    public static unsafe (Vector64<byte>   Value1, Vector64<byte>   Value2) LoadVector64x2AndUnzip(byte*   address);
    public static unsafe (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2) LoadVector64x2AndUnzip(sbyte*  address);
    public static unsafe (Vector64<short>  Value1, Vector64<short>  Value2) LoadVector64x2AndUnzip(short*  address);
    public static unsafe (Vector64<ushort> Value1, Vector64<ushort> Value2) LoadVector64x2AndUnzip(ushort* address);
    public static unsafe (Vector64<int>    Value1, Vector64<int>    Value2) LoadVector64x2AndUnzip(int*    address);
    public static unsafe (Vector64<uint>   Value1, Vector64<uint>   Value2) LoadVector64x2AndUnzip(uint*   address);
    public static unsafe (Vector64<float>  Value1, Vector64<float>  Value2) LoadVector64x2AndUnzip(float*  address);

    public static unsafe (Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3) LoadVector64x3AndUnzip(byte*   address);
    public static unsafe (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3) LoadVector64x3AndUnzip(sbyte*  address);
    public static unsafe (Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3) LoadVector64x3AndUnzip(short*  address);
    public static unsafe (Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3) LoadVector64x3AndUnzip(ushort* address);
    public static unsafe (Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3) LoadVector64x3AndUnzip(int*    address);
    public static unsafe (Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3) LoadVector64x3AndUnzip(uint*   address);
    public static unsafe (Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3) LoadVector64x3AndUnzip(float*  address);
    
    public static unsafe (Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3, Vector64<byte>   Value4) LoadVector64x4AndUnzip(byte*   address);
    public static unsafe (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3, Vector64<sbyte>  Value4) LoadVector64x4AndUnzip(sbyte*  address);
    public static unsafe (Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3, Vector64<short>  Value4) LoadVector64x4AndUnzip(short*  address);
    public static unsafe (Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3, Vector64<ushort> Value4) LoadVector64x4AndUnzip(ushort* address);
    public static unsafe (Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3, Vector64<int>    Value4) LoadVector64x4AndUnzip(int*    address);
    public static unsafe (Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3, Vector64<uint>   Value4) LoadVector64x4AndUnzip(uint*   address);
    public static unsafe (Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3, Vector64<float>  Value4) LoadVector64x4AndUnzip(float*  address);
    
    public static unsafe (Vector64<byte>   Value1, Vector64<byte>   Value2) LoadVector64x2(byte*   address);
    public static unsafe (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2) LoadVector64x2(sbyte*  address);
    public static unsafe (Vector64<short>  Value1, Vector64<short>  Value2) LoadVector64x2(short*  address);
    public static unsafe (Vector64<ushort> Value1, Vector64<ushort> Value2) LoadVector64x2(ushort* address);
    public static unsafe (Vector64<int>    Value1, Vector64<int>    Value2) LoadVector64x2(int*    address);
    public static unsafe (Vector64<uint>   Value1, Vector64<uint>   Value2) LoadVector64x2(uint*   address);
    public static unsafe (Vector64<float>  Value1, Vector64<float>  Value2) LoadVector64x2(float*  address);

    public static unsafe (Vector64<byte>   Value1, Vector64<byte>   Value2) LoadVectorAndInsertScalar64x2((Vector64<byte>   Value1, Vector64<byte>   Value2) value, byte index, byte*   address);
    public static unsafe (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2) LoadVectorAndInsertScalar64x2((Vector64<sbyte>  Value1, Vector64<sbyte>  Value2) value, byte index, sbyte*  address);
    public static unsafe (Vector64<short>  Value1, Vector64<short>  Value2) LoadVectorAndInsertScalar64x2((Vector64<short>  Value1, Vector64<short>  Value2) value, byte index, short*  address);
    public static unsafe (Vector64<ushort> Value1, Vector64<ushort> Value2) LoadVectorAndInsertScalar64x2((Vector64<ushort> Value1, Vector64<ushort> Value2) value, byte index, ushort* address);
    public static unsafe (Vector64<int>    Value1, Vector64<int>    Value2) LoadVectorAndInsertScalar64x2((Vector64<int>    Value1, Vector64<int>    Value2) value, byte index, int*    address);
    public static unsafe (Vector64<uint>   Value1, Vector64<uint>   Value2) LoadVectorAndInsertScalar64x2((Vector64<uint>   Value1, Vector64<uint>   Value2) value, byte index, uint*   address);
    public static unsafe (Vector64<float>  Value1, Vector64<float>  Value2) LoadVectorAndInsertScalar64x2((Vector64<float>  Value1, Vector64<float>  Value2) value, byte index, float*  address);

    public static unsafe (Vector64<byte>   Value1, Vector64<byte>   Value2) LoadAndReplicateToVector64x2(byte*   address);
    public static unsafe (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2) LoadAndReplicateToVector64x2(sbyte*  address);
    public static unsafe (Vector64<short>  Value1, Vector64<short>  Value2) LoadAndReplicateToVector64x2(short*  address);
    public static unsafe (Vector64<ushort> Value1, Vector64<ushort> Value2) LoadAndReplicateToVector64x2(ushort* address);
    public static unsafe (Vector64<int>    Value1, Vector64<int>    Value2) LoadAndReplicateToVector64x2(int*    address);
    public static unsafe (Vector64<uint>   Value1, Vector64<uint>   Value2) LoadAndReplicateToVector64x2(uint*   address);
    public static unsafe (Vector64<float>  Value1, Vector64<float>  Value2) LoadAndReplicateToVector64x2(float*  address);

    public static unsafe (Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3) LoadVector64x3(byte*   address);
    public static unsafe (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3) LoadVector64x3(sbyte*  address);
    public static unsafe (Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3) LoadVector64x3(short*  address);
    public static unsafe (Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3) LoadVector64x3(ushort* address);
    public static unsafe (Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3) LoadVector64x3(int*    address);
    public static unsafe (Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3) LoadVector64x3(uint*   address);
    public static unsafe (Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3) LoadVector64x3(float*  address);

    public static unsafe (Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3) LoadVectorAndInsertScalar64x3((Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3) value, byte index, byte*   address);
    public static unsafe (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3) LoadVectorAndInsertScalar64x3((Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3) value, byte index, sbyte*  address);
    public static unsafe (Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3) LoadVectorAndInsertScalar64x3((Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3) value, byte index, short*  address);
    public static unsafe (Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3) LoadVectorAndInsertScalar64x3((Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3) value, byte index, ushort* address);
    public static unsafe (Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3) LoadVectorAndInsertScalar64x3((Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3) value, byte index, int*    address);
    public static unsafe (Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3) LoadVectorAndInsertScalar64x3((Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3) value, byte index, uint*   address);
    public static unsafe (Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3) LoadVectorAndInsertScalar64x3((Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3) value, byte index, float*  address);

    public static unsafe (Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3) LoadAndReplicateToVector64x3(byte*   address);
    public static unsafe (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3) LoadAndReplicateToVector64x3(sbyte*  address);
    public static unsafe (Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3) LoadAndReplicateToVector64x3(short*  address);
    public static unsafe (Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3) LoadAndReplicateToVector64x3(ushort* address);
    public static unsafe (Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3) LoadAndReplicateToVector64x3(int*    address);
    public static unsafe (Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3) LoadAndReplicateToVector64x3(uint*   address);
    public static unsafe (Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3) LoadAndReplicateToVector64x3(float*  address);

    public static unsafe (Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3, Vector64<byte>   Value4) LoadVector64x4(byte*   address);
    public static unsafe (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3, Vector64<sbyte>  Value4) LoadVector64x4(sbyte*  address);
    public static unsafe (Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3, Vector64<short>  Value4) LoadVector64x4(short*  address);
    public static unsafe (Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3, Vector64<ushort> Value4) LoadVector64x4(ushort* address);
    public static unsafe (Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3, Vector64<int>    Value4) LoadVector64x4(int*    address);
    public static unsafe (Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3, Vector64<uint>   Value4) LoadVector64x4(uint*   address);
    public static unsafe (Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3, Vector64<float>  Value4) LoadVector64x4(float*  address);

    public static unsafe (Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3, Vector64<byte>   Value4) LoadVectorAndInsertScalar64x4((Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3, Vector64<byte>   Value4) value, byte index, byte*   address);
    public static unsafe (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3, Vector64<sbyte>  Value4) LoadVectorAndInsertScalar64x4((Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3, Vector64<sbyte>  Value4) value, byte index, sbyte*  address);
    public static unsafe (Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3, Vector64<short>  Value4) LoadVectorAndInsertScalar64x4((Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3, Vector64<short>  Value4) value, byte index, short*  address);
    public static unsafe (Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3, Vector64<ushort> Value4) LoadVectorAndInsertScalar64x4((Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3, Vector64<ushort> Value4) value, byte index, ushort* address);
    public static unsafe (Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3, Vector64<int>    Value4) LoadVectorAndInsertScalar64x4((Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3, Vector64<int>    Value4) value, byte index, int*    address);
    public static unsafe (Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3, Vector64<uint>   Value4) LoadVectorAndInsertScalar64x4((Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3, Vector64<uint>   Value4) value, byte index, uint*   address);
    public static unsafe (Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3, Vector64<float>  Value4) LoadVectorAndInsertScalar64x4((Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3, Vector64<float>  Value4) value, byte index, float*  address);

    public static unsafe (Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3, Vector64<byte>   Value4) LoadAndReplicateToVector64x4(byte*   address);
    public static unsafe (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3, Vector64<sbyte>  Value4) LoadAndReplicateToVector64x4(sbyte*  address);
    public static unsafe (Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3, Vector64<short>  Value4) LoadAndReplicateToVector64x4(short*  address);
    public static unsafe (Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3, Vector64<ushort> Value4) LoadAndReplicateToVector64x4(ushort* address);
    public static unsafe (Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3, Vector64<int>    Value4) LoadAndReplicateToVector64x4(int*    address);
    public static unsafe (Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3, Vector64<uint>   Value4) LoadAndReplicateToVector64x4(uint*   address);
    public static unsafe (Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3, Vector64<float>  Value4) LoadAndReplicateToVector64x4(float*  address);

    public static unsafe void StoreVector64x2AndZip(byte*   address, (Vector64<byte>   Value1, Vector64<byte>   Value2) value);
    public static unsafe void StoreVector64x2AndZip(sbyte*  address, (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2) value);
    public static unsafe void StoreVector64x2AndZip(short*  address, (Vector64<short>  Value1, Vector64<short>  Value2) value);
    public static unsafe void StoreVector64x2AndZip(ushort* address, (Vector64<ushort> Value1, Vector64<ushort> Value2) value);
    public static unsafe void StoreVector64x2AndZip(int*    address, (Vector64<int>    Value1, Vector64<int>    Value2) value);
    public static unsafe void StoreVector64x2AndZip(uint*   address, (Vector64<uint>   Value1, Vector64<uint>   Value2) value);
    public static unsafe void StoreVector64x2AndZip(float*  address, (Vector64<float>  Value1, Vector64<float>  Value2) value);

    public static unsafe void StoreVector64x3AndZip(byte*   address, (Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3) value);
    public static unsafe void StoreVector64x3AndZip(sbyte*  address, (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3) value);
    public static unsafe void StoreVector64x3AndZip(short*  address, (Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3) value);
    public static unsafe void StoreVector64x3AndZip(ushort* address, (Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3) value);
    public static unsafe void StoreVector64x3AndZip(int*    address, (Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3) value);
    public static unsafe void StoreVector64x3AndZip(uint*   address, (Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3) value);
    public static unsafe void StoreVector64x3AndZip(float*  address, (Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3) value);
    
    public static unsafe void StoreVector64x4AndZip(byte*   address, (Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3, Vector64<byte>   Value4) value);
    public static unsafe void StoreVector64x4AndZip(sbyte*  address, (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3, Vector64<sbyte>  Value4) value);
    public static unsafe void StoreVector64x4AndZip(short*  address, (Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3, Vector64<short>  Value4) value);
    public static unsafe void StoreVector64x4AndZip(ushort* address, (Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3, Vector64<ushort> Value4) value);
    public static unsafe void StoreVector64x4AndZip(int*    address, (Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3, Vector64<int>    Value4) value);
    public static unsafe void StoreVector64x4AndZip(uint*   address, (Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3, Vector64<uint>   Value4) value);
    public static unsafe void StoreVector64x4AndZip(float*  address, (Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3, Vector64<float>  Value4) value);
    
    public static unsafe void StoreVector64x2(byte*   address, (Vector64<byte>   Value1, Vector64<byte>   Value2) value);
    public static unsafe void StoreVector64x2(sbyte*  address, (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2) value);
    public static unsafe void StoreVector64x2(short*  address, (Vector64<short>  Value1, Vector64<short>  Value2) value);
    public static unsafe void StoreVector64x2(ushort* address, (Vector64<ushort> Value1, Vector64<ushort> Value2) value);
    public static unsafe void StoreVector64x2(int*    address, (Vector64<int>    Value1, Vector64<int>    Value2) value);
    public static unsafe void StoreVector64x2(uint*   address, (Vector64<uint>   Value1, Vector64<uint>   Value2) value);
    public static unsafe void StoreVector64x2(float*  address, (Vector64<float>  Value1, Vector64<float>  Value2) value);

    public static unsafe void StoreSelectedScalar64x2(byte*   address, (Vector64<byte>   Value1, Vector64<byte>   Value2) value, byte index);
    public static unsafe void StoreSelectedScalar64x2(sbyte*  address, (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2) value, byte index);
    public static unsafe void StoreSelectedScalar64x2(short*  address, (Vector64<short>  Value1, Vector64<short>  Value2) value, byte index);
    public static unsafe void StoreSelectedScalar64x2(ushort* address, (Vector64<ushort> Value1, Vector64<ushort> Value2) value, byte index);
    public static unsafe void StoreSelectedScalar64x2(int*    address, (Vector64<int>    Value1, Vector64<int>    Value2) value, byte index);
    public static unsafe void StoreSelectedScalar64x2(uint*   address, (Vector64<uint>   Value1, Vector64<uint>   Value2) value, byte index);
    public static unsafe void StoreSelectedScalar64x2(float*  address, (Vector64<float>  Value1, Vector64<float>  Value2) value, byte index);

    public static unsafe void StoreVector64x3(byte*   address, (Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3) value);
    public static unsafe void StoreVector64x3(sbyte*  address, (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3) value);
    public static unsafe void StoreVector64x3(short*  address, (Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3) value);
    public static unsafe void StoreVector64x3(ushort* address, (Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3) value);
    public static unsafe void StoreVector64x3(int*    address, (Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3) value);
    public static unsafe void StoreVector64x3(uint*   address, (Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3) value);
    public static unsafe void StoreVector64x3(float*  address, (Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3) value);

    public static unsafe void StoreSelectedScalar64x3(byte*   address, (Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3) value, byte index);
    public static unsafe void StoreSelectedScalar64x3(sbyte*  address, (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3) value, byte index);
    public static unsafe void StoreSelectedScalar64x3(short*  address, (Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3) value, byte index);
    public static unsafe void StoreSelectedScalar64x3(ushort* address, (Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3) value, byte index);
    public static unsafe void StoreSelectedScalar64x3(int*    address, (Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3) value, byte index);
    public static unsafe void StoreSelectedScalar64x3(uint*   address, (Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3) value, byte index);
    public static unsafe void StoreSelectedScalar64x3(float*  address, (Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3) value, byte index);

    public static unsafe void StoreVector64x4(byte*   address, (Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3, Vector64<byte>   Value4) value);
    public static unsafe void StoreVector64x4(sbyte*  address, (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3, Vector64<sbyte>  Value4) value);
    public static unsafe void StoreVector64x4(short*  address, (Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3, Vector64<short>  Value4) value);
    public static unsafe void StoreVector64x4(ushort* address, (Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3, Vector64<ushort> Value4) value);
    public static unsafe void StoreVector64x4(int*    address, (Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3, Vector64<int>    Value4) value);
    public static unsafe void StoreVector64x4(uint*   address, (Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3, Vector64<uint>   Value4) value);
    public static unsafe void StoreVector64x4(float*  address, (Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3, Vector64<float>  Value4) value);

    public static unsafe void StoreSelectedScalar64x4(byte*   address, (Vector64<byte>   Value1, Vector64<byte>   Value2, Vector64<byte>   Value3, Vector64<byte>   Value4) value, byte index);
    public static unsafe void StoreSelectedScalar64x4(sbyte*  address, (Vector64<sbyte>  Value1, Vector64<sbyte>  Value2, Vector64<sbyte>  Value3, Vector64<sbyte>  Value4) value, byte index);
    public static unsafe void StoreSelectedScalar64x4(short*  address, (Vector64<short>  Value1, Vector64<short>  Value2, Vector64<short>  Value3, Vector64<short>  Value4) value, byte index);
    public static unsafe void StoreSelectedScalar64x4(ushort* address, (Vector64<ushort> Value1, Vector64<ushort> Value2, Vector64<ushort> Value3, Vector64<ushort> Value4) value, byte index);
    public static unsafe void StoreSelectedScalar64x4(int*    address, (Vector64<int>    Value1, Vector64<int>    Value2, Vector64<int>    Value3, Vector64<int>    Value4) value, byte index);
    public static unsafe void StoreSelectedScalar64x4(uint*   address, (Vector64<uint>   Value1, Vector64<uint>   Value2, Vector64<uint>   Value3, Vector64<uint>   Value4) value, byte index);
    public static unsafe void StoreSelectedScalar64x4(float*  address, (Vector64<float>  Value1, Vector64<float>  Value2, Vector64<float>  Value3, Vector64<float>  Value4) value, byte index);

    public partial class Arm64
    {
        public static unsafe (Vector128<byte>   Value1, Vector128<byte>   Value2) LoadVector128x2AndUnzip(byte*   address);
        public static unsafe (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2) LoadVector128x2AndUnzip(sbyte*  address);
        public static unsafe (Vector128<short>  Value1, Vector128<short>  Value2) LoadVector128x2AndUnzip(short*  address);
        public static unsafe (Vector128<ushort> Value1, Vector128<ushort> Value2) LoadVector128x2AndUnzip(ushort* address);
        public static unsafe (Vector128<int>    Value1, Vector128<int>    Value2) LoadVector128x2AndUnzip(int*    address);
        public static unsafe (Vector128<uint>   Value1, Vector128<uint>   Value2) LoadVector128x2AndUnzip(uint*   address);
        public static unsafe (Vector128<long>   Value1, Vector128<long>   Value2) LoadVector128x2AndUnzip(long*   address);
        public static unsafe (Vector128<ulong>  Value1, Vector128<ulong>  Value2) LoadVector128x2AndUnzip(ulong*  address);
        public static unsafe (Vector128<float>  Value1, Vector128<float>  Value2) LoadVector128x2AndUnzip(float*  address);
        public static unsafe (Vector128<double> Value1, Vector128<double> Value2) LoadVector128x2AndUnzip(double* address);

        public static unsafe (Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3) LoadVector128x3AndUnzip(byte*   address);
        public static unsafe (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3) LoadVector128x3AndUnzip(sbyte*  address);
        public static unsafe (Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3) LoadVector128x3AndUnzip(short*  address);
        public static unsafe (Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3) LoadVector128x3AndUnzip(ushort* address);
        public static unsafe (Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3) LoadVector128x3AndUnzip(int*    address);
        public static unsafe (Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3) LoadVector128x3AndUnzip(uint*   address);
        public static unsafe (Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3) LoadVector128x3AndUnzip(long*   address);
        public static unsafe (Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3) LoadVector128x3AndUnzip(ulong*  address);
        public static unsafe (Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3) LoadVector128x3AndUnzip(float*  address);
        public static unsafe (Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3) LoadVector128x3AndUnzip(double* address);

        public static unsafe (Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3, Vector128<byte>   Value4) LoadVector128x4AndUnzip(byte*   address);
        public static unsafe (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3, Vector128<sbyte>  Value4) LoadVector128x4AndUnzip(sbyte*  address);
        public static unsafe (Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3, Vector128<short>  Value4) LoadVector128x4AndUnzip(short*  address);
        public static unsafe (Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3, Vector128<ushort> Value4) LoadVector128x4AndUnzip(ushort* address);
        public static unsafe (Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3, Vector128<int>    Value4) LoadVector128x4AndUnzip(int*    address);
        public static unsafe (Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3, Vector128<uint>   Value4) LoadVector128x4AndUnzip(uint*   address);
        public static unsafe (Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3, Vector128<long>   Value4) LoadVector128x4AndUnzip(long*   address);
        public static unsafe (Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3, Vector128<ulong>  Value4) LoadVector128x4AndUnzip(ulong*  address);
        public static unsafe (Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3, Vector128<float>  Value4) LoadVector128x4AndUnzip(float*  address);
        public static unsafe (Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3, Vector128<double> Value4) LoadVector128x4AndUnzip(double* address);

        public static unsafe (Vector128<byte>   Value1, Vector128<byte>   Value2) LoadVector128x2(byte*   address);
        public static unsafe (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2) LoadVector128x2(sbyte*  address);
        public static unsafe (Vector128<short>  Value1, Vector128<short>  Value2) LoadVector128x2(short*  address);
        public static unsafe (Vector128<ushort> Value1, Vector128<ushort> Value2) LoadVector128x2(ushort* address);
        public static unsafe (Vector128<int>    Value1, Vector128<int>    Value2) LoadVector128x2(int*    address);
        public static unsafe (Vector128<uint>   Value1, Vector128<uint>   Value2) LoadVector128x2(uint*   address);
        public static unsafe (Vector128<long>   Value1, Vector128<long>   Value2) LoadVector128x2(long*   address);
        public static unsafe (Vector128<ulong>  Value1, Vector128<ulong>  Value2) LoadVector128x2(ulong*  address);
        public static unsafe (Vector128<float>  Value1, Vector128<float>  Value2) LoadVector128x2(float*  address);
        public static unsafe (Vector128<double> Value1, Vector128<double> Value2) LoadVector128x2(double* address);


        public static unsafe (Vector128<byte>   Value1, Vector128<byte>   Value2) LoadVectorAndInsertScalar128x2((Vector128<byte>   Value1, Vector128<byte>   Value2) value, byte index, byte*   address);
        public static unsafe (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2) LoadVectorAndInsertScalar128x2((Vector128<sbyte>  Value1, Vector128<sbyte>  Value2) value, byte index, sbyte*  address);
        public static unsafe (Vector128<short>  Value1, Vector128<short>  Value2) LoadVectorAndInsertScalar128x2((Vector128<short>  Value1, Vector128<short>  Value2) value, byte index, short*  address);
        public static unsafe (Vector128<ushort> Value1, Vector128<ushort> Value2) LoadVectorAndInsertScalar128x2((Vector128<ushort> Value1, Vector128<ushort> Value2) value, byte index, ushort* address);
        public static unsafe (Vector128<int>    Value1, Vector128<int>    Value2) LoadVectorAndInsertScalar128x2((Vector128<int>    Value1, Vector128<int>    Value2) value, byte index, int*    address);
        public static unsafe (Vector128<uint>   Value1, Vector128<uint>   Value2) LoadVectorAndInsertScalar128x2((Vector128<uint>   Value1, Vector128<uint>   Value2) value, byte index, uint*   address);
        public static unsafe (Vector128<long>   Value1, Vector128<long>   Value2) LoadVectorAndInsertScalar128x2((Vector128<long>   Value1, Vector128<long>   Value2) value, byte index, long*   address);
        public static unsafe (Vector128<ulong>  Value1, Vector128<ulong>  Value2) LoadVectorAndInsertScalar128x2((Vector128<ulong>  Value1, Vector128<ulong>  Value2) value, byte index, ulong*  address);
        public static unsafe (Vector128<float>  Value1, Vector128<float>  Value2) LoadVectorAndInsertScalar128x2((Vector128<float>  Value1, Vector128<float>  Value2) value, byte index, float*  address);
        public static unsafe (Vector128<double> Value1, Vector128<double> Value2) LoadVectorAndInsertScalar128x2((Vector128<double> Value1, Vector128<double> Value2) value, byte index, double* address);

        public static unsafe (Vector128<byte>   Value1, Vector128<byte>   Value2) LoadAndReplicateToVector128x2(byte*   address);
        public static unsafe (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2) LoadAndReplicateToVector128x2(sbyte*  address);
        public static unsafe (Vector128<short>  Value1, Vector128<short>  Value2) LoadAndReplicateToVector128x2(short*  address);
        public static unsafe (Vector128<ushort> Value1, Vector128<ushort> Value2) LoadAndReplicateToVector128x2(ushort* address);
        public static unsafe (Vector128<int>    Value1, Vector128<int>    Value2) LoadAndReplicateToVector128x2(int*    address);
        public static unsafe (Vector128<uint>   Value1, Vector128<uint>   Value2) LoadAndReplicateToVector128x2(uint*   address);
        public static unsafe (Vector128<long>   Value1, Vector128<long>   Value2) LoadAndReplicateToVector128x2(long*   address);
        public static unsafe (Vector128<ulong>  Value1, Vector128<ulong>  Value2) LoadAndReplicateToVector128x2(ulong*  address);
        public static unsafe (Vector128<float>  Value1, Vector128<float>  Value2) LoadAndReplicateToVector128x2(float*  address);
        public static unsafe (Vector128<double> Value1, Vector128<double> Value2) LoadAndReplicateToVector128x2(double* address);

        public static unsafe (Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3) LoadVector128x3(byte*   address);
        public static unsafe (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3) LoadVector128x3(sbyte*  address);
        public static unsafe (Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3) LoadVector128x3(short*  address);
        public static unsafe (Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3) LoadVector128x3(ushort* address);
        public static unsafe (Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3) LoadVector128x3(int*    address);
        public static unsafe (Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3) LoadVector128x3(uint*   address);
        public static unsafe (Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3) LoadVector128x3(long*   address);
        public static unsafe (Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3) LoadVector128x3(ulong*  address);
        public static unsafe (Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3) LoadVector128x3(float*  address);
        public static unsafe (Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3) LoadVector128x3(double* address);

        public static unsafe (Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3) LoadVectorAndInsertScalar128x3((Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3) value, byte index, byte*   address);
        public static unsafe (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3) LoadVectorAndInsertScalar128x3((Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3) value, byte index, sbyte*  address);
        public static unsafe (Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3) LoadVectorAndInsertScalar128x3((Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3) value, byte index, short*  address);
        public static unsafe (Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3) LoadVectorAndInsertScalar128x3((Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3) value, byte index, ushort* address);
        public static unsafe (Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3) LoadVectorAndInsertScalar128x3((Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3) value, byte index, int*    address);
        public static unsafe (Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3) LoadVectorAndInsertScalar128x3((Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3) value, byte index, uint*   address);
        public static unsafe (Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3) LoadVectorAndInsertScalar128x3((Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3) value, byte index, long*   address);
        public static unsafe (Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3) LoadVectorAndInsertScalar128x3((Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3) value, byte index, ulong*  address);
        public static unsafe (Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3) LoadVectorAndInsertScalar128x3((Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3) value, byte index, float*  address);
        public static unsafe (Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3) LoadVectorAndInsertScalar128x3((Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3) value, byte index, double* address);

        public static unsafe (Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3) LoadAndReplicateToVector128x3(byte*   address);
        public static unsafe (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3) LoadAndReplicateToVector128x3(sbyte*  address);
        public static unsafe (Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3) LoadAndReplicateToVector128x3(short*  address);
        public static unsafe (Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3) LoadAndReplicateToVector128x3(ushort* address);
        public static unsafe (Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3) LoadAndReplicateToVector128x3(int*    address);
        public static unsafe (Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3) LoadAndReplicateToVector128x3(uint*   address);
        public static unsafe (Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3) LoadAndReplicateToVector128x3(long*   address);
        public static unsafe (Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3) LoadAndReplicateToVector128x3(ulong*  address);
        public static unsafe (Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3) LoadAndReplicateToVector128x3(float*  address);
        public static unsafe (Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3) LoadAndReplicateToVector128x3(double* address);

        public static unsafe (Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3, Vector128<byte>   Value4) LoadVector128x4(byte*   address);
        public static unsafe (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3, Vector128<sbyte>  Value4) LoadVector128x4(sbyte*  address);
        public static unsafe (Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3, Vector128<short>  Value4) LoadVector128x4(short*  address);
        public static unsafe (Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3, Vector128<ushort> Value4) LoadVector128x4(ushort* address);
        public static unsafe (Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3, Vector128<int>    Value4) LoadVector128x4(int*    address);
        public static unsafe (Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3, Vector128<uint>   Value4) LoadVector128x4(uint*   address);
        public static unsafe (Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3, Vector128<long>   Value4) LoadVector128x4(long*   address);
        public static unsafe (Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3, Vector128<ulong>  Value4) LoadVector128x4(ulong*  address);
        public static unsafe (Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3, Vector128<float>  Value4) LoadVector128x4(float*  address);
        public static unsafe (Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3, Vector128<double> Value4) LoadVector128x4(double* address);

        public static unsafe (Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3, Vector128<byte>   Value4) LoadVectorAndInsertScalar128x4((Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3, Vector128<byte>   Value4) value, byte index, byte*   address);
        public static unsafe (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3, Vector128<sbyte>  Value4) LoadVectorAndInsertScalar128x4((Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3, Vector128<sbyte>  Value4) value, byte index, sbyte*  address);
        public static unsafe (Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3, Vector128<short>  Value4) LoadVectorAndInsertScalar128x4((Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3, Vector128<short>  Value4) value, byte index, short*  address);
        public static unsafe (Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3, Vector128<ushort> Value4) LoadVectorAndInsertScalar128x4((Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3, Vector128<ushort> Value4) value, byte index, ushort* address);
        public static unsafe (Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3, Vector128<int>    Value4) LoadVectorAndInsertScalar128x4((Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3, Vector128<int>    Value4) value, byte index, int*    address);
        public static unsafe (Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3, Vector128<uint>   Value4) LoadVectorAndInsertScalar128x4((Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3, Vector128<uint>   Value4) value, byte index, uint*   address);
        public static unsafe (Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3, Vector128<long>   Value4) LoadVectorAndInsertScalar128x4((Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3, Vector128<long>   Value4) value, byte index, long*   address);
        public static unsafe (Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3, Vector128<ulong>  Value4) LoadVectorAndInsertScalar128x4((Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3, Vector128<ulong>  Value4) value, byte index, ulong*  address);
        public static unsafe (Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3, Vector128<float>  Value4) LoadVectorAndInsertScalar128x4((Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3, Vector128<float>  Value4) value, byte index, float*  address);
        public static unsafe (Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3, Vector128<double> Value4) LoadVectorAndInsertScalar128x4((Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3, Vector128<double> Value4) value, byte index, double* address);


        public static unsafe(Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3, Vector128<byte>   Value4) LoadAndReplicateToVector128x4(byte*   address);
        public static unsafe(Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3, Vector128<sbyte>  Value4) LoadAndReplicateToVector128x4(sbyte*  address);
        public static unsafe(Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3, Vector128<short>  Value4) LoadAndReplicateToVector128x4(short*  address);
        public static unsafe(Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3, Vector128<ushort> Value4) LoadAndReplicateToVector128x4(ushort* address);
        public static unsafe(Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3, Vector128<int>    Value4) LoadAndReplicateToVector128x4(int*    address);
        public static unsafe(Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3, Vector128<uint>   Value4) LoadAndReplicateToVector128x4(uint*   address);
        public static unsafe(Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3, Vector128<long>   Value4) LoadAndReplicateToVector128x4(long*   address);
        public static unsafe(Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3, Vector128<ulong>  Value4) LoadAndReplicateToVector128x4(ulong*  address);
        public static unsafe(Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3, Vector128<float>  Value4) LoadAndReplicateToVector128x4(float*  address);
        public static unsafe(Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3, Vector128<double> Value4) LoadAndReplicateToVector128x4(double* address);

        public static unsafe void StoreVector128x2AndZip(byte*   address, (Vector128<byte>   Value1, Vector128<byte>   Value2) value);
        public static unsafe void StoreVector128x2AndZip(sbyte*  address, (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2) value);
        public static unsafe void StoreVector128x2AndZip(short*  address, (Vector128<short>  Value1, Vector128<short>  Value2) value);
        public static unsafe void StoreVector128x2AndZip(ushort* address, (Vector128<ushort> Value1, Vector128<ushort> Value2) value);
        public static unsafe void StoreVector128x2AndZip(int*    address, (Vector128<int>    Value1, Vector128<int>    Value2) value);
        public static unsafe void StoreVector128x2AndZip(uint*   address, (Vector128<uint>   Value1, Vector128<uint>   Value2) value);
        public static unsafe void StoreVector128x2AndZip(long*   address, (Vector128<long>   Value1, Vector128<long>   Value2) value);
        public static unsafe void StoreVector128x2AndZip(ulong*  address, (Vector128<ulong>  Value1, Vector128<ulong>  Value2) value);
        public static unsafe void StoreVector128x2AndZip(float*  address, (Vector128<float>  Value1, Vector128<float>  Value2) value);
        public static unsafe void StoreVector128x2AndZip(double* address, (Vector128<double> Value1, Vector128<double> Value2) value);

        public static unsafe void StoreVector128x3AndZip(byte*   address, (Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3) value);
        public static unsafe void StoreVector128x3AndZip(sbyte*  address, (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3) value);
        public static unsafe void StoreVector128x3AndZip(short*  address, (Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3) value);
        public static unsafe void StoreVector128x3AndZip(ushort* address, (Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3) value);
        public static unsafe void StoreVector128x3AndZip(int*    address, (Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3) value);
        public static unsafe void StoreVector128x3AndZip(uint*   address, (Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3) value);
        public static unsafe void StoreVector128x3AndZip(long*   address, (Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3) value);
        public static unsafe void StoreVector128x3AndZip(ulong*  address, (Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3) value);
        public static unsafe void StoreVector128x3AndZip(float*  address, (Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3) value);
        public static unsafe void StoreVector128x3AndZip(double* address, (Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3) value);

        public static unsafe void StoreVector128x4AndZip(byte*   address, (Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3, Vector128<byte>   Value4) value);
        public static unsafe void StoreVector128x4AndZip(sbyte*  address, (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3, Vector128<sbyte>  Value4) value);
        public static unsafe void StoreVector128x4AndZip(short*  address, (Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3, Vector128<short>  Value4) value);
        public static unsafe void StoreVector128x4AndZip(ushort* address, (Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3, Vector128<ushort> Value4) value);
        public static unsafe void StoreVector128x4AndZip(int*    address, (Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3, Vector128<int>    Value4) value);
        public static unsafe void StoreVector128x4AndZip(uint*   address, (Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3, Vector128<uint>   Value4) value);
        public static unsafe void StoreVector128x4AndZip(long*   address, (Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3, Vector128<long>   Value4) value);
        public static unsafe void StoreVector128x4AndZip(ulong*  address, (Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3, Vector128<ulong>  Value4) value);
        public static unsafe void StoreVector128x4AndZip(float*  address, (Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3, Vector128<float>  Value4) value);
        public static unsafe void StoreVector128x4AndZip(double* address, (Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3, Vector128<double> Value4) value);

        public static unsafe void StoreVector128x2(byte*   address, (Vector128<byte>   Value1, Vector128<byte>   Value2) value);
        public static unsafe void StoreVector128x2(sbyte*  address, (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2) value);
        public static unsafe void StoreVector128x2(short*  address, (Vector128<short>  Value1, Vector128<short>  Value2) value);
        public static unsafe void StoreVector128x2(ushort* address, (Vector128<ushort> Value1, Vector128<ushort> Value2) value);
        public static unsafe void StoreVector128x2(int*    address, (Vector128<int>    Value1, Vector128<int>    Value2) value);
        public static unsafe void StoreVector128x2(uint*   address, (Vector128<uint>   Value1, Vector128<uint>   Value2) value);
        public static unsafe void StoreVector128x2(long*   address, (Vector128<long>   Value1, Vector128<long>   Value2) value);
        public static unsafe void StoreVector128x2(ulong*  address, (Vector128<ulong>  Value1, Vector128<ulong>  Value2) value);
        public static unsafe void StoreVector128x2(float*  address, (Vector128<float>  Value1, Vector128<float>  Value2) value);
        public static unsafe void StoreVector128x2(double* address, (Vector128<double> Value1, Vector128<double> Value2) value);

        public static unsafe void StoreSelectedScalar128x2(byte*   address, (Vector128<byte>   Value1, Vector128<byte>   Value2) value, byte index);
        public static unsafe void StoreSelectedScalar128x2(sbyte*  address, (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2) value, byte index);
        public static unsafe void StoreSelectedScalar128x2(short*  address, (Vector128<short>  Value1, Vector128<short>  Value2) value, byte index);
        public static unsafe void StoreSelectedScalar128x2(ushort* address, (Vector128<ushort> Value1, Vector128<ushort> Value2) value, byte index);
        public static unsafe void StoreSelectedScalar128x2(int*    address, (Vector128<int>    Value1, Vector128<int>    Value2) value, byte index);
        public static unsafe void StoreSelectedScalar128x2(uint*   address, (Vector128<uint>   Value1, Vector128<uint>   Value2) value, byte index);
        public static unsafe void StoreSelectedScalar128x2(long*   address, (Vector128<long>   Value1, Vector128<long>   Value2) value, byte index);
        public static unsafe void StoreSelectedScalar128x2(ulong*  address, (Vector128<ulong>  Value1, Vector128<ulong>  Value2) value, byte index);
        public static unsafe void StoreSelectedScalar128x2(float*  address, (Vector128<float>  Value1, Vector128<float>  Value2) value, byte index);
        public static unsafe void StoreSelectedScalar128x2(double* address, (Vector128<double> Value1, Vector128<double> Value2) value, byte index);

        public static unsafe void StoreVector128x3(byte*   address, (Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3) value);
        public static unsafe void StoreVector128x3(sbyte*  address, (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3) value);
        public static unsafe void StoreVector128x3(short*  address, (Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3) value);
        public static unsafe void StoreVector128x3(ushort* address, (Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3) value);
        public static unsafe void StoreVector128x3(int*    address, (Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3) value);
        public static unsafe void StoreVector128x3(uint*   address, (Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3) value);
        public static unsafe void StoreVector128x3(long*   address, (Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3) value);
        public static unsafe void StoreVector128x3(ulong*  address, (Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3) value);
        public static unsafe void StoreVector128x3(float*  address, (Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3) value);
        public static unsafe void StoreVector128x3(double* address, (Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3) value);

        public static unsafe void StoreSelectedScalar128x3(byte*   address, (Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3) value, byte index);
        public static unsafe void StoreSelectedScalar128x3(sbyte*  address, (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3) value, byte index);
        public static unsafe void StoreSelectedScalar128x3(short*  address, (Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3) value, byte index);
        public static unsafe void StoreSelectedScalar128x3(ushort* address, (Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3) value, byte index);
        public static unsafe void StoreSelectedScalar128x3(int*    address, (Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3) value, byte index);
        public static unsafe void StoreSelectedScalar128x3(uint*   address, (Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3) value, byte index);
        public static unsafe void StoreSelectedScalar128x3(long*   address, (Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3) value, byte index);
        public static unsafe void StoreSelectedScalar128x3(ulong*  address, (Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3) value, byte index);
        public static unsafe void StoreSelectedScalar128x3(float*  address, (Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3) value, byte index);
        public static unsafe void StoreSelectedScalar128x3(double* address, (Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3) value, byte index);

        public static unsafe void StoreVector128x4(byte*   address, (Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3, Vector128<byte>   Value4) value);
        public static unsafe void StoreVector128x4(sbyte*  address, (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3, Vector128<sbyte>  Value4) value);
        public static unsafe void StoreVector128x4(short*  address, (Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3, Vector128<short>  Value4) value);
        public static unsafe void StoreVector128x4(ushort* address, (Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3, Vector128<ushort> Value4) value);
        public static unsafe void StoreVector128x4(int*    address, (Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3, Vector128<int>    Value4) value);
        public static unsafe void StoreVector128x4(uint*   address, (Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3, Vector128<uint>   Value4) value);
        public static unsafe void StoreVector128x4(long*   address, (Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3, Vector128<long>   Value4) value);
        public static unsafe void StoreVector128x4(ulong*  address, (Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3, Vector128<ulong>  Value4) value);
        public static unsafe void StoreVector128x4(float*  address, (Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3, Vector128<float>  Value4) value);
        public static unsafe void StoreVector128x4(double* address, (Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3, Vector128<double> Value4) value);

        public static unsafe void StoreSelectedScalar128x4(byte*   address, (Vector128<byte>   Value1, Vector128<byte>   Value2, Vector128<byte>   Value3, Vector128<byte>   Value4) value, byte index);
        public static unsafe void StoreSelectedScalar128x4(sbyte*  address, (Vector128<sbyte>  Value1, Vector128<sbyte>  Value2, Vector128<sbyte>  Value3, Vector128<sbyte>  Value4) value, byte index);
        public static unsafe void StoreSelectedScalar128x4(short*  address, (Vector128<short>  Value1, Vector128<short>  Value2, Vector128<short>  Value3, Vector128<short>  Value4) value, byte index);
        public static unsafe void StoreSelectedScalar128x4(ushort* address, (Vector128<ushort> Value1, Vector128<ushort> Value2, Vector128<ushort> Value3, Vector128<ushort> Value4) value, byte index);
        public static unsafe void StoreSelectedScalar128x4(int*    address, (Vector128<int>    Value1, Vector128<int>    Value2, Vector128<int>    Value3, Vector128<int>    Value4) value, byte index);
        public static unsafe void StoreSelectedScalar128x4(uint*   address, (Vector128<uint>   Value1, Vector128<uint>   Value2, Vector128<uint>   Value3, Vector128<uint>   Value4) value, byte index);
        public static unsafe void StoreSelectedScalar128x4(long*   address, (Vector128<long>   Value1, Vector128<long>   Value2, Vector128<long>   Value3, Vector128<long>   Value4) value, byte index);
        public static unsafe void StoreSelectedScalar128x4(ulong*  address, (Vector128<ulong>  Value1, Vector128<ulong>  Value2, Vector128<ulong>  Value3, Vector128<ulong>  Value4) value, byte index);
        public static unsafe void StoreSelectedScalar128x4(float*  address, (Vector128<float>  Value1, Vector128<float>  Value2, Vector128<float>  Value3, Vector128<float>  Value4) value, byte index);
        public static unsafe void StoreSelectedScalar128x4(double* address, (Vector128<double> Value1, Vector128<double> Value2, Vector128<double> Value3, Vector128<double> Value4) value, byte index);
    }
}
```
## Provide trim/AOT-safe mechanism to (de)serialize enums from strings with JSON source-gen

**Approved** | [#runtime/79311](https://github.com/dotnet/runtime/issues/79311#issuecomment-1579280774) | [Video](https://www.youtube.com/watch?v=qHcX5_hlMRU&t=1h27m38s)

* Looks good as proposed
* While we can register the convert globally, there is no good way to ensure all enums are registered

```C#
namespace System.Text.Json.Serialization;

// Existing:
//
// [RequiresDynamicCode]
// public partial class JsonStringEnumConverter : JsonConverterFactory 
// { 
//     public JsonStringEnumConverter();
//     public JsonStringEnumConverter(JsonNamingPolicy? namingPolicy = null, bool allowIntegerValues = true);
// 
//     public override bool CanConvert(Type type) => type.IsEnum;
// }

public class JsonStringEnumConverter<TEnum> : JsonConverterFactory
    where TEnum : struct, Enum
{
    public JsonStringEnumConverter();
    public JsonStringEnumConverter(JsonNamingPolicy? namingPolicy = null, bool allowIntegerValues = true);

    public override bool CanConvert(Type type) => type == typeof(TEnum);
}
```
## Provide a way to reset an ArrayBufferWriter without clearing the underlying memory

**Approved** | [#runtime/49134](https://github.com/dotnet/runtime/issues/49134#issuecomment-1579297505) | [Video](https://www.youtube.com/watch?v=qHcX5_hlMRU&t=1h43m29s)

* Let's go with `ResetWrittenCount()`

```C#
namespace System.Buffers;

public partial class ArrayBufferWriter<T> : IBufferWriter<T>
{
    public void ResetWrittenCount();
}
```
