# API Review 08/16/2022

## Provide a public api to "lock" a `JsonSerializerOptions`

**Approved** | [#runtime/54482](https://github.com/dotnet/runtime/issues/54482#issuecomment-1216933469) | [Video](https://www.youtube.com/watch?v=gqtLLD_PL3g&t=0h0m0s)

* We'd prefer the read-only terminology because we don't really have the notion of mutable types that can become immutable, we do, however, have the notion of types becoming read only.

```C#
namespace System.Text.Json;

public partial class JsonSerializerOptions 
{
    public bool IsReadOnly { get; }
    public void MakeReadOnly();
}
```
## Add GetDeclaringType to PropertyDefinition and EventDefinition

**Approved** | [#runtime/60380](https://github.com/dotnet/runtime/issues/60380#issuecomment-1216938426) | [Video](https://www.youtube.com/watch?v=gqtLLD_PL3g&t=0h17m39s)

* Looks good as proposed

```C#
namespace System.Reflection.Metadata;

public partial struct PropertyDefinition
{
    public TypeDefinitionHandle GetDeclaringType();
}

public partial struct EventDefinition
{
    public TypeDefinitionHandle GetDeclaringType();
}
```
## Add ReadOnlySequence overload for WriteRawValue

**Approved** | [#runtime/68223](https://github.com/dotnet/runtime/issues/68223#issuecomment-1216955662) | [Video](https://www.youtube.com/watch?v=gqtLLD_PL3g&t=0h22m23s)

* We don't want to add the overload `ReadOnlySequence<char>` because we'd have to deal with transcoding surrogate pairs across elements in the sequence, which is quite a bit of work that nobody seems to be asking for right now.

```C#
namespace System.Text.Json;

public sealed partial class Utf8JsonWriter
{
    public void WriteRawValue(ReadOnlySequence<byte> json, bool skipInputValidation = false);
}
```
## Add constructor with string for SocketException

**Approved** | [#runtime/69266](https://github.com/dotnet/runtime/issues/69266#issuecomment-1216959607) | [Video](https://www.youtube.com/watch?v=gqtLLD_PL3g&t=0h39m34s)

* Looks good as proposed.

```C#
namespace System.Net.Sockets;

public partial class SocketException : Win32Exception
{
    // Existing
    // public SocketException();
    // public SocketException(int errorCode);
    public SocketException(int errorCode, string? message);
}
```
## Add a JsonEncodedText.Value property

**Approved** | [#runtime/69716](https://github.com/dotnet/runtime/issues/69716#issuecomment-1216965749) | [Video](https://www.youtube.com/watch?v=gqtLLD_PL3g&t=0h43m37s)

* Assuming `Value` would either expose the string passed to `Encode` or produce a string when given a span, this looks fine
    - The sketched implementation seems to suggest that for the span case the return value might be the empty string, which seems problematic.
* We should document that the string passed to `Encode` might be a different one from the one returned by `Value`
    - This happens to be the current implementation ;-)

```C#
namespace System.Text.Json;

public partial struct JsonEncodedText
{
    public string Value { get; }
}
```
## Consider obsoleting `RSA.EncryptValue`, `RSA.DecryptValue`

**Approved** | [#runtime/73969](https://github.com/dotnet/runtime/issues/73969#issuecomment-1216982922) | [Video](https://www.youtube.com/watch?v=gqtLLD_PL3g&t=0h49m39s)

* Since the method (and all derivatives) always throw, we should also hide it.

```C#
namespace System.Security.Cryptography;

public partial class RSA : AsymmetricAlgorithm
{
    [Obsolete("RSA.EncryptValue and DecryptValue are not supported and throw NotSupportedException. Use RSA.Encrypt and RSA.Decrypt instead.")]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public virtual byte[] EncryptValue(byte[] rgb);
    
    [Obsolete("RSA.EncryptValue and DecryptValue are not supported and throw NotSupportedException. Use RSA.Encrypt and RSA.Decrypt instead.")]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public virtual byte[] DecryptValue(byte[] rgb);
}

public partial class RSACryptoServiceProvider : RSA
{
    [Obsolete("RSA.EncryptValue and DecryptValue are not supported and throw NotSupportedException. Use RSA.Encrypt and RSA.Decrypt instead.")]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public override byte[] EncryptValue(byte[] rgb);
    
    [Obsolete("RSA.EncryptValue and DecryptValue are not supported and throw NotSupportedException. Use RSA.Encrypt and RSA.Decrypt instead.")]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public override byte[] DecryptValue(byte[] rgb);
}
```
## add SslStreamCertificateContext to SslClientAuthenticationOptions

**Approved** | [#runtime/71194](https://github.com/dotnet/runtime/issues/71194#issuecomment-1216976046) | [Video](https://www.youtube.com/watch?v=gqtLLD_PL3g&t=1h6m18s)

(if the answer is no, we can just approve as proposed)
## Obsolete ServicePointManager

**NeedsWork** | [#runtime/71836](https://github.com/dotnet/runtime/issues/71836#issuecomment-1216998960) | [Video](https://www.youtube.com/watch?v=gqtLLD_PL3g&t=1h10m33s)

* Marking the type seems fine, assuming we believe the three remaining members listed are also obsolete. If we don't believe that, then we should follow the original proposal and only obsolete them proposed members.

```C#
namespace System.Net;

public partial class ServicePointManager
{
    // Remaining members:
    // public static bool CheckCertificateRevocationList { get; set; }
    // public static SecurityProtocolType SecurityProtocol { get; set; }
    // public static RemoteCertificateValidationCallback? ServerCertificateValidationCallback { get; set; }

    [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    public const int DefaultNonPersistentConnectionLimit = 4;
    [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    public const int DefaultPersistentConnectionLimit = 2;
    [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    public static int DefaultConnectionLimit { get; set; }
    [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    public static int DnsRefreshTimeout { get; set; }
    [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    public static bool EnableDnsRoundRobin { get; set; }
    [UnsupportedOSPlatform("browser")]
    [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    public static EncryptionPolicy EncryptionPolicy { get; }
    [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    public static bool Expect100Continue { get; set; }
    [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    public static int MaxServicePointIdleTime { get; set; }
    [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    public static int MaxServicePoints { get; set; }
    [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    public static bool ReusePort { get; set; }
    [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    public static bool UseNagleAlgorithm { get; set; }

    // Already obsolete:
    // [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    // public static ServicePoint FindServicePoint(string uriString, IWebProxy? proxy);
    // [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    // public static ServicePoint FindServicePoint(Uri address);
    // [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    // public static ServicePoint FindServicePoint(Uri address, IWebProxy? proxy);
    // [Obsolete(DiagnosticId = Obsoletions.WebRequestDiagId)]
    // public static void SetTcpKeepAlive(bool enabled, int keepAliveTime, int keepAliveInterval);
}
```
## Add new overloads to ReadFromJsonAsync method in HttpContentJsonExtensions

**NeedsWork** | [#runtime/72103](https://github.com/dotnet/runtime/issues/72103#issuecomment-1217013667) | [Video](https://www.youtube.com/watch?v=gqtLLD_PL3g&t=1h23m12s)

The intended usage

```C#
return await message.Content.ReadAsJsonAsync(cancellationToken);
```

almost works today, by using named arguments:

```C#
return await message.Content.ReadAsJsonAsync(cancellationToken: cancellationToken);
```

Which would bind to this method:

```C#
namespace System.Net.Http.Json;

public static class HttpContentJsonExtensions
{
    [RequiresDynamicCode("...")]
    [RequiresUnreferencedCode("...")]
    public static Task<T> ReadFromJsonAsync<T>(this HttpContent content,
                                               JsonSerializerOptions? options = null,
                                               CancellationToken cancellationToken = default);
}
```

We're not opposed to adding this overload but then we should probably add corresponding ones for the methods on `HttpClientJsonExtensions`.
## `ImmutableArray<T>.Builder.ToImmutableAndClear()`

**NeedsWork** | [#runtime/72625](https://github.com/dotnet/runtime/issues/72625#issuecomment-1217054406) | [Video](https://www.youtube.com/watch?v=gqtLLD_PL3g&t=1h38m45s)

The challenge is that `MoveToImmutable()` results in `Capacity` becoming `0`, while `ToImmutable(); Clear();` would preserve the capacity. This can result in cases where we need go through various resize cycles, making the `Capacity == Count` potentially more expensive.

```C#
namespace System.Collections.Immutable;

public partial struct ImmutableArray<T>
{
    public partial class Builder
    {
        public ImmutableArray<T> ToImmutableAndClear();
    }
}
```
