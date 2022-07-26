# API Review 07/26/2022

## Type.GetEnumValuesAsUnderlyingType

**Approved** | [#runtime/72498](https://github.com/dotnet/runtime/issues/72498#issuecomment-1195765168) | [Video](https://www.youtube.com/watch?v=bDTLx5SXusY&t=0h0m0s)

* Let's add a corresponding method on `Enum` as this would maintain the parity with the existing methods that exist on both `Type` and `Enum`.
* Let's also add a generic version for parity

```C#
namespace System;

public partial class Type
{
    // Existing enum related methods:
    //
    // public virtual string? GetEnumName(Object);
    // public virtual string[] GetEnumNames();
    // public virtual Type GetEnumUnderlyingType();
    // public virtual object[] GetEnumValues();

    public virtual Array GetEnumValuesAsUnderlyingType();
}

public partial class Enum
{
    public static Array GetValuesAsUnderlyingType<TEnum>() where TEnum: struct, Enum;
    public static Array GetValuesAsUnderlyingType(Type type);
}
```
## Add leaveOpen concept to PartitionedRateLimiter.TranslateKey

**Approved** | [#runtime/72551](https://github.com/dotnet/runtime/issues/72551#issuecomment-1195797478) | [Video](https://www.youtube.com/watch?v=bDTLx5SXusY&t=0h18m45s)

* Let's make the parameter explicit to ensure people understand the transfer of ownership
    - Let's also rename it to `WithTranslatedKey`

```diff
namespace System.Threading.RateLimiting;

public abstract class PartitionedRateLimiter<TResource> : IAsyncDisposable, IDisposable
{
-    public PartitionedRateLimiter<TOuter> TranslateKey<TOuter>(Func<TOuter, TResource> keyAdapter);
+    public PartitionedRateLimiter<TOuter> WithTranslatedKey<TOuter>(Func<TOuter, TResource> keyAdapter, bool leaveOpen);
}
```
## Expose signature and protected headers raw bytes

**Approved** | [#runtime/72611](https://github.com/dotnet/runtime/issues/72611#issuecomment-1195807988) | [Video](https://www.youtube.com/watch?v=bDTLx5SXusY&t=0h46m51s)

* Let's rename `EncodedProtectedHeaders` to `RawProtectedHeadersByte`

```diff
 namespace System.Security.Cryptography.Cose;
 
 public partial class CoseMessage
 {
+    public ReadOnlyMemory<byte> RawProtectedHeaders { get; }
 }
 
 public partial class CoseSign1Message : CoseMessage
 {
+    public ReadOnlyMemory<byte> Signature { get; }
 }
 
 public partial class CoseSignature
 {
+    public ReadOnlyMemory<byte> RawProtectedHeaders { get; }
+    public ReadOnlyMemory<byte> Signature { get; }
 }
```
## System.Diagnostics.Eventing.Reader.EventBookmark is internal

**Approved** | [#runtime/49866](https://github.com/dotnet/runtime/issues/49866#issuecomment-1195822000) | [Video](https://www.youtube.com/watch?v=bDTLx5SXusY&t=0h57m3s)

* Let's make this type a pure data holder
    - We can seal it because the existing constructor isn't public

```C#
namespace System.Diagnostics.Eventing.Reader;

public sealed class EventBookmark
{
    public EventBookmark(string bookmarkXml);
    public string BookmarkXml { get; }
}
```
## Add API to check if process had administrator privileges

**Approved** | [#runtime/68770](https://github.com/dotnet/runtime/issues/68770#issuecomment-1195832780) | [Video](https://www.youtube.com/watch?v=bDTLx5SXusY&t=1h10m20s)

* Looks good as proposed

```C#
namespace System;

public partial class Environment
{
    public static bool IsPrivilegedProcess { get; }
}
```
## Additional span overloads for asymmetric signing and encryption

**Approved** | [#runtime/68767](https://github.com/dotnet/runtime/issues/68767#issuecomment-1195840122) | [Video](https://www.youtube.com/watch?v=bDTLx5SXusY&t=1h21m1s)

* Looks good as proposed

```C#
namespace System.Security.Cryptography;

public abstract partial class RSA
{
    public byte[] Encrypt(
        ReadOnlySpan<byte> data,
        RSAEncryptionPadding padding);

    public int Encrypt(
        ReadOnlySpan<byte> data,
        Span<byte> destination,
        RSAEncryptionPadding padding);

    public byte[] Decrypt(
        ReadOnlySpan<byte> data,
        RSAEncryptionPadding padding);

    public int Decrypt(
        ReadOnlySpan<byte> data,
        Span<byte> destination,
        RSAEncryptionPadding padding);

    public byte[] SignData(
        ReadOnlySpan<byte> data,
        HashAlgorithmName hashAlgorithm,
        RSASignaturePadding padding);

    public int SignData(
        ReadOnlySpan<byte> data,
        Span<byte> destination,
        HashAlgorithmName hashAlgorithm,
        RSASignaturePadding padding);

    public byte[] SignHash(
        ReadOnlySpan<byte> hash,
        HashAlgorithmName hashAlgorithm,
        RSASignaturePadding padding);

    public int SignHash(
        ReadOnlySpan<byte> hash,
        Span<byte> destination,
        HashAlgorithmName hashAlgorithm,
        RSASignaturePadding padding);
}

public abstract partial class ECDsa
{
    public byte[] SignData(
        ReadOnlySpan<byte> data,
        HashAlgorithmName hashAlgorithm,
        DSASignatureFormat signatureFormat);

    public byte[] SignData(
        ReadOnlySpan<byte> data,
        HashAlgorithmName hashAlgorithm);
        
    public int SignData(
        ReadOnlySpan<byte> data,
        Span<byte> destination,
        HashAlgorithmName hashAlgorithm,
        DSASignatureFormat signatureFormat);

    public int SignData(
        ReadOnlySpan<byte> data,
        Span<byte> destination,
        HashAlgorithmName hashAlgorithm);

    public byte[] SignHash(
        ReadOnlySpan<byte> hash,
        DSASignatureFormat signatureFormat);

    public byte[] SignHash(
        ReadOnlySpan<byte> hash);
        
    public int SignHash(
        ReadOnlySpan<byte> hash,
        Span<byte> destination,
        DSASignatureFormat signatureFormat);

    public int SignHash(
        ReadOnlySpan<byte> hash,
        Span<byte> destination);
}
```
## API proposal for SP800-108-CTR-HMAC cryptographic algorithm

**Approved** | [#runtime/65577](https://github.com/dotnet/runtime/issues/65577#issuecomment-1195854175) | [Video](https://www.youtube.com/watch?v=bDTLx5SXusY&t=1h27m34s)

* Looks good as proposed
    - For OOBing, `System.Security.Cryptography.SP800108` would be reasonable package/assembly name.

```C#
namespace System.Security.Cryptography;

public sealed class SP800108HmacCounterKdf : IDisposable
{
    public SP800108HmacCounterKdf(ReadOnlySpan<byte> key, HashAlgorithmName hashAlgorithm);
    public SP800108HmacCounterKdf(byte[] key, HashAlgorithmName hashAlgorithm);

    public byte[] DeriveKey(byte[] label, byte[] context, int derivedKeyLengthInBytes);
    public byte[] DeriveKey(ReadOnlySpan<byte> label, ReadOnlySpan<byte> context, int derivedKeyLengthInBytes);
    public byte[] DeriveKey(string label, string context, int derivedKeyLengthInBytes);
    public byte[] DeriveKey(ReadOnlySpan<char> label, ReadOnlySpan<char> context, int derivedKeyLengthInBytes);
    public void DeriveKey(ReadOnlySpan<byte> label, ReadOnlySpan<byte> context, Span<byte> destination);
    public void DeriveKey(ReadOnlySpan<char> label, ReadOnlySpan<char> context, Span<byte> destination);

    public static byte[] DeriveBytes(byte[] key, HashAlgorithmName hashAlgorithm, byte[] label, byte[] context, int derivedKeyLengthInBytes);
    public static byte[] DeriveBytes(ReadOnlySpan<byte> key, HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> label, ReadOnlySpan<byte> context, int derivedKeyLengthInBytes);
    public static byte[] DeriveBytes(byte[] key, HashAlgorithmName hashAlgorithm, string label, string context, int derivedKeyLengthInBytes);
    public static byte[] DeriveBytes(ReadOnlySpan<byte> key, HashAlgorithmName hashAlgorithm, ReadOnlySpan<char> label, ReadOnlySpan<char> context, int derivedKeyLengthInBytes);
    public static void DeriveBytes(ReadOnlySpan<byte> key, HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> label, ReadOnlySpan<byte> context, Span<byte> destination);
    public static void DeriveBytes(ReadOnlySpan<byte> key, HashAlgorithmName hashAlgorithm, ReadOnlySpan<char> label, ReadOnlySpan<char> context, Span<byte> destination);

    public void Dispose();
}
```
## InitialCapacity for AsnWriter

**Approved** | [#runtime/69573](https://github.com/dotnet/runtime/issues/69573#issuecomment-1195858346) | [Video](https://www.youtube.com/watch?v=bDTLx5SXusY&t=1h41m41s)

```C#
namespace System.Formats.Asn1;

public partial class AsnWriter
{
    public AsnWriter(AsnEncodingRules ruleSet, int initialBufferSize);
}
```
## treat calls to ROS<char> op_Equality as warning

**Approved** | [#runtime/54794](https://github.com/dotnet/runtime/issues/54794#issuecomment-1195889341) | [Video](https://www.youtube.com/watch?v=bDTLx5SXusY&t=1h46m2s)

* We'd prefer an analyzer that warns on any `T`, thus effectively deprecating `==` and `!=` for spans
    - We should push people to use `SequenceEqual` and a to-be-added `ReferenceEqual` extension method.
* Severity: Warning
* Category: Reliability?

It seems we should have a method:

```C#
namespace System;

public static class MemoryExtensions
{
    public static bool ReferenceEqual<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other)
        where T: IEquatable<T>!;

    public static bool ReferenceEqual<T>(this Span<T> span, ReadOnlySpan<T> other)
        where T: IEquatable<T>!;
}
```

@GrabYourPitchforks please file an issue for the API.
