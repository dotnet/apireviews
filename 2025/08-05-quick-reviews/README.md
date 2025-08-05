# API Review 08/05/2025

## ExtensionMarkerAttribute

**Approved** | [#runtime/118179](https://github.com/dotnet/runtime/issues/118179#issuecomment-3156036265)

* Type seems like maybe a better thing to hold than name, but since the compiler wants string that's fine.
* "Marker" and "Name" don't feel like they're both necessary, we think "ExtensionMarkerAttribute" is sufficient.

```C#
namespace System.Runtime.CompilerServices;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Struct | AttributeTargets.Enum | AttributeTargets.Method | AttributeTargets.Property | AttributeTargets.Field | AttributeTargets.Event | AttributeTargets.Interface | AttributeTargets.Delegate, Inherited = false)]
public sealed class ExtensionMarkerAttribute : Attribute
{
    public ExtensionMarkerAttribute(string name)
        => Name = name;

    public string Name { get; }
}
```
## Composite ML-DSA

**Approved** | [#runtime/118320](https://github.com/dotnet/runtime/issues/118320#issuecomment-3156120979)

* TryExportCompositeMLDsaPrivateKeyCore => ExportCompositeMLDsaPrivateKeyCore if the platform can guarantee a big enough buffer.
  * Also add the int-returning ones for callers
* Added SignData(ROSpan, ROSpan) => byte[]
* Did not add the max sizes of public/private keys to the CompositeMLDsaAlgorithm type, until there's more justification

```C#
namespace System.Security.Cryptography
{
    // S.S.C and M.B.C.
    [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
    public abstract class CompositeMLDsa : IDisposable
    {
        protected CompositeMLDsa(CompositeMLDsaAlgorithm algorithm);

        public CompositeMLDsaAlgorithm Algorithm { get; }
        public static bool IsSupported { get; }
        public static bool IsAlgorithmSupported(CompositeMLDsaAlgorithm algorithm);

        public byte[] SignData(byte[] data, byte[]? context = null);
        public byte[] SignData(ReadOnlySpan<byte> data, ReadOnlySpan<byte> context = default);
        public int SignData(ReadOnlySpan<byte> data, Span<byte> destination, ReadOnlySpan<byte> context = default(ReadOnlySpan<byte>));
        protected abstract int SignDataCore(ReadOnlySpan<byte> data, ReadOnlySpan<byte> context, Span<byte> destination);

        public bool VerifyData(byte[] data, byte[] signature, byte[]? context = null);
        public bool VerifyData(ReadOnlySpan<byte> data, ReadOnlySpan<byte> signature, ReadOnlySpan<byte> context = default(ReadOnlySpan<byte>));
        protected abstract bool VerifyDataCore(ReadOnlySpan<byte> data, ReadOnlySpan<byte> context, ReadOnlySpan<byte> signature);

        public static CompositeMLDsa GenerateKey(CompositeMLDsaAlgorithm algorithm);

        public static CompositeMLDsa ImportCompositeMLDsaPrivateKey(CompositeMLDsaAlgorithm algorithm, byte[] source);
        public static CompositeMLDsa ImportCompositeMLDsaPrivateKey(CompositeMLDsaAlgorithm algorithm, ReadOnlySpan<byte> source);

        public static CompositeMLDsa ImportCompositeMLDsaPublicKey(CompositeMLDsaAlgorithm algorithm, byte[] source);
        public static CompositeMLDsa ImportCompositeMLDsaPublicKey(CompositeMLDsaAlgorithm algorithm, ReadOnlySpan<byte> source);

        public byte[] ExportCompositeMLDsaPrivateKey();
        public int ExportCompositeMLDsaPrivateKey(Span<byte> destination);
        public bool TryExportCompositeMLDsaPrivateKey(Span<byte> destination, out int bytesWritten);
        protected abstract int ExportCompositeMLDsaPrivateKeyCore(Span<byte> destination, out int bytesWritten);

        public byte[] ExportCompositeMLDsaPublicKey();
        public byte[] ExportCompositeMLDsaPublicKey(Span<byte> destination);
        public bool TryExportCompositeMLDsaPublicKey(Span<byte> destination, out int bytesWritten);
        protected abstract int ExportCompositeMLDsaPublicKeyCore(Span<byte> destination, out int bytesWritten);

        public void Dispose() { }
        protected virtual void Dispose(bool disposing) { }

        public byte[] ExportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters);
        public byte[] ExportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, PbeParameters pbeParameters);
        public byte[] ExportEncryptedPkcs8PrivateKey(string password, PbeParameters pbeParameters);
        public string ExportEncryptedPkcs8PrivateKeyPem(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters);
        public string ExportEncryptedPkcs8PrivateKeyPem(ReadOnlySpan<char> password, PbeParameters pbeParameters);
        public string ExportEncryptedPkcs8PrivateKeyPem(string password, PbeParameters pbeParameters);
        public byte[] ExportPkcs8PrivateKey();
        public string ExportPkcs8PrivateKeyPem();
        public bool TryExportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);
        public bool TryExportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);
        public bool TryExportEncryptedPkcs8PrivateKey(string password, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);
        public bool TryExportPkcs8PrivateKey(Span<byte> destination, out int bytesWritten);
        protected abstract bool TryExportPkcs8PrivateKeyCore(Span<byte> destination, out int bytesWritten);

        public byte[] ExportSubjectPublicKeyInfo();
        public string ExportSubjectPublicKeyInfoPem();
        public bool TryExportSubjectPublicKeyInfo(Span<byte> destination, out int bytesWritten);

        public static CompositeMLDsa ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, ReadOnlySpan<byte> source);
        public static CompositeMLDsa ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, ReadOnlySpan<byte> source);
        public static CompositeMLDsa ImportEncryptedPkcs8PrivateKey(string password, byte[] source);
        public static CompositeMLDsa ImportFromEncryptedPem(ReadOnlySpan<char> source, ReadOnlySpan<byte> passwordBytes);
        public static CompositeMLDsa ImportFromEncryptedPem(ReadOnlySpan<char> source, ReadOnlySpan<char> password);
        public static CompositeMLDsa ImportFromEncryptedPem(string source, byte[] passwordBytes);
        public static CompositeMLDsa ImportFromEncryptedPem(string source, string password);
        public static CompositeMLDsa ImportFromPem(ReadOnlySpan<char> source);
        public static CompositeMLDsa ImportFromPem(string source);
        public static CompositeMLDsa ImportPkcs8PrivateKey(byte[] source);
        public static CompositeMLDsa ImportPkcs8PrivateKey(ReadOnlySpan<byte> source);
        public static CompositeMLDsa ImportSubjectPublicKeyInfo(byte[] source);
        public static CompositeMLDsa ImportSubjectPublicKeyInfo(ReadOnlySpan<byte> source);
    }

    // S.S.C and M.B.C.
    [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
    public sealed class CompositeMLDsaAlgorithm : IEquatable<CompositeMLDsaAlgorithm>
    {
        internal CompositeMLDsaAlgorithm() { }
        public int MaxSignatureSizeInBytes { get; }
        public static CompositeMLDsaAlgorithm MLDsa44WithECDsaP256 { get; }
        public static CompositeMLDsaAlgorithm MLDsa44WithEd25519 { get; }
        public static CompositeMLDsaAlgorithm MLDsa44WithRSA2048Pkcs15 { get; }
        public static CompositeMLDsaAlgorithm MLDsa44WithRSA2048Pss { get; }
        public static CompositeMLDsaAlgorithm MLDsa65WithECDsaBrainpoolP256r1 { get; }
        public static CompositeMLDsaAlgorithm MLDsa65WithECDsaP256 { get; }
        public static CompositeMLDsaAlgorithm MLDsa65WithECDsaP384 { get; }
        public static CompositeMLDsaAlgorithm MLDsa65WithEd25519 { get; }
        public static CompositeMLDsaAlgorithm MLDsa65WithRSA3072Pkcs15 { get; }
        public static CompositeMLDsaAlgorithm MLDsa65WithRSA3072Pss { get; }
        public static CompositeMLDsaAlgorithm MLDsa65WithRSA4096Pkcs15 { get; }
        public static CompositeMLDsaAlgorithm MLDsa65WithRSA4096Pss { get; }
        public static CompositeMLDsaAlgorithm MLDsa87WithECDsaBrainpoolP384r1 { get; }
        public static CompositeMLDsaAlgorithm MLDsa87WithECDsaP384 { get; }
        public static CompositeMLDsaAlgorithm MLDsa87WithECDsaP521 { get; }
        public static CompositeMLDsaAlgorithm MLDsa87WithEd448 { get; }
        public static CompositeMLDsaAlgorithm MLDsa87WithRSA3072Pss { get; }
        public static CompositeMLDsaAlgorithm MLDsa87WithRSA4096Pss { get; }
        public string Name { get; }
        public override bool Equals([NotNullWhen(true)] object? obj);
        public bool Equals([NotNullWhen(true)] CompositeMLDsaAlgorithm? other);
        public override int GetHashCode();
        public static bool operator ==(CompositeMLDsaAlgorithm? left, CompositeMLDsaAlgorithm? right);
        public static bool operator !=(CompositeMLDsaAlgorithm? left, CompositeMLDsaAlgorithm? right);
        public override string ToString();
    }

    // S.S.C and M.B.C.
    // M.B.C is .NET Framework and .NET only. No .NET Standard.
    [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
    public sealed class CompositeMLDsaCng : CompositeMLDsa
    {
        [SupportedOSPlatform("windows")]
        public CompositeMLDsaCng(CngKey key);

        public CngKey GetKey();
    }
}

namespace System.Security.Cryptography.X509Certificates
{
    // S.S.C
    public sealed partial class CertificateRequest
    {
        [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public CertificateRequest(X500DistinguishedName subjectName, CompositeMLDsa key);

        [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public CertificateRequest(string subjectName, CompositeMLDsa key);
    }

    // S.S.C
    public sealed partial class PublicKey
    {
        public PublicKey(CompositeMLDsa key);

        [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        [UnsupportedOSPlatform("browser")]
        public CompositeMLDsa? GetCompositeMLDsaPublicKey();
    }

    // S.S.C
    public partial class X509Certificate2 : X509Certificate
    {
        [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public X509Certificate2 CopyWithPrivateKey(CompositeMLDsa privateKey);

        [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public CompositeMLDsa? GetCompositeMLDsaPrivateKey();

        [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public CompositeMLDsa? GetCompositeMLDsaPublicKey();
    }

    // M.B.C
    public static partial class X509CertificateKeyAccessors
    {
        [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public static CompositeMLDsa? GetCompositeMLDsaPublicKey(this X509Certificate2 certificate);

        [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public static CompositeMLDsa? GetCompositeMLDsaPrivateKey(this X509Certificate2 certificate);

        [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public static X509Certificate2 CopyWithPrivateKey(this X509Certificate2 certificate, CompositeMLDsa privateKey);
    }

    // S.S.C
    public abstract partial class X509SignatureGenerator
    {
        [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public static X509SignatureGenerator CreateForCompositeMLDsa(CompositeMLDsa key);
    }
}

namespace System.Security.Cryptography.Pkcs
{
    public sealed partial class CmsSigner
    {
        // S.S.C.P - NetCoreApp only.
        [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public CmsSigner(SubjectIdentifierType signerIdentifierType, X509Certificate2? certificate, CompositeMLDsa? privateKey);
    }
}
```
## System.ClientModel Add support for proxy reader / writers

**Approved** | [#runtime/115664](https://github.com/dotnet/runtime/issues/115664#issuecomment-3156178983)

* We discussed AddProxy vs SetProxy, and decided that while it has set-semantics (last write wins), since the key is the type parameter it would make code more legible to use "Add"
  * Since this library uses "frozen options" conventions, AddProxy should throw for frozen copies.
* We discussed "ModelProxy" vs "ProxiedModel", and decided the proposed name is slightly better for technical reasons.
* Looks good as proposed.

```C#
namespace System.ClientModel.Primitives;

public partial class ModelReaderWriterOptions
{ 
    public object? ProxiedModel { get; }

    public void AddProxy<T>(IPersistableModel<T> proxy); 
    public IPersistableModel<T> ResolveProxy<T>(IPersistableModel<T> model); 
    public IJsonModel<T> ResolveProxy<T>(IJsonModel<T> model); 
    public bool TryGetProxy<T>(out IPersistableModel<T>? proxy); 
    public bool TryGetProxy<T>(out IJsonModel<T>? proxy); 
}
``` 
