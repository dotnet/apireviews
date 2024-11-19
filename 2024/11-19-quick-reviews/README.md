# API Review 11/19/2024

## The new TimeSpan FromMilliseconds overload breaks System.Linq.Expressions

**Approved** | [#runtime/109833](https://github.com/dotnet/runtime/issues/109833#issuecomment-2486460741) | [Video](https://www.youtube.com/watch?v=LiyMdtA187g&t=0h0m0s)

In general, we still think using default/optional parameters is goodness, but given that all of the other method groups on this type have a "no defaults" option making FromMilliseconds look the same looks "natural", and fixes a compile break here.

```diff
public partial struct TimeSpan
{
+    public static TimeSpan FromMilliseconds(long milliseconds);

+    public static TimeSpan FromMilliseconds(long milliseconds, long microseconds);
-    public static TimeSpan FromMilliseconds(long milliseconds, long microseconds = 0);
}
```
## `ReadOnlySpan<char>` overloads for `StringNormalizationExtensions`

**Approved** | [#runtime/87757](https://github.com/dotnet/runtime/issues/87757#issuecomment-2486546994) | [Video](https://www.youtube.com/watch?v=LiyMdtA187g&t=0h28m25s)

* TryNormalize as proposed mixes semantics of OperationStatus and the Try pattern, and needs to pick one or the other.
  * We went with Try
* We added GetNormalizedLength for callers who are concerned about too many double-and-call-again for the Try-Write.

```c#
namespace System;

public static partial class StringNormalizationExtensions
{
    public static bool IsNormalized(this ReadOnlySpan<char> source, NormalizationForm normalizationForm = NormalizationForm.FormC);
    public static bool TryNormalize(this ReadOnlySpan<char> source, Span<char> destination, out int charsWritten, NormalizationForm normalizationForm = NormalizationForm.FormC);
    public static int GetNormalizedLength(this ReadOnlySpan<char> source, NormalizationForm normalizationForm = NormalizationForm.FormC);
}
```
## StreamReader constructor documentation mismatch with implementation

**Approved** | [#runtime/106236](https://github.com/dotnet/runtime/issues/106236#issuecomment-2486568664) | [Video](https://www.youtube.com/watch?v=LiyMdtA187g&t=1h9m45s)

Keeping the behavior in .NET 10 but correcting the nullability annotations seems correct.  Since .NET Framework does have those Encoding inputs as null-rejecting, the documentation probably needs to call that out as a version differentiated behavior.

```diff
- public StreamReader(Stream stream, Encoding encoding);
- public StreamReader(Stream stream, Encoding encoding, bool detectEncodingFromByteOrderMarks);
- public StreamReader(Stream stream, Encoding encoding, bool detectEncodingFromByteOrderMarks, int bufferSize);
+ public StreamReader(Stream stream, Encoding? encoding);
+ public StreamReader(Stream stream, Encoding? encoding, bool detectEncodingFromByteOrderMarks);
+ public StreamReader(Stream stream, Encoding? encoding, bool detectEncodingFromByteOrderMarks, int bufferSize);
```
## Add AVX10v2 API to add Avx10.2 support

**Approved** | [#runtime/109083](https://github.com/dotnet/runtime/issues/109083#issuecomment-2486636245) | [Video](https://www.youtube.com/watch?v=LiyMdtA187g&t=1h19m18s)

* ConvertToByteWithTruncationSaturationAndWidenToUInt32 => ConvertToByteWithTruncatedSaturationAndWidenToUInt32
  * Truncation => Truncated
* `Vector128<uint> ConvertToVector128UInt32(Vector128<uint> value)` and similar, change to "ConvertScalarTo..."

```c#
namespace System.Runtime.Intrinsics.X86
{
    /// <summary>Provides access to X86 AVX10.1 hardware instructions via intrinsics</summary>
    [Intrinsic]
    [CLSCompliant(false)]
    public abstract class Avx10v2 : Avx10v1
    {
        internal Avx10v2() { }

        public static new bool IsSupported { get => IsSupported; }

        // VMINMAXPD xmm1{k1}{z}, xmm2, xmm3/m128/m64bcst, imm8
        public static Vector128<double> MinMax(Vector128<double> left, Vector128<double> right, [ConstantExpected] byte control) => MinMax(left, right, mode);
        
        // VMINMAXPD ymm1{k1}{z}, ymm2, ymm3/m256/m64bcst {sae}, imm8
        public static Vector256<double> MinMax(Vector256<double> left, Vector256<double> right, [ConstantExpected] byte control) => MinMax(left, right, mode);
        
        // VMINMAXPS xmm1{k1}{z}, xmm2, xmm3/m128/m32bcst, imm8
        public static Vector128<float> MinMax(Vector128<float> left, Vector128<float> right, [ConstantExpected] byte control) => MinMax(left, right, mode);
        
        // VMINMAXPS ymm1{k1}{z}, ymm2, ymm3/m256/m32bcst {sae}, imm8
        public static Vector256<float> MinMax(Vector256<float> left, Vector256<float> right, [ConstantExpected] byte control) => MinMax(left, right, mode);
        
        // VMINMAXSD xmm1{k1}{z}, xmm2, xmm3/m64 {sae}, imm8
        public static Vector128<double> MinMaxScalar(Vector128<double> left, Vector128<double> right, [ConstantExpected] byte control) => MinMaxScalar(left, right, mode);

        // VMINMAXSS xmm1{k1}{z}, xmm2, xmm3/m32 {sae}, imm8
        public static Vector128<float> MinMaxScalar(Vector128<float> left, Vector128<float> right, [ConstantExpected] byte control) => MinMaxScalar(left, right, mode);

        // VADDPD ymm1{k1}{z}, ymm2, ymm3/m256/m64bcst {er}
        public static Vector256<double> Add(Vector256<double> left, Vector256<double> right, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => Add(left, right, mode);

        // VADDPS ymm1{k1}{z}, ymm2, ymm3/m256/m32bcst {er}
        public static Vector256<float> Add(Vector256<float> left, Vector256<float> right, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => Add(left, right, mode);
                
        // VDIVPD ymm1{k1}{z}, ymm2, ymm3/m256/m64bcst {er}
        public static Vector256<double> Divide(Vector256<double> left, Vector256<double> right, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => Divide(left, right, mode);
        
        // VDIVPS ymm1{k1}{z}, ymm2, ymm3/m256/m32bcst {er}
        public static Vector256<float> Divide(Vector256<float> left, Vector256<float> right, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => Divide(left, right, mode);
        
        // VCVTPS2IBS xmm1{k1}{z}, xmm2/m128/m32bcst
        public static Vector128<int> ConvertToByteWithSaturationAndWidenToInt32(Vector128<float> value) => ConvertToByteWithSaturationAndWidenToInt32(value);
                
        // VCVTPS2IBS ymm1{k1}{z}, ymm2/m256/m32bcst {er}
        public static Vector256<int> ConvertToByteWithSaturationAndWidenToInt32(Vector256<float> value) => ConvertToByteWithSaturationAndWidenToInt32(value);
        
        // VCVTPS2IBS ymm1{k1}{z}, ymm2/m256/m32bcst {er}
        public static Vector256<int> ConvertToByteWithSaturationAndWidenToInt32(Vector256<float> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToByteWithSaturationAndWidenToInt32(value, mode);
        
        // VCVTPS2IUBS xmm1{k1}{z}, xmm2/m128/m32bcst
        public static Vector128<uint> ConvertToByteWithSaturationAndWidenToUInt32(Vector128<float> value) => ConvertToByteWithSaturationAndWidenToUInt32(value);
                
        // VCVTPS2IUBS ymm1{k1}{z}, ymm2/m256/m32bcst {er}
        public static Vector256<uint> ConvertToByteWithSaturationAndWidenToUInt32(Vector256<float> value) => ConvertToByteWithSaturationAndWidenToUInt32(value);
        
        // VCVTPS2IUBS ymm1{k1}{z}, ymm2/m256/m32bcst {er}
        public static Vector256<uint> ConvertToByteWithSaturationAndWidenToUInt32(Vector256<float> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToByteWithSaturationAndWidenToUInt32(value, mode);

        // VCVTTPS2IBS xmm1{k1}{z}, xmm2/m128/m32bcst
        public static Vector128<int> ConvertToByteWithTruncatedSaturationAndWidenToInt32(Vector128<float> value) => ConvertToByteWithTruncationSaturationAndWidenToInt32(value);
                
        // VCVTTPS2IBS ymm1{k1}{z}, ymm2/m256/m32bcst {sae}
        public static Vector256<int> ConvertToByteWithTruncatedSaturationAndWidenToInt32(Vector256<float> value) => ConvertToVector256SByteWithTruncationSaturation(value);
                
        // VCVTTPS2IUBS xmm1{k1}{z}, xmm2/m128/m32bcst
        public static Vector128<uint> ConvertToByteWithTruncatedSaturationAndWidenToUInt32(Vector128<float> value) => ConvertToByteWithTruncatedSaturationAndWidenToUInt32(value);
                
        // VCVTTPS2IUBS ymm1{k1}{z}, ymm2/m256/m32bcst {sae}
        public static Vector256<uint> ConvertToByteWithTruncatedSaturationAndWidenToUInt32(Vector256<float> value) => ConvertToByteWithTruncatedSaturationAndWidenToUInt32(value);
        
        // VMOVD xmm1, xmm2/m32
        public static Vector128<uint> ConvertScalarToVector128UInt32(Vector128<uint> value) => ConvertScalarToVector128UInt32(value);
        
        // VMOVW xmm1, xmm2/m16
        public static Vector128<ushort> ConvertScalarToVector128UInt16(Vector128<ushort> value) => ConvertScalarToVector128UInt16(value);
        
        //The below instructions are those where 
        //embedded rouding support have been added 
        //to the existing API

        // VCVTDQ2PS ymm1{k1}{z}, ymm2/m256/m32bcst {er}
        public static Vector256<float> ConvertToVector256Single(Vector256<int> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector256Single(value, mode);
        
        // VCVTPD2DQ xmm1{k1}{z}, ymm2/m256/m64bcst {er}
        public static Vector128<int> ConvertToVector128Int32(Vector256<double> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector128Int32(value, mode);

        // VCVTPD2PS xmm1{k1}{z}, ymm2/m256/m64bcst {er}
        public static Vector128<float> ConvertToVector128Single(Vector256<double> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector128Single(value, mode);

        // VCVTPD2QQ ymm1{k1}{z}, ymm2/m256/m64bcst {er}
        public static Vector256<long> ConvertToVector256Int64(Vector256<double> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector256Int64(value, mode);

        // VCVTPD2UDQ xmm1{k1}{z}, ymm2/m256/m64bcst {er}
        public static Vector128<uint> ConvertToVector128UInt32(Vector256<double> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector128UInt32(value, mode);

        // VCVTPD2UQQ ymm1{k1}{z}, ymm2/m256/m64bcst {er}
        public static Vector256<ulong> ConvertToVector256UInt64(Vector256<double> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector256UInt64(value, mode);

        // VCVTPS2DQ ymm1{k1}{z}, ymm2/m256/m32bcst {er}
        public static Vector256<int> ConvertToVector256Int32(Vector256<float> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector256Int32(value, mode);

        // VCVTPS2QQ ymm1{k1}{z}, xmm2/m128/m32bcst {er}
        public static Vector256<long> ConvertToVector256Int64(Vector128<float> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector256Int64(value, mode);

        // VCVTPS2UDQ ymm1{k1}{z}, ymm2/m256/m32bcst {er}
        public static Vector256<uint> ConvertToVector256UInt32(Vector256<float> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector256UInt32(value, mode);

        // VCVTPS2UQQ ymm1{k1}{z}, xmm2/m128/m32bcst {er}
        public static Vector256<ulong> ConvertToVector256UInt64(Vector128<float> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector256UInt64(value, mode);

        // VCVTQQ2PS xmm1{k1}{z}, ymm2/m256/m64bcst {er}
        public static Vector128<float> ConvertToVector128Single(Vector256<ulong> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector128Single(value, mode);
        
        // VCVTQQ2PD ymm1{k1}{z}, ymm2/m256/m64bcst {er}
        public static Vector256<double> ConvertToVector256Double(Vector256<ulong> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector256Double(value, mode);
        
        // VCVTUDQ2PS ymm1{k1}{z}, ymm2/m256/m32bcst {er}
        public static Vector256<float> ConvertToVector256Single(Vector256<uint> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector256Single(value, mode);
        
        // VCVTUQQ2PS xmm1{k1}{z}, ymm2/m256/m64bcst {er}
        public static Vector128<float> ConvertToVector128Single(Vector256<long> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector128Single(value, mode);
        
        // VCVTUQQ2PD ymm1{k1}{z}, ymm2/m256/m64bcst {er}
        public static Vector256<double> ConvertToVector256Double(Vector256<long> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToVector256Double(value, mode);
        
        // VMULPD ymm1{k1}{z}, ymm2, ymm3/m256/m64bcst {er}
        public static Vector256<double> Multiply(Vector256<double> left, Vector256<double> right, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => Multiply(left, right, mode);
        
        // VMULPS ymm1{k1}{z}, ymm2, ymm3/m256/m32bcst {er}
        public static Vector256<float> Multiply(Vector256<float> left, Vector256<float> right, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => Multiply(left, right, mode);
        
        // VSCALEFPD ymm1{k1}{z}, ymm2, ymm3/m256/m64bcst {er}
        public static Vector256<double> Scale(Vector256<double> left, Vector256<double> right, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => Scale(left, right, mode);
        
        // VSCALEFPS ymm1{k1}{z}, ymm2, ymm3/m256/m32bcst {er}
        public static Vector256<float> Scale(Vector256<float> left, Vector256<float> right, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => Scale(left, right, mode);
        
        // VSQRTPD ymm1{k1}{z}, ymm2/m256/m64bcst {er}
        public static Vector256<double> Sqrt(Vector256<double> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => Sqrt(value, mode);
        
        // VSQRTPS ymm1{k1}{z}, ymm2/m256/m32bcst {er}
        public static Vector256<float> Sqrt(Vector256<float> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => Sqrt(value, mode);
        
        // VSUBPD ymm1{k1}{z}, ymm2, ymm3/m256/m64bcst {er}
        public static Vector256<double> Subtract(Vector256<double> left, Vector256<double> right, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => Subtract(left, right, mode);
        
        // VSUBPS ymm1{k1}{z}, ymm2, ymm3/m256/m32bcst {er}
        public static Vector256<float> Subtract(Vector256<float> left, Vector256<float> right, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => Subtract(left, right, mode);
        
        [Intrinsic]
        public new abstract class X64 : Avx10v1.X64
        {
            internal X64() { }

            public static new bool IsSupported { get => IsSupported; }
        }

        [Intrinsic]
        public abstract class V512 : Avx10v1.V512
        {
            internal V512() { }

            public static new bool IsSupported { get => IsSupported; }
    
            // VMINMAXPD zmm1{k1}{z}, zmm2, zmm3/m512/m64bcst {sae}, imm8
            public static Vector512<double> MinMax(Vector512<double> left, Vector512<double> right, [ConstantExpected] byte control) => MinMax(left, right, mode);
            
            // VMINMAXPS zmm1{k1}{z}, zmm2, zmm3/m512/m32bcst {sae}, imm8
            public static Vector512<float> MinMax(Vector512<float> left, Vector512<float> right, [ConstantExpected] byte control) => MinMax(left, right, mode);
            
            // VCVTPS2IBS zmm1{k1}{z}, zmm2/m512/m32bcst {er}
            public static Vector512<int> ConvertToByteWithSaturationAndWidenToInt32(Vector512<float> value) => ConvertToByteWithSaturationAndWidenToInt32(value);
            
            // VCVTPS2IBS zmm1{k1}{z}, zmm2/m512/m32bcst {er}
            public static Vector512<int> ConvertToByteWithSaturationAndWidenToInt32(Vector512<float> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToByteWithSaturationAndWidenToInt32(value, mode);
            
            // VCVTPS2IUBS zmm1{k1}{z}, zmm2/m512/m32bcst {er}
            public static Vector512<uint> ConvertToByteWithSaturationAndWidenToUInt32(Vector512<float> value) => ConvertToByteWithSaturationAndWidenToUInt32(value);
            
            // VCVTPS2IUBS zmm1{k1}{z}, zmm2/m512/m32bcst {er}
            public static Vector512<uint> ConvertToByteWithSaturationAndWidenToUInt32(Vector512<float> value, [ConstantExpected(Max = FloatRoundingMode.ToZero)] FloatRoundingMode mode) => ConvertToByteWithSaturationAndWidenToUInt32(value, mode);

            // VCVTTPS2IUBS zmm1{k1}{z}, zmm2/m512/m32bcst {sae}
            public static Vector512<int> ConvertToByteWithTruncatedSaturationAndWidenToInt32(Vector512<float> value) => ConvertToByteWithTruncatedSaturationAndWidenToInt32(value);
                        
            // VCVTTPS2IUBS zmm1{k1}{z}, zmm2/m512/m32bcst {sae}
            public static Vector512<uint> ConvertToByteWithTruncatedSaturationAndWidenToUInt32(Vector512<float> value) => ConvertToByteWithTruncatedSaturationAndWidenToUInt32(value);
            
            // This is a 512 extension of previously existing 128/26 inrinsic
            // VMPSADBW zmm1{k1}{z}, zmm2, zmm3/m512, imm8
            public static Vector512<ushort> MultipleSumAbsoluteDifferences(Vector512<byte> left, Vector512<byte> right, [ConstantExpected] byte mask) => MultipleSumAbsoluteDifferences(left, right, mask);

            [Intrinsic]
            public new abstract class X64 : Avx10v1.V512.X64
            {
                internal X64() { }

                public static new bool IsSupported { get => IsSupported; }
            }
        }
    }
}
```
