# API Review 04/21/2026

## Introduce FullJoin() LINQ operator

**Approved** | [#runtime/124787](https://github.com/dotnet/runtime/issues/124787#issuecomment-4290491177) | [Video](https://www.youtube.com/watch?v=srolGxN1N-U&t=0h0m0s)

* In SQL terminology, "inner" and "outer" don't seem like the correct terms, but since "Join" (SQL INNER JOIN) already used "outer" as the "this" and "inner" as the "other" (instead of "left" and "right"), so the names seem right here.
* We've already started using default parameters in System.Linq.Enumerable, so let's collapse the overloads down and default the comparer

```csharp
namespace System.Linq;

public static partial class Enumerable
{
    public static IEnumerable<TResult> FullJoin<TOuter, TInner, TKey, TResult>(
        this IEnumerable<TOuter> outer,
        IEnumerable<TInner> inner,
        Func<TOuter, TKey> outerKeySelector,
        Func<TInner, TKey> innerKeySelector,
        Func<TOuter?, TInner?, TResult> resultSelector,
        IEqualityComparer<TKey>? comparer = null);

    public static IEnumerable<(TOuter? Outer, TInner? Inner)> FullJoin<TOuter, TInner, TKey>(
        this IEnumerable<TOuter> outer,
        IEnumerable<TInner> inner,
        Func<TOuter, TKey> outerKeySelector,
        Func<TInner, TKey> innerKeySelector,
        IEqualityComparer<TKey>? comparer = null);
}

public static partial class Queryable
{
    public static IQueryable<TResult> FullJoin<TOuter, TInner, TKey, TResult>(
        this IQueryable<TOuter> outer,
        IEnumerable<TInner> inner,
        Expression<Func<TOuter, TKey>> outerKeySelector,
        Expression<Func<TInner, TKey>> innerKeySelector,
        Expression<Func<TOuter?, TInner?, TResult>> resultSelector,
        IEqualityComparer<TKey>? comparer = null);

    public static IQueryable<(TOuter? Outer, TInner? Inner)> FullJoin<TOuter, TInner, TKey>(
        this IQueryable<TOuter> outer,
        IEnumerable<TInner> inner,
        Expression<Func<TOuter, TKey>> outerKeySelector,
        Expression<Func<TInner, TKey>> innerKeySelector,
        IEqualityComparer<TKey>? comparer = null);
}

public static partial class AsyncEnumerable
{
    public static IAsyncEnumerable<TResult> FullJoin<TOuter, TInner, TKey, TResult>(
        this IAsyncEnumerable<TOuter> outer,
        IAsyncEnumerable<TInner> inner,
        Func<TOuter, TKey> outerKeySelector,
        Func<TInner, TKey> innerKeySelector,
        Func<TOuter?, TInner?, TResult> resultSelector,
        IEqualityComparer<TKey>? comparer = null);

    public static IAsyncEnumerable<TResult> FullJoin<TOuter, TInner, TKey, TResult>(
        this IAsyncEnumerable<TOuter> outer,
        IAsyncEnumerable<TInner> inner,
        Func<TOuter, CancellationToken, ValueTask<TKey>> outerKeySelector,
        Func<TInner, CancellationToken, ValueTask<TKey>> innerKeySelector,
        Func<TOuter?, TInner?, CancellationToken, ValueTask<TResult>> resultSelector,
        IEqualityComparer<TKey>? comparer = null);

    public static IAsyncEnumerable<(TOuter? Outer, TInner? Inner)> FullJoin<TOuter, TInner, TKey>(
        this IAsyncEnumerable<TOuter> outer,
        IAsyncEnumerable<TInner> inner,
        Func<TOuter, TKey> outerKeySelector,
        Func<TInner, TKey> innerKeySelector,
        IEqualityComparer<TKey>? comparer = null);

    public static IAsyncEnumerable<(TOuter? Outer, TInner? Inner)> FullJoin<TOuter, TInner, TKey>(
        this IAsyncEnumerable<TOuter> outer,
        IAsyncEnumerable<TInner> inner,
        Func<TOuter, CancellationToken, ValueTask<TKey>> outerKeySelector,
        Func<TInner, CancellationToken, ValueTask<TKey>> innerKeySelector,
        IEqualityComparer<TKey>? comparer = null);
}
```
## QUIC stream priority support

**Approved** | [#runtime/90281](https://github.com/dotnet/runtime/issues/90281#issuecomment-4290641129) | [Video](https://www.youtube.com/watch?v=srolGxN1N-U&t=0h13m24s)

* We added a DefaultPriority const to help express that we start in the middle of the range.
  * byte.MinValue is min-priority, byte.MaxValue is max-priority.  We can add other names later if needed.
* We discussed byte (0..FF) vs sbyte (negative is low, positive is high), and landed on byte for commonality.

```csharp
namespace System.Net.Quic;

public partial class QuicStream
{
    public const byte DefaultPriority = 0x7F;

    public byte Priority { get; set; }
}
```
## Add Common Video Types to `System.Net.Mime.MediaTypeNames`

**Approved** | [#runtime/124392](https://github.com/dotnet/runtime/issues/124392#issuecomment-4290678614) | [Video](https://www.youtube.com/watch?v=srolGxN1N-U&t=0h39m20s)

* Confirmed that QuickTime correctly has a capital T
* Changed Webm to WebM to match their website branding.
* We feel that "Mp4" most closely matches expectations of casing.

```csharp
namespace System.Net.Mime;

public static partial class MediaTypeNames
{
    public static partial class Video
    {
        public const string Mp4 = "video/mp4";
        public const string Mpeg = "video/mpeg";
        public const string Ogg = "video/ogg";
        public const string QuickTime = "video/quicktime";
        public const string WebM = "video/webm";
    }
}
```
## File open should have non-throwing alternatives

**NeedsWork** | [#runtime/27217](https://github.com/dotnet/runtime/issues/27217#issuecomment-4290804191) | [Video](https://www.youtube.com/watch?v=srolGxN1N-U&t=0h46m7s)

* All of the FileMode-based TryOpen could be collapsed to a single one with defaulted parameters (and then a no-default parameters by pattern)
* There was pushback on a false return for access denied -- it often would warrant different recovery than file not found.
  * The suggestion was maybe we need an IO Status enum, like OperationStatus, or a FileSystemError enum, or whatever.

```csharp
namespace System.IO;

public partial class File
{
    public static bool TryOpen(
        string path,
        [NotNullWhen(true)] out FileStream? fileStream,
        FileMode mode = FileMode.Open,
        FileAccess access = default, // determined by mode
        FileShare share = FileShare.None);

    public static bool TryOpen(
        string path,
        FileStreamOptions options,
        [NotNullWhen(true)] out FileStream? fileStream);

    // TODO: Consider defaulted parameters
    public static bool TryOpenHandle(
        string path,
        FileMode mode,
        FileAccess access,
        FileShare share,
        [NotNullWhen(true)] out SafeFileHandle? fileHandle);

    public static bool TryOpenHandle(
        string path,
        FileMode mode,
        FileAccess access,
        FileShare share,
        FileOptions options,
        long preallocationSize,
        [NotNullWhen(true)] out SafeFileHandle? fileHandle);
}
```
## Random.Next<T>

**Approved** | [#runtime/75431](https://github.com/dotnet/runtime/issues/75431#issuecomment-4290988350) | [Video](https://www.youtube.com/watch?v=srolGxN1N-U&t=1h7m58s)


* Even though the existing "Next" is for Int32, the consensus was to use "NextInteger" in case a different generic version of Next was invented later.
* We discussed virtual or not, and feel that not-virtual is better (at least for now)
* Renamed NextFloat to NextBinaryFloat to leave space for NextDecimalFloat and also to avoid having NextSingle and NextFloat, with the latter not being the same as the `float` keyword.
* The parameterless NextInteger should be capable of returning the MaxValue value (full range).

```csharp
namespace System;

public partial class Random
{
    // This version should be capable of returning maxValue, not defer to NextInteger(T.MaxValue)
    public T NextInteger<T>() where T : System.Numerics.IBinaryInteger<T>, System.Numerics.IMinMaxValue<T>;

    public T NextInteger<T>(T maxValue) where T : System.Numerics.IBinaryInteger<T>;
    public T NextInteger<T>(T minValue, T maxValue) where T : System.Numerics.IBinaryInteger<T>;

    public T NextBinaryFloat<T>() where T : IBinaryFloatingPointIeee754<T>
}
```
## Add a fluent `OptionsBuilder` method that adds validator types

**Approved** | [#runtime/115200](https://github.com/dotnet/runtime/issues/115200#issuecomment-4291052383) | [Video](https://www.youtube.com/watch?v=srolGxN1N-U&t=1h38m20s)

* Given that the existing Validate methods instantiate a ValidateOptions, this feels like a natural overload, no new method group needed.

```csharp
namespace Microsoft.Extensions.Options;

public partial class OptionsBuilder<TOptions> where TOptions : class
{
    public virtual OptionsBuilder<TOptions> Validate<TValidateOptions>() where TValidateOptions : class, IValidateOptions<TOptions>;
}
```
## X25519DiffieHellman

**Approved** | [#runtime/126206](https://github.com/dotnet/runtime/issues/126206#issuecomment-4291166288) | [Video](https://www.youtube.com/watch?v=srolGxN1N-U&t=1h50m13s)

* The X25519DiffieHellmanOpenSsl constructor should have the UnsupportedOS attributes.

```csharp
namespace System.Security.Cryptography;

public abstract partial class X25519DiffieHellman : IDisposable
{
    // RFC 7748 Section 5:
    // the inputs and outputs are 32-byte strings (for X25519)
    public const int SecretAgreementSizeInBytes = 32;
    public const int PrivateKeySizeInBytes = 32;
    public const int PublicKeySizeInBytes = 32;

    // Downlevel Windows and Android may return false
    public static bool IsSupported { get; }

    // No public instantiation, derived classes can.
    protected X25519DiffieHellman();

    public static X25519DiffieHellman GenerateKey();

    public static X25519DiffieHellman ImportPublicKey(byte[] source);
    public static X25519DiffieHellman ImportPublicKey(ReadOnlySpan<byte> source);

    public static X25519DiffieHellman ImportPrivateKey(byte[] source);
    public static X25519DiffieHellman ImportPrivateKey(ReadOnlySpan<byte> source);

    public byte[] ExportPublicKey();
    public void ExportPublicKey(Span<byte> destination);
    protected abstract void ExportPublicKeyCore(Span<byte> destination);

    public byte[] ExportPrivateKey();
    public void ExportPrivateKey(Span<byte> destination);
    protected abstract void ExportPrivateKeyCore(Span<byte> destination);

    public byte[] DeriveRawSecretAgreement(X25519DiffieHellman otherParty);
    public void DeriveRawSecretAgreement(X25519DiffieHellman otherParty, Span<byte> destination);
    protected abstract void DeriveRawSecretAgreementCore(X25519DiffieHellman otherParty, Span<byte> destination);

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

    public static X25519DiffieHellman ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, ReadOnlySpan<byte> source);
    public static X25519DiffieHellman ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, ReadOnlySpan<byte> source);
    public static X25519DiffieHellman ImportEncryptedPkcs8PrivateKey(string password, byte[] source);

    public static X25519DiffieHellman ImportFromEncryptedPem(ReadOnlySpan<char> source, ReadOnlySpan<byte> passwordBytes);
    public static X25519DiffieHellman ImportFromEncryptedPem(string source, byte[] passwordBytes);
    public static X25519DiffieHellman ImportFromEncryptedPem(ReadOnlySpan<char> source, ReadOnlySpan<char> password);
    public static X25519DiffieHellman ImportFromEncryptedPem(string source, string password);

    public static X25519DiffieHellman ImportFromPem(ReadOnlySpan<char> source);
    public static X25519DiffieHellman ImportFromPem(string source);

    public static X25519DiffieHellman ImportPkcs8PrivateKey(byte[] source);
    public static X25519DiffieHellman ImportPkcs8PrivateKey(ReadOnlySpan<byte> source);

    public static X25519DiffieHellman ImportSubjectPublicKeyInfo(byte[] source);
    public static X25519DiffieHellman ImportSubjectPublicKeyInfo(ReadOnlySpan<byte> source);

    public void Dispose();
    protected virtual void Dispose(bool disposing);
}

public sealed class X25519DiffieHellmanOpenSsl : X25519DiffieHellman
{
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("osx")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("windows")]
    public X25519DiffieHellmanOpenSsl(SafeEvpPKeyHandle pkeyHandle);
    public SafeEvpPKeyHandle DuplicateKeyHandle();
}

// Consider:
// We should not add CngAlgorithm or related identifiers.
public sealed class X25519DiffieHellmanCng : X25519DiffieHellman
{
    [SupportedOSPlatformAttribute("windows")]
    public X25519DiffieHellmanCng(CngKey key);
    
    public CngKey GetKey();
}
```
