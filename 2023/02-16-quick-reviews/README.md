# API Review 02/16/2023

## Add Decimal32, Decimal64, and Decimal128 from the IEEE 754-2019 standard.

**NeedsWork** | [#runtime/81376](https://github.com/dotnet/runtime/issues/81376#issuecomment-1433633909)

* `Quantize` doesn't really make sense to any of us, but is the verb from the spec, so it's what we should use
* `Quantum` is lacking a verb, so it should be `GetQuantum`, similarly `SameQuantum` should be `HaveSameQuantum`
* `EncodeDecimal` (and friends) don't play well with generic math.  Consider an interfacable overload in the future, such as `static int EncodeDecimal(TSelf value, Span<byte> destination>)`
* A constructor that takes in pseudo-scientific notation seems to make sense, e.g. `new Decimal32(-1000, -3)` for `-1.000`, but the parts need naming

Since we lost quorum before finalizing the ctor-vs-named-static point (and the names of the parameters), and the pattern for the conversion operators, we'll need to discuss this again before approval.

```C#
namespace System.Numerics
{

    /// <summary>Defines an IEEE 754 floating-point type that is represented in a base-10 format.</summary>
    /// <typeparam name="TSelf">The type that implements the interface.</typeparam>
    public interface IDecimalFloatingPointIeee754<TSelf> // PLATINUM
        : IFloatingPointIeee754<TSelf>
        where TSelf : IDecimalFloatingPointIeee754<TSelf>
    {
        // IEEE Spec 5.3.2
        static abstract TSelf Quantize(TSelf x, TSelf y);
        static abstract TSelf GetQuantum(TSelf x);

        // IEEE Spec 5.7.3
        static abstract bool HaveSameQuantum(TSelf x, TSelf y);
    }

    //
    // Summary:
    //     Represents a 32-bit IEEE decimal floating-point number
    [StructLayout(LayoutKind.Sequential)]
    public readonly struct Decimal32
        : IComparable<Decimal32>,
          IComparable,
          ISpanFormattable,
          ISpanParsable<Decimal32>,
          IEquatable<Decimal32>,
          IFloatingPoint<Decimal32>,/*PLATINUM: Replace with IDecimalFloatingPointIeee754<Decimal32>,*/
          IMinMaxValue<Decimal32>
    {
        internal readonly uint _value; // NOTE: this is a ulong for Decimal64, and a UInt128 for Decimal128

        //
        // Parsing (INumberBase, IParsable, ISpanParsable, other)
        //

        public static Decimal32 Parse(string s);
        public static Decimal32 Parse(string s, NumberStyles style);
        public static Decimal32 Parse(ReadOnlySpan<char> s, IFormatProvider? provider);
        public static Decimal32 Parse(string s, IFormatProvider? provider);
        public static Decimal32 Parse(ReadOnlySpan<char> s, NumberStyles style = DefaultParseStyle, IFormatProvider? provider = null);
        public static Decimal32 Parse(string s, NumberStyles style, IFormatProvider? provider);
        public static bool TryParse([NotNullWhen(true)] string? s, out Decimal32 result);
        public static bool TryParse(ReadOnlySpan<char> s, out Decimal32 result);
        public static bool TryParse(ReadOnlySpan<char> s, IFormatProvider? provider, [MaybeNullWhen(false)] out Decimal32 result);
        public static bool TryParse([NotNullWhen(true)] string? s, IFormatProvider? provider, [MaybeNullWhen(false)] out Decimal32 result);
        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, [MaybeNullWhen(false)] out Decimal32 result);
        public static bool TryParse([NotNullWhen(true)] string? s, NumberStyles style, IFormatProvider? provider, [MaybeNullWhen(false)] out Decimal32 result);

        //
        // Misc. Methods (including IComparable, IEquatable, other)
        //

        public int CompareTo(object? obj);
        public int CompareTo(Decimal32 other);
        public override bool Equals([NotNullWhen(true)] object? obj);
        public bool Equals(Decimal32 other);
        public override int GetHashCode();
        // 5.5.2 of the IEEE Spec
        public static uint EncodeDecimal(Decimal32 x); // NOTE: this is a ulong for Decimal64, and a UInt128 for Decimal128
        public static Decimal32 DecodeDecimal(uint x); // NOTE: this is a ulong for Decimal64, and a UInt128 for Decimal128
        public static uint EncodeBinary(Decimal32 x); // NOTE: this is a ulong for Decimal64, and a UInt128 for Decimal128
        public static Decimal32 DecodeBinary(uint x); // NOTE: this is a ulong for Decimal64, and a UInt128 for Decimal128

        //
        // Formatting (IFormattable, ISpanFormattable, other)
        //
        
        public override string ToString();
        public string ToString([StringSyntax(StringSyntaxAttribute.NumericFormat)] string? format);
        public string ToString(IFormatProvider? provider);
        public string ToString([StringSyntax(StringSyntaxAttribute.NumericFormat)] string? format, IFormatProvider? provider);
        public bool TryFormat(Span<char> destination, out int charsWritten, [StringSyntax(StringSyntaxAttribute.NumericFormat)] ReadOnlySpan<char> format = default, IFormatProvider? provider = null);

        //
        // Explicit Convert To Decimal32
        // (T -> Decimal32 is lossy)
        //

        public static explicit operator Decimal32(int value); // NOTE: Decimal64 and Decimal128 will have this as *implicit*
        public static explicit operator Decimal32(uint value); // NOTE: Decimal64 and Decimal128 will have this as *implicit*
        public static explicit operator Decimal32(nint value);  // NOTE: Decimal128 will have this as *implicit*
        public static explicit operator Decimal32(nuint value); // NOTE: Decimal128 will have this as *implicit*
        public static explicit operator Decimal32(long value); // NOTE: Decimal128 will have this as *implicit*
        public static explicit operator Decimal32(ulong value); // NOTE: Decimal128 will have this as *implicit*
        public static explicit operator Decimal32(Int128 value);
        public static explicit operator Decimal32(UInt128 value);
        public static explicit operator Decimal32(Half value);
        public static explicit operator Decimal32(float value);
        public static explicit operator Decimal32(double value);
        public static explicit operator Decimal32(decimal value); // NOTE: Decimal128 will have this as *implicit*
        public static explicit operator Decimal32(Decimal64 value);
        public static explicit operator Decimal32(Decimal128 value);

        //
        // Explicit Convert From Decimal32
        // (Decimal32 -> T is lossy)
        // - Includes a "checked" conversion if T cannot represent infinity and NaN
        //
        public static explicit operator byte(Decimal32 value);
        public static explicit operator checked byte(Decimal32 value);
        public static explicit operator sbyte(Decimal32 value);
        public static explicit operator checked sbyte(Decimal32 value);
        public static explicit operator char(Decimal32 value);
        public static explicit operator checked char(Decimal32 value);
        public static explicit operator short(Decimal32 value);
        public static explicit operator checked short(Decimal32 value);
        public static explicit operator ushort(Decimal32 value);
        public static explicit operator checked ushort(Decimal32 value);
        public static explicit operator int(Decimal32 value);
        public static explicit operator checked int(Decimal32 value);
        public static explicit operator uint(Decimal32 value);
        public static explicit operator checked uint(Decimal32 value);
        public static explicit operator nint(Decimal32 value);        
        public static explicit operator checked nint(Decimal32 value);
        public static explicit operator nuint(Decimal32 value);
        public static explicit operator checked nuint(Decimal32 value);
        public static explicit operator long(Decimal32 value);
        public static explicit operator checked long(Decimal32 value);
        public static explicit operator ulong(Decimal32 value);
        public static explicit operator checked ulong(Decimal32 value);
        public static explicit operator Int128(Decimal32 value);       
        public static explicit operator checked Int128(Decimal32 value);
        public static explicit operator UInt128(Decimal32 value);
        public static explicit operator checked UInt128(Decimal32 value);
        public static explicit operator Half(Decimal32 value);
        public static explicit operator float(Decimal32 value);
        public static explicit operator double(Decimal32 value);
        public static explicit operator decimal(Decimal32 value); // Doesn't have a "checked" for historical reasons


        //
        // Implicit Convert To Decimal32
        // (T -> Decimal32 is not lossy)
        //
        public static implicit operator Decimal32(byte value);
        public static implicit operator Decimal32(sbyte value);
        public static implicit operator Decimal32(char value);
        public static implicit operator Decimal32(short value);
        public static implicit operator Decimal32(ushort value);

        //
        // Implicit Convert From Decimal32
        // (Decimal32 -> T is not lossy)
        //
        public static implicit operator Decimal64(Decimal32 value);
        public static implicit operator Decimal128(Decimal32 value);

        //
        // IAdditionOperators
        //
        public static Decimal32 operator +(Decimal32 left, Decimal32 right);

        //
        // IAdditiveIdentity
        //
        static Decimal32 IAdditiveIdentity<Decimal32, Decimal32>.AdditiveIdentity { get; }


        //
        // IComparisonOperators
        //
        public static bool operator <(Decimal32 left, Decimal32 right);
        public static bool operator >(Decimal32 left, Decimal32 right);
        public static bool operator <=(Decimal32 left, Decimal32 right);
        public static bool operator >=(Decimal32 left, Decimal32 right);

        //
        // IDecimalFloatingPointIeee754
        //
        public static Decimal32 Quantize(Decimal32 x, Decimal32 y);
        public static Decimal32 Quantum(Decimal32 x);
        public static bool SameQuantum(Decimal32 x, Decimal32 y);

        //
        // IDecrementOperators
        //
        public static Decimal32 operator --(Decimal32 value);

        //
        // IDivisionOperators
        //
        public static Decimal32 operator /(Decimal32 left, Decimal32 right);

        //
        // IEqualityOperators
        //
        public static bool operator ==(Decimal32 left, Decimal32 right);
        public static bool operator !=(Decimal32 left, Decimal32 right);

        //
        // IExponentialFunctions
        //
        public static Decimal32 Exp(Decimal32 x); // PLATINUM
        public static Decimal32 ExpM1(Decimal32 x); // PLATINUM
        public static Decimal32 Exp2(Decimal32 x); // PLATINUM
        public static Decimal32 Exp2M1(Decimal32 x); // PLATINUM
        public static Decimal32 Exp10(Decimal32 x); // PLATINUM
        public static Decimal32 Exp10M1(Decimal32 x); // PLATINUM

        //
        // IFloatingPoint
        //
        public static Decimal32 Ceiling(Decimal32 x);
        public static Decimal32 Floor(Decimal32 x);
        public static Decimal32 Round(Decimal32 x);
        public static Decimal32 Round(Decimal32 x, int digits);
        public static Decimal32 Round(Decimal32 x, MidpointRounding mode);
        public static Decimal32 Round(Decimal32 x, int digits, MidpointRounding mode);
        public static Decimal32 Truncate(Decimal32 x);
        int IFloatingPoint<Decimal32>.GetExponentByteCount();
        int IFloatingPoint<Decimal32>.GetExponentShortestBitLength();
        int IFloatingPoint<Decimal32>.GetSignificandBitLength();
        int IFloatingPoint<Decimal32>.GetSignificandByteCount();
        bool IFloatingPoint<Decimal32>.TryWriteExponentBigEndian(Span<byte> destination, out int bytesWritten);
        bool IFloatingPoint<Decimal32>.TryWriteExponentLittleEndian(Span<byte> destination, out int bytesWritten);
        bool IFloatingPoint<Decimal32>.TryWriteSignificandBigEndian(Span<byte> destination, out int bytesWritten);
        bool IFloatingPoint<Decimal32>.TryWriteSignificandLittleEndian(Span<byte> destination, out int bytesWritten);

        //
        // IFloatingPointConstants
        //
        public static Decimal32 E { get; }
        public static Decimal32 Pi { get; }
        public static Decimal32 Tau { get; }

        //
        // IFloatingPointIeee754
        //
        public static Decimal32 Epsilon { get; }
        public static Decimal32 NaN { get; }
        public static Decimal32 NegativeInfinity { get; }
        public static Decimal32 NegativeZero { get; }
        public static Decimal32 PositiveInfinity { get; }
        public static Decimal32 Atan2(Decimal32 y, Decimal32 x); // PLATINUM
        public static Decimal32 Atan2Pi(Decimal32 y, Decimal32 x); // PLATINUM
        public static Decimal32 BitDecrement(Decimal32 x);
        public static Decimal32 BitIncrement(Decimal32 x);
        public static Decimal32 FusedMultiplyAdd(Decimal32 left, Decimal32 right, Decimal32 addend);
        public static Decimal32 Ieee754Remainder(Decimal32 left, Decimal32 right);
        public static int ILogB(Decimal32 x);
        public static Decimal32 Lerp(Decimal32 value1, Decimal32 value2, Decimal32 amount);
        public static Decimal32 ReciprocalEstimate(Decimal32 x);
        public static Decimal32 ReciprocalSqrtEstimate(Decimal32 x);
        public static Decimal32 ScaleB(Decimal32 x, int n);
        // public static Decimal32 Compound(Half x, Decimal32 n); (Already approved in API review but not implemented yet) // PLATINUM

        //
        // IHyperbolicFunctions
        //
        public static Decimal32 Acosh(Decimal32 x); // PLATINUM
        public static Decimal32 Asinh(Decimal32 x); // PLATINUM
        public static Decimal32 Atanh(Decimal32 x); // PLATINUM
        public static Decimal32 Cosh(Decimal32 x); // PLATINUM
        public static Decimal32 Sinh(Decimal32 x); // PLATINUM
        public static Decimal32 Tanh(Decimal32 x); // PLATINUM

        //
        // IIncrementOperators
        //
        public static Decimal32 operator ++(Decimal32 value);

        //
        // ILogarithmicFunctions
        //
        public static Decimal32 Log(Decimal32 x); // PLATINUM
        public static Decimal32 Log(Decimal32 x, Decimal32 newBase); // PLATINUM
        public static Decimal32 Log10(Decimal32 x); // PLATINUM
        public static Decimal32 LogP1(Decimal32 x); // PLATINUM
        public static Decimal32 Log2(Decimal32 x); // PLATINUM
        public static Decimal32 Log2P1(Decimal32 x); // PLATINUM
        public static Decimal32 Log10P1(Decimal32 x); // PLATINUM

        //
        // IMinMaxValue
        //
        public static Decimal32 MaxValue { get; }
        public static Decimal32 MinValue { get; }

        //
        // IModulusOperators
        //
        public static Decimal32 operator %(Decimal32 left, Decimal32 right);

        //
        // IMultiplicativeIdentity
        //
        public static Decimal32 MultiplicativeIdentity { get; }

        //
        // IMultiplyOperators
        //
        public static Decimal32 operator *(Decimal32 left, Decimal32 right);

        //
        // INumber
        //
        public static Decimal32 Clamp(Decimal32 value, Decimal32 min, Decimal32 max);
        public static Decimal32 CopySign(Decimal32 value, Decimal32 sign);
        public static Decimal32 Max(Decimal32 x, Decimal32 y);
        public static Decimal32 MaxNumber(Decimal32 x, Decimal32 y);
        public static Decimal32 Min(Decimal32 x, Decimal32 y);
        public static Decimal32 MinNumber(Decimal32 x, Decimal32 y);
        public static int Sign(Decimal32 value);


        //
        // INumberBase (well defined/commonly used values)
        //
        public static Decimal32 One { get; }
        static int INumberBase<Decimal32>.Radix; // Note: this ideally should be exposed implicitly as it is required by IEEE
        public static Decimal32 Zero { get; }
        public static Decimal32 Abs(Decimal32 value);
        public static Decimal32 CreateChecked<TOther>(TOther value);
        public static Decimal32 CreateSaturating<TOther>(TOther value);
        public static Decimal32 CreateTruncating<TOther>(TOther value);
        static bool INumberBase<Decimal32>.IsCanonical(Decimal32 value); // Note: this ideally should be exposed implicitly as it is required by IEEE
        static bool INumberBase<Decimal32>.IsComplexNumber(Decimal32 value);
        public static bool IsEvenInteger(Decimal32 value);
        public static bool IsFinite(Decimal32 value);
        static bool INumberBase<Decimal32>.IsImaginaryNumber(Decimal32 value);
        public static bool IsInfinity(Decimal32 value);
        public static bool IsInteger(Decimal32 value);
        public static bool IsNaN(Decimal32 value);
        public static bool IsNegative(Decimal32 value);
        public static bool IsNegativeInfinity(Decimal32 value);
        public static bool IsNormal(Decimal32 value);
        public static bool IsOddInteger(Decimal32 value);
        public static bool IsPositive(Decimal32 value);
        public static bool IsPositiveInfinity(Decimal32 value);
        public static bool IsRealNumber(Decimal32 value);
        public static bool IsSubnormal(Decimal32 value);
        static bool INumberBase<Decimal32>.IsZero(Decimal32 value); // Note: this ideally should be exposed implicitly as it is required by IEEE
        public static Decimal32 MaxMagnitude(Decimal32 x, Decimal32 y);
        public static Decimal32 MaxMagnitudeNumber(Decimal32 x, Decimal32 y);
        public static Decimal32 MinMagnitude(Decimal32 x, Decimal32 y);
        public static Decimal32 MinMagnitudeNumber(Decimal32 x, Decimal32 y);
        static bool INumberBase<Decimal32>.TryConvertFromChecked<TOther>(TOther value, out Decimal32 result);
        static bool INumberBase<Decimal32>.TryConvertFromSaturating<TOther>(TOther value, out Decimal32 result);
        static bool INumberBase<Decimal32>.TryConvertFromTruncating<TOther>(TOther value, out Decimal32 result);
        static bool INumberBase<Decimal32>.TryConvertToChecked<TOther>(Decimal32 value, [MaybeNullWhen(false)] out TOther result);
        static bool INumberBase<Decimal32>.TryConvertToSaturating<TOther>(Decimal32 value, [MaybeNullWhen(false)] out TOther result);
        static bool INumberBase<Decimal32>.TryConvertToTruncating<TOther>(Decimal32 value, [MaybeNullWhen(false)] out TOther result);

        //
        // IPowerFunctions
        //
        public static Decimal32 Pow(Decimal32 x, Decimal32 y); // PLATINUM

        //
        // IRootFunctions
        //
        public static Decimal32 Cbrt(Decimal32 x); // PLATINUM
        public static Decimal32 Hypot(Decimal32 x, Decimal32 y); // PLATINUM
        public static Decimal32 RootN(Decimal32 x, int n); // PLATINUM
        public static Decimal32 Sqrt(Decimal32 x);

        //
        // ISignedNumber
        //
        public static Decimal32 NegativeOne { get; }

        //
        // ISubtractionOperators
        //
        public static Decimal32 operator -(Decimal32 left, Decimal32 right);

        //
        // ITrigonometricFunctions
        //
        public static Decimal32 Acos(Decimal32 x); // PLATINUM
        public static Decimal32 AcosPi(Decimal32 x); // PLATINUM
        public static Decimal32 Asin(Decimal32 x); // PLATINUM
        public static Decimal32 AsinPi(Decimal32 x); // PLATINUM
        public static Decimal32 Atan(Decimal32 x); // PLATINUM
        public static Decimal32 AtanPi(Decimal32 x); // PLATINUM
        public static Decimal32 Cos(Decimal32 x); // PLATINUM
        public static Decimal32 CosPi(Decimal32 x); // PLATINUM
        public static Decimal32 Sin(Decimal32 x); // PLATINUM
        public static (Decimal32 Sin, Decimal32 Cos) SinCos(Decimal32 x); // PLATINUM
        public static (Decimal32 SinPi, Decimal32 CosPi) SinCosPi(Decimal32 x); // PLATINUM
        public static Decimal32 SinPi(Decimal32 x); // PLATINUM
        public static Decimal32 Tan(Decimal32 x); // PLATINUM
        public static Decimal32 TanPi(Decimal32 x); // PLATINUM

        //
        // IUnaryNegationOperators
        //
        public static Decimal32 operator -(Decimal32 value);

        //
        // IUnaryPlusOperators
        //
        public static Decimal32 operator +(Decimal32 value);
    }
}

```
