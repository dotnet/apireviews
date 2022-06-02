# API Review 06/02/2022

## Expose a high level authentication API

**Approved** | [#runtime/69920](https://github.com/dotnet/runtime/issues/69920#issuecomment-1145134016) | [Video](https://www.youtube.com/watch?v=NkWHoc8LjG8&t=0h0m0s)

* Looks generally good
* We considered a "less visible" namespace, but `System.Net.Security` is already fairly advanced, so this feels it belongs there.
* We considered making the options `get; init;` but since the constructors logically copy, there is no point.
* `NegotiateAuthentication`
    - Should either be sealed or implement the `Dispose` pattern. Given that nothing is `virtual`, sealing seems more appropriate.
    - A lot of the properties are settable; it seems they all should `get`-only as they are merely indicate where the internal state machine is at.
* `NegotiateAuthenticationStatusCode`
    - Consider trimming it down to the errors listed in the spec and/or the ones we need/think are important and treat the remainder as a pass through.


```C#
namespace System.Net.Security;

public class NegotiateAuthenticationClientOptions
{
    public string Package { get; set; }
    public NetworkCredential Credential { get; set; }
    public string? TargetName { get; set; }
    public ChannelBinding? Binding { get; set; }
    ProtectionLevel RequiredProtectionLevel { get; set; }
}
public class NegotiateAuthenticationServerOptions
{
    public string Package { get; set; }
    public NetworkCredential Credential { get; set; }
    public ChannelBinding? Binding { get; set; }
    ProtectionLevel RequiredProtectionLevel { get; set; }
}
public sealed class NegotiateAuthentication : IDisposable
{
    public NegotiateAuthentication(NegotiateAuthenticationClientOptions clientOptions);
    public NegotiateAuthentication(NegotiateAuthenticationServerOptions serverOptions);
    public bool IsAuthenticated { get; }
    public ProtectionLevel ProtectionLevel { get; }
    public bool IsSigned { get; }
    public bool IsEncrypted { get; }
    public bool IsMutuallyAuthenticated { get; }
    public bool IsServer { get; }
    public string Package { get; }
    public string? TargetName { get; }
    public IIdentity RemoteIdentity { get };
    public byte[] GetOutgoingBlob(ReadOnlySpan<byte> incomingBlob, out NegotiateAuthenticationStatusCode statusCode);
    public string GetOutgoingBlob(string incomingBlob, out NegotiateAuthenticationStatusCode statusCode);
}
public enum NegotiateAuthenticationStatusCode
{
    NotSet = 0,
    OK,
    ContinueNeeded,
    CompleteNeeded,
    CompleteAndContinue,
    ContextExpired,
    CredentialsNeeded,
    Renegotiate,
    TryAgain,
    OutOfMemory,
    InvalidHandle,
    Unsupported,
    TargetUnknown,
    InternalError,
    PackageNotFound,
    NotOwner,
    CannotInstall,
    InvalidToken,
    CannotPack,
    QopNotSupported,
    NoImpersonation,
    LogonDenied,
    UnknownCredentials,
    NoCredentials,
    MessageAltered,
    OutOfSequence,
    NoAuthenticatingAuthority,
    IncompleteMessage,
    IncompleteCredentials,
    BufferNotEnough,
    WrongPrincipal,
    TimeSkew,
    UntrustedRoot,
    IllegalMessage,
    CertUnknown,
    CertExpired,
    DecryptFailure,
    AlgorithmMismatch,
    SecurityQosFailed,
    SmartCardLogonRequired,
    UnsupportedPreauth,
    BadBinding,
    DowngradeDetected,
    ApplicationProtocolMismatch,
    NoRenegotiation
}
```
## Some System.Data abstraction types don't properly implement disposable pattern

**Rejected** | [#runtime/70049](https://github.com/dotnet/runtime/issues/70049#issuecomment-1145156269) | [Video](https://www.youtube.com/watch?v=NkWHoc8LjG8&t=0h41m48s)

* We concluded that doing nothing here is appropriate
* @terrajobst will file a separate issue for a generic analyzer, but this wouldn't apply here.
## [Analyzer]: Enforce Curiously Recurring Template Pattern in Generic Math interfaces

**Approved** | [#runtime/69775](https://github.com/dotnet/runtime/issues/69775#issuecomment-1145175168) | [Video](https://www.youtube.com/watch?v=NkWHoc8LjG8&t=0h58m14s)

* We rejected that attribute based approach because there is a chance this will become a general C# feature. Having a BCL attribute and analyzer would sort of create a competing language feature.
* The analyzer should be thought of as a stop gap solution for generic math until there is a C# feature. Whether or not we'll be able to use that is TBD, and largely depends on how the C# feature is expressed in metadata and whether we can update the generic math APIs in a binary compatible way.

Severity: **Warning**
Category: **Usage**
## Add APIs for COSE signing

**NeedsWork** | [#runtime/69896](https://github.com/dotnet/runtime/issues/69896#issuecomment-1145226538) | [Video](https://www.youtube.com/watch?v=NkWHoc8LjG8&t=1h17m59s)

* Consider either marking the entire assembly as `[UnsupportedOSPlatform("browser")]` or just the leaf nodes (e.g. `CoseSigner`'s constructor).
* Looks like the assembly isn't fully null annotated (e.g. the `byte[]` parameters)
    - We should do that, even if it's targeting .NET Standard 2.0.
* Consider making `CoseHeaderMap` a dictionary of `CoseHeaderKey`/`CoseHeaderValue`, which makes the operations more uniform and also solves the problem that all operations can work on `byte[]` not just `ReadOnlyMemory<byte>`

```C#
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
    [UnsupportedOSPlatform("browser")]
    public static byte[] SignDetached(byte[] detachedContent, CoseSigner signer, byte[]? associatedData = null);
    [UnsupportedOSPlatform("browser")]
    public static byte[] SignDetached(ReadOnlySpan<byte> detachedContent, CoseSigner signer, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public static byte[] SignDetached(Stream detachedContent, CoseSigner signer, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public static Task<byte[]> SignDetachedAsync(Stream detachedContent, CoseSigner signer, ReadOnlyMemory<byte> associatedData = default, CancellationToken cancellationToken = default(CancellationToken));
    [UnsupportedOSPlatform("browser")]
    public static byte[] SignEmbedded(byte[] embeddedContent, CoseSigner signer, byte[]? associatedData = null);
    [UnsupportedOSPlatform("browser")]
    public static byte[] SignEmbedded(ReadOnlySpan<byte> embeddedContent, CoseSigner signer, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public static bool TrySignEmbedded(ReadOnlySpan<byte> embeddedContent, Span<byte> destination, CoseSigner signer, out int bytesWritten, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public static bool TrySignDetached(ReadOnlySpan<byte> detachedContent, Span<byte> destination, CoseSigner signer, out int bytesWritten, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public bool VerifyDetached(AsymmetricAlgorithm key, byte[] detachedContent, byte[]? associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public bool VerifyDetached(AsymmetricAlgorithm key, ReadOnlySpan<byte> detachedContent, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public bool VerifyDetached(AsymmetricAlgorithm key, Stream detachedContent, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public Task<bool> VerifyDetachedAsync(AsymmetricAlgorithm key, Stream detachedContent, ReadOnlyMemory<byte> associatedData, CancellationToken cancellationToken = default(CancellationToken));

    [UnsupportedOSPlatform("browser")]
    public bool VerifyEmbedded(AsymmetricAlgorithm key, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public bool VerifyEmbedded(AsymmetricAlgorithm key, byte[]? associatedData = null);

    public override bool TryEncode(Span<byte> destination, out int bytesWritten);
    public override int GetEncodedLength();
}
public sealed class CoseMultiSignMessage : CoseMessage
{
    [UnsupportedOSPlatform("browser")]
    public static byte[] SignDetached(byte[] detachedContent, CoseSigner signer, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, byte[]? associatedData = null);
    [UnsupportedOSPlatform("browser")]
    public static byte[] SignDetached(ReadOnlySpan<byte> detachedContent, CoseSigner signer, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public static byte[] SignDetached(Stream detachedContent, CoseSigner signer, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public static Task<byte[]> SignDetachedAsync(Stream detachedContent, CoseSigner signer, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, ReadOnlyMemory<byte> associatedData = default, CancellationToken cancellationToken = default(CancellationToken));
    [UnsupportedOSPlatform("browser")]
    public static byte[] SignEmbedded(byte[] embeddedContent, CoseSigner signer, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, byte[]? associatedData = null);
    [UnsupportedOSPlatform("browser")]
    public static byte[] SignEmbedded(ReadOnlySpan<byte> embeddedContent, CoseSigner signer, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public static bool TrySignEmbedded(ReadOnlySpan<byte> embeddedContent, CoseSigner signer, out int bytesWritten, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public static bool TrySignDetached(ReadOnlySpan<byte> detachedContent, CoseSigner signer, out int bytesWritten, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null, ReadOnlySpan<byte> associatedData = default);
    public ReadOnlyCollection<CoseSignature> Signatures { get; }
    [UnsupportedOSPlatform("browser")]
    public void AddSignature(CoseSigner signer, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public void AddSignature(CoseSigner signer, byte[]? associatedData = null);
    public void RemoveSignature(CoseSignature signature);
    public void RemoveSignature(int index);
    public override bool TryEncode(Span<byte> destination, out int bytesWritten);
    public override int GetEncodedLength();
}
public sealed class CoseSignature
{
    public CoseHeaderMap ProtectedHeaders { get; }
    public CoseHeaderMap UnprotectedHeaders { get; }
    [UnsupportedOSPlatform("browser")]
    public bool VerifyEmbedded(AsymmetricAlgorithm key, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public bool VerifyDetached(AsymmetricAlgorithm key, byte[] detachedContent, byte[]? associatedData = null);
    [UnsupportedOSPlatform("browser")]
    public bool VerifyDetached(AsymmetricAlgorithm key, ReadOnlySpan<byte> detachedContent, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public bool VerifyDetached(AsymmetricAlgorithm key, Stream detachedContent, ReadOnlySpan<byte> associatedData = default);
    [UnsupportedOSPlatform("browser")]
    public Task<bool> VerifyDetachedAsync(AsymmetricAlgorithm key, Stream detachedContent, ReadOnlyMemory<byte> associatedData = default);
}
public sealed class CoseSigner
{
    public CoseSigner(AsymmetricAlgorithm key, HashAlgorithmName hashAlgorithm, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null);
    public CoseSigner(RSA key, RSASignaturePadding signaturePadding, HashAlgorithmName hashAlgorithm, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null);
    public AsymmetricAlgorithm Key { get; }
    public HashAlgorithmName HashAlgorithm { get; }
    public CoseHeaderMap? ProtectedHeaders { get; }
    public CoseHeaderMap? UnprotectedHeaders { get; }
    public RSASignaturePadding? RSASignaturePadding { get; }
}
public sealed class CoseHeaderMap : IEnumerable<(CoseHeaderLabel Label, ReadOnlyMemory<byte> EncodedValue)>, IEnumerable
{
    public CoseHeaderMap();
    public bool IsReadOnly { get; }
    public ReadOnlyMemory<byte> GetEncodedValue(CoseHeaderLabel label);
    public ReadOnlySpan<byte> GetValueAsBytes(CoseHeaderLabel label);
    public int GetValueAsInt32(CoseHeaderLabel label);
    public string GetValueAsString(CoseHeaderLabel label);
    public bool TryGetEncodedValue(CoseHeaderLabel label, out ReadOnlyMemory<byte> encodedValue);
    public void SetEncodedValue(CoseHeaderLabel label, ReadOnlySpan<byte> encodedValue);
    public void SetEncodedValue(CoseHeaderLabel label, byte[] encodedValue);
    public void SetValue(CoseHeaderLabel label, int value);
    public void SetValue(CoseHeaderLabel label, ReadOnlySpan<byte> value);
    public void SetValue(CoseHeaderLabel label, string value);
    public void Remove(CoseHeaderLabel label);
    public CoseHeaderMap.Enumerator GetEnumerator();
    IEnumerator IEnumerable.GetEnumerator();
    IEnumerator<(CoseHeaderLabel Label, ReadOnlyMemory<byte> EncodedValue)> IEnumerable<(CoseHeaderLabel Label, ReadOnlyMemory<Byte> EncodedValue)>.GetEnumerator();
    public struct Enumerator : IEnumerator<(CoseHeaderLabel Label, ReadOnlyMemory<byte> EncodedValue)>, IEnumerator, IDisposable
    {
        public readonly (CoseHeaderLabel Label, ReadOnlyMemory<byte> EncodedValue) Current { get; }
        object IEnumerator.Current { get; }
        public void Dispose();
        public bool MoveNext();
        public void Reset();
    }
}
public readonly struct CoseHeaderLabel : IEquatable<CoseHeaderLabel>
{
    public CoseHeaderLabel(int label);
    public CoseHeaderLabel(string label);
    public static CoseHeaderLabel Algorithm { get; }
    public static CoseHeaderLabel ContentType { get; }
    public static CoseHeaderLabel CounterSignature { get; }
    public static CoseHeaderLabel Critical { get; }
    public static CoseHeaderLabel IV { get; }
    public static CoseHeaderLabel KeyIdentifier { get; }
    public static CoseHeaderLabel PartialIV { get; }
    public override bool Equals([NotNullWhenAttribute(true)] object? obj);
    public bool Equals(CoseHeaderLabel other);
    public override int GetHashCode();
    public static bool operator ==(CoseHeaderLabel left, CoseHeaderLabel right);
    public static bool operator !=(CoseHeaderLabel left, CoseHeaderLabel right);
}
```
