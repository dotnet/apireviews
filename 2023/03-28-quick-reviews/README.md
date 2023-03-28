# API Review 03/28/2023

## Make ParameterBuilder abstract

**Approved** | [#runtime/83831](https://github.com/dotnet/runtime/issues/83831#issuecomment-1487312750) | [Video](https://www.youtube.com/watch?v=qrtsyWZkTK0&t=0h0m0s)

* The core method should be based on `ReadOnlySpan<byte>`
    - We should align the other ones we added this release

```diff
namespace System.Reflection.Emit;

-public class ParameterBuilder
+public abstract class ParameterBuilder
 {
-    internal ParameterBuilder();
+    protected ParameterBuilder();
     public virtual int Attributes { get; }
     public bool IsIn { get; }
     public bool IsOptional { get; }
     public bool IsOut { get; }
     public virtual string? Name { get; }
     public virtual int Position { get; }
     public virtual void SetConstant(object? defaultValue);
     public void SetCustomAttribute(ConstructorInfo con, byte[] binaryAttribute);
+    protected abstract void SetCustomAttributeCore(ConstructorInfo con, ReadOnlySpan<byte> binaryAttribute);
     public void SetCustomAttribute(CustomAttributeBuilder customBuilder);
 }
```
## Add persist able AssemblyBuilder implementation

**Approved** | [#runtime/83988](https://github.com/dotnet/runtime/issues/83988#issuecomment-1487355324) | [Video](https://www.youtube.com/watch?v=qrtsyWZkTK0&t=0h15m16s)

* Looks good as proposed

```C#
namespace System.Reflection.Emit;

public partial class AssemblyBuilder
{
    // Existing APIs
    // [RequiresDynamicCode("Defining a dynamic assembly requires dynamic code.")]
    // public static AssemblyBuilder DefineDynamicAssembly(AssemblyName name, AssemblyBuilderAccess access);
    // [RequiresDynamicCode("Defining a dynamic assembly requires dynamic code.")]
    // public static AssemblyBuilder DefineDynamicAssembly(AssemblyName name, AssemblyBuilderAccess access, System.Collections.Generic.IEnumerable<CustomAttributeBuilder>? assemblyAttributes);

    // New API - note that it does not have RequiresDynamicCode annotation
    public static AssemblyBuilder DefinePersistedAssembly(AssemblyName name, Assembly coreAssembly, IEnumerable<CustomAttributeBuilder>? assemblyAttributes = null);

    public void Save(Stream stream);
    public void Save(string assemblyFileName);
    protected abstract void SaveCore(Stream stream);
}
```
## Add support for Ed25519

**Approved** | [#runtime/63174](https://github.com/dotnet/runtime/issues/63174#issuecomment-1487413860) | [Video](https://www.youtube.com/watch?v=qrtsyWZkTK0&t=0h44m38s)

* The `IsSupported` properties should use `[UnsupportedOSPlatformGuard]` annotations

```C#
namespace System.Security.Cryptography;

public abstract partial class Ed25519 : AsymmetricAlgorithm
{
    public const int SignatureSizeInBytes = 64;

    [UnsupportedOSPlatformGuard("android")]
    [UnsupportedOSPlatformGuard("browser")]
    [UnsupportedOSPlatformGuard("ios")]
    [UnsupportedOSPlatformGuard("tvos")]
    [UnsupportedOSPlatformGuard("windows")]
    public static bool IsSupported { get; }

    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("windows")]
    public static new Ed25519 Create();

    protected Ed25519();
    public abstract bool HasPrivateKey { get; }

    public byte[] ExportPrivateKey();
    public int ExportPrivateKey(Span<byte> destination);
    public bool TryExportPrivateKey(Span<byte> destination, out int bytesWritten);
    protected abstract int ExportPrivateKeyCore(Span<byte> destination);
    public byte[] ExportPublicKey();
    public int ExportPublicKey(Span<byte> destination);
    public bool TryExportPublicKey(Span<byte> destination, out int bytesWritten);
    protected abstract int ExportPublicKeyCore(Span<byte> destination);

    public abstract void GenerateKey();

    public void ImportPrivateKey(byte[] privateKey);
    public void ImportPrivateKey(ReadOnlySpan<byte> privateKey);
    protected abstract void ImportPrivateKeyCore(ReadOnlySpan<byte> privateKey);

    public void ImportPublicKey(byte[] publicKey);
    public void ImportPublicKey(ReadOnlySpan<byte> publicKey);
    protected abstract void ImportPublicKeyCore(ReadOnlySpan<byte> publicKey);
    public byte[] SignData(byte[] data);
    public byte[] SignData(ReadOnlySpan<byte> data);
    public int SignData(ReadOnlySpan<byte> data, Span<byte> destination);
    public bool TrySignData(ReadOnlySpan<byte> data, Span<byte> destination, out int bytesWritten);
    protected abstract int SignDataCore(ReadOnlySpan<byte> data, Span<byte> destination);

    public bool VerifyData(byte[] data, byte[] signature);
    public bool VerifyData(ReadOnlySpan<byte> data, ReadOnlySpan<byte> signature);
    protected abstract bool VerifyDataCore(ReadOnlySpan<byte> data, ReadOnlySpan<byte> signature);
}
public sealed partial class Ed25519OpenSsl : Ed25519
{
    [UnsupportedOSPlatformGuard("android")]
    [UnsupportedOSPlatformGuard("browser")]
    [UnsupportedOSPlatformGuard("ios")]
    [UnsupportedOSPlatformGuard("tvos")]
    [UnsupportedOSPlatformGuard("windows")]
    public static new bool IsSupported { get; }

    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("windows")]
    public Ed25519OpenSsl(SafeEvpPKeyHandle pkeyHandle);

    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("windows")]
    public Ed25519OpenSsl();
}
```

```C#
namespace System.Security.Cryptography.X509Certificates;

public sealed partial class PublicKey
{
    public Ed25519? GetEd25519PublicKey();
}

public partial class X509Certificate2
{
    public Ed25519? GetEd25519PublicKey();
    public Ed25519? GetEd25519PrivateKey();
    public X509Certificate2 CopyWithPrivateKey(Ed25519 privateKey);
}
```
## Convert.TryFromHexString

**Approved** | [#runtime/78472](https://github.com/dotnet/runtime/issues/78472#issuecomment-1487470651) | [Video](https://www.youtube.com/watch?v=qrtsyWZkTK0&t=1h26m14s)

* We should align the shape with the existing `Try(From/To)Base64` API
    - If we accept a span to write to, we should accept a span as the source
    - For string, it makes sense to have a null-rejecting overload
* `TryFromHexString` *should* throw if destination is too small because `Try`-pattern are supposed to have a single failure mode
    - However, that's not what the existing `TryFromBase64` method does
    - Convenience wise, we believe it's worth to return `false` for both cases here

```C#
namespace System;

public partial class Convert
{
    public static bool TryFromHexString(string source, Span<byte> destination, out int bytesWritten);
    public static bool TryFromHexString(ReadOnlySpan<char> source, Span<byte> destination, out int bytesWritten);
    public static bool TryToHexString(ReadOnlySpan<byte> source, Span<char> destination, out int charsWritten);
}
```

