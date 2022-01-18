# API Review 01/18/2022

## UnreachableException

**Approved** | [#runtime/35324](https://github.com/dotnet/runtime/issues/35324#issuecomment-1015687417) | [Video](https://www.youtube.com/watch?v=I9gitKnAn7c&t=0h0m0s)

We seem to have hit the most agreeable (perhaps "least disagreeable") position with System.Diagnostics, on the basis that it's rather like an assert (`System.Diagnostics.Debug.Assert`); and based on @jkotas feedback, the exception isn't really special... so `: Exception` instead of `: SystemException`.

```C#
namespace System.Diagnostics
{
    public sealed class UnreachableException : Exception
    {
        public UnreachableException()
            : base("The program executed an instruction that was thought to be unreachable.")
        {
        }

        public UnreachableException(string? message)
            : base(message)
        {
        }

        public UnreachableException(string? message, Exception? innerException)
            : base(message, innerException)
        {
        }
    }
}
```
## Extend `System.Runtime.InteropServices.NFloat` so it can replace the Xamarin in-box `nfloat` for .NET 7

**Approved** | [#runtime/63801](https://github.com/dotnet/runtime/issues/63801#issuecomment-1015702196) | [Video](https://www.youtube.com/watch?v=I9gitKnAn7c&t=0h13m1s)

Unless there's a very strong need for IConvertable, we should leave that out.  Otherwise, looks good as proposed.

```C#
namespace System.Runtime.InteropServices;

public readonly struct NFloat
    : IComparable,
      IComparable<NFloat>,
      IEquatable<NFloat>,   // Already exposed by 13788
      IFormattable
{
    // Xamarin exposes these as `static readonly` but that is "less optimizable" and not our normal convention
    public static NFloat Epsilon { get; }
    public static NFloat MaxValue { get; }
    public static NFloat MinValue { get; }
    public static NFloat NaN { get; }
    public static NFloat NegativeInfinity { get; }
    public static NFloat PositiveInfinity { get; }
    public static NFloat Size { get; }

    public static bool IsInfinity(NFloat value);
    public static bool IsNaN(NFloat value);
    public static bool IsNegativeInfinity(NFloat value);
    public static bool IsPositiveInfinity(NFloat value);

    // Parsing APIs, would match IParseable<NFloat> and IBinaryFloatingPoint<NFloat> if exposed
    public static NFloat Parse(string s);
    public static NFloat Parse(string s, NumberStyles style);
    public static NFloat Parse(string s, IFormatProvider? provider);
    public static NFloat Parse(string s, NumberStyles style, IFormatProvider? provider);
    public static bool TryParse([NotNullWhen(true)] string? s, out NFloat result);
    public static bool TryParse([NotNullWhen(true)] string? s, NumberStyles style, IFormatProvider? provider, out NFloat result);

    // Arithmetic Operators
    public static NFloat operator +(NFloat left, NFloat right);
    public static NFloat operator --(NFloat value);
    public static NFloat operator /(NFloat left, NFloat right);
    public static NFloat operator ++(NFloat value);
    public static NFloat operator %(NFloat left, NFloat right);
    public static NFloat operator *(NFloat left, NFloat right);
    public static NFloat operator -(NFloat left, NFloat right);
    public static NFloat operator +(NFloat value);
    public static NFloat operator -(NFloat value);

    // Comparison Operators
    public static bool operator ==(NFloat left, NFloat right);
    public static bool operator >(NFloat left, NFloat right);
    public static bool operator >=(NFloat left, NFloat right);
    public static bool operator !=(NFloat left, NFloat right);
    public static bool operator <(NFloat left, NFloat right);
    public static bool operator <=(NFloat left, NFloat right);

    // Explicit * to NFloat conversions
    public static explicit operator NFloat(decimal value);
    public static explicit operator NFloat(double value);
    public static explicit operator NFloat(nint value);     // Explicit is inconsistent with int and long
    public static explicit operator NFloat(nuint value);    // Explicit is inconsistent with uint and ulong, Xamarin is missing this one

    // Explicit NFloat to * conversions
    public static explicit operator byte(NFloat value);
    public static explicit operator char(NFloat value);
    public static explicit operator decimal(NFloat value);
    public static explicit operator short(NFloat value);
    public static explicit operator int(NFloat value);
    public static explicit operator long(NFloat value);
    public static explicit operator nint(NFloat value);
    public static explicit operator sbyte(NFloat value);
    public static explicit operator float(NFloat value);
    public static explicit operator ushort(NFloat value);
    public static explicit operator uint(NFloat value);
    public static explicit operator ulong(NFloat value);
    public static explicit operator nuint(NFloat value);   // Xamarin is missing this one

    // Implicit * to NFloat conversions
    public static implicit operator NFloat(byte value);
    public static implicit operator NFloat(char value);
    public static implicit operator NFloat(short value);
    public static implicit operator NFloat(int value);
    public static implicit operator NFloat(long value);
    public static implicit operator NFloat(sbyte value);
    public static implicit operator NFloat(float value);
    public static implicit operator NFloat(ushort value);
    public static implicit operator NFloat(uint value);
    public static implicit operator NFloat(ulong value);

    // Implicit NFloat to * conversions
    public static implicit operator double(NFloat value);

    // From IComparable and IComparable<NFloat>
    public bool CompareTo(NFloat other);
    public bool CompareTo(object other);

    // From object and IEquatable<NFloat>, already exposed
    public bool Equals(NFloat other);
    public bool Equals(object other);

    // From object and IFormattable
    public string ToString();
    public string ToString(IFormatProvider? provider);
    public string ToString(string? format);
    public string ToString(string? format, IFormatProvider? provider);
}
```
## Add CancellationToken to StreamReader.Read* methods

**Approved** | [#runtime/20824](https://github.com/dotnet/runtime/issues/20824#issuecomment-1015726336) | [Video](https://www.youtube.com/watch?v=I9gitKnAn7c&t=0h31m39s)

Looks good as proposed

```C#
namespace System.IO
{
    public partial class TextReader
    {
        public virtual ValueTask<string?> ReadLineAsync(CancellationToken cancellationToken) => throw null;
        public virtual Task<string> ReadToEndAsync(CancellationToken cancellationToken) => throw null;
    }
}
```
## Introduce RandomAccess.SetLength(SafeFileHandle handle) method

**Approved** | [#runtime/58376](https://github.com/dotnet/runtime/issues/58376#issuecomment-1015728073) | [Video](https://www.youtube.com/watch?v=I9gitKnAn7c&t=0h48m30s)

Looks good as proposed

```C#
namespace System.IO
{
    public partial static class RandomAccess
    {
        public static void SetLength(SafeFileHandle handle, long length);
    }
}
```
## Add IEquatable<T> to public Equals-overriding value types to enable CA1066/1067

**Approved** | [#runtime/63687](https://github.com/dotnet/runtime/issues/63687#issuecomment-1015741152) | [Video](https://www.youtube.com/watch?v=I9gitKnAn7c&t=0h50m52s)

Looks good as proposed.  (Declare on the duck-typeable ones, provide the implementation on the semantic-providing ones, fix AdjustmentRule, and suppress on the three outliers)
## Support for Intel SHA extensions

**Approved** | [#runtime/256](https://github.com/dotnet/runtime/issues/256#issuecomment-1015752414) | [Video](https://www.youtube.com/watch?v=I9gitKnAn7c&t=1h8m8s)

Generally looks good as proposed.

Parameter names and the `*Rounds` method names were aligned with the previously approved version.

```C#
namespace System.Runtime.Intrinsics.X86
{
    public abstract class Sha : X86Base
    {
       public static bool IsSupported { get; }

       public static class X64 : X86Base.X64 // unless it's SSE(2).
       {
           public static bool IsSupported { get; }
       }

        /// <summary>
        /// __m128i _mm_sha1msg1_epu32 (__m128i a, __m128i b)
        /// SHA1MSG1 xmm, xmm/m128
        /// </summary>
        public static Vector128<byte> Sha1MessageSchedule1(Vector128<byte> a, Vector128<byte> b) => MessageSchedule1(a, b);

        /// <summary>
        /// __m128i _mm_sha1msg2_epu32 (__m128i a, __m128i b)
        /// SHA1MSG2 xmm, xmm/m128
        /// </summary>
        public static Vector128<byte> Sha1MessageSchedule2(Vector128<byte> a, Vector128<byte> b) => MessageSchedule2(a, b);

        /// <summary>
        /// __m128i _mm_sha1nexte_epu32 (__m128i a, __m128i b)
        /// SHA1NEXTE xmm, xmm/m128
        /// </summary>
        public static Vector128<byte> Sha1NextE(Vector128<byte> a, Vector128<byte> b) => NextE(a, b);

        /// <summary>
        /// __m128i _mm_sha1rnds4_epu32 (__m128i a, __m128i b, const int func)
        /// SHA1RNDS4 xmm, xmm/m128, imm8
        /// </summary>
        public static Vector128<byte> Sha1FourRounds(Vector128<byte> a, Vector128<byte> b, byte func) => Rounds(state1, state2, func);

        /// <summary>
        /// __m128i _mm_sha256msg1_epu32 (__m128i a, __m128i b)
        /// SHA256MSG1 xmm, xmm/m128
        /// </summary>
        public static Vector128<byte> Sha256MessageSchedule1(Vector128<byte> a, Vector128<byte> b) => MessageSchedule1(a, b);

        /// <summary>
        /// __m128i _mm_sha256msg2_epu32 (__m128i a, __m128i b)
        /// SHA256MSG2 xmm, xmm/m128
        /// </summary>
        public static Vector128<byte> Sha256MessageSchedule2(Vector128<byte> a, Vector128<byte> b) => MessageSchedule2(a, b);

        /// <summary>
        /// __m128i _mm_sha256rnds2_epu32 (__m128i a, __m128i b, __m128i k)
        /// SHA256RNDS2 xmm, xmm/m128, &lt;XMM0&gt;
        /// </summary>
        public static Vector128<byte> Sha256TwoRounds(Vector128<byte> a, Vector128<byte> b, Vector128<byte> k) => Rounds(state1, state2, message);
    }
}
```
