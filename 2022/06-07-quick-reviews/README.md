# API Review 06/07/2022

## JavaScript interop with [JSImport] and [JSExport] attributes and Roslyn

**NeedsWork** | [#runtime/70133](https://github.com/dotnet/runtime/issues/70133#issuecomment-1149047500) | [Video](https://www.youtube.com/watch?v=g9Xu9OzSgyc&t=0h0m0s)

* It seems we may want to have a default for common types, such as `string`.
* We'd like away to annotate the managed declaration as `void` even if the JS side returns a value
    - `[return: JSMarshalAs(JSType.Discard)]` might be a good way of doing that
* We need to decide what to do with nullable reference type annotations
    - We seem to have a bias towards ignoring, i.e. we wouldn't emit checks in the generated code that guards for null-values
* It seems there might be some benefits of using an established RPC envelope rather than a bespoke layout.
    - Are we sure that we're able to keep it a private implementation detail that we can change later?
## Add APIs for COSE signing

**Approved** | [#runtime/69896](https://github.com/dotnet/runtime/issues/69896#issuecomment-1149061469) | [Video](https://www.youtube.com/watch?v=g9Xu9OzSgyc&t=1h50m54s)

* `CoseHeaderMap`
    - Add `IReadOnlyDictionary<CoseHeaderLabel, CoseHeaderValue>`

```C#
[assembly: System.Runtime.Versioning.UnsupportedOSPlatform("browser")]
namespace System.Security.Cryptography.Cose;

public abstract class CoseMessage
{
    public ReadOnlyMemory<byte>? Content { get; }
    public CoseHeaderMap ProtectedHeaders { get; }
    public CoseHeaderMap UnprotectedHeaders { get; }
    public static CoseSign1Message DecodeSign1(byte[] cborPayload);
    public static CoseSign1Message DecodeSign1(ReadOnlySpan<byte> cborPayload);
    public static CoseMultiSignMessage DecodeMultiSign(byte[] cborPayload);
    public static CoseMultiSignMessage DecodeMultiSign(ReadOnlySpan<byte> cborPayload);
    public byte[] Encode();
    public int Encode(Span<byte> destination);
    public abstract bool TryEncode(Span<byte> destination, out int bytesWritten);
    public abstract int GetEncodedLength();
}

public sealed class CoseSign1Message : CoseMessage
{
    public static byte[] SignDetached(byte[] detachedContent, CoseSigner signer, byte[]? associatedData = null);
    public static byte[] SignDetached(ReadOnlySpan<byte> detachedContent, CoseSigner signer, ReadOnlySpan<byte> associatedData = default);
    public static byte[] SignDetached(Stream detachedContent, CoseSigner signer, ReadOnlySpan<byte> associatedData = default);
    public static Task<byte[]> SignDetachedAsync(Stream detachedContent, CoseSigner signer, ReadOnlyMemory<byte> associatedData = default, CancellationToken cancellationToken = default(CancellationToken));
    
    public static byte[] SignEmbedded(byte[] embeddedContent, CoseSigner signer, byte[]? associatedData = null);
    public static byte[] SignEmbedded(ReadOnlySpan<byte> embeddedContent, CoseSigner signer, ReadOnlySpan<byte> associatedData = default);
    
    public static bool TrySignEmbedded(ReadOnlySpan<byte> embeddedContent, Span<byte> destination, CoseSigner signer, out int bytesWritten, ReadOnlySpan<byte> associatedData = default);
    public static bool TrySignDetached(ReadOnlySpan<byte> detachedContent, Span<byte> destination, CoseSigner signer, out int bytesWritten, ReadOnlySpan<byte> associatedData = default);
    
    public bool VerifyDetached(AsymmetricAlgorithm key, byte[] detachedContent, byte[]? associatedData = null);
    public bool VerifyDetached(AsymmetricAlgorithm key, ReadOnlySpan<byte> detachedContent, ReadOnlySpan<byte> associatedData = default);
    public bool VerifyDetached(AsymmetricAlgorithm key, Stream detachedContent, ReadOnlySpan<byte> associatedData = default);
    public Task<bool> VerifyDetachedAsync(AsymmetricAlgorithm key, Stream detachedContent, ReadOnlyMemory<byte> associatedData = default, CancellationToken cancellationToken = default(CancellationToken));
    public bool VerifyEmbedded(AsymmetricAlgorithm key, byte[]? associatedData = null);
    public bool VerifyEmbedded(AsymmetricAlgorithm key, ReadOnlySpan<byte> associatedData = default);
    
    public override bool TryEncode(Span<byte> destination, out int bytesWritten);
    public override int GetEncodedLength();
}

public sealed class CoseMultiSignMessage : CoseMessage
{
    public static byte[] SignDetached(byte[] detachedContent, CoseSigner signer, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, byte[]? associatedData = null);
    public static byte[] SignDetached(ReadOnlySpan<byte> detachedContent, CoseSigner signer, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, ReadOnlySpan<byte> associatedData = default);
    public static byte[] SignDetached(Stream detachedContent, CoseSigner signer, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, ReadOnlySpan<byte> associatedData = default);
    public static Task<byte[]> SignDetachedAsync(Stream detachedContent, CoseSigner signer, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, ReadOnlyMemory<byte> associatedData = default, CancellationToken cancellationToken = default(CancellationToken));
    public static byte[] SignEmbedded(byte[] embeddedContent, CoseSigner signer, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, byte[]? associatedData = null);
    public static byte[] SignEmbedded(ReadOnlySpan<byte> embeddedContent, CoseSigner signer, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, ReadOnlySpan<byte> associatedData = default);
    
    public static bool TrySignEmbedded(ReadOnlySpan<byte> embeddedContent, CoseSigner signer, out int bytesWritten, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, ReadOnlySpan<byte> associatedData = default);
    public static bool TrySignDetached(ReadOnlySpan<byte> detachedContent, CoseSigner signer, out int bytesWritten, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, ReadOnlySpan<byte> associatedData = default);

    public ReadOnlyCollection<CoseSignature> Signatures { get; }
    public void AddSignature(CoseSigner signer, byte[]? associatedData = null);
    public void AddSignature(CoseSigner signer, ReadOnlySpan<byte> associatedData = default);
    public void RemoveSignature(CoseSignature signature);
    public void RemoveSignature(int index);

    public override bool TryEncode(Span<byte> destination, out int bytesWritten);
    public override int GetEncodedLength();
}

public sealed class CoseSignature
{
    public CoseHeaderMap ProtectedHeaders { get; }
    public CoseHeaderMap UnprotectedHeaders { get; }
    public bool VerifyEmbedded(AsymmetricAlgorithm key, byte[]? associatedData = null);
    public bool VerifyEmbedded(AsymmetricAlgorithm key, ReadOnlySpan<byte> associatedData = default);
    public bool VerifyDetached(AsymmetricAlgorithm key, byte[] detachedContent, byte[]? associatedData = null);
    public bool VerifyDetached(AsymmetricAlgorithm key, ReadOnlySpan<byte> detachedContent, ReadOnlySpan<byte> associatedData = default);
    public bool VerifyDetached(AsymmetricAlgorithm key, Stream detachedContent, ReadOnlySpan<byte> associatedData = default);
    public Task<bool> VerifyDetachedAsync(AsymmetricAlgorithm key, Stream detachedContent, ReadOnlyMemory<byte> associatedData = default);
}

public sealed class CoseSigner
{
    public CoseSigner(AsymmetricAlgorithm key, HashAlgorithmName hashAlgorithm, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null);
    public CoseSigner(RSA key, RSASignaturePadding signaturePadding, HashAlgorithmName hashAlgorithm, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null);
    public AsymmetricAlgorithm Key { get; }
    public HashAlgorithmName HashAlgorithm { get; }
    public CoseHeaderMap ProtectedHeaders { get; }
    public CoseHeaderMap UnprotectedHeaders { get; }
    public RSASignaturePadding? RSASignaturePadding{ get; }
}

public sealed partial class CoseHeaderMap : IDictionary<CoseHeaderLabel, CoseHeaderValue>
{
    public CoseHeaderValue this[CoseHeaderLabel key] { get; set; }
    public ICollection<CoseHeaderLabel> Keys { get; }
    public ICollection<CoseHeaderValue> Values { get; }
    public int Count { get; }
    public bool IsReadOnly { get; }

    public void Add(CoseHeaderLabel key, CoseHeaderValue value);
    public void Add(KeyValuePair<CoseHeaderLabel, CoseHeaderValue> item);
    public void Clear();
    public bool Contains(KeyValuePair<CoseHeaderLabel, CoseHeaderValue> item);
    public bool ContainsKey(CoseHeaderLabel key);
    public void CopyTo(KeyValuePair<CoseHeaderLabel, CoseHeaderValue>[] array, int arrayIndex);
    public IEnumerator<KeyValuePair<CoseHeaderLabel, CoseHeaderValue>> GetEnumerator();
    public bool Remove(CoseHeaderLabel key);
    public bool Remove(KeyValuePair<CoseHeaderLabel, CoseHeaderValue> item);
    public bool TryGetValue(CoseHeaderLabel key, [MaybeNullWhen(false)] out CoseHeaderValue value);
    IEnumerator IEnumerable.GetEnumerator();

    public void Add(CoseHeaderLabel label, int value);
    public void Add(CoseHeaderLabel label, string value);
    public void Add(CoseHeaderLabel label, byte[] value);
    public void Add(CoseHeaderLabel label, ReadOnlySpan<byte> value);
    public int GetValueAsInt32(CoseHeaderLabel label);
    public string GetValueAsString(CoseHeaderLabel label);
    public byte[] GetValueAsBytes(CoseHeaderLabel label);
    public int GetValueAsBytes(CoseHeaderLabel label, Span<byte> destination);
}

public readonly struct CoseHeaderLabel : IEquatable<CoseHeaderLabel>
{
    public CoseHeaderLabel(int label);
    public CoseHeaderLabel(string label);
    public static CoseHeaderLabel Algorithm { get; }
    public static CoseHeaderLabel ContentType { get; }
    public static CoseHeaderLabel CounterSignature { get; }
    public static CoseHeaderLabel CriticalHeaders { get; }
    public static CoseHeaderLabel KeyIdentifier { get; }

    public override bool Equals([NotNullWhenAttribute(true)] object? obj);
    public bool Equals(CoseHeaderLabel other);
    public override int GetHashCode();
    public static bool operator ==(CoseHeaderLabel left, CoseHeaderLabel right);
    public static bool operator !=(CoseHeaderLabel left, CoseHeaderLabel right);
}

public readonly struct CoseHeaderValue : IEquatable<CoseHeaderValue>
{
    public readonly ReadOnlyMemory<byte> EncodedValue { get; }
    public static CoseHeaderValue FromEncodedValue(byte[] encodedValue);
    public static CoseHeaderValue FromEncodedValue(ReadOnlySpan<byte> encodedValue);
    public static CoseHeaderValue FromInt32(int value);
    public static CoseHeaderValue FromString(string value);
    public static CoseHeaderValue FromBytes(byte[] value);
    public static CoseHeaderValue FromBytes(ReadOnlySpan<byte> value);
    public int GetValueAsInt32();
    public string GetValueAsString();
    public byte[] GetValueAsBytes();
    public int GetValueAsBytes(Span<byte> destination);

    public override bool Equals([NotNullWhenAttribute(true)] object? obj);
    public bool Equals(CoseHeaderValue other);
    public override int GetHashCode();
    public static bool operator ==(CoseHeaderValue left, CoseHeaderValue right);
    public static bool operator !=(CoseHeaderValue left, CoseHeaderValue right);
}
```
