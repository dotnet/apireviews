# API Review 05/05/2026

## Process.TryGetProcessById()

**Approved** | [#runtime/101582](https://github.com/dotnet/runtime/issues/101582#issuecomment-4381432601) | [Video](https://www.youtube.com/watch?v=Z3vqvBGpFrk&t=0h0m0s)

Looks good as proposed.

```csharp
namespace System.Diagnostics
{
    public partial class Process
    {
        // we already have Process.GetProcessById(processId
        public static bool TryGetProcessById(int processId, [NotNullWhen(true)] out Process process);
        // overload with machineName is not included on purpose: it's Windows-specific
    }
}

namespace Microsoft.Win32.SafeHandles
{
    public partial class SafeProcessHandle
    {
        // we already have SafeProcessHandle.Open
        public static bool TryOpen(int processId, [NotNullWhen(true)] out SafeProcessHandle? processHandle);
    }
}
```
## MemoryExtensions.Min/Max

**Approved** | [#runtime/127083](https://github.com/dotnet/runtime/issues/127083#issuecomment-4381624410) | [Video](https://www.youtube.com/watch?v=Z3vqvBGpFrk&t=0h5m25s)

* Let's leave the TComparer overloads out until we see a comprehensive need (or a language feature to benefit from it, like functional interfaces)
* Should follow up to see what other Enumerable members should be copied over
* Since these are emulating Enumerable.Min/Max, they should behave the same... so if T is a value type empty throws, if it is a reference type it returns null.

```csharp
namespace System;

public partial static class MemoryExtensions
{
    public T? Min<T>(this ReadOnlySpan<T> span);
    public T? Min<T>(this ReadOnlySpan<T> span, IComparer<T> comparer);

    public T? Max<T>(this ReadOnlySpan<T> span);
    public T? Max<T>(this ReadOnlySpan<T> span, IComparer<T> comparer);
}
```
## System.Collection.BitArray - New constructors to pass a ReadOnlySpan<T> where T is int, bool or byte

**Approved** | [#runtime/80263](https://github.com/dotnet/runtime/issues/80263#issuecomment-4381646879) | [Video](https://www.youtube.com/watch?v=Z3vqvBGpFrk&t=0h35m14s)

Looks good as proposed

```csharp
namespace System.Collections;

public sealed partial class BitArray
{
    public BitArray(ReadOnlySpan<byte> bytes);
    public BitArray(ReadOnlySpan<bool> values);
    public BitArray(ReadOnlySpan<int> values);
}
```
## Add support for Ed25519

**Approved** | [#runtime/63174](https://github.com/dotnet/runtime/issues/63174#issuecomment-4381742605) | [Video](https://www.youtube.com/watch?v=Z3vqvBGpFrk&t=0h38m16s)

* Corrected typo in Ed25519OpenSsl..ctor
* Otherwise, looks good as proposed.

```csharp

namespace System.Security.Cryptography
{
    public abstract partial class Ed25519 : System.IDisposable
    {
        //  RFC 8032
        public const int SignatureSizeInBytes = 64;
        public const int PrivateKeySizeInBytes = 32;
        public const int PublicKeySizeInBytes = 32;

        protected Ed25519();

        public static bool IsSupported { get; }

        public static Ed25519 GenerateKey();

        public static Ed25519 ImportPublicKey(byte[] source);
        public static Ed25519 ImportPublicKey(ReadOnlySpan<byte> source);

        public static Ed25519 ImportPrivateKey(byte[] source);
        public static Ed25519 ImportPrivateKey(ReadOnlySpan<byte> source);

        public byte[] ExportPublicKey();
        public void ExportPublicKey(Span<byte> destination);
        protected abstract void ExportPublicKeyCore(Span<byte> destination);

        public byte[] ExportPrivateKey();
        public void ExportPrivateKey(Span<byte> destination);
        protected abstract void ExportPrivateKeyCore(Span<byte> destination);

        public byte[] SignData(byte[] data);
        public void SignData(ReadOnlySpan<byte> data, Span<byte> destination);
        protected abstract void SignDataCore(ReadOnlySpan<byte> data, Span<byte> destination);

        public bool VerifyData(byte[] data, byte[] signature);
        public bool VerifyData(ReadOnlySpan<byte> data, ReadOnlySpan<byte> signature);
        protected abstract bool VerifyDataCore(ReadOnlySpan<byte> data, ReadOnlySpan<byte> signature);

        public void Dispose();
        protected virtual void Dispose(bool disposing);

        public bool TryExportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);
        public bool TryExportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);
        public bool TryExportEncryptedPkcs8PrivateKey(string password, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);

        public byte[] ExportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters);
        public byte[] ExportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, PbeParameters pbeParameters);
        public byte[] ExportEncryptedPkcs8PrivateKey(string password, PbeParameters pbeParameters);

        public string ExportEncryptedPkcs8PrivateKeyPem(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters);
        public string ExportEncryptedPkcs8PrivateKeyPem(ReadOnlySpan<char> password, PbeParameters pbeParameters);
        public string ExportEncryptedPkcs8PrivateKeyPem(string password, PbeParameters pbeParameters);

        public byte[] ExportPkcs8PrivateKey();
        public string ExportPkcs8PrivateKeyPem();
        public bool TryExportPkcs8PrivateKey(Span<byte> destination, out int bytesWritten);
        protected abstract bool TryExportPkcs8PrivateKeyCore(Span<byte> destination, out int bytesWritten);

        public byte[] ExportSubjectPublicKeyInfo();
        public bool TryExportSubjectPublicKeyInfo(Span<byte> destination, out int bytesWritten);
        public string ExportSubjectPublicKeyInfoPem();

        public static Ed25519 ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, ReadOnlySpan<byte> source);
        public static Ed25519 ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, ReadOnlySpan<byte> source);
        public static Ed25519 ImportEncryptedPkcs8PrivateKey(string password, byte[] source);

        public static Ed25519 ImportFromEncryptedPem(ReadOnlySpan<char> source, ReadOnlySpan<byte> passwordBytes);
        public static Ed25519 ImportFromEncryptedPem(string source, byte[] passwordBytes);
        public static Ed25519 ImportFromEncryptedPem(ReadOnlySpan<char> source, ReadOnlySpan<char> password);
        public static Ed25519 ImportFromEncryptedPem(string source, string password);

        public static Ed25519 ImportFromPem(ReadOnlySpan<char> source);
        public static Ed25519 ImportFromPem(string source);

        public static Ed25519 ImportPkcs8PrivateKey(byte[] source);
        public static Ed25519 ImportPkcs8PrivateKey(ReadOnlySpan<byte> source);

        public static Ed25519 ImportSubjectPublicKeyInfo(byte[] source);
        public static Ed25519 ImportSubjectPublicKeyInfo(ReadOnlySpan<byte> source);
    }

    public sealed partial class Ed25519OpenSsl : Ed25519
    {
        [UnsupportedOSPlatformAttribute("android")]
        [UnsupportedOSPlatformAttribute("browser")]
        [UnsupportedOSPlatformAttribute("ios")]
        [UnsupportedOSPlatformAttribute("osx")]
        [UnsupportedOSPlatformAttribute("tvos")]
        [UnsupportedOSPlatformAttribute("windows")]
        public Ed25519OpenSsl(SafeEvpPKeyHandle pkeyHandle);

        public SafeEvpPKeyHandle DuplicateKeyHandle();
    }
}
```
## Add [Try]Parse to Rune

**Approved** | [#runtime/60231](https://github.com/dotnet/runtime/issues/60231#issuecomment-4381785420) | [Video](https://www.youtube.com/watch?v=Z3vqvBGpFrk&t=0h52m45s)


* Since IUtf8SpanFormattable/Parsable was explicitly implemented, let's do the same here.

```diff
public struct Rune : IComparable, IComparable<Rune>, IEquatable<Rune>
     , IUtf8SpanFormattable, IUtf8SpanParsable<Rune> // added in .NET 8
+    , IParsable<Rune>, ISpanParsable<Rune> 
{
+    static Rune IParsable<Rune>.Parse(string s, IFormatProvider? provider);
+    static bool IParsable<Rune>.TryParse(string s, IFormatProvider? provider, out Rune result);

+    static Rune ISpanParsable<Rune>.Parse(ReadOnlySpan<char> s, IFormatProvider? provider);
+    static bool ISpanParsable<Rune>.TryParse(ReadOnlySpan<char> s, IFormatProvider? provider, out Rune result);
}
```
## EmptyServiceProvider

**Approved** | [#runtime/120198](https://github.com/dotnet/runtime/issues/120198#issuecomment-4381818447) | [Video](https://www.youtube.com/watch?v=Z3vqvBGpFrk&t=0h59m46s)

* ServiceProvider.Empty was discussed as an alternative, but we leaned away from that given that ServiceProvider is sealed and the Empty instance would probably not want to return one.
* So, looks good as proposed.

```csharp
namespace Microsoft.Extensions.DependencyInjection;

public sealed class EmptyServiceProvider : IKeyedServiceProvider, IServiceProviderIsKeyedService
{
    public static EmptyServiceProvider Instance { get; }

    // explicit implementations of the interface methods:
    object? IKeyedServiceProvider.GetKeyedService(Type serviceType, object? serviceKey);
    object IKeyedServiceProvider.GetRequiredKeyedService(Type serviceType, object? serviceKey);
    object? IServiceProvider.GetService(Type serviceType);
    bool IServiceProviderIsKeyedService.IsKeyedService(Type serviceType, object? serviceKey);
    bool IServiceProviderIsService.IsService(Type serviceType);
}
```
## CryptographicOperations.FixedTimeIsZeros

**Approved** | [#runtime/127584](https://github.com/dotnet/runtime/issues/127584#issuecomment-4381869989) | [Video](https://www.youtube.com/watch?v=Z3vqvBGpFrk&t=1h4m53s)

* Rather than special casing zero, we went with the "compare to a scalar" approach

```csharp
namespace System.Security.Cryptography;

public static partial class CryptographicOperations
{
    [MethodImpl(MethodImplOptions.NoInlining | MethodImplOptions.NoOptimization)]
    public static bool FixedTimeEquals(ReadOnlySpan<byte> source, byte value);
}
```
## Get a Read-only view of a StringBuilder body and clear the StringBuilder.

**Approved** | [#runtime/97570](https://github.com/dotnet/runtime/issues/97570#issuecomment-4382244077) | [Video](https://www.youtube.com/watch?v=Z3vqvBGpFrk&t=1h16m10s)

* After a 40 minute discussion on naming, we ended up with static `MoveChunks`.
* We left out a `MoveChunks(from, to)` because it has questions about the behavior of `to` when non-empty (it can be added in the future)

```csharp
namespace System.Text;

public partial class StringBuilder
{
    public static StringBuilder MoveChunks(StringBuilder source);
}
```
