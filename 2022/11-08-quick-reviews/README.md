# API Review 11/08/2022

## Vectorize IndexOfAny on more than 5 chars

**Approved** | [#runtime/68328](https://github.com/dotnet/runtime/issues/68328#issuecomment-1307661637) | [Video](https://www.youtube.com/watch?v=_pCaRrqBQdQ&t=0h0m0s)

* Let's move the types to `System.Buffers` as they are more advanced
* Let's not have a generic factory method for now as we're only planning to optimize for `byte` and `char`
* We should make sure that the debugger displays something useful for instances of `IndexOfAnyValues<T>`

```C#
namespace System;

public static partial class MemoryExtensions
{
    public static int IndexOfAny<T>(this ReadOnlySpan<T> span, IndexOfAnyValues<T> values) where T : IEquatable<T>?;
    public static int IndexOfAnyExcept<T>(this ReadOnlySpan<T> span, IndexOfAnyValues<T> values) where T : IEquatable<T>?;
    public static int LastIndexOfAny<T>(this ReadOnlySpan<T> span, IndexOfAnyValues<T> values) where T : IEquatable<T>?;
    public static int LastIndexOfAnyExcept<T>(this ReadOnlySpan<T> span, IndexOfAnyValues<T> values) where T : IEquatable<T>?;
}
```

```C#
namespace System.Buffers;

public static class IndexOfAnyValues
{
    public static IndexOfAnyValues<byte> Create(ReadOnlySpan<byte> values);
    public static IndexOfAnyValues<char> Create(ReadOnlySpan<char> values);
}

public class IndexOfAnyValues<T> where T : IEquatable<T>?
{
}
```
## ArgumentOutOfRangeException helper methods

**Approved** | [#runtime/69590](https://github.com/dotnet/runtime/issues/69590#issuecomment-1307687726) | [Video](https://www.youtube.com/watch?v=_pCaRrqBQdQ&t=0h28m18s)

* We should remove the `struct` constraint as it's not necessary
* On the first three methods, we don't need the `ISignedNumber<T>` and `IComparable<T>` constraints
* We don't like `ThrowIfNotBetween` due the ambiguity if the third argument is inclusive/exclusive and/or a boundary or a length.

```C#
namespace System;

public partial class ArgumentOutOfRangeException
{
    public static void ThrowIfZero<T>(T value, [CallerArgumentExpression("value")] string? paramName = null) 
        where T : INumberBase<T>;

    public static void ThrowIfNegative<T>(T value, [CallerArgumentExpression("value")] string? paramName = null) 
        where T : INumberBase<T>;

    public static void ThrowIfNegativeOrZero<T>(T value, [CallerArgumentExpression("value")] string? paramName = null) 
        where T : INumberBase<T>;
    
    public static void ThrowIfGreaterThan<T>(T value, T other, [CallerArgumentExpression("value")] string? paramName = null) 
        where T : IComparable<T>;

    public static void ThrowIfGreaterThanOrEqual<T>(T value, T other, [CallerArgumentExpression("value")] string? paramName = null) 
        where T : IComparable<T>;
    
    public static void ThrowIfLessThan<T>(T value, T other, [CallerArgumentExpression("value")] string? paramName = null)
        where T : IComparable<T>;

    public static void ThrowIfLessThanOrEqual<T>(T value, T other, [CallerArgumentExpression("value")] string? paramName = null) 
        where T : IComparable<T>;
}
```
## Align System.Reflection.PortableExecutable.DllCharacteristics with PE spec

**Approved** | [#runtime/71362](https://github.com/dotnet/runtime/issues/71362#issuecomment-1307707216) | [Video](https://www.youtube.com/watch?v=_pCaRrqBQdQ&t=1h2m7s)

* Seems fine as the underlying format is based on the Windows spec and this enum was designed as a pass-through

```C#
namespace System.Reflection.PortableExecutable;

public enum DllCharacteristics : ushort
{
    // Existing values
    ForceIntegrity = 0x0080,
    ControlFlowGuard = 0x4000
} 
```
## Base64.IsValid

**Approved** | [#runtime/76020](https://github.com/dotnet/runtime/issues/76020#issuecomment-1307735825) | [Video](https://www.youtube.com/watch?v=_pCaRrqBQdQ&t=1h7m3s)

* We'd like to pursue option (4) from @stephentoub's [suggestion](https://github.com/dotnet/runtime/issues/76020#issuecomment-1274968258):
    - Change `Base64` to allow the same whitespace characters as `Convert.ToBase64`
    - We should output the sizes
    - If this proves problematic, we can produce overloads/alternative methods that disallow whitespace
    - Which happens to match the [previously approved API shape](https://github.com/dotnet/runtime/issues/76020#issuecomment-1259936196)

```C#
namespace System.Buffers.Text;

public static class Base64
{
    public static bool IsValid(ReadOnlySpan<char> base64Text);
    public static bool IsValid(ReadOnlySpan<char> base64Text, out int decodedLength);
    public static bool IsValid(ReadOnlySpan<byte> base64TextUtf8);
    public static bool IsValid(ReadOnlySpan<byte> base64TextUtf8, out int decodedLength);
}
```
## XxHash128

**Approved** | [#runtime/77885](https://github.com/dotnet/runtime/issues/77885#issuecomment-1307753878) | [Video](https://www.youtube.com/watch?v=_pCaRrqBQdQ&t=1h33m15s)

* Note: .NET Standard won't have the `UInt128` methods as the type doesn't exist for .NET Standard today

```C#
namespace System.IO.Hashing;

public partial sealed class Crc32 : NonCryptographicHashAlgorithm
{
    public static uint HashToUInt32(ReadOnlySpan<byte> source);
    public uint GetCurrentHashAsUInt32();
}

public partial sealed class XxHash32 : NonCryptographicHashAlgorithm
{
    public static uint HashToUInt32(ReadOnlySpan<byte> source, int seed = 0);
    public uint GetCurrentHashAsUInt32();
}

public partial sealed class Crc64 : NonCryptographicHashAlgorithm
{
    public static ulong HashToUInt64(ReadOnlySpan<byte> source);
    public ulong GetCurrentHashAsInt64();
}

public partial sealed class XxHash64 : NonCryptographicHashAlgorithm
{
    public static ulong HashToUInt64(ReadOnlySpan<byte> source, long seed = 0);
    public ulong GetCurrentHashAsUInt64();
}

public partial sealed class XxHash3 : NonCryptographicHashAlgorithm
{
    public static ulong HashToUInt64(ReadOnlySpan<byte> source, long seed = 0);
    public ulong GetCurrentHashAsUInt64();
}

public partial sealed class XxHash128 : NonCryptographicHashAlgorithm
{
    public static Int128 HashToUInt128(ReadOnlySpan<byte> source, long seed = 0);
    public Int128 GetCurrentHashAsUInt128();
}
```
