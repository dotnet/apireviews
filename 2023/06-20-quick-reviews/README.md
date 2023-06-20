# API Review 06/20/2023

## Attribute definitions included in assemblies using Nullable on the latest framework

**Approved** | [#runtime/85612](https://github.com/dotnet/runtime/issues/85612#issuecomment-1599239752) | [Video](https://www.youtube.com/watch?v=K-IlNtfztE0&t=0h0m0s)

* Looks good as proposed
    - Made the attributes public, added constructors, and removed marker attributes that specify these are generated/embedded.
* Moving forward, we'll generally strive add them to the framework, but there might be cases where for expedience and ability for the compiler to tweak the encoding, we start with embedding and later make the final shape a public API
    - Generally speaking, these attributes would go into corlib
* Is this the full list?

```C#
namespace System.Runtime.CompilerServices;

public sealed class IsUnmanagedAttribute : Attribute
{
    public IsUnmanagedAttribute();
}

[AttributeUsage(AttributeTargets.Parameter, AllowMultiple = false, Inherited = false)]
public sealed class ScopedRefAttribute : Attribute
{
    public ScopedRefAttribute();
}

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Property | AttributeTargets.Field | AttributeTargets.Event | AttributeTargets.Parameter | AttributeTargets.ReturnValue | AttributeTargets.GenericParameter, AllowMultiple = false, Inherited = false)]
public sealed partial class NullableAttribute : Attribute
{
    public readonly byte[] NullableFlags;
    public NullableAttribute(byte value);
    public NullableAttribute(byte[] value);
}

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Struct | AttributeTargets.Method | AttributeTargets.Interface | AttributeTargets.Delegate, AllowMultiple = false, Inherited = false)]
public sealed partial class NullableContextAttribute : Attribute
{
    public readonly byte Flag;
    public NullableContextAttribute(byte value);
}

[AttributeUsage(AttributeTargets.Module, AllowMultiple = false, Inherited = false)]
public sealed partial class NullablePublicOnlyAttribute : Attribute
{
    public readonly bool IncludesInternals;
    public NullablePublicOnlyAttribute(bool value);
}
```
## RandomAccess.FlushToDisk

**Approved** | [#runtime/86836](https://github.com/dotnet/runtime/issues/86836#issuecomment-1599256531) | [Video](https://www.youtube.com/watch?v=K-IlNtfztE0&t=0h40m31s)

* Looks good as proposed

```C#
namespace System.IO;

public static class RandomAccess
{
    public static void FlushToDisk(SafeFileHandle handle);
}
```
## Provide a new NumberStyles option to stop parsing after the first invalid character

**Approved** | [#runtime/87171](https://github.com/dotnet/runtime/issues/87171#issuecomment-1599263704) | [Video](https://www.youtube.com/watch?v=K-IlNtfztE0&t=0h53m52s)

* Looks good as proposed
* For the default implementation we have three options:
    1. `TryParse` throws for `AllowTrailingInvalidCharacters`
    2. `TryParse` delegates to `Parse` masking off `AllowTrailingInvalidCharacters`
    3. `TryParse` delegates to `Parse` without masking and assumes `Parse` will throw when it doesn't understand `AllowTrailingInvalidCharacters`
* We decided for option (3)

```C#
namespace System.Globalization
{
    [Flags]
    public partial enum NumberStyles
    {
        AllowTrailingInvalidCharacters = 0x800
    }
}

namespace System.Numerics
{
    public interface INumberBase<TSelf>
    {
        public static virtual bool TryParse([NotNullWhen(true)] string? s,
                                            NumberStyles style,
                                            IFormatProvider? provider,
                                            [MaybeNullWhen(false)] out TSelf result,
                                            out int charsConsumed);
        public static virtual bool TryParse(ReadOnlySpan<char> s,
                                            NumberStyles style,
                                            IFormatProvider? provider,
                                            [MaybeNullWhen(false)] out TSelf result,
                                            out int charsConsumed); 
        public static virtual bool TryParse(ReadOnlySpan<byte> s,
                                            NumberStyles style,
                                            IFormatProvider? provider,
                                            [MaybeNullWhen(false)] out TSelf result,
                                            out int bytesConsumed);
    }
}
```
