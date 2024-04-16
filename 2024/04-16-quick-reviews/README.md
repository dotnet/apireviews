# API Review 04/16/2024

## New AssemblyName-like type for TypeName parsing

**Approved** | [#runtime/100867](https://github.com/dotnet/runtime/issues/100867#issuecomment-2059692150) | [Video](https://www.youtube.com/watch?v=SWpe2MQstt4&t=0h0m0s)


* An AssemblyNameInfo(AssemblyName) ctor was proposed, but decided it was not needed.
* We talked about adding other properties from AssemblyName that are representable in the textual form, but decided they can be added later.
* We removed the IEquatable-ness from the type and from TypeName.  When we figure out what the semantics should be we can add it back.

```c#
namespace System.Reflection.Metadata;

public sealed class AssemblyNameInfo
{
    public AssemblyNameInfo(string name, Version? version = null, string? cultureName = null, AssemblyNameFlags flags = AssemblyNameFlags.None,
   Collections.Immutable.ImmutableArray<byte> publicKeyOrToken = default);
    public string Name { get; }
    public string FullName { get; }
    public Version? Version { get; }
    public string? CultureName { get; }
    public AssemblyNameFlags Flags { get; }
    public ImmutableArray<byte> PublicKeyOrToken { get; }
    
    public AssemblyName ToAssemblyName();

    public static AssemblyNameInfo Parse(ReadOnlySpan<char> assemblyName);
    public static bool TryParse(ReadOnlySpan<char> assemblyName, [NotNullWhenAttribute(true)] out AssemblyNameInfo? result);
}
```

```diff
-public sealed partial class TypeName : IEquatable<TypeName>
+public sealed partial class TypeName
{
-    public string AssemblySimpleName { get; }
-    public System.Reflection.AssemblyName? GetAssemblyName();
+    public AssemblyNameInfo? AssemblyName { get; }
}
```

## Expose content length decoder on AsnDecoder

**Approved** | [#runtime/101125](https://github.com/dotnet/runtime/issues/101125#issuecomment-2059719398) | [Video](https://www.youtube.com/watch?v=SWpe2MQstt4&t=1h16m56s)

Looks good as proposed

```c#
namespace System.Formats.Asn1
{
    public static partial class AsnDecoder
    {
        // returns null for "indefinite length"
        // (local length 0, but keep seeking until you find the "end of indefinite length" marker)
        public static int? DecodeLength(
            ReadOnlySpan<byte> source,
            AsnEncodingRules ruleSet,
            out int bytesConsumed);
       
        public static bool TryDecodeLength(
            ReadOnlySpan<byte> source,
            AsnEncodingRules ruleSet,
            out int? decodedLength,
            out int bytesConsumed);
    }
}
```
## SHAKE squeezing

**Approved** | [#runtime/99645](https://github.com/dotnet/runtime/issues/99645#issuecomment-2059748295) | [Video](https://www.youtube.com/watch?v=SWpe2MQstt4&t=1h34m3s)


* We renamed Duplicate to Clone
  * Discussed if it needed a "Deep" or "Shallow" prefix and decided "no".
  * And IClonable is still a no-no.
* We discussed adding a capability API (IsReadSupported, CanRead, etc) and decided we will leave it out and hope that the gap is closed before it's a real problem.

```c#
namespace System.Security.Cryptography;

public sealed partial class Shake128 : IDisposable {
    public byte[] Read(int outputLength);
    public void Read(System.Span<byte> destination);
    public void Reset();
    public Shake128 Clone();
}

public sealed partial class Shake256 : IDisposable {
    public byte[] Read(int outputLength);
    public void Read(System.Span<byte> destination);
    public void Reset();
    public Shake256 Clone();
}
```

