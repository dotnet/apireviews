# API Review 07/14/2026

## Configurable HTTP connection eviction

**Approved** | [#runtime/130102](https://github.com/dotnet/runtime/issues/130102#issuecomment-4972323029) | [Video](https://www.youtube.com/watch?v=m-lWuE4xdx0&t=0h0m0s)

* It was asked if we should call the Version property `HttpVersion` or `NegotiatedHttpVersion` (to match SocketsHttpPlaintextStreamFilterContext), and no one pushed for the longer name.


```csharp
namespace System.Net.Http;

public partial sealed class SocketsHttpHandler
{
    [Experimental]
    public Func<SocketsHttpConnectionEvictionContext, CancellationToken, Task<bool>>? ShouldEvictConnection { get; set; }
}

[Experimental]
public sealed class SocketsHttpConnectionEvictionContext
{
    internal SocketsHttpConnectionEvictionContext();

    public TimeSpan Age { get; }
    public long ConnectionId { get; }
    public Version HttpVersion { get; }
    public DnsEndPoint DnsEndPoint { get; }
    public IPEndPoint? RemoteEndPoint { get; }
}

public partial sealed class SocketsHttpConnectionContext
{
    [Experimental]
    public long ConnectionId { get; }
}

public partial sealed class SocketsHttpPlaintextStreamFilterContext
{
    [Experimental]
    public long ConnectionId { get; }
}
```
## Enable easier correlation of HTTP requests to the ConnectionId

**Approved** | [#runtime/130108](https://github.com/dotnet/runtime/issues/130108#issuecomment-4972484318) | [Video](https://www.youtube.com/watch?v=m-lWuE4xdx0&t=0h23m15s)

* Looks good as proposed, use the same experimental ID as #130102.

```csharp
namespace System.Net.Http;

public partial class HttpRequestMessage
{
    [Experimental]
    public long? ConnectionId { get; set; }
}
```
## Amend `WritableMemoryStream` and `ReadOnlyMemoryStream` to derive from `Stream`, not `MemoryStream`

**Approved** | [#runtime/130330](https://github.com/dotnet/runtime/issues/130330#issuecomment-4972566095) | [Video](https://www.youtube.com/watch?v=m-lWuE4xdx0&t=0h42m8s)

Looks good as proposed.

```diff
namespace System.IO;

-public sealed partial class WritableMemoryStream : MemoryStream
+public sealed partial class WritableMemoryStream : Stream
{
-   public override int Capacity { get; set; }
-   public override byte[] GetBuffer();
-   public override bool TryGetBuffer(out ArraySegment<byte> buffer);
-   public override byte[] ToArray();
-   public override void WriteTo(Stream stream);
}

-public sealed partial class ReadOnlyMemoryStream : MemoryStream
+public sealed partial class ReadOnlyMemoryStream : Stream
{
-   public override int Capacity { get; set; }
-   public override byte[] GetBuffer();
-   public override bool TryGetBuffer(out ArraySegment<byte> buffer);
-   public override byte[] ToArray();
-   public override void WriteTo(Stream stream);
}
```
## Adjust API surface for stopping parsing after the first invalid character

**Approved** | [#runtime/130537](https://github.com/dotnet/runtime/issues/130537#issuecomment-4972889686) | [Video](https://www.youtube.com/watch?v=m-lWuE4xdx0&t=0h51m47s)

* We don't feel that it's appropriate to take as a general disallowed-breaking rule that we can't introduce new out-arity overloads, but it is important to make TryParse work with match in F#.
* Rather than an IScannable interface, we went with TryParsePartial.
* TryParsePartial obviates the need for NumberStyles.AllowTrailingInvalidCharacters
* Let's go ahead and apply the TryParsePartial pattern to IParsable and friends, via DIMs to TryParse.

```csharp
namespace System
{
    public partial interface IParsable<TSelf>
    {
        // DIM defers to TryParse
        public static virtual bool TryParsePartial(string? s, IFormatProvider? provider, out TSelf result, out int charsConsumed);
    }

    public partial interface ISpanParsable<TSelf>
    {
        // DIM defers to TryParse
        public static virtual bool TryParsePartial(ReadOnlySpan<char> s, IFormatProvider? provider, out TSelf result, out int charsConsumed);
    }

    public interface IUtf8SpanParsable<TSelf>
    {
        // DIM defers to TryParse
        public static virtual bool TryParsePartial(ReadOnlySpan<byte> utf8Text, IFormatProvider? provider, out TSelf result, out int bytesConsumed);
    }
}

namespace System.Numerics
{
    public interface INumberBase<TSelf>
    {
        public static virtual bool TryParsePartial([NotNullWhen(true)] string? s, NumberStyles style, IFormatProvider? provider, [MaybeNullWhen(false)] out TSelf result, out int charsConsumed);

        public static virtual bool TryParsePartial(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, [MaybeNullWhen(false)] out TSelf result, out int charsConsumed);
        
        public static virtual bool TryParsePartial(ReadOnlySpan<byte> s, NumberStyles style, IFormatProvider? provider, [MaybeNullWhen(false)] out TSelf result, out int bytesConsumed);
    }
}
```

```diff
namespace System.Globalization
{
    [Flags]
    public partial enum NumberStyles
    {
-       AllowTrailingInvalidCharacters = 0x800
    }
}

namespace System.Numerics
{
    public interface INumberBase<TSelf>
    {
-       public static virtual bool TryParse([NotNullWhen(true)] string? s, NumberStyles style, IFormatProvider? provider, [MaybeNullWhen(false)] out TSelf result, out int charsConsumed);

-       public static virtual bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, [MaybeNullWhen(false)] out TSelf result, out int charsConsumed);
        
-       public static virtual bool TryParse(ReadOnlySpan<byte> s, NumberStyles style, IFormatProvider? provider, [MaybeNullWhen(false)] out TSelf result, out int bytesConsumed);
    }
}
```
