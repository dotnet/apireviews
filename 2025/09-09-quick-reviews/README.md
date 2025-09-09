# API Review 09/09/2025

## Move MLDsa Experimental to appropriate members

**Approved** | [#runtime/119289](https://github.com/dotnet/runtime/pull/119289#issuecomment-3271601123)

API Review: Looks good as proposed

```diff
namespace System.Security.Cryptography
{
    public partial class CngAlgorithm
    {
-       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.CngAlgorithm MLDsa { get; }
    }
    public partial class CngAlgorithmGroup
    {
-       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.CngAlgorithmGroup MLDsa { get; }
    }
    public partial class CngBlobFormat
    {
-       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.CngKeyBlobFormat PQDsaPrivateBlob { get; }
-       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.CngKeyBlobFormat PQDsaPrivateSeedBlob { get; }
-       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.CngKeyBlobFormat PQDsaPublicBlob { get; }
    }
-   [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
    public partial class MLDsa : System.IDisposable
    {
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public byte[] ExportEncryptedPkcs8PrivateKey(System.ReadOnlySpan<byte> passwordBytes, System.Security.Cryptography.PbeParameters pbeParameters);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public byte[] ExportEncryptedPkcs8PrivateKey(System.ReadOnlySpan<char> password, System.Security.Cryptography.PbeParameters pbeParameters);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public byte[] ExportEncryptedPkcs8PrivateKey(string password, System.Security.Cryptography.PbeParameters pbeParameters);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public string ExportEncryptedPkcs8PrivateKeyPem(System.ReadOnlySpan<byte> passwordBytes, System.Security.Cryptography.PbeParameters pbeParameters);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public string ExportEncryptedPkcs8PrivateKeyPem(System.ReadOnlySpan<char> password, System.Security.Cryptography.PbeParameters pbeParameters);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public string ExportEncryptedPkcs8PrivateKeyPem(string password, System.Security.Cryptography.PbeParameters pbeParameters);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public byte[] ExportPkcs8PrivateKey();
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public string ExportPkcs8PrivateKeyPem();
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public byte[] ExportSubjectPublicKeyInfo();
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public string ExportSubjectPublicKeyInfoPem();
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.MLDsa ImportEncryptedPkcs8PrivateKey(System.ReadOnlySpan<byte> passwordBytes, System.ReadOnlySpan<byte> source);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.MLDsa ImportEncryptedPkcs8PrivateKey(System.ReadOnlySpan<char> password, System.ReadOnlySpan<byte> source);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.MLDsa ImportEncryptedPkcs8PrivateKey(string password, byte[] source);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.MLDsa ImportFromEncryptedPem(System.ReadOnlySpan<char> source, System.ReadOnlySpan<byte> passwordBytes);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.MLDsa ImportFromEncryptedPem(System.ReadOnlySpan<char> source, System.ReadOnlySpan<char> password);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.MLDsa ImportFromEncryptedPem(string source, byte[] passwordBytes);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.MLDsa ImportFromEncryptedPem(string source, string password);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.MLDsa ImportFromPem(System.ReadOnlySpan<char> source);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.MLDsa ImportFromPem(string source);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.MLDsa ImportPkcs8PrivateKey(byte[] source);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.MLDsa ImportPkcs8PrivateKey(System.ReadOnlySpan<byte> source);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.MLDsa ImportSubjectPublicKeyInfo(byte[] source);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static System.Security.Cryptography.MLDsa ImportSubjectPublicKeyInfo(System.ReadOnlySpan<byte> source);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public byte[] SignMu(byte[] externalMu);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public byte[] SignMu(System.ReadOnlySpan<byte> externalMu);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public void SignMu(System.ReadOnlySpan<byte> externalMu, System.Span<byte> destination) { }
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        protected abstract void SignMuCore(System.ReadOnlySpan<byte> externalMu, System.Span<byte> destination);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public byte[] SignPreHash(byte[] hash, string hashAlgorithmOid, byte[]? context = null);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public void SignPreHash(System.ReadOnlySpan<byte> hash, System.Span<byte> destination, string hashAlgorithmOid, System.ReadOnlySpan<byte> context = default(System.ReadOnlySpan<byte>)) { }
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        protected abstract void SignPreHashCore(System.ReadOnlySpan<byte> hash, System.ReadOnlySpan<byte> context, string hashAlgorithmOid, System.Span<byte> destination);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public bool TryExportEncryptedPkcs8PrivateKey(System.ReadOnlySpan<byte> passwordBytes, System.Security.Cryptography.PbeParameters pbeParameters, System.Span<byte> destination, out int bytesWritten);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public bool TryExportEncryptedPkcs8PrivateKey(System.ReadOnlySpan<char> password, System.Security.Cryptography.PbeParameters pbeParameters, System.Span<byte> destination, out int bytesWritten);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public bool TryExportEncryptedPkcs8PrivateKey(string password, System.Security.Cryptography.PbeParameters pbeParameters, System.Span<byte> destination, out int bytesWritten);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public bool TryExportPkcs8PrivateKey(System.Span<byte> destination, out int bytesWritten);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        protected abstract bool TryExportPkcs8PrivateKeyCore(System.Span<byte> destination, out int bytesWritten);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public bool TryExportSubjectPublicKeyInfo(System.Span<byte> destination, out int bytesWritten);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public bool VerifyMu(byte[] externalMu, byte[] signature);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public bool VerifyMu(System.ReadOnlySpan<byte> externalMu, System.ReadOnlySpan<byte> signature);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        protected abstract bool VerifyMuCore(System.ReadOnlySpan<byte> externalMu, System.ReadOnlySpan<byte> signature);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public bool VerifyPreHash(byte[] hash, byte[] signature, string hashAlgorithmOid, byte[]? context = null);
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public bool VerifyPreHash(System.ReadOnlySpan<byte> hash, System.ReadOnlySpan<byte> signature, string hashAlgorithmOid, System.ReadOnlySpan<byte> context = default(System.ReadOnlySpan<byte>));
+       [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        protected abstract bool VerifyPreHashCore(System.ReadOnlySpan<byte> hash, System.ReadOnlySpan<byte> context, string hashAlgorithmOid, System.ReadOnlySpan<byte> signature);
}
```
## Introduce Build ambient metadata

**Approved** | [#extensions/6628](https://github.com/dotnet/extensions/issues/6628#issuecomment-3271692959)

* This seems designed to break reproducible builds. 
  * The generation trigger needs to be something more than a transitive package reference.
  * Coordinate with the relevant compiler people who will have opinions here.
* BuildDateTime seems like it should be a DateTimeOffset, not a string.

```C#
namespace Microsoft.Extensions.AmbientMetadata
{
  public class BuildMetadata
  {
      public string? BuildId { get; set; }
      public string? BuildNumber { get; set; }
      public string? SourceBranchName { get; set; }
      public string? SourceVersion { get; set; }
      public DateTimeOffset? BuildDateTime { get; set; }
  }
}

namespace Microsoft.Extensions.DependencyInjection
{
    public static class BuildMetadataExtensions
    {
        public static IServiceCollection AddBuildMetadata(this IServiceCollection services, IConfigurationSection section);
        public static IServiceCollection AddBuildMetadata(this IServiceCollection services, Action<BuildMetadata> configure);
    }
}
```
## Make some Telemetry helper types public

**NeedsWork** | [#extensions/6737](https://github.com/dotnet/extensions/issues/6737#issuecomment-3271765073)

This doesn't yet seem ready for review.

The existing internal API shape doesn't really have relevance for API review, everything should be presented as completely new API.  And someone needs to represent the feature to explain when each part gets used, et cetera.

Many of the new classes are using the `abstract` keyword, but don't seem to need to, when they're just fine as a "I have virtual members" base class.  So that seems to require more thought.
## Verify HMAC/KMAC APIs

**Approved** | [#runtime/116028](https://github.com/dotnet/runtime/issues/116028#issuecomment-3271891651)


* Corrected the Async methods to use `ValueTask<bool>` instead of `bool`.
  * Will follow up offline to see if it should just be `Task<bool>` given async2
* We had a discussion about whether the variable length types (Kmac128, et al) should have an `expectedLength` parameter to guard against truncation.  In the end, we decided to reject empty, but allow any other length and not add a parameter.
  * We do feel like it probably wants a warning in docs that the caller should check minimum lengths for their use case.

```C#
namespace System.Security.Cryptography;

public partial class HMACMD5 {
    public static bool Verify(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, byte[] source, byte[] hash);
    
    public static bool Verify(ReadOnlySpan<byte> key, System.IO.Stream source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, System.IO.Stream source, byte[] hash);
    public static ValueTask<bool> VerifyAsync(byte[] key, System.IO.Stream source, byte[] hash, CancellationToken cancellationToken = default);
    public static ValueTask<bool> VerifyAsync(ReadOnlyMemory<byte> key, System.IO.Stream source, ReadOnlyMemory<byte> hash, CancellationToken cancellationToken = default);
}

public partial class HMACSHA1 {
    public static bool Verify(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, byte[] source, byte[] hash);
    
    public static bool Verify(ReadOnlySpan<byte> key, System.IO.Stream source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, System.IO.Stream source, byte[] hash);
    public static ValueTask<bool> VerifyAsync(byte[] key, System.IO.Stream source, byte[] hash, CancellationToken cancellationToken = default);
    public static ValueTask<bool> VerifyAsync(ReadOnlyMemory<byte> key, System.IO.Stream source, ReadOnlyMemory<byte> hash, CancellationToken cancellationToken = default);
}

public partial class HMACSHA256 {
    public static bool Verify(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, byte[] source, byte[] hash);
    
    public static bool Verify(ReadOnlySpan<byte> key, System.IO.Stream source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, System.IO.Stream source, byte[] hash);
    public static ValueTask<bool> VerifyAsync(byte[] key, System.IO.Stream source, byte[] hash, CancellationToken cancellationToken = default);
    public static ValueTask<bool> VerifyAsync(ReadOnlyMemory<byte> key, System.IO.Stream source, ReadOnlyMemory<byte> hash, CancellationToken cancellationToken = default);
}

public partial class HMACSHA384 {
    public static bool Verify(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, byte[] source, byte[] hash);

    public static bool Verify(ReadOnlySpan<byte> key, System.IO.Stream source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, System.IO.Stream source, byte[] hash);
    public static ValueTask<bool> VerifyAsync(byte[] key, System.IO.Stream source, byte[] hash, CancellationToken cancellationToken = default);
    public static ValueTask<bool> VerifyAsync(ReadOnlyMemory<byte> key, System.IO.Stream source, ReadOnlyMemory<byte> hash, CancellationToken cancellationToken = default);
}

public partial class HMACSHA512 {
    public static bool Verify(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, byte[] source, byte[] hash);
    
    public static bool Verify(ReadOnlySpan<byte> key, System.IO.Stream source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, System.IO.Stream source, byte[] hash);
    public static ValueTask<bool> VerifyAsync(byte[] key, System.IO.Stream source, byte[] hash, CancellationToken cancellationToken = default);
    public static ValueTask<bool> VerifyAsync(ReadOnlyMemory<byte> key, System.IO.Stream source, ReadOnlyMemory<byte> hash, CancellationToken cancellationToken = default);
}

public partial class HMACSHA3_256 {
    public static bool Verify(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, byte[] source, byte[] hash);
    
    public static bool Verify(ReadOnlySpan<byte> key, System.IO.Stream source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, System.IO.Stream source, byte[] hash);
    public static ValueTask<bool> VerifyAsync(byte[] key, System.IO.Stream source, byte[] hash, CancellationToken cancellationToken = default);
    public static ValueTask<bool> VerifyAsync(ReadOnlyMemory<byte> key, System.IO.Stream source, ReadOnlyMemory<byte> hash, CancellationToken cancellationToken = default);
}

public partial class HMACSHA3_384 {
    public static bool Verify(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, byte[] source, byte[] hash);
    
    public static bool Verify(ReadOnlySpan<byte> key, System.IO.Stream source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, System.IO.Stream source, byte[] hash);
    public static ValueTask<bool> VerifyAsync(byte[] key, System.IO.Stream source, byte[] hash, CancellationToken cancellationToken = default);
    public static ValueTask<bool> VerifyAsync(ReadOnlyMemory<byte> key, System.IO.Stream source, ReadOnlyMemory<byte> hash, CancellationToken cancellationToken = default);
}

public partial class HMACSHA3_512 {
    public static bool Verify(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, byte[] source, byte[] hash);
    
    public static bool Verify(ReadOnlySpan<byte> key, System.IO.Stream source, ReadOnlySpan<byte> hash);
    public static bool Verify(byte[] key, System.IO.Stream source, byte[] hash);
    public static ValueTask<bool> VerifyAsync(byte[] key, System.IO.Stream source, byte[] hash, CancellationToken cancellationToken = default);
    public static ValueTask<bool> VerifyAsync(ReadOnlyMemory<byte> key, System.IO.Stream source, ReadOnlyMemory<byte> hash, CancellationToken cancellationToken = default);
}

public partial class Kmac128 {
    public static bool Verify(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, ReadOnlySpan<byte> hash, ReadOnlySpan<byte> customizationString = default);
    public static bool Verify(byte[] key, byte[] source, byte[] hash, byte[]? customizationString = null);

    public static bool Verify(ReadOnlySpan<byte> key, System.IO.Stream source, ReadOnlySpan<byte> hash, ReadOnlySpan<byte> customizationString = default);
    public static bool Verify(byte[] key, System.IO.Stream source, byte[] hash, byte[] customizationString = null);
    public static ValueTask<bool> VerifyAsync(byte[] key, System.IO.Stream source, byte[] hash, byte[] customizationString = null, CancellationToken cancellationToken = default);
    public static ValueTask<bool> VerifyAsync(ReadOnlyMemory<byte> key, System.IO.Stream source, ReadOnlyMemory<byte> hash, ReadOnlyMemory<byte> customizationString = default, CancellationToken cancellationToken = default);

    public bool VerifyCurrentHash(ReadOnlySpan<byte> hash);
    public bool VerifyCurrentHash(byte[] hash);

    public bool VerifyHashAndReset(ReadOnlySpan<byte> hash);
    public bool VerifyHashAndReset(byte[] hash);
}

public partial class Kmac256 {
    public static bool Verify(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, ReadOnlySpan<byte> hash, ReadOnlySpan<byte> customizationString = default);
    public static bool Verify(byte[] key, byte[] source, byte[] hash, byte[]? customizationString = null);

    public static bool Verify(ReadOnlySpan<byte> key, System.IO.Stream source, ReadOnlySpan<byte> hash, ReadOnlySpan<byte> customizationString = default);
    public static bool Verify(byte[] key, System.IO.Stream source, byte[] hash, byte[] customizationString = null);
    public static ValueTask<bool> VerifyAsync(byte[] key, System.IO.Stream source, byte[] hash, byte[] customizationString = null, CancellationToken cancellationToken = default);
    public static ValueTask<bool> VerifyAsync(ReadOnlyMemory<byte> key, System.IO.Stream source, ReadOnlyMemory<byte> hash, ReadOnlyMemory<byte> customizationString = default, CancellationToken cancellationToken = default);

    public bool VerifyCurrentHash(ReadOnlySpan<byte> hash);
    public bool VerifyCurrentHash(byte[] hash);

    public bool VerifyHashAndReset(ReadOnlySpan<byte> hash);
    public bool VerifyHashAndReset(byte[] hash);
}

public partial class KmacXof128 {
    public static bool Verify(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, ReadOnlySpan<byte> hash, ReadOnlySpan<byte> customizationString = default);
    public static bool Verify(byte[] key, byte[] source, byte[] hash, byte[]? customizationString = null);

    public static bool Verify(ReadOnlySpan<byte> key, System.IO.Stream source, ReadOnlySpan<byte> hash, ReadOnlySpan<byte> customizationString = default);
    public static bool Verify(byte[] key, System.IO.Stream source, byte[] hash, byte[] customizationString = null);
    public static ValueTask<bool> VerifyAsync(byte[] key, System.IO.Stream source, byte[] hash, byte[] customizationString = null, CancellationToken cancellationToken = default);
    public static ValueTask<bool> VerifyAsync(ReadOnlyMemory<byte> key, System.IO.Stream source, ReadOnlyMemory<byte> hash, ReadOnlyMemory<byte> customizationString = default, CancellationToken cancellationToken = default);

    public bool VerifyCurrentHash(ReadOnlySpan<byte> hash);
    public bool VerifyCurrentHash(byte[] hash);

    public bool VerifyHashAndReset(ReadOnlySpan<byte> hash);
    public bool VerifyHashAndReset(byte[] hash);
}

public partial class KmacXof256 {
    public static bool Verify(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, ReadOnlySpan<byte> hash, ReadOnlySpan<byte> customizationString = default);
    public static bool Verify(byte[] key, byte[] source, byte[] hash, byte[]? customizationString = null);

    public static bool Verify(ReadOnlySpan<byte> key, System.IO.Stream source, ReadOnlySpan<byte> hash, ReadOnlySpan<byte> customizationString = default);
    public static bool Verify(byte[] key, System.IO.Stream source, byte[] hash, byte[] customizationString = null);
    public static ValueTask<bool> VerifyAsync(byte[] key, System.IO.Stream source, byte[] hash, byte[] customizationString = null, CancellationToken cancellationToken = default);
    public static ValueTask<bool> VerifyAsync(ReadOnlyMemory<byte> key, System.IO.Stream source, ReadOnlyMemory<byte> hash, ReadOnlyMemory<byte> customizationString = default, CancellationToken cancellationToken = default);

    public bool VerifyCurrentHash(ReadOnlySpan<byte> hash);
    public bool VerifyCurrentHash(byte[] hash);

    public bool VerifyHashAndReset(ReadOnlySpan<byte> hash);
    public bool VerifyHashAndReset(byte[] hash);
}

public partial class IncrementalHash {
    public bool VerifyCurrentHash(ReadOnlySpan<byte> hash);
    public bool VerifyCurrentHash(byte[] hash);

    public bool VerifyHashAndReset(ReadOnlySpan<byte> hash);
    public bool VerifyHashAndReset(byte[] hash);
}

public partial class CryptographicOperations {
    public static bool VerifyHmac(HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, ReadOnlySpan<byte> hash);
    public static bool VerifyHmac(HashAlgorithmName hashAlgorithm, byte[] key, byte[] source, byte[] hash);

    public static bool VerifyHmac(HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> key, Stream source, ReadOnlySpan<byte> hash);
    public static bool VerifyHmac(HashAlgorithmName hashAlgorithm, byte[] key, Stream source, byte[] hash);
    
    public static ValueTask<bool> VerifyHmacAsync(HashAlgorithmName hashAlgorithm, ReadOnlyMemory<byte> key, Stream source, ReadOnlyMemory<byte> hash, CancellationToken cancellationToken = default);
    public static ValueTask<bool> VerifyHmacAsync(HashAlgorithmName hashAlgorithm, byte[] key, Stream source, byte[] hash, CancellationToken cancellationToken = default);
}
```
