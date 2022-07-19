# API Review 07/19/2022

## Add SqlTypes members to remove SqlClient internal access hacks

**Approved** | [#runtime/51836](https://github.com/dotnet/runtime/issues/51836#issuecomment-1189374660) | [Video](https://www.youtube.com/watch?v=R6anESydWvY&t=0h0m0s)

* `SqlDateTime`
    - The name `FromInternalRepresentation` feels off
    - Why does `SqlDateTime.FromInternalRepresentation` return a DateTime and not a `SqlDateTime`?
    - Do we need this at all?
    - How about `public static SqlDateTime FromParts(int dayPart, int timePart)`
* `SqlMoney`
    - How about `FromTdsValues` to `ToTdsValue`? This makes it clear what "internal representation" it refers to.
* `SqlDecimal`
    - Should return the number of elements written to the supplied span
    - `int GetTdsValue(Span<uint> destination)`
    - Don't you need a `SqlDecimal FromTdsValue(ReadOnlySpan<uint> source)`?
    - Why `int`? The constructor takes them as four `int` values.
* `SqlBinary`, `SqlGuid`
    - APIs that don't copy are unusual, but this works

```C#
namespace System.Data.SqlTypes;

public partial struct SqlDateTime
{
    public static SqlDateTime FromParts(int dayPart, int timePart);
}

public partial struct SqlMoney
{
    public static SqlMoney FromTdsValue(long value);
    public long GetTdsValue();
}

public partial struct SqlDecimal
{
    public int WriteTdsValue(Span<uint> destination);
}

public partial struct SqlBinary
{
    public static SqlBinary WrapBytes(byte[] bytes);
}

public partial struct SqlGuid
{
    public static SqlGuid WrapBytes(byte[] bytes);
}
```
## X509AuthorityKeyIdentifierExtension should be public

**Approved** | [#runtime/24931](https://github.com/dotnet/runtime/issues/24931#issuecomment-1189393946) | [Video](https://www.youtube.com/watch?v=R6anESydWvY&t=0h32m54s)

* `X509AuthorityKeyIdentifierExtension`
    - `SimpleIssuer` -> `NamedIssuer`
* `X509Certificate`
    - `SerialNumberBytes` -> `SerialNumberMemory` (to be consistent with `X509Certificate2.RawDataMemory`)

```C#
namespace System.Security.Cryptography.X509Certificates;

public sealed partial class X509AuthorityKeyIdentifierExtension : X509Extension
{
    public X509AuthorityKeyIdentifierExtension();
    public X509AuthorityKeyIdentifierExtension(byte[] rawData, bool critical = false);
    public X509AuthorityKeyIdentifierExtension(ReadOnlySpan<byte> rawData, bool critical = false);

    public ReadOnlyMemory<byte>? KeyIdentifier { get; }
    public ReadOnlyMemory<byte>? RawIssuer { get; }
    public X500DistinguishedName? NamedIssuer { get; }
    public ReadOnlyMemory<byte>? SerialNumber { get; }

    public static X509AuthorityKeyIdentifierExtension CreateFromSubjectKeyIdentifier(byte[] subjectKeyIdentifier);
    public static X509AuthorityKeyIdentifierExtension CreateFromSubjectKeyIdentifier(ReadOnlySpan<byte> subjectKeyIdentifier);
    public static X509AuthorityKeyIdentifierExtension CreateFromSubjectKeyIdentifier(X509SubjectKeyIdentifierExtension subjectKeyIdentifier);

    public static X509AuthorityKeyIdentifierExtension CreateFromIssuerNameAndSerialNumber(X500DistinguishedName issuerName, byte[] serialNumber);
    public static X509AuthorityKeyIdentifierExtension CreateFromIssuerNameAndSerialNumber(X500DistinguishedName issuerName, ReadOnlySpan<byte> serialNumber);

    public static X509AuthorityKeyIdentifierExtension Create(byte[] keyIdentifier, X500DistinguishedName issuerName, byte[] serialNumber);
    public static X509AuthorityKeyIdentifierExtension Create(ReadOnlySpan<byte> keyIdentifier, X500DistinguishedName issuerName, ReadOnlySpan<byte> serialNumber);

    public static X509AuthorityKeyIdentifierExtension CreateFromCertificate(X509Certificate2 certificate, bool includeKeyIdentifier, bool includeIssuerAndSerial);
}

public partial class X509Certificate
{
    public ReadOnlyMemory<byte> SerialNumberMemory { get; }
}

public partial class X509SubjectKeyIdentifierExtension
{
    public ReadOnlyMemory<byte> SubjectKeyIdentifierBytes { get; }
}
```
## Provide a way to load a CertificateRequest from a byte[]

**Approved** | [#runtime/29547](https://github.com/dotnet/runtime/issues/29547#issuecomment-1189429434) | [Video](https://www.youtube.com/watch?v=R6anESydWvY&t=0h54m45s)

* `CertificateRevocationListBuilder`
    - `Build`: let's make `thisUpdate` optional
    - Let's remove `ExpireEntries` until someone asks for it   
* `CertificateRequest` let's not use `unsafe` parameters because they can disappear from the call sites. Suggestion from YouTube was to use an enum, which we liked.

```C#
namespace System.Security.Cryptography.X509Certificates;

public sealed partial class CertificateRevocationListBuilder
{
    public CertificateRevocationListBuilder();

    public void AddEntry(byte[] serialNumber,
                         DateTimeOffset? revocationTime = default,
                         X509RevocationReason? reason = default);
    public void AddEntry(ReadOnlySpan<byte> serialNumber,
                         DateTimeOffset? revocationTime = default,
                         X509RevocationReason? reason = default);

    public void AddEntry(X509Certificate2 certificate,
                         DateTimeOffset? revocationTime = default,
                         X509RevocationReason? reason = default);
    
    public byte[] Build(X509Certificate2 issuerCertificate,
                        BigInteger crlNumber,
                        DateTimeOffset nextUpdate,
                        HashAlgorithmName hashAlgorithm,
                        RSASignaturePadding? rsaSignaturePadding = null,
                        DateTimeOffset? thisUpdate = null);

    public byte[] Build(X500DistinguishedName issuerName,
                        X509SignatureGenerator generator,
                        BigInteger crlNumber,
                        DateTimeOffset nextUpdate,
                        HashAlgorithmName hashAlgorithm,
                        X509AuthorityKeyIdentifierExtension akid,
                        DateTimeOffset? thisUpdate = null));


    public static CertificateRevocationListBuilder Load(byte[] currentCrl,
                                                        out BigInteger currentCrlNumber);

    public static CertificateRevocationListBuilder Load(ReadOnlySpan<byte> currentCrl,
                                                        out BigInteger currentCrlNumber,
                                                        out int bytesConsumed);

    public static CertificateRevocationListBuilder LoadPem(string currentCrl,
                                                           out BigInteger currentCrlNumber);

    public static CertificateRevocationListBuilder LoadPem(ReadOnlySpan<char> currentCrl,
                                                           out BigInteger currentCrlNumber);

    public bool RemoveEntry(byte[] serialNumber);
    public bool RemoveEntry(ReadOnlySpan<byte> serialNumber);

    public static X509Extension BuildCrlDistributionPointExtension(IEnumerable<string> uris,
                                                                   bool critical = false);
}

public enum X509RevocationReason
{
    Unspecified = 0,
    KeyCompromise = 1,
    CACompromise = 2,
    AffiliationChanged = 3,
    Superseded = 4,
    CessationOfOperation = 5,
    CertificateHold = 6,
    RemoveFromCrl = 8,
    PrivilegeWithdrawn = 9,
    AACompromise = 10,
    WeakAlgorithmOrKey = 11,
}

public partial class CertificateRequest
{
    public CertificateRequest(X500DistinguishedName subjectName,
                              PublicKey publicKey,
                              HashAlgorithmName hashAlgorithm,
                              RSASignaturePadding? rsaSignaturePadding = default);

    public Collection<AsnEncodedData> OtherRequestAttributes { get; }

    public static CertificateRequest LoadSigningRequest(byte[] pkcs10,
                                                        HashAlgorithmName signerHashAlgorithm,
                                                        CertificateRequestLoadOptions options = default,
                                                        RSASignaturePadding? signerSignaturePadding = null);

    public static CertificateRequest LoadSigningRequest(ReadOnlySpan<byte> pkcs10,
                                                        HashAlgorithmName signerHashAlgorithm,
                                                        out int bytesConsumed,
                                                        CertificateRequestLoadOptions options = default,
                                                        RSASignaturePadding? signerSignaturePadding = null);

    public static CertificateRequest LoadSigningRequestPem(ReadOnlySpan<char> pkcs10Pem,
                                                           HashAlgorithmName signerHashAlgorithm,
                                                           CertificateRequestLoadOptions options = default,
                                                           RSASignaturePadding? signerSignaturePadding = null);

    public static CertificateRequest LoadSigningRequestPem(string pkcs10Pem,
                                                           HashAlgorithmName signerHashAlgorithm,
                                                           CertificateRequestLoadOptions options = default,
                                                           RSASignaturePadding? signerSignaturePadding = null);

    public string CreateSigningRequestPem();
    public string CreateSigningRequestPem(X509SignatureGenerator signatureGenerator);
}

[Flags]
public enum CertificateRequestLoadOptions
{
    Default = 0x0,
    SkipSignatureValidation = 0x01,
    UnsafeLoadCertificateExtensions = 0x02
}

public partial class X509BasicConstraintsExtension
{
    public static X509BasicConstraintsExtension CreateForCertificateAuthority(int? pathLengthConstraint = null);
    public static X509BasicConstraintsExtension CreateForEndEntity(bool critical = false);
}
```
## make RateLimiterOptions follow Options pattern

**Approved** | [#runtime/72389](https://github.com/dotnet/runtime/issues/72389#issuecomment-1189436811) | [Video](https://www.youtube.com/watch?v=R6anESydWvY&t=1h31m31s)

* Looks good as proposed
    - @halter73 will update the issue (and this comment) with all the option types
* One problem with the option callback pattern is that there is no way to annotate what is required

```diff
namespace System.Threading.RateLimiting;

public class sealed TokenBucketRateLimiterOptions
{
-     public TokenBucketRateLimiterOptions(int tokenLimit, QueueProcessingOrder queueProcessingOrder, int queueLimit, TimeSpan replenishmentPeriod, int tokensPerPeriod, bool autoReplenishment = true);
+     public TokenBucketRateLimiterOptions();

-     public TimeSpan ReplenishmentPeriod { get; }
+     public TimeSpan ReplenishmentPeriod { get;  set; }
-     public int TokensPerPeriod { get; }
+     public int TokensPerPeriod { get; set; }
-     public bool AutoReplenishment { get; }
+     public bool AutoReplenishment { get; set; }
-     public int TokenLimit { get; }
+     public int TokenLimit { get; set; }
-     public QueueProcessingOrder QueueProcessingOrder { get; }
+     public QueueProcessingOrder QueueProcessingOrder { get; set; }
-     public int QueueLimit { get; }
+     public int QueueLimit { get; set; }
}
```
## Consider moving UnsupportedOSPlatform on MD5 from the class to methods

**Approved** | [#runtime/68556](https://github.com/dotnet/runtime/issues/68556#issuecomment-1189462587) | [Video](https://www.youtube.com/watch?v=R6anESydWvY&t=1h40m2s)

This makes it consistent with the the other classes, so it looks good as proposed.

```diff
namespace System.Security.Cryptography
{
-   [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
    public partial class HMACMD5 : System.Security.Cryptography.HMAC
    {
        public const int HashSizeInBits = 128;
        public const int HashSizeInBytes = 16;
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public HMACMD5() { }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public HMACMD5(byte[] key) { }
        public override byte[] Key { get { throw null; } set { } }
        protected override void Dispose(bool disposing) { }
        protected override void HashCore(byte[] rgb, int ib, int cb) { }
        protected override void HashCore(System.ReadOnlySpan<byte> source) { }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static byte[] HashData(byte[] key, byte[] source) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static byte[] HashData(byte[] key, System.IO.Stream source) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static byte[] HashData(System.ReadOnlySpan<byte> key, System.IO.Stream source) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static int HashData(System.ReadOnlySpan<byte> key, System.IO.Stream source, System.Span<byte> destination) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static byte[] HashData(System.ReadOnlySpan<byte> key, System.ReadOnlySpan<byte> source) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static int HashData(System.ReadOnlySpan<byte> key, System.ReadOnlySpan<byte> source, System.Span<byte> destination) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static System.Threading.Tasks.ValueTask<byte[]> HashDataAsync(byte[] key, System.IO.Stream source, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken)) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static System.Threading.Tasks.ValueTask<int> HashDataAsync(System.ReadOnlyMemory<byte> key, System.IO.Stream source, System.Memory<byte> destination, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken)) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static System.Threading.Tasks.ValueTask<byte[]> HashDataAsync(System.ReadOnlyMemory<byte> key, System.IO.Stream source, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken)) { throw null; }
        protected override byte[] HashFinal() { throw null; }
        public override void Initialize() { }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static bool TryHashData(System.ReadOnlySpan<byte> key, System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten) { throw null; }
        protected override bool TryHashFinal(System.Span<byte> destination, out int bytesWritten) { throw null; }
    }

-   [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
    public abstract partial class MD5 : System.Security.Cryptography.HashAlgorithm
    {
        public const int HashSizeInBits = 128;
        public const int HashSizeInBytes = 16;
        protected MD5() { }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static new System.Security.Cryptography.MD5 Create() { throw null; }
        [System.Diagnostics.CodeAnalysis.RequiresUnreferencedCodeAttribute("The default algorithm implementations might be removed, use strong type references like 'RSA.Create()' instead.")]
        public static new System.Security.Cryptography.MD5? Create(string algName) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static byte[] HashData(byte[] source) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static byte[] HashData(System.IO.Stream source) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static int HashData(System.IO.Stream source, System.Span<byte> destination) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static byte[] HashData(System.ReadOnlySpan<byte> source) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static int HashData(System.ReadOnlySpan<byte> source, System.Span<byte> destination) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static System.Threading.Tasks.ValueTask<int> HashDataAsync(System.IO.Stream source, System.Memory<byte> destination, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken)) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static System.Threading.Tasks.ValueTask<byte[]> HashDataAsync(System.IO.Stream source, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken)) { throw null; }
+       [System.Runtime.Versioning.UnsupportedOSPlatformAttribute("browser")]
        public static bool TryHashData(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten) { throw null; }
    }
```
