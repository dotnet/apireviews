# API Review 05/09/2023

## Add invoker classes for fast invoke APIs

**Approved** | [#runtime/85539](https://github.com/dotnet/runtime/issues/85539#issuecomment-1540590887) | [Video](https://www.youtube.com/watch?v=06SaaTRZyAs&t=0h0m0s)

* There was a question of why the Invoke methods took a Span instead of ReadOnlySpan.  The answer was for ref/out semantics.
* The base class is an implementation detail that shouldn't leak into the ref.  It should be removed.
* For MethodInvoker.Invoke, we checked that `obj` is the consistent name for the target/instance parameter.
* We discussed the number of overloads/number of argN options, and felt that 3 or 4 are both OK (but that we really want `params Span`)
* If there are places in reflection that use arg0..argN instead of arg1..argN, we should change the indexing here to match.
* `Create` vs `.ctor` came up, and `Create` seems justified (e.g. future caching opportunities)

```C#
namespace System.Reflection
{
    public sealed class ConstructorInvoker
    {
        public static ConstructorInvoker Create(ConstructorInfo constructor);

        private ConstructorInvoker();

        public object? Invoke();
        public object? Invoke(object? arg1);
        public object? Invoke(object? arg1, object? arg2);
        public object? Invoke(object? arg1, object? arg2, object? arg3);
        public object? Invoke(object? arg1, object? arg2, object? arg3, object? arg4);
        public object? Invoke(Span<object?> arguments);
    }

    public sealed class MethodInvoker
    {
        public static MethodInvoker Create(MethodInfo method);

        private MethodInvoker();

        public object? Invoke(object? obj);
        public object? Invoke(object? obj, object? arg1);
        public object? Invoke(object? obj, object? arg1, object? arg2);
        public object? Invoke(object? obj, object? arg1, object? arg2, object? arg3);
        public object? Invoke(object? obj, object? arg1, object? arg2, object? arg3, object? arg4);
        public object? Invoke(object? obj, Span<object?> arguments);
    }
}
```
## Define parameter marshalling of interfaces

**Approved** | [#runtime/85797](https://github.com/dotnet/runtime/issues/85797#issuecomment-1540608109) | [Video](https://www.youtube.com/watch?v=06SaaTRZyAs&t=0h31m20s)

* We removed `ComInterfaceMarshaller<T, U>` in lieu of answering whether or not the generic arity overloading was obvious for the scenario.
* We raised the question of whether or not `UniqueComInterfaceMarshaller` needed "Instance" in the name, and decided it was OK as-is because of a low number of users (who are expected to be domain semi-experts) and that the name was already long enough.

```diff
namespace System.Runtime.InteropServices.Marshalling;

- [AttributeUsage(AttributeTargets.Struct | AttributeTargets.Class | AttributeTargets.Enum | AttributeTargets.Delegate)]
+ [AttributeUsage(AttributeTargets.Struct | AttributeTargets.Class | AttributeTargets.Enum | AttributeTargets.Interface | AttributeTargets.Delegate)]
public sealed class NativeMarshallingAttribute : Attribute
{
}
```

```C#
namespace System.Runtime.InteropServices.Marshalling;

/// <summary>
/// COM interface marshaller using a StrategyBasedComWrappers instance
/// </summary>
/// <remarks>
/// This marshaller will always pass the <see cref="CreateObjectFlags.Unwrap"/> flag
/// to <see cref="ComWrappers.GetOrCreateObjectForComInstance(IntPtr, CreateObjectFlags)"/>.
/// </remarks>
[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder), MarshalMode.Default, typeof(ComInterfaceMarshaller<>))]
public static class ComInterfaceMarshaller<T>
{
    public static void* ConvertToUnmanaged(T managed);
    public static T ConvertToManaged(void* unmanaged);
}

/// <summary>
/// COM interface marshaller using a StrategyBasedComWrappers instance
/// that will only create unique native object wrappers (RCW).
/// </summary>
/// <remarks>
/// This marshaller will always pass the <see cref="CreateObjectFlags.Unwrap"/> and <see cref="CreateObjectFlags.UniqueInstance"/> flags
/// to <see cref="ComWrappers.GetOrCreateObjectForComInstance(IntPtr, CreateObjectFlags)"/>.
/// </remarks>
[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder), MarshalMode.Default, typeof(UniqueComInterfaceMarshaller<>))]
public static class UniqueComInterfaceMarshaller<T>
{
    public static void* ConvertToUnmanaged(T managed);
    public static T ConvertToManaged(void* unmanaged);
}
```
## System.CommandLine:  Validation

**NeedsWork** | [#runtime/84178](https://github.com/dotnet/runtime/issues/84178) | [Video](https://www.youtube.com/watch?v=06SaaTRZyAs&t=0h44m30s)

## GC.RefreshMemoryLimit

**Approved** | [#runtime/70601](https://github.com/dotnet/runtime/issues/70601#issuecomment-1540637779) | [Video](https://www.youtube.com/watch?v=06SaaTRZyAs&t=1h0m59s)

* We generally believe that the API should be returning an enum, instead of having the docs describing integer returns.  But, given that there's a proposal that it should actually be using HRESULT on the basis that it's expected to be called by native hosts (and HRESULT is the host interface response, even on non-Windows), `int` seems OK.

```C#
namespace System;
public static partial class GC
{
    [System.Runtime.Versioning.RequiresPreviewFeaturesAttribute("RefreshMemoryLimit is in preview.")]
    public static int RefreshMemoryLimit()
}
```
## ArgumentException helper methods

**Approved** | [#runtime/77749](https://github.com/dotnet/runtime/issues/77749#issuecomment-1540658549) | [Video](https://www.youtube.com/watch?v=06SaaTRZyAs&t=1h4m32s)

* We discussed having the return type be `string` vs `void`.  We felt that this should be the same as the rest, so `void`, but the question of changing them all (except the ones that already shipped) to returning the argument can be raised at a later date.

```C#
public partial class ArgumentException
{
   public static void ThrowIfNullOrWhiteSpace([NotNull] string? argument, [CallerArgumentExpression(nameof(argument))] string? paramName = null);
}
## Add support for SHA3 (Keccak)

**Approved** | [#runtime/20342](https://github.com/dotnet/runtime/issues/20342#issuecomment-1540668616) | [Video](https://www.youtube.com/watch?v=06SaaTRZyAs&t=1h15m5s)

We discussed removing the attributes and treating it similar to how we do the intrinsics... if there's an IsSupported it's up to you to know to check it (or to not check it because you have no fallback).

We should similarly remove the OS-support attributes from Ed25519 types.

```C#
namespace System.Security.Cryptography;

public partial struct HashAlgorithmName {
    public static HashAlgorithmName SHA3_256 { get; }
    public static HashAlgorithmName SHA3_384 { get; }
    public static HashAlgorithmName SHA3_512 { get; }
}

public sealed partial class RSAEncryptionPadding {
    public static RSAEncryptionPadding OaepSHA3_256 { get; }
    public static RSAEncryptionPadding OaepSHA3_384 { get; }
    public static RSAEncryptionPadding OaepSHA3_512 { get; }
}

public abstract partial class SHA3_256 : HashAlgorithm {
    public const int HashSizeInBits = 256;
    public const int HashSizeInBytes = 32;

    protected SHA3_256();

    public static bool IsSupported { get; }

    public static new SHA3_256 Create();

    public static byte[] HashData(byte[] source);
    public static byte[] HashData(ReadOnlySpan<byte> source);
    public static int HashData(ReadOnlySpan<byte> source, Span<byte> destination);
    public static bool TryHashData(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);

    public static byte[] HashData(Stream source);
    public static int HashData(Stream source, Span<byte> destination);
    public static ValueTask<byte[]> HashDataAsync(Stream source, CancellationToken cancellationToken = default);
    public static ValueTask<int> HashDataAsync(
        Stream source,
        Memory<byte> destination,
        CancellationToken cancellationToken = default);
}

public abstract partial class SHA3_384 : HashAlgorithm {
    public const int HashSizeInBits = 384;
    public const int HashSizeInBytes = 48;

    protected SHA3_384();

    public static bool IsSupported { get; }

    public static new SHA3_384 Create();

    public static byte[] HashData(byte[] source);
    public static byte[] HashData(ReadOnlySpan<byte> source);
    public static int HashData(ReadOnlySpan<byte> source, Span<byte> destination);
    public static bool TryHashData(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);

    public static byte[] HashData(Stream source);
    public static int HashData(Stream source, Span<byte> destination);
    public static ValueTask<byte[]> HashDataAsync(Stream source, CancellationToken cancellationToken = default);
    public static ValueTask<int> HashDataAsync(
        Stream source,
        Memory<byte> destination,
        CancellationToken cancellationToken = default);
}

public abstract partial class SHA3_512 : HashAlgorithm {
    public const int HashSizeInBits = 512;
    public const int HashSizeInBytes = 64;

    protected SHA3_512();

    public static bool IsSupported { get; }

    public static new SHA3_512 Create();

    public static byte[] HashData(byte[] source);
    public static byte[] HashData(ReadOnlySpan<byte> source);
    public static int HashData(ReadOnlySpan<byte> source, Span<byte> destination);
    public static bool TryHashData(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);

    public static byte[] HashData(Stream source);
    public static int HashData(Stream source, Span<byte> destination);
    public static ValueTask<byte[]> HashDataAsync(Stream source, CancellationToken cancellationToken = default);
    public static ValueTask<int> HashDataAsync(
        Stream source,
        Memory<byte> destination,
        CancellationToken cancellationToken = default);
}

public partial class HMACSHA3_256 : HMAC {
    public const int HashSizeInBits = 256;
    public const int HashSizeInBytes = 32;

    public HMACSHA3_256();
    public HMACSHA3_256(byte[] key);

    public static bool IsSupported { get; } // New

    public static byte[] HashData(byte[] key, byte[] source);
    public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source);
    public static int HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination);

    public static bool TryHashData(
        ReadOnlySpan<byte> key,
        ReadOnlySpan<byte> source,
        Span<byte> destination,
        out int bytesWritten);

    public static byte[] HashData(byte[] key, Stream source);
    public static byte[] HashData(ReadOnlySpan<byte> key, Stream source);
    public static int HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination);
    public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, CancellationToken cancellationToken = default);
    public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, CancellationToken cancellationToken = default);
    public static ValueTask<int> HashDataAsync(
        ReadOnlyMemory<byte> key,
        Stream source,
        Memory<byte> destination,
        CancellationToken cancellationToken = default);
}

public partial class HMACSHA3_384 : HMAC {
    public const int HashSizeInBits = 384;
    public const int HashSizeInBytes = 48;

    public HMACSHA3_384();
    public HMACSHA3_384(byte[] key);

    public static bool IsSupported { get; } // New

    public static byte[] HashData(byte[] key, byte[] source);
    public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source);
    public static int HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination);
    public static bool TryHashData(
        ReadOnlySpan<byte> key,
        ReadOnlySpan<byte> source,
        Span<byte> destination,
        out int bytesWritten);

    public static byte[] HashData(byte[] key, Stream source);
    public static byte[] HashData(ReadOnlySpan<byte> key, Stream source);
    public static int HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination);
    public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, CancellationToken cancellationToken = default);
    public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, CancellationToken cancellationToken = default);
    public static ValueTask<int> HashDataAsync(
        ReadOnlyMemory<byte> key,
        Stream source,
        Memory<byte> destination,
        CancellationToken cancellationToken = default);
}

public partial class HMACSHA3_512 : HMAC {
    public const int HashSizeInBits = 512;
    public const int HashSizeInBytes = 64;

    public HMACSHA3_512();
    public HMACSHA3_512(byte[] key);

    public static bool IsSupported { get; } // New

    public static byte[] HashData(byte[] key, byte[] source);
    public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source);
    public static int HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination);
    public static bool TryHashData(
        ReadOnlySpan<byte> key,
        ReadOnlySpan<byte> source,
        Span<byte> destination,
        out int bytesWritten);

    public static byte[] HashData(byte[] key, Stream source);
    public static byte[] HashData(ReadOnlySpan<byte> key, Stream source);
    public static int HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination);
    public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, CancellationToken cancellationToken = default);
    public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, CancellationToken cancellationToken = default);
    public static ValueTask<int> HashDataAsync(
        ReadOnlyMemory<byte> key,
        Stream source,
        Memory<byte> destination,
        CancellationToken cancellationToken = default);
}

public sealed partial class Shake128 : IDisposable {

    public Shake128();

    public static bool IsSupported { get; }

    public void AppendData(byte[] data);
    public void AppendData(ReadOnlySpan<byte> data);
    public byte[] GetCurrentHash(int outputLength);
    public void GetCurrentHash(Span<byte> destination);
    public byte[] GetHashAndReset(int outputLength);
    public void GetHashAndReset(Span<byte> destination);
    public void Dispose();

    public static byte[] HashData(byte[] source, int outputLength);
    public static byte[] HashData(ReadOnlySpan<byte> source, int outputLength);
    public static void HashData(ReadOnlySpan<byte> source, Span<byte> destination);

    public static byte[] HashData(Stream source, int outputLength);
    public static void HashData(Stream source, Span<byte> destination);
    public static ValueTask HashDataAsync(Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
    public static ValueTask<byte[]> HashDataAsync(Stream source, int outputLength, CancellationToken cancellationToken = default);
}

public sealed partial class Shake256 : IDisposable {

    public Shake256();

    public static bool IsSupported { get; }

    public void AppendData(byte[] data);
    public void AppendData(ReadOnlySpan<byte> data);
    public byte[] GetCurrentHash(int outputLength);
    public void GetCurrentHash(Span<byte> destination);
    public byte[] GetHashAndReset(int outputLength);
    public void GetHashAndReset(Span<byte> destination);
    public void Dispose();

    public static byte[] HashData(byte[] source, int outputLength);
    public static byte[] HashData(ReadOnlySpan<byte> source, int outputLength);
    public static void HashData(ReadOnlySpan<byte> source, Span<byte> destination);

    public static byte[] HashData(Stream source, int outputLength);
    public static void HashData(Stream source, Span<byte> destination);
    public static ValueTask HashDataAsync(Stream source, Memory<byte> destination, CancellationToken cancellationToken = default);
    public static ValueTask<byte[]> HashDataAsync(Stream source, int outputLength, CancellationToken cancellationToken = default);
}
```
## System.Security.Cryptography.AesGcm - decryption allows truncated tags

**Approved** | [#runtime/71366](https://github.com/dotnet/runtime/issues/71366#issuecomment-1540687119) | [Video](https://www.youtube.com/watch?v=06SaaTRZyAs&t=1h21m44s)

Looks good as proposed.

```diff
namespace System.Security.Cryptography;

public partial class AesGcm {

+  public AesGcm(byte[] key, int tagSizeInBytes);
+  public AesGcm(ReadOnlySpan<byte> key, int tagSizeInBytes);

+  [Obsolete("AesGcm should indicate the required tag size for encryption and decryption. Use a constructor that accepts the tag size.", DiagnosticId = "SYSLIB9999")]
   public AesGcm(byte[] key);
+  [Obsolete("AesGcm should indicate the required tag size for encryption and decryption. Use a constructor that accepts the tag size.", DiagnosticId = "SYSLIB9999")]
   public AesGcm(ReadOnlySpan<byte> key);

+  public int? TagSizeInBytes { get; }
}
## add more entries to MediaTypeNames

**Approved** | [#runtime/85807](https://github.com/dotnet/runtime/issues/85807#issuecomment-1540692065) | [Video](https://www.youtube.com/watch?v=06SaaTRZyAs&t=1h31m13s)

Looks good as proposed

```diff
namespace System.Net.Mime;

    public static class MediaTypeNames
    {
        public static class Text
        {
            public const string Plain = "text/plain";
            public const string Html = "text/html";
            public const string Xml = "text/xml";
            public const string RichText = "text/richtext";
+           public const string Css = "text/css";                 // RFC2318
+           public const string Csv = "text/csv";                 // RFC4180
+           public const string JavaScript = "text/javascript";   // RFC9239
+           public const string Markdown = "text/markdown";       // RFC7763
+           public const string Rtf = "text/rtf";
        }

        public static class Application
        {
            public const string Soap = "application/soap+xml";
            public const string Octet = "application/octet-stream";
            public const string Rtf = "application/rtf";
            public const string Pdf = "application/pdf";
            public const string Zip = "application/zip";
            public const string Json = "application/json";
            public const string Xml = "application/xml";
+           public const string FormUrlEncoded = "application/x-www-form-urlencoded"; 
+           public const string JsonPatch = "application/json-patch+json"; // RFC6902
+           public const string JsonSequence = "application/json-seq";     // RFC7464
+           public const string Manifest = "application/manifest+json";    // W3C
+           public const string ProblemJson = "application/problem+json";  // RFC7807
+           public const string ProblemXml = "application/problem+xml";    // RFC7807
+           public const string Wasm = "application/wasm";                 // W3C
+           public const string XmlDtd = "application/xml-dtd";            // RFC7303
+           public const string XmlPatch = "application/xml-patch+xml";    // RFC735
        }

        public static class Image
        {
            public const string Gif = "image/gif";
            public const string Tiff = "image/tiff";
            public const string Jpeg = "image/jpeg";
+           public const string Avif = "image/avif";
+           public const string Bmp = "image/bmp";     // RFC7903
+           public const string Icon = "image/x-icon"; 
+           public const string Png = "image/png";     // W3C
+           public const string Svg = "image/svg+xml"; // W3C
+           public const string Webp = "image/webp";
        }

+        public static class Multipart
+        {
+           public const string ByteRanges = "multipart/byteranges";  // RFC9110
+           public const string FormData = "multipart/form-data";     // RFC7578
+        }

+        public static class Font
+        {
+           public const string Collection = "font/collection"; // RFC8081
+           public const string Otf = "font/otf";               // RFC8081
+           public const string Sfnt = "font/sfnt";             // RFC8081
+           public const string Ttf = "font/ttf";               // RFC8081
+           public const string Woff = "font/woff";             // RFC8081
+           public const string Woff2 = "font/woff2";           // RFC8081
+        } 
    }
```
## Zero-overhead member access with suppressed visibility checks

**Approved** | [#runtime/81741](https://github.com/dotnet/runtime/issues/81741#issuecomment-1540725660) | [Video](https://www.youtube.com/watch?v=06SaaTRZyAs&t=1h34m15s)

Generally looks good as proposed, but we sealed the attribute.

```C#
namespace System.Runtime.CompilerServices;

[AttributeUsage(AttributeTargets.Method, AllowMultiple = false, Inherited = false)]
public sealed class UnsafeAccessorAttribute : Attribute
{
    public UnsafeAccessorAttribute(UnsafeAccessorKind kind);

    public UnsafeAccessorKind Kind { get; }

    // The name defaults to the annotated method name if not specified.
    // The name must be null for constructors
    public string? Name { get; set; }
}

public enum UnsafeAccessorKind
{
    Constructor, // call instance constructor (`newobj` in IL)
    Method, // call instance method (`callvirt` in IL)
    StaticMethod, // call static method (`call` in IL)
    Field, // address of instance field (`ldflda` in IL)
    StaticField // address of static field (`ldsflda` in IL)

    // Potential additions to handle niche cases
    // FieldGet, // get value of instance field (`ldfld` in IL). Required for `ref` fields.  
    // FieldSet, // get value of instance field (`stfld` in IL). Required for `ref` fields.  
    // NonVirtualMethod, // call instance method non-virtually (`call` in IL).
};
```
