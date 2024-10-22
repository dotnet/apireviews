# API Review 10/22/2024

## Add `JsonElement.Parse` accelerator methods

**Approved** | [#runtime/108930](https://github.com/dotnet/runtime/issues/108930#issuecomment-2429876608) | [Video](https://www.youtube.com/watch?v=EuWLdNQdueA&t=0h0m0s)

* We added the JsonDocumentOptions parameters for consistency with JsonDocument
* We added a ReadOnlySpan{char} overload for consistency with JsonDocument

```C#
namespace System.Text.Json;

public partial struct JsonElement
{
    public static JsonElement Parse([StringSyntax("json")] string json, JsonDocumentOptions options = default);
    public static JsonElement Parse([StringSyntax("json")] ReadOnlySpan<char> json, JsonDocumentOptions options = default);
    public static JsonElement Parse([StringSyntax("json")] ReadOnlySpan<byte> utf8Json, JsonDocumentOptions options = default);
}
```
## VPCLMULQDQ Intrinsics

**Approved** | [#runtime/95772](https://github.com/dotnet/runtime/issues/95772#issuecomment-2429884156) | [Video](https://www.youtube.com/watch?v=EuWLdNQdueA&t=0h28m43s)

Looks good as proposed

```C#
namespace System.Runtime.Intrinsics.X86;

public abstract class Pclmulqdq : Sse2
{
    public abstract class V256
    {
        public static new bool IsSupported { get; }

        public static Vector256<long> CarrylessMultiply(Vector256<long> left, Vector256<long> right, [ConstantExpected] byte control);
        public static Vector256<ulong> CarrylessMultiply(Vector256<ulong> left, Vector256<ulong> right, [ConstantExpected] byte control);
    }

    public abstract class V512
    {
        public static new bool IsSupported { get; }

        public static Vector512<long> CarrylessMultiply(Vector512<long> left, Vector512<long> right, [ConstantExpected] byte control);
        public static Vector512<ulong> CarrylessMultiply(Vector512<ulong> left, Vector512<ulong> right, [ConstantExpected] byte control);
    }
}
```
## GFNI Intrinsics

**Approved** | [#runtime/96170](https://github.com/dotnet/runtime/issues/96170#issuecomment-2429891660) | [Video](https://www.youtube.com/watch?v=EuWLdNQdueA&t=0h32m57s)

* `[ConstantExpected] byte b` should be `[ConstantExpected] byte control`
* Otherwise, looks good as proposed

```C#
namespace System.Runtime.Intrinsics.X86;

public abstract class Gfni : Sse41
{
    public static bool IsSupported { get; }

    public static Vector128<byte> GaloisFieldAffineTransformInverse(Vector128<byte> x, Vector128<byte> a, [ConstantExpected] byte control);
    public static Vector128<byte> GaloisFieldAffineTransform(Vector128<byte> x, Vector128<byte> a, [ConstantExpected] byte control);
    public static Vector128<byte> GaloisFieldMultiply(Vector128<byte> left, Vector128<byte> right);

    public abstract class X64 : Sse41.X64
    {
        public static bool IsSupported { get; }
    }

    public abstract class V256
    {
        public static new bool IsSupported { get; }

        public static Vector256<byte> GaloisFieldAffineTransformInverse(Vector256<byte> x, Vector256<byte> a, [ConstantExpected] byte control);
        public static Vector256<byte> GaloisFieldAffineTransform(Vector256<byte> x, Vector256<byte> a, [ConstantExpected] byte control);
        public static Vector256<byte> GaloisFieldMultiply(Vector256<byte> left, Vector256<byte> right);
    }

    public abstract class V512
    {
        public static new bool IsSupported { get; }

        public static Vector512<byte> GaloisFieldAffineTransformInverse(Vector512<byte> x, Vector512<byte> a, [ConstantExpected] byte control);
        public static Vector512<byte> GaloisFieldAffineTransform(Vector512<byte> x, Vector512<byte> a, [ConstantExpected] byte control);
        public static Vector512<byte> GaloisFieldMultiply(Vector512<byte> left, Vector512<byte> right);
    }
}
```
## PemEncoding for UTF8

**Approved** | [#runtime/99614](https://github.com/dotnet/runtime/issues/99614#issuecomment-2429910500) | [Video](https://www.youtube.com/watch?v=EuWLdNQdueA&t=0h36m45s)

* `label` in (Try)Write was renamed to `utf8Label` to distinguish the string parameter from the binary data parameter

```C#
namespace System.Security.Cryptography;

public static partial class PemEncoding {
   public static PemFields FindUtf8(ReadOnlySpan<byte> pemData);
   public static bool TryFindUtf8(ReadOnlySpan<byte> pemData, out PemFields fields);

   public static bool TryWriteUtf8(ReadOnlySpan<byte> utf8Label, ReadOnlySpan<byte> data, Span<byte> destination, out int bytesWritten);
   public static byte[] WriteUtf8(ReadOnlySpan<byte> utf8Label, System.ReadOnlySpan<byte> data);
}
```
## X509Certificate2Collection.FindByThumbprint for more digest algorithms

**Approved** | [#runtime/104643](https://github.com/dotnet/runtime/issues/104643#issuecomment-2429936838) | [Video](https://www.youtube.com/watch?v=EuWLdNQdueA&t=0h47m27s)

* We renamed the `thumbprint` parameter that is ReadOnlySpan{byte} to `thumbprintBytes` to further clarify it's not a UTF-8 string.
* Making it an extension method for down-level compatibility came up.  We decided no (at least for now).

```C#
namespace System.Security.Cryptography.X509Certificates;

public partial class X509Certificate2Collection
{
    public X509Certificate2Collection FindByThumbprint(HashAlgorithmName hashAlgorithm, string thumbprintHex);
    public X509Certificate2Collection FindByThumbprint(HashAlgorithmName hashAlgorithm, ReadOnlySpan<char> thumbprintHex);
    public X509Certificate2Collection FindByThumbprint(HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> thumbprintBytes);
}
```
