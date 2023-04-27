# API Review 04/27/2023

## ImmutableArray.CreateUnsafe<T>(T[] array)

**Approved** | [#runtime/83141](https://github.com/dotnet/runtime/issues/83141#issuecomment-1526056576) | [Video](https://www.youtube.com/watch?v=_QzUF-EVUYw&t=0h0m0s)

Looks good as proposed

```C#
namespace System.Runtime.InteropServices;

public static class ImmutableCollectionsMarshal
{
    public static ImmutableArray<T> AsImmutableArray<T>(T[]? array);
    public static T[]? AsArray<T>(ImmutableArray<T> array);
}
```
## Add support for SHA3 (Keccak)

**Approved** | [#runtime/20342](https://github.com/dotnet/runtime/issues/20342#issuecomment-1526135709) | [Video](https://www.youtube.com/watch?v=_QzUF-EVUYw&t=0h11m26s)

* We had a discussion about the OS guard attributes.  We're leaving them as-proposed but will have a secondary discussion as to whether we want to remove them (and just say to call `IsSupported`)
* We removed `abstract class Shake`, and just copied the instance members up to the derived types.  We can insert it later if we change our minds.
* We had a discussion around the SHAKE `void GetCurrentHash(Span<byte> destination)` methods (because they will fill the entire buffer). We discussed things like `void FillWithCurrentHash` and `int GetCurrentHash`.  In the end, we kept it as `void GetCurrentHash`

```C#
namespace System.Security.Cryptography;

public partial struct HashAlgorithmName {
    public static HashAlgorithmName SHA3_256 { get; }
    public static HashAlgorithmName SHA3_384 { get; }
    public static HashAlgorithmName SHA3_512 { get; }
}

public sealed partial class RSAEncryptionPadding {
    public static RSAEncryptionPadding OaepSHA3_256 { get; }
    public static RSAEncryptionPadding OaepSHA3_384 { get; }
    public static RSAEncryptionPadding OaepSHA3_512 { get; }
}

public abstract partial class SHA3_256 : HashAlgorithm {
    public const int HashSizeInBits = 256;
    public const int HashSizeInBytes = 32;

    protected SHA3_256();

    [UnsupportedOSPlatformGuard("ios")]
    [UnsupportedOSPlatformGuard("tvos")]
    [UnsupportedOSPlatformGuard("android")]
    [UnsupportedOSPlatformGuard("browser")]
    [UnsupportedOSPlatformGuard("osx")]
    public static bool IsSupported { get; }

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static new SHA3_256 Create();

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(byte[] source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(ReadOnlySpan<byte> source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static int HashData(ReadOnlySpan<byte> source, Span<byte> destination);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static bool TryHashData(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(Stream source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static int HashData(Stream source, Span<byte> destination);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<byte[]> HashDataAsync(Stream source, CancellationToken cancellationToken = default);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<int> HashDataAsync(
        Stream source,
        Memory<byte> destination,
        CancellationToken cancellationToken = default);
}


public abstract partial class SHA3_384 : HashAlgorithm {
    public const int HashSizeInBits = 384;
    public const int HashSizeInBytes = 48;

    protected SHA3_384();

    [UnsupportedOSPlatformGuard("ios")]
    [UnsupportedOSPlatformGuard("tvos")]
    [UnsupportedOSPlatformGuard("android")]
    [UnsupportedOSPlatformGuard("browser")]
    [UnsupportedOSPlatformGuard("osx")]
    public static bool IsSupported { get; }

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static new SHA3_384 Create();

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(byte[] source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(ReadOnlySpan<byte> source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static int HashData(ReadOnlySpan<byte> source, Span<byte> destination);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static bool TryHashData(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(Stream source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static int HashData(Stream source, Span<byte> destination);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<byte[]> HashDataAsync(Stream source, CancellationToken cancellationToken = default);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<int> HashDataAsync(
        Stream source,
        Memory<byte> destination,
        CancellationToken cancellationToken = default);
}

public abstract partial class SHA3_512 : HashAlgorithm {
    public const int HashSizeInBits = 512;
    public const int HashSizeInBytes = 64;

    protected SHA3_512();

    [UnsupportedOSPlatformGuard("ios")]
    [UnsupportedOSPlatformGuard("tvos")]
    [UnsupportedOSPlatformGuard("android")]
    [UnsupportedOSPlatformGuard("browser")]
    [UnsupportedOSPlatformGuard("osx")]
    public static bool IsSupported { get; }

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static new SHA3_512 Create();

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(byte[] source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(ReadOnlySpan<byte> source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static int HashData(ReadOnlySpan<byte> source, Span<byte> destination);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static bool TryHashData(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(Stream source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static int HashData(Stream source, Span<byte> destination);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<byte[]> HashDataAsync(Stream source, CancellationToken cancellationToken = default);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<int> HashDataAsync(
        Stream source,
        Memory<byte> destination,
        CancellationToken cancellationToken = default);
}

public partial class HMACSHA3_256 : HMAC {
    public const int HashSizeInBits = 256;
    public const int HashSizeInBytes = 32;

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public HMACSHA3_256();

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public HMACSHA3_256(byte[] key);

    [UnsupportedOSPlatformGuard("ios")]
    [UnsupportedOSPlatformGuard("tvos")]
    [UnsupportedOSPlatformGuard("android")]
    [UnsupportedOSPlatformGuard("browser")]
    [UnsupportedOSPlatformGuard("osx")]
    public static bool IsSupported { get; } // New

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(byte[] key, byte[] source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static int HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static bool TryHashData(
        ReadOnlySpan<byte> key,
        ReadOnlySpan<byte> source,
        Span<byte> destination,
        out int bytesWritten);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(byte[] key, Stream source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(ReadOnlySpan<byte> key, Stream source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static int HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, CancellationToken cancellationToken = default);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, CancellationToken cancellationToken = default);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<int> HashDataAsync(
        ReadOnlyMemory<byte> key,
        Stream source,
        Memory<byte> destination,
        CancellationToken cancellationToken = default);
}

public partial class HMACSHA3_384 : HMAC {
    public const int HashSizeInBits = 384;
    public const int HashSizeInBytes = 48;

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public HMACSHA3_384();

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public HMACSHA3_384(byte[] key);

    [UnsupportedOSPlatformGuard("ios")]
    [UnsupportedOSPlatformGuard("tvos")]
    [UnsupportedOSPlatformGuard("android")]
    [UnsupportedOSPlatformGuard("browser")]
    [UnsupportedOSPlatformGuard("osx")]
    public static bool IsSupported { get; } // New

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(byte[] key, byte[] source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static int HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static bool TryHashData(
        ReadOnlySpan<byte> key,
        ReadOnlySpan<byte> source,
        Span<byte> destination,
        out int bytesWritten);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(byte[] key, Stream source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(ReadOnlySpan<byte> key, Stream source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static int HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, CancellationToken cancellationToken = default);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, CancellationToken cancellationToken = default);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<int> HashDataAsync(
        ReadOnlyMemory<byte> key,
        Stream source,
        Memory<byte> destination,
        CancellationToken cancellationToken = default);
}

public partial class HMACSHA3_512 : HMAC {
    public const int HashSizeInBits = 512;
    public const int HashSizeInBytes = 64;

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public HMACSHA3_512();

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public HMACSHA3_512(byte[] key);

    [UnsupportedOSPlatformGuard("ios")]
    [UnsupportedOSPlatformGuard("tvos")]
    [UnsupportedOSPlatformGuard("android")]
    [UnsupportedOSPlatformGuard("browser")]
    [UnsupportedOSPlatformGuard("osx")]
    public static bool IsSupported { get; } // New

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(byte[] key, byte[] source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static int HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static bool TryHashData(
        ReadOnlySpan<byte> key,
        ReadOnlySpan<byte> source,
        Span<byte> destination,
        out int bytesWritten);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(byte[] key, Stream source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static byte[] HashData(ReadOnlySpan<byte> key, Stream source);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static int HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, CancellationToken cancellationToken = default);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, CancellationToken cancellationToken = default);

    [UnsupportedOSPlatform("ios")]
    [UnsupportedOSPlatform("tvos")]
    [UnsupportedOSPlatform("android")]
    [UnsupportedOSPlatform("browser")]
    [UnsupportedOSPlatform("osx")]
    public static ValueTask<int> HashDataAsync(
        ReadOnlyMemory<byte> key,
        Stream source,
        Memory<byte> destination,
        CancellationToken cancellationToken = default);
}

[UnsupportedOSPlatform("ios")]
[UnsupportedOSPlatform("tvos")]
[UnsupportedOSPlatform("android")]
[UnsupportedOSPlatform("browser")]
[UnsupportedOSPlatform("osx")]
public sealed partial class Shake128 : IDisposable {

    public Shake128();

    [UnsupportedOSPlatformGuard("ios")]
    [UnsupportedOSPlatformGuard("tvos")]
    [UnsupportedOSPlatformGuard("android")]
    [UnsupportedOSPlatformGuard("browser")]
    [UnsupportedOSPlatformGuard("osx")]
    public static bool IsSupported { get; }

    public void AppendData(byte[] data);
    public void AppendData(ReadOnlySpan<byte> data);
    public byte[] GetCurrentHash(int outputLength);
    public void GetCurrentHash(Span<byte> destination);
    public byte[] GetHashAndReset(int outputLength);
    public void GetHashAndReset(Span<byte> destination);
    public void Dispose();

    public static byte[] HashData(byte[] source, int outputLength);
    public static byte[] HashData(ReadOnlySpan<byte> source, int outputLength);
    public static void HashData(ReadOnlySpan<byte> source, Span<byte> destination);

    public static byte[] HashData(Stream source, int outputLength);
    public static void HashData(Stream source, Span<byte> destination);
    public static ValueTask HashDataAsync(Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
    public static ValueTask<byte[]> HashDataAsync(Stream source, int outputLength, CancellationToken cancellationToken = default);
}

[UnsupportedOSPlatform("ios")]
[UnsupportedOSPlatform("tvos")]
[UnsupportedOSPlatform("android")]
[UnsupportedOSPlatform("browser")]
[UnsupportedOSPlatform("osx")]
public sealed partial class Shake256 : IDisposable {

    public Shake256();

    [UnsupportedOSPlatformGuard("ios")]
    [UnsupportedOSPlatformGuard("tvos")]
    [UnsupportedOSPlatformGuard("android")]
    [UnsupportedOSPlatformGuard("browser")]
    [UnsupportedOSPlatformGuard("osx")]
    public static bool IsSupported { get; }

    public void AppendData(byte[] data);
    public void AppendData(ReadOnlySpan<byte> data);
    public byte[] GetCurrentHash(int outputLength);
    public void GetCurrentHash(Span<byte> destination);
    public byte[] GetHashAndReset(int outputLength);
    public void GetHashAndReset(Span<byte> destination);
    public void Dispose();

    public static byte[] HashData(byte[] source, int outputLength);
    public static byte[] HashData(ReadOnlySpan<byte> source, int outputLength);
    public static void HashData(ReadOnlySpan<byte> source, Span<byte> destination);

    public static byte[] HashData(Stream source, int outputLength);
    public static void HashData(Stream source, Span<byte> destination);
    public static ValueTask HashDataAsync(Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
    public static ValueTask<byte[]> HashDataAsync(Stream source, int outputLength, CancellationToken cancellationToken = default);
}
```
## System.IO.Compression.ZipFile.CreateFromDirectory: overloads to write to  a stream rather than a file

**Approved** | [#runtime/1555](https://github.com/dotnet/runtime/issues/1555#issuecomment-1526144628) | [Video](https://www.youtube.com/watch?v=_QzUF-EVUYw&t=1h20m24s)

Looks good as proposed

```C#
namespace System.IO.Compression;

public static partial class ZipFile
{
    public static void CreateFromDirectory(string sourceDirectoryName, Stream destination);
    public static void CreateFromDirectory(string sourceDirectoryName, Stream destination, CompressionLevel compressionLevel, bool includeBaseDirectory);
    public static void CreateFromDirectory(string sourceDirectoryName, Stream destination, CompressionLevel compressionLevel, bool includeBaseDirectory, Encoding? entryNameEncoding);

    public static void ExtractToDirectory(Stream source, string destinationDirectoryName) { }
    public static void ExtractToDirectory(Stream source, string destinationDirectoryName, bool overwriteFiles) { }
    public static void ExtractToDirectory(Stream source, string destinationDirectoryName, Encoding? entryNameEncoding) { }
    public static void ExtractToDirectory(Stream source, string destinationDirectoryName, Encoding? entryNameEncoding, bool overwriteFiles) { }
}
```
## Add StringMarshalling property to GeneratedComInterfaceAttribute

**Approved** | [#runtime/84662](https://github.com/dotnet/runtime/issues/84662#issuecomment-1526152979) | [Video](https://www.youtube.com/watch?v=_QzUF-EVUYw&t=1h26m53s)

Looks good as proposed

```C#
namespace System.Runtime.InteropServices.Marshalling;

[AttributeUsage(AttributeTargets.Interface)]
public partial class GeneratedComInterfaceAttribute : Attribute
{
    public StringMarshalling StringMarshalling { get; set; }
    public Type? StringMarshallingCustomType { get; set; }
}
```
## Add overload of ToDictionary that turns an IEnumerable of KeyValuePair or ValueTuple

**Approved** | [#runtime/65925](https://github.com/dotnet/runtime/issues/65925#issuecomment-1526164569) | [Video](https://www.youtube.com/watch?v=_QzUF-EVUYw&t=1h34m45s)

* We changed the `ValueTuple<TKey, TValue>` syntax to `(TKey Key, TValue Value)`
* We added the KVP+EqualityComparer overload to square off the proposal.
* Discussion brought up that `dict.ToDictionary()` looks like a clone, but actually always uses the default comparer, which might be a loss of data integrity... and this should be pointed out in docs.

```C#
namespace System.Collections.Generic;

public static partial class Enumerable
{
    public static Dictionary<TKey, TValue> ToDictionary<TKey, TValue>(this IEnumerable<KeyValuePair<TKey, TValue>> source) where TKey : notnull;
    public static Dictionary<TKey, TValue> ToDictionary<TKey, TValue>(this IEnumerable<KeyValuePair<TKey, TValue>> source, IEqualityComparer<TKey> comparer) where TKey : notnull;
    public static Dictionary<TKey, TValue> ToDictionary<TKey, TValue>(this IEnumerable<(TKey Key, TValue Value)> source) where TKey : notnull;
    public static Dictionary<TKey, TValue> ToDictionary<TKey, TValue>(this IEnumerable<(TKey Key, TValue Value)> source, IEqualityComparer<TKey> comparer) where TKey : notnull;
}
```
## Add readonly Capacity property to HashSet<T> and Dictionary<K,V> and TrimExcess to HashSet<T>

**Approved** | [#runtime/66426](https://github.com/dotnet/runtime/issues/66426#issuecomment-1526202382) | [Video](https://www.youtube.com/watch?v=_QzUF-EVUYw&t=1h44m41s)

We looked at some other types in the namespace that had the same pattern of EnsureCapacity(int)+TrimExcess(void) and squared it off to have both get_Capacity+TrimExcess(int).

```C#
namespace System.Collections.Generic
{
    public partial class HashSet<T>
    {
        /// <summary>
        /// Gets the total number of elements the internal data structure can hold
        /// without resizing.
        /// </summary>
        public int Capacity { get; }

        /// <summary>
        /// Sets the capacity of this set to hold up a specified number of elements
        /// without any further expansion of its backing storage.
        /// </summary>
        public void TrimExcess(int capacity);
    }

    public partial class Dictionary<TKey, TValue>
    {
        /// <summary>
        /// Gets the total number of elements the internal data structure can hold
        /// without resizing.
        /// </summary>
        public int Capacity { get; }
    }

    public partial class Queue<T>
    {
        public int Capacity { get; }
        public void TrimExcess(int capacity);
    }

    public partial class Stack<T>
    {
        public int Capacity { get; }
        public void TrimExcess(int capacity);
    }
}
```
