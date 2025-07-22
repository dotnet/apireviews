# API Review 07/22/2025

## Add `LocalPath` property to `RuntimeFile` and `ResourceAssembly` in `Microsoft.Extensions.DependencyModel`

**Approved** | [#runtime/117682](https://github.com/dotnet/runtime/issues/117682#issuecomment-3103931007) | [Video](https://www.youtube.com/watch?v=MXg3EromQLM&t=0h0m0s)

Looks good as proposed

```C#
namespace Microsoft.Extensions.DependencyModel;

public partial class ResourceAssembly
{
    public ResourceAssembly(string path, string locale, string? localPath);
    public string? LocalPath { get; }
}

public partial class RuntimeFile
{
    public RuntimeFile(string path, string? assemblyVersion, string? fileVersion, string? localPath);
    public string? LocalPath { get; }
}
```
## SlhDsaCng

**Approved** | [#runtime/117814](https://github.com/dotnet/runtime/issues/117814#issuecomment-3103957595) | [Video](https://www.youtube.com/watch?v=MXg3EromQLM&t=0h7m41s)

Looks good as proposed

```C#
namespace System.Security.Cryptography
{
    // New class in both Microsoft.Bcl.Cryptography and System.Security.Cryptography
    // No .NET Standard availability in M.B.C. Only netcoreapp and netfx.
    [ExperimentalAttribute("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
    public sealed class SlhDsaCng : SlhDsa
    {
        [SupportedOSPlatform("windows")]
        public SlhDsaCng(CngKey key);

        public CngKey GetKey();
    }

    // New property on existing type; System.Security.Cryptography only
    public sealed partial class CngAlgorithm
    {
        [ExperimentalAttribute("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public static CngAlgorithm SlhDsa { get; }
    }

    // New property on existing type; System.Security.Cryptography only
    public sealed partial class CngAlgorithmGroup
    {
        [ExperimentalAttribute("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public static CngAlgorithmGroup SlhDsa { get; }
    }
}
```
## ML-DSA

**Approved** | [#runtime/117911](https://github.com/dotnet/runtime/issues/117911#issuecomment-3104130910) | [Video](https://www.youtube.com/watch?v=MXg3EromQLM&t=0h16m21s)

* SecretKey => PrivateKey for consistency
* Otherwise, looks good as proposed

```C#
namespace System.Security.Cryptography
{
    // S.S.C and M.B.C.
    [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
    public abstract partial class MLDsa : System.IDisposable
    {
        protected MLDsa(MLDsaAlgorithm algorithm);

        public MLDsaAlgorithm Algorithm { get; }
        public static bool IsSupported { get; }

        public static MLDsa GenerateKey(MLDsaAlgorithm algorithm);

        public static MLDsa ImportMLDsaPrivateSeed(MLDsaAlgorithm algorithm, byte[] source);
        public static MLDsa ImportMLDsaPrivateSeed(MLDsaAlgorithm algorithm, ReadOnlySpan<byte> source);

        public static MLDsa ImportMLDsaPublicKey(MLDsaAlgorithm algorithm, byte[] source);
        public static MLDsa ImportMLDsaPublicKey(MLDsaAlgorithm algorithm, ReadOnlySpan<byte> source);

        public static MLDsa ImportMLDsaPrivateKey(MLDsaAlgorithm algorithm, byte[] source);
        public static MLDsa ImportMLDsaPrivateKey(MLDsaAlgorithm algorithm, ReadOnlySpan<byte> source);

        public byte[] ExportMLDsaPrivateSeed();
        public void ExportMLDsaPrivateSeed(Span<byte> destination);
        protected abstract void ExportMLDsaPrivateSeedCore(Span<byte> destination);

        public byte[] ExportMLDsaPublicKey();
        public void ExportMLDsaPublicKey(Span<byte> destination);
        protected abstract void ExportMLDsaPublicKeyCore(Span<byte> destination);

        public byte[] ExportMLDsaPrivateKey();
        public void ExportMLDsaPrivateKey(Span<byte> destination);
        protected abstract void ExportMLDsaPrivateKeyCore(Span<byte> destination);

        public byte[] SignData(byte[] data, byte[]? context = null);
        public void SignData(ReadOnlySpan<byte> data, Span<byte> destination, ReadOnlySpan<byte> context = default(ReadOnlySpan<byte>));
        protected abstract void SignDataCore(ReadOnlySpan<byte> data, ReadOnlySpan<byte> context, Span<byte> destination);

        public byte[] SignPreHash(byte[] hash, string hashAlgorithmOid, byte[]? context = null);
        public void SignPreHash(ReadOnlySpan<byte> hash, Span<byte> destination, string hashAlgorithmOid, ReadOnlySpan<byte> context = default(ReadOnlySpan<byte>));
        protected abstract void SignPreHashCore(ReadOnlySpan<byte> hash, ReadOnlySpan<byte> context, string hashAlgorithmOid, Span<byte> destination);

        public bool VerifyData(byte[] data, byte[] signature, byte[]? context = null);
        public bool VerifyData(ReadOnlySpan<byte> data, ReadOnlySpan<byte> signature, ReadOnlySpan<byte> context = default(ReadOnlySpan<byte>));
        protected abstract bool VerifyDataCore(ReadOnlySpan<byte> data, ReadOnlySpan<byte> context, ReadOnlySpan<byte> signature);

        public bool VerifyPreHash(byte[] hash, byte[] signature, string hashAlgorithmOid, byte[]? context = null);
        public bool VerifyPreHash(ReadOnlySpan<byte> hash, ReadOnlySpan<byte> signature, string hashAlgorithmOid, ReadOnlySpan<byte> context = default(ReadOnlySpan<byte>));
        protected abstract bool VerifyPreHashCore(ReadOnlySpan<byte> hash, ReadOnlySpan<byte> context, string hashAlgorithmOid, ReadOnlySpan<byte> signature);

        public void Dispose();
        protected virtual void Dispose(bool disposing);

    #region Already Approved PKCS#8 / SPKI
        public byte[] ExportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters);
        public byte[] ExportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, PbeParameters pbeParameters);
        public byte[] ExportEncryptedPkcs8PrivateKey(string password, PbeParameters pbeParameters);
        public string ExportEncryptedPkcs8PrivateKeyPem(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters);
        public string ExportEncryptedPkcs8PrivateKeyPem(ReadOnlySpan<char> password, PbeParameters pbeParameters);
        public string ExportEncryptedPkcs8PrivateKeyPem(string password, PbeParameters pbeParameters);

        public byte[] ExportPkcs8PrivateKey();
        public string ExportPkcs8PrivateKeyPem();
        public byte[] ExportSubjectPublicKeyInfo();
        public string ExportSubjectPublicKeyInfoPem();

        public bool TryExportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);
        public bool TryExportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);
        public bool TryExportEncryptedPkcs8PrivateKey(string password, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);

        public bool TryExportPkcs8PrivateKey(Span<byte> destination, out int bytesWritten);
        protected abstract bool TryExportPkcs8PrivateKeyCore(Span<byte> destination, out int bytesWritten);

        public bool TryExportSubjectPublicKeyInfo(Span<byte> destination, out int bytesWritten);

        public static MLDsa ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, ReadOnlySpan<byte> source);
        public static MLDsa ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, ReadOnlySpan<byte> source);
        public static MLDsa ImportEncryptedPkcs8PrivateKey(string password, byte[] source);

        public static MLDsa ImportFromEncryptedPem(ReadOnlySpan<char> source, ReadOnlySpan<byte> passwordBytes);
        public static MLDsa ImportFromEncryptedPem(ReadOnlySpan<char> source, ReadOnlySpan<char> password);
        public static MLDsa ImportFromEncryptedPem(string source, byte[] passwordBytes);
        public static MLDsa ImportFromEncryptedPem(string source, string password);

        public static MLDsa ImportFromPem(ReadOnlySpan<char> source);
        public static MLDsa ImportFromPem(string source);

        public static MLDsa ImportPkcs8PrivateKey(byte[] source);
        public static MLDsa ImportPkcs8PrivateKey(ReadOnlySpan<byte> source);

        public static MLDsa ImportSubjectPublicKeyInfo(byte[] source);
        public static MLDsa ImportSubjectPublicKeyInfo(ReadOnlySpan<byte> source);
    #endregion
    }

    // S.S.C and M.B.C.
    [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
    public sealed partial class MLDsaAlgorithm : IEquatable<MLDsaAlgorithm>
    {
        internal MLDsaAlgorithm();
        public static MLDsaAlgorithm MLDsa44 { get; }
        public static MLDsaAlgorithm MLDsa65 { get; }
        public static MLDsaAlgorithm MLDsa87 { get; }

        public string Name { get; }
        public int PrivateSeedSizeInBytes { get; }
        public int PublicKeySizeInBytes { get; }
        public int PrivateKeySizeInBytes { get; }
        public int SignatureSizeInBytes { get; }

        public bool Equals([NotNullWhenAttribute(true)] MLDsaAlgorithm? other);
        public static bool operator ==(MLDsaAlgorithm? left, MLDsaAlgorithm? right);
        public static bool operator !=(MLDsaAlgorithm? left, MLDsaAlgorithm? right);
    }

    // S.S.C and M.B.C.
    // M.B.C is .NET Framework and .NET only. No .NET Standard.
    [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
    public sealed partial class MLDsaCng : MLDsa
    {
        [SupportedOSPlatformAttribute("windows")]
        public MLDsaCng(CngKey key);

        public CngKey GetKey();
    }

    // S.S.C
    [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
    public sealed partial class MLDsaOpenSsl : MLDsa
    {
        [UnsupportedOSPlatformAttribute("android")]
        [UnsupportedOSPlatformAttribute("browser")]
        [UnsupportedOSPlatformAttribute("ios")]
        [UnsupportedOSPlatformAttribute("osx")]
        [UnsupportedOSPlatformAttribute("tvos")]
        [UnsupportedOSPlatformAttribute("windows")]
        public MLDsaOpenSsl(SafeEvpPKeyHandle pkeyHandle);

        public SafeEvpPKeyHandle DuplicateKeyHandle();
    }

    // S.S.C
    public sealed partial class CngAlgorithm
    {
        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static CngAlgorithm MLDsa { get; }
    }

    // S.S.C
    public sealed partial class CngAlgorithmGroup
    {
        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static CngAlgorithmGroup MLDsa { get; }
    }

    // S.S.C
    public sealed partial class CngKeyBlobFormat
    {
        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static CngKeyBlobFormat PQDsaPrivateBlob { get; }
    
        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static CngKeyBlobFormat PQDsaPrivateSeedBlob { get; }
        
        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static CngKeyBlobFormat PQDsaPublicBlob { get; }
    }
}

namespace System.Security.Cryptography.X509Certificates
{
    // Present in S.S.C
    public sealed partial class CertificateRequest
    {
        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public CertificateRequest(X500DistinguishedName subjectName, MLDsa key);

        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public CertificateRequest(string subjectName, MLDsa key);
    }

    // Present in S.S.C
    public sealed partial class PublicKey
    {
        public PublicKey(MLDsa key);

        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        [UnsupportedOSPlatformAttribute("browser")]
        public MLDsa? GetMLDsaPublicKey();
    }

    // Present in S.S.C
    public partial class X509Certificate2 : X509Certificate
    {
        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public X509Certificate2 CopyWithPrivateKey(MLDsa privateKey);

        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public MLDsa? GetMLDsaPrivateKey();

        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public MLDsa? GetMLDsaPublicKey();
    }

    // M.B.C
    public static partial class X509CertificateKeyAccessors
    {
        [ExperimentalAttribute("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public static MLDsa? GetMLDsaPublicKey(this X509Certificate2 certificate);

        [ExperimentalAttribute("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public static MLDsa? GetMLDsaPrivateKey(this X509Certificate2 certificate);

        [ExperimentalAttribute("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public static X509Certificate2 CopyWithPrivateKey(this X509Certificate2 certificate, MLDsa privateKey);
    }

    // S.S.C
    public abstract partial class X509SignatureGenerator
    {
        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static X509SignatureGenerator CreateForMLDsa(MLDsa key);
    }
}

namespace System.Security.Cryptography.Pkcs
{
    public sealed partial class CmsSigner
    {
        // S.S.C.P - NetCoreApp only.
        [ExperimentalAttribute("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public CmsSigner(SubjectIdentifierType signerIdentifierType, X509Certificate2? certificate, MLDsa? privateKey);
    }
}
```
## Expose floating-point constants for generic Vector types using extension properties

**Approved** | [#runtime/117569](https://github.com/dotnet/runtime/issues/117569#issuecomment-3104180000) | [Video](https://www.youtube.com/watch?v=MXg3EromQLM&t=0h54m18s)

We added ISignedNumber while we were at it.  Otherwise, looks good as proposed.

```C#
namespace System.Numerics
{
    public static partial class Vector
    {
        extension<T>(Vector<T>)
            where T : IFloatingPointConstants<T>
        {
            public static Vector<T> Epsilon { get; }
            public static Vector<T> NaN { get; }
            public static Vector<T> NegativeInfinity { get; }
            public static Vector<T> NegativeZero { get; }
            public static Vector<T> PositiveInfinity { get; }
            public static Vector<T> E { get; }
            public static Vector<T> Pi { get; }
            public static Vector<T> Tau { get; }
        }

        extension<T>(Vector<T>)
            where T : ISignedNumber<T>
        {
            public static Vector<T> NegativeOne { get; }
        }
    }
}

namespace System.Runtime.Intrinsics
{
    public static partial class Vector128
    {
        extension<T>(Vector128<T>)
            where T : IFloatingPointConstants<T>
        {
            public static Vector128<T> Epsilon { get; }
            public static Vector128<T> NaN { get; }
            public static Vector128<T> NegativeInfinity { get; }
            public static Vector128<T> NegativeZero { get; }
            public static Vector128<T> PositiveInfinity { get; }
            public static Vector128<T> E { get; }
            public static Vector128<T> Pi { get; }
            public static Vector128<T> Tau { get; }
        }

        extension<T>(Vector128<T>)
            where T : ISignedNumber<T>
        {
            public static Vector128<T> NegativeOne { get; }
        }
    }

    public static partial class Vector256
    {
        extension<T>(Vector256<T>)
            where T : IFloatingPointConstants<T>
        {
            public static Vector256<T> Epsilon { get; }
            public static Vector256<T> NaN { get; }
            public static Vector256<T> NegativeInfinity { get; }
            public static Vector256<T> NegativeZero { get; }
            public static Vector256<T> PositiveInfinity { get; }
            public static Vector256<T> E { get; }
            public static Vector256<T> Pi { get; }
            public static Vector256<T> Tau { get; }
        }

        extension<T>(Vector256<T>)
            where T : ISignedNumber<T>
        {
            public static Vector256<T> NegativeOne { get; }
        }
    }

    public static partial class Vector512
    {
        extension<T>(Vector512<T>)
            where T : IFloatingPointConstants<T>
        {
            public static Vector512<T> Epsilon { get; }
            public static Vector512<T> NaN { get; }
            public static Vector512<T> NegativeInfinity { get; }
            public static Vector512<T> NegativeZero { get; }
            public static Vector512<T> PositiveInfinity { get; }
            public static Vector512<T> E { get; }
            public static Vector512<T> Pi { get; }
            public static Vector512<T> Tau { get; }
        }

        extension<T>(Vector512<T>)
            where T : ISignedNumber<T>
        {
            public static Vector512<T> NegativeOne { get; }
        }
    }

    public static partial class Vector64
    {
        extension<T>(Vector64<T>)
            where T : IFloatingPointConstants<T>
        {
            public static Vector64<T> Epsilon { get; }
            public static Vector64<T> NaN { get; }
            public static Vector64<T> NegativeInfinity { get; }
            public static Vector64<T> NegativeZero { get; }
            public static Vector64<T> PositiveInfinity { get; }
            public static Vector64<T> E { get; }
            public static Vector64<T> Pi { get; }
            public static Vector64<T> Tau { get; }
        }

        extension<T>(Vector64<T>)
            where T : ISignedNumber<T>
        {
            public static Vector64<T> NegativeOne { get; }
        }
    }
}
```
## Convert.{From/To}HexString(utf8)

**Approved** | [#runtime/114079](https://github.com/dotnet/runtime/issues/114079#issuecomment-3104224176) | [Video](https://www.youtube.com/watch?v=MXg3EromQLM&t=1h4m35s)

* It was asked if we felt we needed "Utf8" in any of the method names, and no one felt strongly.

```C#
namespace System;

public static class Convert
{
        public static byte[] FromHexString(ReadOnlySpan<byte> utf8Source);
        public static OperationStatus FromHexString(ReadOnlySpan<byte> utf8Source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);
        public static bool TryToHexString(ReadOnlySpan<byte> source, Span<byte> utf8Destination, out int bytesWritten);
        public static bool TryToHexStringLower(ReadOnlySpan<byte> source, Span<byte> utf8Destination, out int bytesWritten);
}
```
## Don't use `stringBuilder.Append(new string(c, repeatCount))`

**Approved** | [#runtime/117643](https://github.com/dotnet/runtime/issues/117643#issuecomment-3104230092) | [Video](https://www.youtube.com/watch?v=MXg3EromQLM&t=1h14m55s)

Looks good as proposed

Category: Performance
Severity: Info
