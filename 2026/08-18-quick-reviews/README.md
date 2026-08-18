# API Review 08/18/2026

## HPKE (Hybrid Public Key Encryption)

**Approved** | [#runtime/129308](https://github.com/dotnet/runtime/issues/129308#issuecomment-5332356482) | [Video](https://www.youtube.com/watch?v=0JG4_jJmMqY&t=0h0m0s)


* Moved HpkeSuite.Aead/Kdf/Kem out from nested enums (to HpkeAead, et al), and got rid of the accelerators (BECAUSE_THEY_REQUIRE_LOTS_OF_SCREAM_CASE).  We can always add accelerators back later.
* For the suite part enums we decided to juse use the IANA casing (though replacing hyphens with underscores), even though it violates normal naming guidelines, because the "properly" named alternatives ended up subjectively looking less legible due to the number of concatenated ideas.
* Renamed "aad" to "associatedData"
* Removed the "fill an array" overloads on HpkeSender.Seal and HpkeRecipient.Open and their Export methods
* Added HpkeSuite as a ctor parameter and state property for HpkeSender and HpkeRecipient, removed the abstract GetAeadTagSizeInBytes.
* We left all instances of "psk" as-is, as that's a recognized industry "word".
* Replaced the "Setup" verb with "Create".
* Swapped SenderPsk to PskSender, e.g. CreatePskSender

```csharp
namespace System.Security.Cryptography;

[Experimental(/* Next SYSLIB for experimental available */)]
public sealed partial class HpkeSuite : IEquatable<HpkeSuite>
{
    public HpkeSuite(HpkeKem kem, HpkeKdf kdf, HpkeAead aead);

    public HpkeAead AeadAlgorithm { get; }
    public HpkeKdf KdfAlgorithm { get; }
    public HpkeKem KemAlgorithm { get; }

    public int AeadTagSizeInBytes { get; }
    public int DecapsulationKeySizeInBytes { get; }
    public int EncapsulatedSecretSizeInBytes { get; }
    public int EncapsulationKeySizeInBytes { get; }
    public string Name { get; }

    public int GetCiphertextLength(int plaintextLength);

    public override bool Equals(object? obj);
    public bool Equals(HpkeSuite? other);
    public override int GetHashCode();
    public override string ToString();

    public static bool operator ==(HpkeSuite? left, HpkeSuite? right);
    public static bool operator !=(HpkeSuite? left, HpkeSuite? right);
}

[Experimental(/* Next SYSLIB for experimental available */)]
public enum HpkeAead
{
    // Values match IANA designations https://www.iana.org/assignments/hpke/hpke.xhtml
    AES_128_GCM = 1,
    AES_256_GCM = 2,
    ChaCha20Poly1305 = 3,
}

[Experimental(/* Next SYSLIB for experimental available */)]
public enum HpkeKdf
{
    // Values match IANA designations https://www.iana.org/assignments/hpke/hpke.xhtml
    HKDF_SHA256 = 1,
    HKDF_SHA384 = 2,
    HKDF_SHA512 = 3,
    SHAKE128 = 16,
    SHAKE256 = 17
}

[Experimental(/* Next SYSLIB for experimental available */)]
public enum HpkeKem
{
    // Values match IANA designations https://www.iana.org/assignments/hpke/hpke.xhtml
    DHKEM_P256_HKDF_SHA256 = 16,
    DHKEM_P384_HKDF_SHA384 = 17,

    DHKEM_X25519_HKDF_SHA256 = 32,
    
    MLKEM_512 = 64,
    
    MLKEM_768 = 65,
    MLKEM_1024 = 66,
    MLKEM768_P256 = 80, 
    MLKEM1024_P384 = 81,
}

[Experimental(/* Next SYSLIB for experimental available */)]
public abstract class Hpke : IDisposable
{
    protected Hpke(HpkeSuite suite);

    public static bool IsSupported(HpkeSuite suite);
    public HpkeSuite Suite { get; }

    // Key management
    public static Hpke GenerateKey(HpkeSuite suite);
    public static Hpke ImportDecapsulationKey(HpkeSuite suite, byte[] source);
    public static Hpke ImportDecapsulationKey(HpkeSuite suite, ReadOnlySpan<byte> source);
    public static Hpke ImportEncapsulationKey(HpkeSuite suite, byte[] source);
    public static Hpke ImportEncapsulationKey(HpkeSuite suite, ReadOnlySpan<byte> source);

    public byte[] ExportDecapsulationKey();
    public void ExportDecapsulationKey(Span<byte> destination);
    protected abstract void ExportDecapsulationKeyCore(Span<byte> destination);

    public byte[] ExportEncapsulationKey();
    public void ExportEncapsulationKey(Span<byte> destination);
    protected abstract void ExportEncapsulationKeyCore(Span<byte> destination);

    // Single-shot Seal (Base mode)
    public void Seal(ReadOnlySpan<byte> plaintext, out byte[] encapsulatedSecret, out byte[] ciphertext, ReadOnlySpan<byte> associatedData = default, ReadOnlySpan<byte> info = default);
    public void Seal(byte[] plaintext, out byte[] encapsulatedSecret, out byte[] ciphertext, byte[]? associatedData = null, byte[]? info = null);
    public void Seal(ReadOnlySpan<byte> plaintext, Span<byte> encapsulatedSecret, Span<byte> ciphertext, ReadOnlySpan<byte> associatedData = default, ReadOnlySpan<byte> info = default);
    protected abstract void SealCore(ReadOnlySpan<byte> plaintext, Span<byte> encapsulatedSecret, Span<byte> ciphertext, ReadOnlySpan<byte> associatedData, ReadOnlySpan<byte> info);

    // Single-shot Open (Base mode)
    public byte[] Open(ReadOnlySpan<byte> encapsulatedSecret, ReadOnlySpan<byte> ciphertext, ReadOnlySpan<byte> associatedData = default, ReadOnlySpan<byte> info = default);
    public byte[] Open(byte[] encapsulatedSecret, byte[] ciphertext, byte[]? associatedData = null, byte[]? info = null);
    public void Open(ReadOnlySpan<byte> encapsulatedSecret, ReadOnlySpan<byte> ciphertext, Span<byte> plaintext, ReadOnlySpan<byte> associatedData = default, ReadOnlySpan<byte> info = default);
    protected abstract void OpenCore(ReadOnlySpan<byte> encapsulatedSecret, ReadOnlySpan<byte> ciphertext, Span<byte> plaintext, ReadOnlySpan<byte> associatedData, ReadOnlySpan<byte> info);

    // SetupSender — Base mode
    public HpkeSender CreateSender(out byte[] encapsulatedSecret, ReadOnlySpan<byte> info = default);
    public HpkeSender CreateSender(Span<byte> encapsulatedSecret, ReadOnlySpan<byte> info = default);
    protected abstract HpkeSender CreateSenderCore(Span<byte> encapsulatedSecret, ReadOnlySpan<byte> info);

    // SetupRecipient — Base mode
    public HpkeRecipient CreateRecipient(ReadOnlySpan<byte> encapsulatedSecret, ReadOnlySpan<byte> info = default);
    public HpkeRecipient CreateRecipient(byte[] encapsulatedSecret, byte[]? info = null);
    protected abstract HpkeRecipient CreateRecipientCore(ReadOnlySpan<byte> encapsulatedSecret, ReadOnlySpan<byte> info);

    // SetupSender — PSK mode
    public HpkeSender CreatePskSender(ReadOnlySpan<byte> psk, ReadOnlySpan<byte> pskId, out byte[] encapsulatedSecret, ReadOnlySpan<byte> info = default);
    public HpkeSender CreatePskSender(byte[] psk, byte[] pskId, out byte[] encapsulatedSecret, byte[]? info = null);
    public HpkeSender CreatePskSender(ReadOnlySpan<byte> psk, ReadOnlySpan<byte> pskId, Span<byte> encapsulatedSecret, ReadOnlySpan<byte> info = default);
    protected abstract HpkeSender CreatePskSenderCore(Span<byte> encapsulatedSecret, ReadOnlySpan<byte> info, ReadOnlySpan<byte> psk, ReadOnlySpan<byte> pskId);

    // SetupRecipient — PSK mode
    public HpkeRecipient CreatePskRecipient(ReadOnlySpan<byte> encapsulatedSecret, ReadOnlySpan<byte> psk, ReadOnlySpan<byte> pskId, ReadOnlySpan<byte> info = default);
    public HpkeRecipient CreatePskRecipient(byte[] encapsulatedSecret, byte[] psk, byte[] pskId, byte[]? info = null);
    protected abstract HpkeRecipient CreatePskRecipientCore(ReadOnlySpan<byte> encapsulatedSecret, ReadOnlySpan<byte> info, ReadOnlySpan<byte> psk, ReadOnlySpan<byte> pskId);

    // Dispose
    public void Dispose();
    protected virtual void Dispose(bool disposing);
}

[Experimental(/* Next SYSLIB for experimental available */)]
public abstract class HpkeSender : IDisposable
{
    protected HpkeSender(HpkeSuite suite);

    public HpkeSuite Suite { get; }

    public byte[] Seal(ReadOnlySpan<byte> plaintext, ReadOnlySpan<byte> associatedData = default);
    public byte[] Seal(byte[] plaintext, byte[]? associatedData = null);
    public void Seal(ReadOnlySpan<byte> plaintext, Span<byte> ciphertext, ReadOnlySpan<byte> associatedData = default);
    protected abstract void SealCore(ReadOnlySpan<byte> plaintext, Span<byte> ciphertext, ReadOnlySpan<byte> associatedData);

    public byte[] Export(ReadOnlySpan<byte> exporterContext, int length);
    public byte[] Export(byte[] exporterContext, int length);
    public void Export(ReadOnlySpan<byte> exporterContext, Span<byte> destination);
    protected abstract void ExportCore(ReadOnlySpan<byte> exporterContext, Span<byte> destination);

    public void Dispose();
    protected virtual void Dispose(bool disposing);
}

[Experimental(/* Next SYSLIB for experimental available */)]
public abstract class HpkeRecipient : IDisposable
{
    protected HpkeRecipient(HpkeSuite suite);

    public HpkeSuite Suite { get; }

    public byte[] Open(ReadOnlySpan<byte> ciphertext, ReadOnlySpan<byte> associatedData = default);
    public byte[] Open(byte[] ciphertext, byte[]? associatedData = null);
    public void Open(ReadOnlySpan<byte> ciphertext, Span<byte> plaintext, ReadOnlySpan<byte> associatedData = default);
    protected abstract void OpenCore(ReadOnlySpan<byte> ciphertext, Span<byte> plaintext, ReadOnlySpan<byte> associatedData);

    public byte[] Export(ReadOnlySpan<byte> exporterContext, int length);
    public byte[] Export(byte[] exporterContext, int length);
    public void Export(ReadOnlySpan<byte> exporterContext, Span<byte> destination);
    protected abstract void ExportCore(ReadOnlySpan<byte> exporterContext, Span<byte> destination);

    public void Dispose();
    protected virtual void Dispose(bool disposing);
}
```
## Initial HTTP/2 stream limit on SocketsHttpHandler

**Approved** | [#runtime/132457](https://github.com/dotnet/runtime/issues/132457#issuecomment-5332688882) | [Video](https://www.youtube.com/watch?v=0JG4_jJmMqY&t=1h19m24s)

* After a very long debate, we removed the question mark, and the property will default to the implementation value 100.

```csharp
namespace System.Net.Http;

public partial class SocketsHttpHandler
{
    public int InitialHttp2MaxConcurrentStreams { get; set; }
}
```
