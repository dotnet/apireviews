# API Review 08/11/2026

## Obsolete IStartupValidator in favor of IAsyncStartupValidator

**Approved** | [#runtime/131906](https://github.com/dotnet/runtime/issues/131906#issuecomment-5256443299) | [Video](https://www.youtube.com/watch?v=Jwa-CuHL8gY&t=0h0m0s)

Looks good as proposed. (Using the next available warning number)

```csharp
namespace Microsoft.Extensions.Options;

[Obsolete("Implement IAsyncStartupValidator instead.")] // NEW
public partial interface IStartupValidator
{
    // EXISTING
    // void Validate();
}

// EXISTING: remains independent of IStartupValidator.
// public interface IAsyncStartupValidator
// {
//     Task ValidateAsync(CancellationToken cancellationToken = default);
// }
```
## Composite ML-KEM

**Approved** | [#runtime/129633](https://github.com/dotnet/runtime/issues/129633#issuecomment-5256629671) | [Video](https://www.youtube.com/watch?v=Jwa-CuHL8gY&t=0h9m22s)

* Looks good as proposed.
* Leaving out the PublicKey members is fine, as well as the other certificate-attachment API (if there's not currently use for them)

```csharp
namespace System.Security.Cryptography
{
    // System.Security.Cryptography and Microsoft.Bcl.Cryptography
    [Experimental("SYSLIB5006")]
    public abstract class CompositeMLKem : IDisposable
    {
        protected CompositeMLKem(CompositeMLKemAlgorithm algorithm);

        public static bool IsAlgorithmSupported(CompositeMLKemAlgorithm algorithm);

        public CompositeMLKemAlgorithm Algorithm { get; }

        public byte[] Decapsulate(byte[] ciphertext);
        public void Decapsulate(ReadOnlySpan<byte> ciphertext, Span<byte> sharedSecret);
        protected abstract void DecapsulateCore(ReadOnlySpan<byte> ciphertext, Span<byte> sharedSecret);

        public void Encapsulate(out byte[] ciphertext, out byte[] sharedSecret);
        public void Encapsulate(Span<byte> ciphertext, Span<byte> sharedSecret);
        protected abstract void EncapsulateCore(Span<byte> ciphertext, Span<byte> sharedSecret);

        public byte[] ExportDecapsulationKey();
        public int ExportDecapsulationKey(Span<byte> destination);
        public bool TryExportDecapsulationKey(Span<byte> destination, out int bytesWritten);
        protected abstract int ExportDecapsulationKeyCore(Span<byte> destination);

        public byte[] ExportEncapsulationKey();
        public int ExportEncapsulationKey(Span<byte> destination);
        public bool TryExportEncapsulationKey(Span<byte> destination, out int bytesWritten);
        protected abstract int ExportEncapsulationKeyCore(Span<byte> destination);

        public static CompositeMLKem GenerateKey(CompositeMLKemAlgorithm algorithm);

        public static CompositeMLKem ImportEncapsulationKey(CompositeMLKemAlgorithm algorithm, ReadOnlySpan<byte> source);
        public static CompositeMLKem ImportEncapsulationKey(CompositeMLKemAlgorithm algorithm, byte[] source);

        public static CompositeMLKem ImportDecapsulationKey(CompositeMLKemAlgorithm algorithm, ReadOnlySpan<byte> source);
        public static CompositeMLKem ImportDecapsulationKey(CompositeMLKemAlgorithm algorithm, byte[] source);

        public void Dispose();
        protected virtual void Dispose(bool disposing);

        // PKCS#8 / SPKI / PEM import/export
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
        public string ExportSubjectPublicKeyInfoPem();
        public bool TryExportSubjectPublicKeyInfo(Span<byte> destination, out int bytesWritten);

        public static CompositeMLKem ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, ReadOnlySpan<byte> source);
        public static CompositeMLKem ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, ReadOnlySpan<byte> source);
        public static CompositeMLKem ImportEncryptedPkcs8PrivateKey(string password, byte[] source);

        public static CompositeMLKem ImportFromEncryptedPem(ReadOnlySpan<char> source, ReadOnlySpan<byte> passwordBytes);
        public static CompositeMLKem ImportFromEncryptedPem(ReadOnlySpan<char> source, ReadOnlySpan<char> password);
        public static CompositeMLKem ImportFromEncryptedPem(string source, byte[] passwordBytes);
        public static CompositeMLKem ImportFromEncryptedPem(string source, string password);

        public static CompositeMLKem ImportFromPem(ReadOnlySpan<char> source);
        public static CompositeMLKem ImportFromPem(string source);

        public static CompositeMLKem ImportPkcs8PrivateKey(byte[] source);
        public static CompositeMLKem ImportPkcs8PrivateKey(ReadOnlySpan<byte> source);

        public static CompositeMLKem ImportSubjectPublicKeyInfo(byte[] source);
        public static CompositeMLKem ImportSubjectPublicKeyInfo(ReadOnlySpan<byte> source);
    }
    
    // System.Security.Cryptography and Microsoft.Bcl.Cryptography
    [Experimental("SYSLIB5006")]
    public sealed class CompositeMLKemAlgorithm : IEquatable<CompositeMLKemAlgorithm>
    {
        internal CompositeMLKemAlgorithm();

        public static CompositeMLKemAlgorithm MLKem768WithRsaOaep2048 { get; }
        public static CompositeMLKemAlgorithm MLKem768WithRsaOaep3072 { get; }
        public static CompositeMLKemAlgorithm MLKem768WithRsaOaep4096 { get; }
        public static CompositeMLKemAlgorithm MLKem768WithX25519 { get; }
        public static CompositeMLKemAlgorithm MLKem768WithECDiffieHellmanP256 { get; }
        public static CompositeMLKemAlgorithm MLKem768WithECDiffieHellmanP384 { get; }
        public static CompositeMLKemAlgorithm MLKem768WithECDiffieHellmanBrainpoolP256r1 { get; }

        public static CompositeMLKemAlgorithm MLKem1024WithRsaOaep3072 { get; }
        public static CompositeMLKemAlgorithm MLKem1024WithECDiffieHellmanP384 { get; }
        public static CompositeMLKemAlgorithm MLKem1024WithECDiffieHellmanBrainpoolP384r1 { get; }
        public static CompositeMLKemAlgorithm MLKem1024WithX448 { get; }
        public static CompositeMLKemAlgorithm MLKem1024WithECDiffieHellmanP521 { get; }

        public string Name { get; }

        public int CiphertextSizeInBytes { get; }
        public int SharedSecretSizeInBytes { get; }

        public bool Equals([NotNullWhen(true)] CompositeMLKemAlgorithm? other);
        public override bool Equals([NotNullWhen(true)] object? obj);
        public override int GetHashCode();
        public override string ToString();

        public static bool operator ==(CompositeMLKemAlgorithm? left, CompositeMLKemAlgorithm? right);
        public static bool operator !=(CompositeMLKemAlgorithm? left, CompositeMLKemAlgorithm? right);
    }

    // System.Security.Cryptography and Microsoft.Bcl.Cryptography
    [Experimental("SYSLIB5006")]
    [SupportedOSPlatform("windows")]
    public sealed partial class CompositeMLKemCng : CompositeMLKem
    {
        public CompositeMLKemCng(CngKey key);

        public CngKey GetKey();
    }
}

namespace System.Security.Cryptography.X509Certificates
{
    // System.Security.Cryptography only
    public sealed partial class PublicKey
    {
        [Experimental("SYSLIB5006")]
        public PublicKey(CompositeMLKem key);

        [Experimental("SYSLIB5006")]
        [UnsupportedOSPlatform("browser")]
        public CompositeMLKem? GetCompositeMLKemPublicKey();
    }

    // System.Security.Cryptography only
    public partial class X509Certificate2
    {
        [Experimental("SYSLIB5006")]
        public CompositeMLKem? GetCompositeMLKemPublicKey();

        [Experimental("SYSLIB5006")]
        public CompositeMLKem? GetCompositeMLKemPrivateKey();

        [Experimental("SYSLIB5006")]
        public X509Certificate2 CopyWithPrivateKey(CompositeMLKem privateKey);
    }

    // Microsoft.Bcl.Cryptography only
    public static partial class X509CertificateKeyAccessors
    {
        [Experimental("SYSLIB5006")]
        public static CompositeMLKem? GetCompositeMLKemPublicKey(this X509Certificate2 certificate);

        [Experimental("SYSLIB5006")]
        public static CompositeMLKem? GetCompositeMLKemPrivateKey(this X509Certificate2 certificate);

        [Experimental("SYSLIB5006")]
        public static X509Certificate2 CopyWithPrivateKey(this X509Certificate2 certificate, CompositeMLKem privateKey);
    }
}
```
## Add incremental chunked reading support to CborReader similar to Utf8JsonReader style

**Approved** | [#runtime/129718](https://github.com/dotnet/runtime/issues/129718#issuecomment-5256840181) | [Video](https://www.youtube.com/watch?v=Jwa-CuHL8gY&t=0h24m27s)

Looks good as proposed

```csharp
namespace System.Formats.Cbor;

public partial enum CborReaderState
{
    NeedsMoreData,
}

public partial class CborReader
{
    public CborReader(ReadOnlyMemory<byte> data, CborReaderOptions? options, bool isFinalBlock);

    public void SlideData(ReadOnlyMemory<byte> data, bool isFinalBlock);

    public void Reset(ReadOnlyMemory<byte> data, bool isFinalBlock);

    public bool TrySkipValue(bool disableConformanceModeChecks = false);
    public bool TrySkipToParent(bool disableConformanceModeChecks = false);
}
```
## SVE AES: change arg names and types

**Approved** | [#runtime/131204](https://github.com/dotnet/runtime/issues/131204#issuecomment-5256872322) | [Video](https://www.youtube.com/watch?v=Jwa-CuHL8gY&t=0h44m53s)

Looks good as proposed.

```csharp
namespace System.Runtime.Intrinsics.Arm;

public abstract class SveAes : AdvSimd /// Feature: FEAT_SVE_AES
{

  public static Vector<byte> InverseMixColumns(Vector<byte> value); // AESIMC

  public static Vector<byte> MixColumns(Vector<byte> value); // AESMC

  public static Vector<byte> Decrypt(Vector<byte> value, Vector<byte> roundKey); // AESD

  public static Vector<byte> Encrypt(Vector<byte> value, Vector<byte> roundKey); // AESE

  public static Vector<ushort> PolynomialMultiplyWideningEven(Vector<byte> left, Vector<byte> right); // PMULLB

  public static Vector<ulong> PolynomialMultiplyWideningEven(Vector<uint> left, Vector<uint> right); // PMULLB

  public static Vector<ushort> PolynomialMultiplyWideningOdd(Vector<byte> left, Vector<byte> right); // PMULLT

  public static Vector<ulong> PolynomialMultiplyWideningOdd(Vector<uint> left, Vector<uint> right); // PMULLT

}
```
## Add CompositeMLDsa property to CngAlgorithm and CngAlgorithmGroup

**Approved** | [#runtime/130052](https://github.com/dotnet/runtime/issues/130052#issuecomment-5257014636) | [Video](https://www.youtube.com/watch?v=Jwa-CuHL8gY&t=0h48m2s)

* It was asked if Experimental is still needed, and we hedged with "yes"

```csharp
public sealed partial class CngAlgorithm
{
    [Experimental("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
    public static CngAlgorithm CompositeMLDsa { get; }
}

public sealed partial class CngAlgorithmGroup
{
    [Experimental("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
    public static CngAlgorithmGroup CompositeMLDsa { get; }
}
```
## KEM support for EnvelopedCms

**Approved** | [#runtime/130883](https://github.com/dotnet/runtime/issues/130883#issuecomment-5257188259) | [Video](https://www.youtube.com/watch?v=Jwa-CuHL8gY&t=1h1m51s)

* Let's go ahead and approve the CompositeMLDsa overload to Decrypt.
* Otherwise, looks good as proposed.

```csharp
namespace System.Security.Cryptography.Pkcs;

public sealed class KemRecipientInfo : RecipientInfo
{
    internal KemRecipientInfo();

    public override int Version { get; }
    public override SubjectIdentifier RecipientIdentifier { get; }
    public override AlgorithmIdentifier KeyEncryptionAlgorithm { get; }
    public override byte[] EncryptedKey { get; }

    public AlgorithmIdentifier KeyEncapsulationAlgorithm { get; }
    public ReadOnlyMemory<byte> KeyEncapsulationCiphertext { get; }
    public AlgorithmIdentifier KeyDerivationAlgorithm { get; }
    public int KeyEncryptionKeyLengthInBytes { get; }

    // Important, "null" means the OPTIONAL OCTET STRING is missing entirely.
    // Present, but empty, means the OCTET STRING was present with a length of zero.
    public ReadOnlyMemory<byte>? UserKeyingMaterial { get; }
}

public partial enum RecipientInfoType
{
    // Existing values omitted.
    KeyEncapsulation = 3,
}

public sealed partial class EnvelopedCms
{
    public void Decrypt(KemRecipientInfo recipientInfo, MLKem privateKey);
}

public sealed partial class CmsRecipient
{
    public static CmsRecipient CreateForKeyEncapsulation(
        X509Certificate2 certificate,
        ReadOnlySpan<byte> userKeyingMaterial);

    public static CmsRecipient CreateForKeyEncapsulation(
        SubjectIdentifierType recipientIdentifierType,
        X509Certificate2 certificate,
        ReadOnlySpan<byte> userKeyingMaterial);
}
```
## Add RFC3394 KeyWrap

**Approved** | [#runtime/130490](https://github.com/dotnet/runtime/issues/130490#issuecomment-5256985095) | [Video](https://www.youtube.com/watch?v=Jwa-CuHL8gY&t=1h18m28s)

* We discussed no-suffix vs "Unpadded" and went with no-suffix as that matches the algorithm name.

```csharp
namespace System.Security.Cryptography
{
    public partial class Aes
    {
        public byte[] EncryptKeyWrap(byte[] plaintext);
        public byte[] EncryptKeyWrap(ReadOnlySpan<byte> plaintext);
        public void EncryptKeyWrap(ReadOnlySpan<byte> plaintext, Span<byte> destination);
        protected virtual void EncryptKeyWrapCore(ReadOnlySpan<byte> source, Span<byte> destination);

        public byte[] DecryptKeyWrap(byte[] ciphertext);
        public byte[] DecryptKeyWrap(ReadOnlySpan<byte> ciphertext);
        public int DecryptKeyWrap(ReadOnlySpan<byte> ciphertext, Span<byte> destination);
        protected virtual int DecryptKeyWrapCore(ReadOnlySpan<byte> source, Span<byte> destination);
        public bool TryDecryptKeyWrap(ReadOnlySpan<byte> ciphertext, Span<byte> destination, out int bytesWritten);

        public static int GetKeyWrapLength(int plaintextLengthInBytes);
    }
}
```
## Add QUERY verb support to HttpClient

**Approved** | [#runtime/113522](https://github.com/dotnet/runtime/issues/113522#issuecomment-5257275199) | [Video](https://www.youtube.com/watch?v=Jwa-CuHL8gY&t=1h18m49s)

We know that there will be more coming throughout the release, but this part looks coherent as-is.

Looks good as proposed.

```csharp
namespace System.Net.Http;

public partial class HttpClient : HttpMessageInvoker
{
    // Exactly the same shape as PostAsync, just replacing the method name
    public Task<HttpResponseMessage> QueryAsync([StringSyntax("Uri")] string? requestUri, HttpContent? content);
    public Task<HttpResponseMessage> QueryAsync([StringSyntax("Uri")] string? requestUri, HttpContent? content, CancellationToken cancellationToken);
    public Task<HttpResponseMessage> QueryAsync(Uri? requestUri, HttpContent? content);
    public Task<HttpResponseMessage> QueryAsync(Uri? requestUri, HttpContent? content, CancellationToken cancellationToken)
}
```
## NetworkChange.IsSupported

**Approved** | [#runtime/129943](https://github.com/dotnet/runtime/issues/129943#issuecomment-5257306733) | [Video](https://www.youtube.com/watch?v=Jwa-CuHL8gY&t=1h26m53s)

Looks good as proposed

```csharp
namespace System.Net.NetworkInformation
{
    public partial class NetworkChange
    {
        public static bool IsSupported { get; }
    }
}
```
## InlineArray to Span conversion without Unsafe.As

**Approved** | [#runtime/105586](https://github.com/dotnet/runtime/issues/105586#issuecomment-5257378940) | [Video](https://www.youtube.com/watch?v=Jwa-CuHL8gY&t=1h29m40s)

Looks good as proposed.

There was a late question of "should we also add where TInlineArray : struct"?  And we'll just accept whatever position @jkotas wants to take.

```csharp
namespace System.Runtime.CompilerServices;

public class RuntimeHelpers
{
    public static Span<TElement> InlineArrayAsSpan<TInlineArray, TElement>(ref TInlineArray inlineArray) where TInlineArray: allows ref struct;
    public static ReadOnlySpan<TElement> InlineArrayAsReadOnlySpan<TInlineArray, TElement>(ref readonly TInlineArray inlineArray) where TInlineArray: allows ref struct;

}
```
## Consider adding a LOCK CMPXCHG16B intrinsic method

**Approved** | [#runtime/28711](https://github.com/dotnet/runtime/issues/28711#issuecomment-5257427370) | [Video](https://www.youtube.com/watch?v=Jwa-CuHL8gY&t=1h36m26s)

Added the missing "unsafe", otherwise looks good as proposed.

```csharp
namespace System.Runtime.Intrinsics.X86;

public static class Cmpxchg16B
{
    // Cx16 flag check using the CPUID instruction (cached).
    public static bool IsSupported { get; }
    
    // Returns the old value at destination.
    public static unsafe (ulong Lower, ulong Upper) InterlockedCompareExchange(void* destination, (ulong Lower, ulong Upper) value, (ulong Lower, ulong Upper) comparand);
}
```
## Option to limit SseParser's internal buffer size

**Approved** | [#runtime/129168](https://github.com/dotnet/runtime/issues/129168#issuecomment-5257579215) | [Video](https://www.youtube.com/watch?v=Jwa-CuHL8gY&t=1h42m8s)

* The default value of MaxBufferSize should be -1, rather than the actual buffer size value.

```csharp
namespace System.Net.ServerSentEvents;

public sealed class SseParserOptions<T>
{
    public SseParserOptions(SseItemParser<T> itemParser);

    public SseItemParser<T> ItemParser { get; }
    public int MaxBufferSize { get; set; } = -1;
}

public static partial class SseParser
{
    //public static SseParser<string> Create(Stream sseStream);
    //public static SseParser<T> Create<T>(Stream sseStream, SseItemParser<T> itemParser);
    public static SseParser<T> Create<T>(Stream sseStream, SseParserOptions<T> options);
}
```
