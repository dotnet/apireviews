# API Review 08/26/2025

## Expose a new `Complex<T>` given `where T : IFloatingPointIeee754<T>`

**Approved** | [#runtime/117451](https://github.com/dotnet/runtime/issues/117451#issuecomment-3225152399) | [Video](https://www.youtube.com/watch?v=5rKlRoQHCVU&t=0h0m0s)

* It's unusual to have both a ctor and a named Create with the same signature, but with two patterns at play here, both seem fine.
* On Complex (non-generic), we should [EditorBrowsable(Never)] the existing FromPolarCoordinates so that all the creation methods are aligned with having the "Create" prefix
* Once we're using EB-Never to "upgrade" the API to the newer names, let's do that with the Phase and Magnitude properties, too.

```C#
namespace System.Numerics;

public interface IComplexNumber<TSelf, T>
    : INumberBase<TSelf>,
      IExponentialFunctions<TSelf>,
      IHyperbolicFunctions<TSelf>,
      ILogarithmicFunctions<TSelf>,
      IPowerFunctions<TSelf>,
      IRootFunctions<TSelf>,
      ITrigonometricFunctions<TSelf>
    where TSelf : IComplexNumber<TSelf, T>
    where T : IFloatingPointIeee754<T>
{
    static abstract TSelf Create(T real, T imaginary);
    static abstract TSelf CreateFromPolarCoordinates(T magnitude, T phase);

    T Imaginary { get; }
    T Real { get; }

    static abstract TSelf operator +(TSelf left, T right);
    static abstract TSelf operator +(T left, TSelf right);

    static abstract TSelf operator /(TSelf left, T right);
    static abstract TSelf operator /(T left, TSelf right);

    static abstract bool operator ==(TSelf left, T right);
    static abstract bool operator ==(T left, TSelf right);

    static abstract implicit operator TSelf(T value);

    static abstract bool operator !=(TSelf left, T right);
    static abstract bool operator !=(T left, TSelf right);

    static abstract TSelf operator *(TSelf left, T right);
    static abstract TSelf operator *(T left, TSelf right);

    static abstract TSelf operator -(TSelf left, T right);
    static abstract TSelf operator -(T left, TSelf right);

    static abstract new T Abs(Complex<T> value);

    static abstract TSelf Conjugate(TSelf value);

    T GetMagnitude();
    T GetPhase();

    static abstract TSelf Hypot(TSelf x, T y);

    static abstract TSelf Log(T value, TSelf newBase);
    static abstract TSelf Log(TSelf value, T newBase);

    static abstract TSelf MaxMagnitude(TSelf x, T y);
    static abstract TSelf MaxMagnitudeNumber(TSelf x, T y);

    static abstract TSelf MinMagnitude(TSelf x, T y);
    static abstract TSelf MinMagnitudeNumber(TSelf x, T y);

    static abstract TSelf MultiplyAddEstimate(TSelf left, TSelf right, T addend);
    static abstract TSelf MultiplyAddEstimate(TSelf left, T right, TSelf addend);
    static abstract TSelf MultiplyAddEstimate(TSelf left, T right, T addend);
    static abstract TSelf MultiplyAddEstimate(T left, T right, TSelf addend);

    static abstract TSelf Pow(TSelf value, T power);
    static abstract TSelf Pow(T value, TSelf power);
}

public readonly struct Complex<T> : IComplexNumber<Complex<T>, T>, ISignedNumber<Complex<T>>
    where T : IFloatingPointIeee754<T>
{
    public Complex(T real, T imaginary);

    public T Imaginary { get; }
    public T Real { get; }

    public static Complex<T> operator +(Complex<T> left, Complex<T> right);
    public static Complex<T> operator +(T left, Complex<T> right);
    public static Complex<T> operator +(Complex<T> left, T right);

    public static Complex<T> operator --(Complex<T> value);

    public static Complex<T> operator /(Complex<T> left, Complex<T> right);
    public static Complex<T> operator /(T left, Complex<T> right);
    public static Complex<T> operator /(Complex<T> left, T right);

    public static bool operator ==(Complex<T> left, Complex<T> right);
    public static bool operator ==(Complex<T> left, T right);
    public static bool operator ==(T left, Complex<T> right);

    static abstract implicit operator Complex<T>(T value);

    public static Complex<T> operator ++(Complex<T> value);

    public static bool operator !=(Complex<T> left, Complex<T> right);
    public static bool operator !=(Complex<T> left, T right);
    public static bool operator !=(T left, Complex<T> right);

    public static Complex<T> operator *(Complex<T> left, Complex<T> right);
    public static Complex<T> operator *(T left, Complex<T> right);
    public static Complex<T> operator *(Complex<T> left, T right);

    public static Complex<T> operator -(Complex<T> left, Complex<T> right);
    public static Complex<T> operator -(T left, Complex<T> right);
    public static Complex<T> operator -(Complex<T> left, T right);

    public static Complex<T> operator -(Complex<T> value);
    public static Complex<T> operator +(Complex<T> value);

    public static T Abs(Complex<T> value);

    public static Complex<T> Acos(Complex<T> value);
    public static Complex<T> Acosh(Complex<T> value);
    public static Complex<T> AcosPi(Complex<T> value);

    public static Complex<T> Asin(Complex<T> value);
    public static Complex<T> Asinh(Complex<T> value);
    public static Complex<T> AsinPi(Complex<T> value);

    public static Complex<T> Atan(Complex<T> value);
    public static Complex<T> Atanh(Complex<T> value);
    public static Complex<T> AtanPi(Complex<T> value);

    public static Complex<T> Conjugate(Complex<T> value);

    public static Complex<T> Create(T real, T imaginary);
    public static Complex<T> CreateFromPolarCoordinates(T magnitude, T phase);

    public static Complex<T> Cos(Complex<T> value);
    public static Complex<T> Cosh(Complex<T> value);
    public static Complex<T> CosPi(Complex<T> value);

    public static Complex<T> CreateChecked<TOther>(TOther value) where TOther : INumberBase<TOther>;
    public static Complex<T> CreateSaturating<TOther>(TOther value) where TOther : INumberBase<TOther>;
    public static Complex<T> CreateTruncating<TOther>(TOther value) where TOther : INumberBase<TOther>;

    public static Complex<T> DegreesToRadians(Complex<T> value);

    public static Complex<T> Exp(Complex<T> value);
    public static Complex<T> ExpM1(Complex<T> value);
    public static Complex<T> Exp2(Complex<T> value);
    public static Complex<T> Exp2M1(Complex<T> value);
    public static Complex<T> Exp10(Complex<T> value);
    public static Complex<T> Exp10M1(Complex<T> value);

    public T GetMagnitude(Complex<T> value);
    public T GetPhase(Complex<T> value);

    public static Complex<T> Hypot(Complex<T> x, Complex<T> y);
    public static Complex<T> Hypot(Complex<T> x, T y);

    public static bool IsComplexNumber(Complex<T> value);
    public static bool IsEvenInteger(Complex<T> value);
    public static bool IsImaginaryNumber(Complex<T> value);
    public static bool IsInteger(Complex<T> value);
    public static bool IsNegative(Complex<T> value);
    public static bool IsNegativeInfinity(Complex value);
    public static bool IsNormal(Complex value);
    public static bool IsOddInteger(Complex value);
    public static bool IsPositive(Complex value);
    public static bool IsPositiveInfinity(Complex value);
    public static bool IsRealNumber(Complex value);
    public static bool IsSubnormal(Complex value);

    public static Complex<T> Log(Complex<T> value);
    public static Complex<T> Log(Complex<T> value, Complex<T> newBase);
    public static Complex<T> Log(T value, Complex<T> newBase);
    public static Complex<T> Log(Complex<T> value, T newBase);
    public static Complex<T> LogP1(Complex<T> value);
    public static Complex<T> Log2(Complex<T> value);
    public static Complex<T> Log2P1(Complex<T> value);
    public static Complex<T> Log10(Complex<T> value);
    public static Complex<T> Log10P1(Complex<T> value);

    public static Complex<T> MaxMagnitude(Complex<T> x, Complex<T> y);
    public static Complex<T> MaxMagnitude(Complex<T> x, T y);

    public static Complex<T> MaxMagnitudeNumber(Complex<T> x, Complex<T> y);
    public static Complex<T> MaxMagnitudeNumber(Complex<T> x, T y);

    public static Complex<T> MinMagnitude(Complex<T> x, Complex<T> y);
    public static Complex<T> MinMagnitude(Complex<T> x, T y);

    public static Complex<T> MinMagnitudeNumber(Complex<T> x, Complex<T> y);
    public static Complex<T> MinMagnitudeNumber(Complex<T> x, T y);

    public static Complex<T> MultiplyAddEstimate(Complex<T> left, Complex<T> right, Complex<T> addend);
    public static Complex<T> MultiplyAddEstimate(Complex<T> left, Complex<T> right, T addend);
    public static Complex<T> MultiplyAddEstimate(Complex<T> left, T right, Complex<T> addend);
    public static Complex<T> MultiplyAddEstimate(Complex<T> left, T right, T addend);
    public static Complex<T> MultiplyAddEstimate(T left, T right, Complex<T> addend);

    public static Complex<T> RadiansToDegrees(Complex<T> value);

    public static Complex<T> RootN(Complex<T> value, int n);

    public static Complex<T> Parse(string s, IFormatProvider? provider);
    public static Complex<T> Parse(string s, NumberStyles style, IFormatProvider? provider);

    public static Complex<T> Parse(ReadOnlySpan<char> s, IFormatProvider? provider);
    public static Complex<T> Parse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider);

    public static Complex<T> Parse(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider);
    public static Complex<T> Parse(ReadOnlySpan<byte> utf8Text, NumberStyles style, IFormatProvider? provider);

    public static Complex<T> Pow(Complex<T> value, Complex<T> power);
    public static Complex<T> Pow(Complex<T> value, T power);
    public static Complex<T> Pow(T value, Complex<T> power);

    public static Complex<T> Reciprocal(Complex<T> value);

    public static Complex<T> Sin(Complex<T> value);
    public static Complex<T> Sinh(Complex<T> value);
    public static Complex<T> SinPi(Complex<T> value);

    public static (Complex<T> Sin, Complex<T> Cos) SinCos(Complex<T> value);
    public static (Complex<T> SinPi, Complex<T> CosPi) SinCosPi(Complex<T> value);

    public static Complex<T> Sqrt(Complex<T> value);

    public static Complex<T> Tan(Complex<T> value);
    public static Complex<T> Tanh(Complex<T> value);

    public static bool TryParse([NotNullWhen(true)] string? s, IFormatProvider? provider, out Complex<T> result);
    public static bool TryParse([NotNullWhen(true)] string? s, NumberStyles style, IFormatProvider? provider, out Complex<T> result);

    public static bool TryParse(ReadOnlySpan<char> s, IFormatProvider? provider, out Complex<T> result);
    public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out Complex<T> result);

    public static bool TryParse(ReadOnlySpan<byte> utf8Text, NumberStyles style, IFormatProvider? provider, out Complex<T> result);
    public static bool TryParse(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider, out Complex<T> result);

    public bool Equals(Complex<T> other);

    public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format, IFormatProvider? provider);
    public bool TryFormat(Span<byte> utf8Destination, out int bytesWritten, ReadOnlySpan<char> format, IFormatProvider? provider);

    public string ToString(IFormatProvider? provider);
    public string ToString([StringSyntax(StringSyntaxAttribute.NumericFormat)] string? format);
    public string ToString([StringSyntax(StringSyntaxAttribute.NumericFormat)] string? format, IFormatProvider? provider);

    // IAdditiveIdentity<TSelf>

    static Complex<T> IAdditiveIdentity<Complex<T>>.AdditiveIdentity { get; }

    // IFloatingPointConstants<TSelf>

    static Complex<T> IFloatingPointConstants<Complex<T>>.E { get; }

    static Complex<T> IFloatingPointConstants<Complex<T>>.Pi { get; }

    static Complex<T> IFloatingPointConstants<Complex<T>>.Tau { get; }

    // IMultiplicativeIdentity<TSelf>

    static Complex<T> IMultiplicativeIdentity<Complex<T>>.MultiplicativeIdentity { get; }

    // INumberBase<TSelf>

    static Complex<T> INumberBase<Complex<T>>.One { get; }
    
    static int INumberBase<Complex<T>>.Radix { get; }
    
    static Complex<T> INumberBase<Complex<T>>.Zero { get; }

    static Complex<T> INumberBase<Complex<T>>.Abs(Complex<T> value);

    static bool INumberBase<Complex<T>>.IsCanonical(Complex<T> value);
    static bool INumberBase<Complex<T>>.IsZero(Complex<T> value);

    // ISignedNumber<TSelf>

    static Complex<T> ISignedNumber<Complex<T>>.NegativeOne { get; }
}

public readonly partial struct Complex : IComplexNumber<Complex, double>
{
    public static Complex Acosh(Complex value);
    public static Complex Asinh(Complex value);
    public static Complex Atanh(Complex value);

    public static Complex CosPi(Complex value);

    public static Complex Cbrt(Complex value);

    public static Complex DegreesToRadians(Complex value);

    public static Complex ExpM1(Complex value);
    public static Complex Exp2(Complex value);
    public static Complex Exp2M1(Complex value);
    public static Complex Exp10(Complex value);
    public static Complex Exp10M1(Complex value);

    public static Complex Hypot(Complex x, Complex y);
    public static Complex Hypot(Complex x, double y);

    public static Complex Log(Complex value, Complex newBase);
    public static Complex Log(double value, Complex newBase);
    public static Complex LogP1(Complex value);
    public static Complex Log2(Complex value);
    public static Complex Log2P1(Complex value);
    public static Complex Log10P1(Complex value);

    public static Complex MaxMagnitude(Complex x, double y);

    public static Complex MaxMagnitudeNumber(Complex x, Complex y);
    public static Complex MaxMagnitudeNumber(Complex x, double y);

    public static Complex MinMagnitude(Complex x, double y);

    public static Complex MinMagnitudeNumber(Complex x, Complex y);
    public static Complex MinMagnitudeNumber(Complex x, double y);

    public static Complex MultiplyAddEstimate(Complex left, Complex right, Complex addend);
    public static Complex MultiplyAddEstimate(Complex left, Complex right, double addend);
    public static Complex MultiplyAddEstimate(Complex left, double right, Complex addend);
    public static Complex MultiplyAddEstimate(Complex left, double right, double addend);
    public static Complex MultiplyAddEstimate(double left, double right, Complex addend);

    public static Complex Pow(double value, Complex power);

    public static Complex RadiansToDegrees(Complex value);

    public static Complex RootN(Complex value, int n);

    public static Complex<T> Parse(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider);
    public static Complex<T> Parse(ReadOnlySpan<byte> utf8Text, NumberStyles style, IFormatProvider? provider);

    public static Complex SinPi(Complex value);

    public static (Complex<T> Sin, Complex<T> Cos) SinCos(Complex<T> value);
    public static (Complex<T> SinPi, Complex<T> CosPi) SinCosPi(Complex<T> value);

    public static Complex TanPi(Complex value);

    public static bool TryParse(ReadOnlySpan<byte> utf8Text, NumberStyles style, IFormatProvider? provider, out Complex<T> result);
    public static bool TryParse(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider, out Complex<T> result);

    // IComplexNumber<TSelf, T>

    static Complex Create(T real, T imaginary);
    static Complex CreateFromPolarCoordinates(T magnitude, T phase);

    static double GetMagnitude(Complex value);
    static double GetPhase(Complex value);

    // Gains EB-Never
    [EditorBrowsable(EditorBrowsableState.Never)]
    static Complex FromPolarCoordinates(T magnitude, T phase);
    
    [EditorBrowsable(EditorBrowsableState.Never)]
    double Magnitude { get; }

    [EditorBrowsable(EditorBrowsableState.Never)]
    double Phase { get; }
}
``` 
## Add Log10 to IBinaryInteger

**Approved** | [#runtime/116043](https://github.com/dotnet/runtime/issues/116043#issuecomment-3225237374) | [Video](https://www.youtube.com/watch?v=5rKlRoQHCVU&t=0h46m53s)

* Looks good as proposed.
* This will be implemented implicitly (visible) on all IBinaryInteger types except BigInteger, which has a conflicting signature.

```C#
public interface IBinaryInteger<TSelf>
{
    // Will be appropriately DIMmed
    static virtual TSelf Log10(TSelf value);
}
```
## Adding a Cardinality/PopCount property to BitArray

**Approved** | [#runtime/104299](https://github.com/dotnet/runtime/issues/104299#issuecomment-3225270114) | [Video](https://www.youtube.com/watch?v=5rKlRoQHCVU&t=1h13m31s)

* While PopCount is static everywhere else, it should be instance here to be consistent with other BitArray members
* We should add lzcount and tzcount at the same time

```C#
namespace System.Collections;

public partial class BitArray
{
    public int PopCount();
    public int LeadingZeroCount();
    public int TrailingZeroCount();
}
```
## `RuntimeHelpers.AllocateTypeAssociatedMemory` overload with alignment

**Approved** | [#runtime/118897](https://github.com/dotnet/runtime/issues/118897#issuecomment-3225292521) | [Video](https://www.youtube.com/watch?v=5rKlRoQHCVU&t=1h23m26s)

Looks good as proposed

```C#
namespace System.Runtime.CompilerServices;

public static class RuntimeHelpers
{
    public static IntPtr AllocateTypeAssociatedMemory(Type type, int size, int alignment);
}
```
## AVX10.2 saturating floating point to integer conversions

**Approved** | [#runtime/117573](https://github.com/dotnet/runtime/issues/117573#issuecomment-3225205759) | [Video](https://www.youtube.com/watch?v=5rKlRoQHCVU&t=1h31m43s)

Looks good as proposed

```C#
namespace System.Runtime.Intrinsics.X86;

public abstract class Avx10v2 : Avx10v1
{
    //VCVTTPS2DQS xmm1,xmm2/m128
    public static Vector128<int> ConvertToVector128Int32WithTruncatedSaturation(Vector128<float> value);
    //VCVTTPS2DQS ymm1,ymm2/m256
    public static Vector256<int> ConvertToVector256Int32WithTruncatedSaturation(Vector256<float> value);

    //VCVTTPS2UDQS xmm1,xmm2/m128
    public static Vector128<uint> ConvertToVector128UInt32WithTruncatedSaturation(Vector128<float> value);
    //VCVTTPS2UDQS ymm1,ymm2/m256
    public static Vector256<uint> ConvertToVector256UInt32WithTruncatedSaturation(Vector256<float> value);

    //VCVTTPS2QQS xmm1,xmm2/m64
    public static Vector128<long> ConvertToVector128Int64WithTruncatedSaturation(Vector128<float> value);
    //VCVTTPS2QQS ymm1,xmm2/m128
    public static Vector256<long> ConvertToVector256Int64WithTruncatedSaturation(Vector128<float> value);

    //VCVTTPS2UQQS xmm1,xmm2/m64
    public static Vector128<ulong> ConvertToVector128UInt64WithTruncatedSaturation(Vector128<float> value);
    //VCVTTPS2UQQS ymm1,xmm2/m128
    public static Vector256<ulong> ConvertToVector256UInt64WithTruncatedSaturation(Vector128<float> value);


    //VCVTTPD2DQS xmm1,xmm2/m128
    public static Vector128<int> ConvertToVector128Int32WithTruncatedSaturation(Vector128<double> value);
    //VCVTTPD2DQS xmm1,ymm2/m256
    public static Vector128<int> ConvertToVector128Int32WithTruncatedSaturation(Vector256<double> value);

    //VCVTTPD2UDQS xmm1,xmm2/m128
    public static Vector128<uint> ConvertToVector128UInt32WithTruncatedSaturation(Vector128<double> value);
    //VCVTTPD2UDQS xmm1,ymm2/m256
    public static Vector128<uint> ConvertToVector128UInt32WithTruncatedSaturation(Vector256<double> value);

    //VCVTTPD2QQS xmm1,xmm2/m128
    public static Vector128<long> ConvertToVector128Int64WithTruncatedSaturation(Vector128<double> value);
    //VCVTTPD2QQS ymm1,ymm2/m256
    public static Vector256<long> ConvertToVector256Int64WithTruncatedSaturation(Vector256<double> value);

    //VCVTTPD2UQQS xmm1,xmm2/m128
    public static Vector128<ulong> ConvertToVector128UInt64WithTruncatedSaturation(Vector128<double> value);
    //VCVTTPD2UQQS ymm1,ymm2/m256
    public static Vector256<ulong> ConvertToVector256UInt64WithTruncatedSaturation(Vector256<double> value);


    //VCVTTSS2SIS r32,xmm1/m32
    public static int ConvertToInt32WithTruncatedSaturation(Vector128<float> value);

    //VCVTTSS2USIS r32,xmm1/m32
    public static uint ConvertToUInt32WithTruncatedSaturation(Vector128<float> value);

    //VCVTTSD2SIS r32,xmm1/m64
    public static int ConvertToInt32WithTruncatedSaturation(Vector128<double> value);

    //VCVTTSD2USIS r32,xmm1/m64
    public static uint ConvertToUInt32WithTruncatedSaturation(Vector128<double> value);


    public new abstract class V512 : Avx10v1.V512
    {
        //VCVTTPS2DQS zmm1,zmm2/m512
        public static Vector512<int> ConvertToVector512Int32WithTruncatedSaturation(Vector512<float> value);

        //VCVTTPS2UDQS zmm1,zmm2/m512
        public static Vector512<uint> ConvertToVector512UInt32WithTruncatedSaturation(Vector512<float> value);

        //VCVTTPS2QQS zmm1,ymm2/m256
        public static Vector512<long> ConvertToVector512Int64WithTruncatedSaturation(Vector256<float> value);

        //VCVTTPS2UQQS zmm1,ymm2/m256
        public static Vector512<ulong> ConvertToVector512UInt64WithTruncatedSaturation(Vector256<float> value);


        //VCVTTPD2DQS ymm1,zmm2/m512
        public static Vector256<int> ConvertToVector256Int32WithTruncatedSaturation(Vector512<double> value);

        //VCVTTPD2UDQS ymm1,zmm2/m512
        public static Vector256<uint> ConvertToVector256UInt32WithTruncatedSaturation(Vector512<double> value);

        //VCVTTPD2QQS zmm1,zmm2/m512
        public static Vector512<long> ConvertToVector512Int64WithTruncatedSaturation(Vector512<double> value);

        //VCVTTPD2UQQS zmm1,zmm2/m512
        public static Vector512<ulong> ConvertToVector512UInt64WithTruncatedSaturation(Vector512<double> value);
    }


    public new abstract class X64 : Avx10v1.X64
    {
        //VCVTTSS2SIS r64,xmm1/m32
        public static long ConvertToInt64WithTruncatedSaturation(Vector128<float> value);

        //VCVTTSS2USIS r64,xmm1/m32
        public static ulong ConvertToUInt64WithTruncatedSaturation(Vector128<float> value);

        //VCVTTSD2SIS r64,xmm1/m64
        public static long ConvertToInt64WithTruncatedSaturation(Vector128<double> value);

        //VCVTTSD2USIS r64,xmm1/m64
        public static ulong ConvertToUInt64WithTruncatedSaturation(Vector128<double> value);
    }
}
```
