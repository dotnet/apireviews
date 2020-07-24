# Quick Reviews 07/24/2020

## Consider adding HalfToInt16Bits and Int16BitsToHalf to BitConverter

**Approved** | [#runtime/38288](https://github.com/dotnet/runtime/issues/38288#issuecomment-663641179) | [Video](https://www.youtube.com/watch?v=TofmrPLEeFI&t=0h0m0s)

Looks good

```C#
namespace System
{
    public sealed class BitConverter
    {
        [MethodImpl(MethodImplOptions.AggressiveInlining)]
        public static short HalfToInt16Bits(Half value);

        [MethodImpl(MethodImplOptions.AggressiveInlining)]
        public static Half Int16BitsToHalf(short value);
    }
}
```

## Consider adding Half support to the BinaryPrimitives class

**Approved** | [#runtime/38456](https://github.com/dotnet/runtime/issues/38456#issuecomment-663643468) | [Video](https://www.youtube.com/watch?v=TofmrPLEeFI&t=0h5m36s)

* Looks good
* But we should also do `BinaryReader` and `BinaryWriter` as they are the more high-level counterparts
* @tannergooding, please scout the framework for support of `float` and `double` and see what makes sense for `Half`
* We should consider `Convert`, but the interface dispatch due to lack of type code might be challenging.

```C#
namespace System.Buffers.Binary
{
    public static class BinaryPrimitives
    {
        public static Half ReadHalfLittleEndian(ReadOnlySpan<byte> source);
        public static Half ReadHalfBigEndian(ReadOnlySpan<byte> source);
        public static bool TryReadHalfLittleEndian(ReadOnlySpan<byte> source, out Half);
        public static bool TryReadHalfBigEndian(ReadOnlySpan<byte> source, out Half);

        public static void WriteHalfLittleEndian(Span<byte> destination, Half value);
        public static void WriteHalfBigEndian(Span<byte> destination, Half value);
        public static bool TryWriteHalfLittleEndian(Span<byte> destination, Half value);
        public static bool TryWriteHalfBigEndian(Span<byte> destination, Half value);
    }
}
namespace System.IO
{
    public partial class BinaryReader
    {
        public virtual Half ReadHalf();
    }
    public partial class BinaryWriter
    {
        public virtual Half Write(Half value);
    }
}
```

## Make Process.Start have a option to change handle inheritance

**Approved** | [#runtime/13943](https://github.com/dotnet/runtime/issues/13943#issuecomment-663652191) | [Video](https://www.youtube.com/watch?v=TofmrPLEeFI&t=0h10m48s)

* It shouldn't default to `true`, it should return the actual behavior of the underlying platform.
* Marking the setter as platform specific makes sense, assuming we can't (or don't want to) implement the necessary gymnastics.
* @adamsitnik @eiriktsarpalis please check what the possible behavior/desirable behavior for Linux/Unix/macOS is

```C#
namespace System.Diagnostics
{
    public partial class ProcessStartInfo
    {
        public bool InheritHandles
        {
            get;
            [MinimumOSPlatform("windows7.0")]
            set;
        }
    }
}
```

## Consider exposing `System.Diagnostics.StackTraceHiddenAttribute` publicly

**Approved** | [#runtime/29681](https://github.com/dotnet/runtime/issues/29681#issuecomment-663656251) | [Video](https://www.youtube.com/watch?v=TofmrPLEeFI&t=0h31m0s)

* Since we can apply it to classes and structs, it seem one should be able to apply it to properties and events as well. Should we add `Property`, `Event`, `Delegate`?
* We considered `Interface` (DIM), but that seems ill-defined for non-DIMs. If people wanted them on DIMs, they can apply it to the method

```C#
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Class |
                    AttributeTargets.Method |
                    AttributeTargets.Constructor |
                    AttributeTargets.Struct, Inherited = false)]
    public sealed class StackTraceHiddenAttribute : Attribute
    {
        public StackTraceHiddenAttribute();
    }
}
```

## Add ReadOnlySpan<char> overloads to JsonSerializer.Deserialize

**Approved** | [#runtime/1235](https://github.com/dotnet/runtime/issues/1235#issuecomment-663657309) | [Video](https://www.youtube.com/watch?v=TofmrPLEeFI&t=0h41m26s)

* Looks good as proposed

```C#
public static partial class JsonSerializer
{
    public static object? Deserialize(ReadOnlySpan<char> json, Type returnType, JsonSerializerOptions? options = null);
    [return: MaybeNull]
    public static TValue Deserialize<TValue>(ReadOnlySpan<char> json, JsonSerializerOptions? options = null);
}
```

## Add the ability to parse/format a float/double from/to a hexadecimal literal

**Approved** | [#runtime/1630](https://github.com/dotnet/runtime/issues/1630#issuecomment-663670728) | [Video](https://www.youtube.com/watch?v=TofmrPLEeFI&t=0h43m55s)

* API wise, the only change is the `NumberStyles` addition of `HexFloat`
* Behavior wise, this changes parsing of `float` and `double` to support the `x` format specifier. This isn't a breaking change because `float` and `double` throw when `x` is passed.
* We considered adding `AllowHexPrefix` because integers don't allow `0x` as  the prefix, while `float` and `double` would. However, for floats we want the prefix to be required, so `AllowHexPrefix` would necessary be a separate bit. Combining it with `HexFloat` would make it optional. Thus, we can do it later.

```C#
namespace System.Globalization
{
    public partial enum NumberStyles
    {
        HexFloat = AllowLeadingWhite | AllowTrailingWhite | AllowLeadingSign | AllowHexSpecifier | AllowDecimalPoint
    }
}
```

## Provide an overload of float/double.ToString that allow precision greater than 99

**Approved** | [#runtime/30657](https://github.com/dotnet/runtime/issues/30657#issuecomment-663675920) | [Video](https://www.youtube.com/watch?v=TofmrPLEeFI&t=1h14m26s)

* There are issues with supporting more thant 99 digits of precision, some are in parsing behavior as pointed out by the OP, some of them are in public data structures which are size limited, and some are likely in the implementation.
* Normally, it would seem better to extend the existing format specifier because it works better with resource strings and localization
* However, given that production code is very unlikely to ever use more than 99 digits, it seems acceptable to have separate overloads to override the precision for debugging purposes

```C#
namespace System
{
    public struct Single
    {
        public string ToString(char format, int precision);
    }
    public struct Double
    {
        public string ToString(char format, int precision);
    }
}
```

## Introduce overloads of Enum.Parse/TryParse that accept ReadOnlySpan<char>

**Approved** | [#runtime/1916](https://github.com/dotnet/runtime/issues/1916#issuecomment-663679040) | [Video](https://www.youtube.com/watch?v=TofmrPLEeFI&t=1h28m11s)

* We should remove the `Enum` constraint to match the other overloads
* Otherwise, looks good

```C#
namespace System
{
    public partial class Enum
    {
        public static TEnum Parse<TEnum>(ReadOnlySpan<char> value) where TEnum : struct;
        public static TEnum Parse<TEnum>(ReadOnlySpan<char> value, bool ignoreCase) where TEnum : struct;
        public static bool TryParse<TEnum>(ReadOnlySpan<char> value, out TEnum result) where TEnum : struct;
        public static bool TryParse<TEnum>(ReadOnlySpan<char> value, bool ignoreCase, out TEnum result) where TEnum : struct;

        public static object Parse(Type enumType, ReadOnlySpan<char> value);
        public static object Parse(Type enumType, ReadOnlySpan<char> value, bool ignoreCase);
        public static bool TryParse(Type enumType, ReadOnlySpan<char> value, out object result);
        public static bool TryParse(Type enumType, ReadOnlySpan<char> value, bool ignoreCase, out object result);
    }
}
```

## Add a delegate for OnDirectoryFinished()

**NeedsWork** | [#runtime/1953](https://github.com/dotnet/runtime/issues/1953#issuecomment-663688012) | [Video](https://www.youtube.com/watch?v=TofmrPLEeFI&t=1h36m5s)

* It seems `OnDirectoryFinishedDelegate` should include the `FileSystemEntry entry`
* Should `ContinueOnErrorPredicate` provide some context? It seems one would want the container and the name the child to do anything useful for logging. If we see potential for more state, we may want to allow a struct, such as `FileSystemError` instead.
* @carlossanlop you may want to sync with @JeremyKuhne on how to evolve this API

```C#
namespace System.IO.Enumeration
{
    public partial class FileSystemEnumerable<TResult> : IEnumerable<TResult>
    {
        public delegate void OnDirectoryFinishedDelegate(ref FileSystemEntry entry);
        public delegate bool ContinueOnErrorPredicate(ref FileSystemEntry parent, ReadOnlySpan<char> entry, int error);

        public OnDirectoryFinishedDelegate? OnDirectoryFinishedAction { get; set; }
        public ContinueOnErrorPredicate? ShouldContinueOnErrorPredicate { get; set; }
     }
}
```

