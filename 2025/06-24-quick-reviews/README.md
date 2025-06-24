# API Review 06/24/2025

## Add APIs to WebSocket which allow it to be read as a Stream

**Approved** | [#runtime/111217](https://github.com/dotnet/runtime/issues/111217#issuecomment-3001280878) | [Video](https://www.youtube.com/watch?v=eFQFAQnyLxc&t=0h0m0s)

* We discussed the naming space (dispose vs close; write vs send) and feel that the proposed is sound.
* We discussed defaulting the WebSocketMessageType parameter.  "Text" is a more appropriate default from a spec perspective, "Binary" is a more sensible default from a flexility perspective... and so "let the user decide" is the best answer.
* Therefore, the updated proposal looks good as proposed.

```C#
namespace System.Net.WebSockets;

public class WebSocketStream : Stream
{
    internal WebSocketStream();

    public WebSocket WebSocket { get; }

    public static WebSocketStream Create(
        WebSocket webSocket,
        WebSocketMessageType writeMessageType,
        bool ownsWebSocket = false); // if ownsWebSocket == true, (default) closeTimeout = 60s

    public static WebSocketStream Create( // implicit ownsWebSocket = true
        WebSocket webSocket,
        WebSocketMessageType writeMessageType,
        TimeSpan closeTimeout); // if closeTimeout == 0, Dispose aborts the WebSocket

    public static WebSocketStream CreateWritableMessageStream(
        WebSocket webSocket,
        WebSocketMessageType writeMessageType);

    public static WebSocketStream CreateReadableMessageStream(WebSocket webSocket);

    ... // relevant Stream overrides
}
```

## Add a constructor for AesCng to accept a CngKey reference

**Approved** | [#runtime/116842](https://github.com/dotnet/runtime/issues/116842#issuecomment-3001291742) | [Video](https://www.youtube.com/watch?v=eFQFAQnyLxc&t=0h13m37s)

* We discussed also adding the constructor for TripleDESCng, and decided to hold off until/unless there's a demonstrated need/user.
* Looks good as proposed.

```C#
namespace System.Cryptography.Aes;

public partial class AesCng
{
    [System.Runtime.Versioning.SupportedOSPlatform("windows")]
    public AesCng(System.Security.Cryptography.CngKey key);
}
```
## Add RFC5649 KeyWrap

**Approved** | [#runtime/108332](https://github.com/dotnet/runtime/issues/108332#issuecomment-3001350465) | [Video](https://www.youtube.com/watch?v=eFQFAQnyLxc&t=0h20m25s)

Looks good as proposed.

```C#
namespace System.Security.Cryptography
{
    public partial class Aes
    {
        public byte[] DecryptKeyWrapPadded(byte[] ciphertext);
        public byte[] DecryptKeyWrapPadded(System.ReadOnlySpan<byte> ciphertext);
        public int DecryptKeyWrapPadded(System.ReadOnlySpan<byte> ciphertext, System.Span<byte> destination);
        protected virtual int DecryptKeyWrapPaddedCore(System.ReadOnlySpan<byte> source, System.Span<byte> destination);
        public bool TryDecryptKeyWrapPadded(System.ReadOnlySpan<byte> ciphertext, System.Span<byte> destination, out int bytesWritten);

        public static int GetKeyWrapPaddedLength(int plaintextLengthInBytes);

        public byte[] EncryptKeyWrapPadded(byte[] plaintext);
        public byte[] EncryptKeyWrapPadded(System.ReadOnlySpan<byte> plaintext);
        public void EncryptKeyWrapPadded(System.ReadOnlySpan<byte> plaintext, System.Span<byte> destination);
        protected virtual void EncryptKeyWrapPaddedCore(System.ReadOnlySpan<byte> source, System.Span<byte> destination);
    }
}
```
## Add ObsoleteAttribute and EditorBrowsable(Never) to DynamicDependency.Condition

**Approved** | [#runtime/107104](https://github.com/dotnet/runtime/pull/107104) | [Video](https://www.youtube.com/watch?v=eFQFAQnyLxc&t=0h37m11s)

## ML-DSA + COSE

**Approved** | [#runtime/116942](https://github.com/dotnet/runtime/issues/116942#issuecomment-3001429042) | [Video](https://www.youtube.com/watch?v=eFQFAQnyLxc&t=0h42m29s)

* CoseKey.FromKey => CoseKey..ctor
* Added the missing overloads taking `, byte[] detachedContent, byte[]? associatedData = default)`

```c#
namespace System.Security.Cryptography.Cose
{
    public sealed class CoseKey
    {
        public CoseKey(ECDsa key, HashAlgorithmName hashAlgorithm);
        public CoseKey(RSA key, RSASignaturePadding signaturePadding, HashAlgorithmName hashAlgorithm);

        [System.Diagnostics.CodeAnalysis.ExperimentalAttribute("SYSLIB5006")]
        public CoseKey(MLDsa key);
    }

    public partial class CoseSign1Message
    {
        public bool VerifyDetached(CoseKey key, Stream detachedContent, ReadOnlySpan<byte> associatedData = default);
        public bool VerifyDetached(CoseKey key, byte[] detachedContent, byte[] associatedData = default);
        public bool VerifyDetached(CoseKey key, System.ReadOnlySpan<byte> detachedContent, ReadOnlySpan<byte> associatedData = default);
        public Task<bool> VerifyDetachedAsync(CoseKey key, Stream detachedContent, ReadOnlyMemory<byte> associatedData = default, CancellationToken cancellationToken = default);
        public bool VerifyEmbedded(CoseKey key, ReadOnlySpan<byte> associatedData = default);
    }

    public partial class CoseSignature
    {
        public bool VerifyDetached(CoseKey key, Stream detachedContent, ReadOnlySpan<byte> associatedData = default);
        public bool VerifyDetached(CoseKey key, byte[] detachedContent, byte[]? associatedData = default);
        public bool VerifyDetached(CoseKey key, ReadOnlySpan<byte> detachedContent, ReadOnlySpan<byte> associatedData = default);
        public Task<bool> VerifyDetachedAsync(CoseKey key, Stream detachedContent, ReadOnlyMemory<byte> associatedData = default, CancellationToken cancellationToken = default);
        public bool VerifyEmbedded(CoseKey key, ReadOnlySpan<byte> associatedData = default);
    }

    public partial class CoseSigner
    {
        public CoseSigner(CoseKey key, CoseHeaderMap? protectedHeaders = null, CoseHeaderMap? unprotectedHeaders = null);
    }
}
```

as well as the nullability change:

```diff
namespace System.Security.Cryptography.Cose
{
    public partial class CoseSigner
    {
-        public System.Security.Cryptography.AsymmetricAlgorithm Key { get; }
+        public System.Security.Cryptography.AsymmetricAlgorithm? Key { get; }
    }
}
```
## Implement `IParsable<TSelf>` et cetera on `IPEndPoint`

**Approved** | [#runtime/114405](https://github.com/dotnet/runtime/issues/114405#issuecomment-3001438579) | [Video](https://www.youtube.com/watch?v=eFQFAQnyLxc&t=1h1m13s)

* Looks consistent with how ISpanFormattable (et al) are implemented when the `format` parameter has no impact.
* Looks good as proposed

```diff
namespace System.Net;

public partial class IPEndPoint : EndPoint
+   , ISpanFormattable, ISpanParsable<IPEndPoint>, IUtf8SpanFormattable, IUtf8SpanParsable<IPEndPoint>
{
    public static IPEndPoint Parse(ReadOnlySpan<char> s);
    public static IPEndPoint Parse(string s);
    public static bool TryParse(ReadOnlySpan<char> s, [NotNullWhen(true)] out IPEndPoint? result);
    public static bool TryParse(string s, [NotNullWhen(true)] out IPEndPoint? result);

+   public static IPEndPoint Parse(ReadOnlySpan<byte> utf8Text);
+   static IPEndPoint ISpanParsable<IPEndPoint>.Parse(ReadOnlySpan<char> s, IFormatProvider? provider);
+   static IPEndPoint IParsable<IPEndPoint>.Parse(string s, IFormatProvider? provider);
+   static IPEndPoint IUtf8SpanParsable<IPEndPoint>.Parse(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider);

+   public static bool TryParse(ReadOnlySpan<byte> utf8Text, [NotNullWhen(true)] out IPEndPoint? result);
+   static bool ISpanParsable<IPEndPoint>.TryParse(ReadOnlySpan<char> s, IFormatProvider? provider, [NotNullWhen(true)] out IPEndPoint? result);
+   static bool IParsable<IPEndPoint>.TryParse([NotNullWhen(true)] string? s, IFormatProvider? provider, [NotNullWhen(true)] out IPEndPoint? result);
+   static bool IUtf8SpanParsable<IPEndPoint>.TryParse(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider, [NotNullWhen(true)] out IPEndPoint? result);

+   string IFormattable.ToString(string? format, IFormatProvider? formatProvider);
+   public bool TryFormat(Span<char> destination, out int charsWritten);
+   public bool TryFormat(Span<byte> utf8Destination, out int bytesWritten);
+   bool ISpanFormattable.TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format, IFormatProvider? provider);
+   bool IUtf8SpanFormattable.TryFormat(Span<byte> utf8Destination, out int bytesWritten, ReadOnlySpan<char> format, IFormatProvider? provider);
}
```

## FakeTimeProvider, option to allow going backwards

**Approved** | [#extensions/5072](https://github.com/dotnet/extensions/issues/5072#issuecomment-3001490945) | [Video](https://www.youtube.com/watch?v=eFQFAQnyLxc&t=1h4m56s)

* We feel the new property is unnecessary.  Rather, SetTimeUtc should just cease throwing the exception when time goes backwards.
  * When the new time is older, SetTimeUtc should defer to AdjustTime.
* Given no proposals exist to modify the existing AdjustTime method, we feel that it should cease being experimental (and, if there's disagreement, then the owners should come back to us).


```diff
namespace Microsoft.Extensions.Time.Testing;

public partial class FakeTimeProvider
{
-   [Experimental(diagnosticId: DiagnosticIds.Experiments.TimeProvider, UrlFormat = DiagnosticIds.UrlFormat)]
    public void AdjustTime(DateTimeOffset value)
}
```

## Arm64 SVE: Add LoadAndReplicateToVector for SVE

**Approved** | [#runtime/116006](https://github.com/dotnet/runtime/issues/116006#issuecomment-3001501573) | [Video](https://www.youtube.com/watch?v=eFQFAQnyLxc&t=1h25m4s)

Looks good as proposed

```c#
namespace System.Runtime.Intrinsics.Arm;

public partial class Sve
{
    // ld1rb ld1rd ld1rh ld1rsb ld1rsh ld1rsw ld1rw
    /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
    public static unsafe Vector<T> LoadAndReplicateToVector(T* address, byte control);
}
```
## EqualityComparer.Create from selecting a key property

**Approved** | [#runtime/115797](https://github.com/dotnet/runtime/issues/115797#issuecomment-3001535862) | [Video](https://www.youtube.com/watch?v=eFQFAQnyLxc&t=1h29m35s)

* For symmetry, if it's being done for EqualityComparer, it seems like it should be done for Comparer, too.
* We're not entirely sure that these convenience methods are "useful enough", but if the area owners feel they are warranted, go for it.

```c#
namespace System.Collections.Generic;

public partial class EqualityComparer<T>
{
    public static EqualityComparer<T> Create<TKey>(
        Func<T?, TKey?> keySelector,
        IEqualityComparer<TKey>? keyComparer = null);
}

public partial class Comparer<T>
{
    public static Comparer<T> Create<TKey>(
        Func<T?, TKey?> keySelector,
        IComparer<TKey>? keyComparer = null);
}
```
## Add ConsumedSequence property to SequenceReader<T>

**Rejected** | [#runtime/56273](https://github.com/dotnet/runtime/issues/56273#issuecomment-3001549664) | [Video](https://www.youtube.com/watch?v=eFQFAQnyLxc&t=1h44m12s)

The proposed usages suffer from an assumption that the reader started the method at position 0.  Any real use of ConsumedSequence that we can think of in the general case would be about a slice thereof, at which point the property isn't really providing inherent value.

Without a stronger use case, this shouldn't be added, as it's more confusing than valueable.

```C#
namespace System.Buffers
{
     public ref struct SequenceReader<T> where T : unmanaged, IEquatable<T>
     {
          public readonly ReadOnlySequence<T> ConsumedSequence { get; }
     }
}
```
