# API Review 04/14/2022

## Interlocked.CompareExchange for byte/short/sbyte/ushort

**Approved** | [#runtime/64658](https://github.com/dotnet/runtime/issues/64658#issuecomment-1099428747)

* Looks good, but we should probably also do `Exchange`

```C#
namespace System.Threading
{
    public static class Interlocked
    {
        public static byte CompareExchange(ref byte location1, byte value, byte comparand);
        public static sbyte CompareExchange(ref sbyte location1, sbyte value, sbyte comparand);
        public static short CompareExchange(ref short location1, short value, short comparand);
        public static ushort CompareExchange(ref ushort location1, ushort value, ushort comparand);

        public static byte Exchange(ref byte location1, byte value);
        public static sbyte Exchange(ref sbyte location1, sbyte value);
        public static short Exchange(ref short location1, short value);
        public static ushort Exchange(ref ushort location1, ushort value);
    }
}
```
## like #24694，Interlocked.CompareExchange/Exchange still missing for UIntPtr

**Approved** | [#runtime/45103](https://github.com/dotnet/runtime/issues/45103#issuecomment-1099438873)

* Looks good as proposed

```C#
namespace System.Threading
{
    public static class Interlocked
    {
        public static extern UIntPtr CompareExchange(ref UIntPtr location1, UIntPtr value, UIntPtr comparand);
        public static extern UIntPtr Exchange(ref UIntPtr location1, UIntPtr value);
    }
}
```
## Add support for Int128 and UInt128 data types

**Approved** | [#runtime/67151](https://github.com/dotnet/runtime/issues/67151#issuecomment-1099468310)

* We should swap the constructor parameter
    - This way a logical 128 bit literal like `0x8000_0000_0000_0000_0000_0000_0000_0000` would be written like `new Int128(0x8000_0000_0000_0000, 0x0000_0000_0000_0000)`
* We probably want to file bugs for serializers to consider supporting this type

```C#
namespace System
{
    public readonly struct Int128
        : IComparable,
          IComparable<Int128>,
          IEquatable<Int128>,
          IBinaryInteger<Int128>,
          IMinMaxValue<Int128>,
          ISpanFormattable,
          ISignedNumber<Int128>
    {
        // Constructors

        public Int128(ulong upper, ulong lower);

        // Properties

        public static Int128 AdditiveIdentity { get; }

        public static Int128 MaxValue { get; }
        public static Int128 MinValue { get; }

        public static Int128 MultiplicativeIdentity { get; }

        public static Int128 NegativeOne { get; }
        public static Int128 One { get; }
        public static Int128 Zero { get; }

        // Addition Operators

        public static Int128 operator +(Int128 left, Int128 right);
        public static Int128 operator checked +(Int128 left, Int128 right);

        // Bitwise Operators

        public static Int128 operator &(Int128 left, Int128 right);
        public static Int128 operator |(Int128 left, Int128 right);
        public static Int128 operator ^(Int128 left, Int128 right);
        public static Int128 operator ~(Int128 left, Int128 right);

        // Comparison Operators

        public static bool operator <(Int128 left, Int128 right);
        public static bool operator <=(Int128 left, Int128 right);
        public static bool operator >(Int128 left, Int128 right);
        public static bool operator >=(Int128 left, Int128 right);

        // Conversion From Operators

        public static implicit operator Int128(byte value);
        public static implicit operator Int128(char value);
        public static implicit operator Int128(short value);
        public static implicit operator Int128(int value);
        public static implicit operator Int128(long value);
        public static implicit operator Int128(nint value);
        public static implicit operator Int128(sbyte value);
        public static implicit operator Int128(ushort value);
        public static implicit operator Int128(uint value);
        public static implicit operator Int128(ulong value);
        public static implicit operator Int128(nuint value);

        public static explicit operator Int128(double value);
        public static explicit operator Int128(decimal value);
        public static explicit operator Int128(Half value);
        public static explicit operator Int128(float value);

        public static explicit operator Int128(UInt128 value);

        // Conversion To Operators

        public static explicit operator byte(Int128 value);
        public static explicit operator char(Int128 value);
        public static explicit operator short(Int128 value);
        public static explicit operator int(Int128 value);
        public static explicit operator long(Int128 value);
        public static explicit operator nint(Int128 value);
        public static explicit operator sbyte(Int128 value);
        public static explicit operator ushort(Int128 value);
        public static explicit operator uint(Int128 value);
        public static explicit operator ulong(Int128 value);
        public static explicit operator nuint(Int128 value);

        public static explicit operator double(Int128 value);
        public static explicit operator decimal(Int128 value);
        public static explicit operator Half(Int128 value);
        public static explicit operator float(Int128 value);

        // Decrement Operators

        public static Int128 operator --(Int128 value);
        public static Int128 operator checked --(Int128 value);

        // Division Operators

        public static Int128 operator /(Int128 left, Int128 right);
        public static Int128 operator checked /(Int128 left, Int128 right);

        // Equality Operators

        public static bool operator ==(Int128 left, Int128 right);
        public static bool operator !=(Int128 left, Int128 right);

        // Increment Operators

        public static Int128 operator ++(Int128 value);
        public static Int128 operator checked ++(Int128 value);

        // Modulus Operators

        public static Int128 operator %(Int128 left, Int128 right);

        // Multiply Operators

        public static Int128 operator *(Int128 left, Int128 right);
        public static Int128 operator checked *(Int128 left, Int128 right);

        // Shift Operators

        public static Int128 operator <<(Int128 value, int shiftAmount);
        public static Int128 operator >>(Int128 value, int shiftAmount);
        public static Int128 operator >>>(Int128 value, int shiftAmount);

        // Subtraction Operators

        public static Int128 operator -(Int128 left, Int128 right);
        public static Int128 operator checked -(Int128 left, Int128 right);

        // Unary Negation/Plus Operators

        public static Int128 operator +(Int128 value);
        public static Int128 operator -(Int128 value);
        public static Int128 operator checked -(Int128 value);

        // Comparison Methods

        public int CompareTo(object? obj);
        public int CompareTo(Int128 other);

        // Equality Methods

        public override bool Equals([NotNullWhen(true)] object? obj);
        public bool Equals(Int128 other);

        // Hashing Methods

        public override int GetHashCode();

        // Parsing Methods

        public static Int128 Parse(string s);
        public static Int128 Parse(string s, NumberStyles style);
        public static Int128 Parse(string s, IFormatProvider? provider);
        public static Int128 Parse(string s, NumberStyles style, IFormatProvider? provider);
        public static Int128 Parse(ReadOnlySpan<char> s, IFormatProvider? provider);
        public static Int128 Parse(ReadOnlySpan<char> s, NumberStyles style = NumberStyles.Integer, IFormatProvider? provider = null);

        public static bool TryParse([NotNullWhen(true)] string? s, out Int128 result);
        public static bool TryParse(ReadOnlySpan<char> s, out Int128 result);
        public static bool TryParse([NotNullWhen(true)] string? s, IFormatProvider? provider, out long result);
        public static bool TryParse([NotNullWhen(true)] string? s, NumberStyles style, IFormatProvider? provider, out Int128 result);
        public static bool TryParse(ReadOnlySpan<char> s, IFormatProvider? provider, out Int128 result);
        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out Int128 result);

        // Formatting Methods

        public override string ToString();
        public string ToString(IFormatProvider? provider);
        public string ToString(string? format);
        public string ToString(string? format, IFormatProvider? provider);

        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default, IFormatProvider? provider = null);

        // Binary Integer Methods

        public static Int128 LeadingZeroCount(Int128 value);
        public static Int128 PopCount(Int128 value);
        public static Int128 RotateLeft(Int128 value);
        public static Int128 RotateRight(Int128 value);
        public static Int128 TrailingZeroCount(Int128 value);

        // Binary Number Methods

        public static bool IsPow2(Int128 value);
        public static Int128 Log2(Int128 value);

        // Number Methods

        public static Int128 Abs(Int128 value);
        public static Int128 Clamp(Int128 value, Int128 min, Int128 max);
        public static Int128 CreateChecked<TOther>(TOther value) where TOther : INumber<TOther>;
        public static Int128 CreateSaturating<TOther>(TOther value) where TOther : INumber<TOther>;
        public static Int128 CreateTruncating<TOther>(TOther value) where TOther : INumber<TOther>;
        public static (Int128 Quotient, Int128 Remainder) DivRem(Int128 left, Int128 right);
        public static Int128 Max(Int128 x, Int128 y);
        public static Int128 Min(Int128 x, Int128 y);
        public static Int128 Sign(Int128 value);
        public static bool TryCreate<TOther>(TOther value, out Int128 result) where TOther : INumber<TOther>;
    }

    public readonly struct UInt128
        : IComparable,
          IComparable<UInt128>,
          IEquatable<UInt128>,
          IBinaryInteger<UInt128>,
          IMinMaxValue<UInt128>,
          ISpanFormattable,
          IUnsignedNumber<UInt128>
    {
        // Same members as Int128, but taking/returning UInt128
        // NegativeOne is not defined/exposed
        // Conversion From/To operators differ as per below

        // Conversion From Operators

        public static implicit operator UInt128(byte value);
        public static implicit operator UInt128(char value);
        public static implicit operator UInt128(ushort value);
        public static implicit operator UInt128(uint value);
        public static implicit operator UInt128(ulong value);
        public static implicit operator UInt128(nuint value);

        public static explicit operator UInt128(short value);
        public static explicit operator UInt128(int value);
        public static explicit operator UInt128(long value);
        public static explicit operator UInt128(nint value);
        public static explicit operator UInt128(sbyte value);

        public static explicit operator UInt128(double value);
        public static explicit operator UInt128(decimal value);
        public static explicit operator UInt128(Half value);
        public static explicit operator UInt128(float value);

        public static explicit operator UInt128(Int128 value);

        // Conversion To Operators

        public static explicit operator byte(UInt128 value);
        public static explicit operator char(UInt128 value);
        public static explicit operator short(UInt128 value);
        public static explicit operator int(UInt128 value);
        public static explicit operator long(UInt128 value);
        public static explicit operator nint(UInt128 value);
        public static explicit operator sbyte(UInt128 value);
        public static explicit operator ushort(UInt128 value);
        public static explicit operator uint(UInt128 value);
        public static explicit operator ulong(UInt128 value);
        public static explicit operator nuint(UInt128 value);

        public static explicit operator double(UInt128 value);
        public static explicit operator decimal(UInt128 value);
        public static explicit operator Half(UInt128 value);
        public static explicit operator float(UInt128 value);
    }
}
```
## {ReadOnly}Span(ref T) constructor

**Approved** | [#runtime/67445](https://github.com/dotnet/runtime/issues/67445#issuecomment-1099481860)

* Looks good as proposed
* Once C# supports `ref readonly` (csharplang#6010) we should change the constructor for `ReadOnlySpan<T>` to use that instead.

```C#
namespace System;

public partial ref struct Span<T>
{
    public Span(ref T reference);
}
public partial ref struct ReadOnlySpan<T>
{
    public ReadOnlySpan(in T reference);
}
```
## Add RuntimeFeature field to identify support for numeric IntPtr type

**Approved** | [#runtime/67966](https://github.com/dotnet/runtime/issues/67966#issuecomment-1099483217)

* Looks good as proposed

```C#
namespace System.Runtime.CompilerServices;

public static partial class RuntimeFeature
{
    public const string NumericIntPtr = nameof(NumericIntPtr);
}
```
## Expose Buffer.ZeroMemory(void*, nuint)

**Approved** | [#runtime/63547](https://github.com/dotnet/runtime/issues/63547#issuecomment-1099498469)

* Makes sense but we believe it should live on `NativeMemory` instead.

```C#
namespace System.Runtime.InteropServices;

public static class NativeMemory
{
    public static void ZeroMemory(void* ptr, nuint byteCount);
}

```
## Add ServiceCollection.MakeReadOnly()

**Approved** | [#runtime/66126](https://github.com/dotnet/runtime/issues/66126#issuecomment-1099509121)

* Looks good as proposed

```C#
namespace Microsoft.Extensions.DependencyInjection;

public partial class ServiceCollection
{
    public void MakeReadOnly();
}
```
## Add support for converting to/from `IntPtr` for some handles

**Approved** | [#runtime/67090](https://github.com/dotnet/runtime/issues/67090#issuecomment-1099523062)

* For consistency, you might want to *also* add explicit casting operators too, like `GCHandle` does.

```C#
namespace System;

public struct RuntimeTypeHandle
{
    public static RuntimeTypeHandle FromIntPtr(IntPtr value);
    public static IntPtr ToIntPtr(RuntimeTypeHandle value);
}

public struct RuntimeFieldHandle
{
    public static RuntimeFieldHandle FromIntPtr(IntPtr value);
    public static IntPtr ToIntPtr(RuntimeFieldHandle value);
}

public struct RuntimeMethodHandle
{
    public static RuntimeMethodHandle FromIntPtr(IntPtr value);
    public static IntPtr ToIntPtr(RuntimeMethodHandle value);
}
```
## Add ObsoleteAttribute.BinaryCompatOnly

**NeedsWork** | [#runtime/65965](https://github.com/dotnet/runtime/issues/65965#issuecomment-1099543834)

We should probably sit down and list all the places where this would be interesting to use and see what the alternatives would be. For instance, for `CallerIdentity` one options is to just remove the old members for the ref assembly and ensure that F# and VB and support the new attribute as well.

But we love the idea :-)
