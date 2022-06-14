# API Review 06/14/2022

## [QUIC] QuicProvider

**NeedsWork** | [#runtime/70424](https://github.com/dotnet/runtime/issues/70424#issuecomment-1155501385) | [Video](https://www.youtube.com/watch?v=eL_tQFSs54Q&t=0h0m0s)

* Consider removing `QuicProvider` and instead have the factory methods on `QuicListener` and `QuicConnection`.
    - This also allows modelling platforms that only support just one side of QUIC

```C#
namespace System.Net.Quic;

public static class QuicProvider
{
    public static bool IsSupported { get; }
    public static ValueTask<QuicListener> CreateListenerAsync(QuicListenerOptions options, CancellationToken cancellationToken = default);
    public static ValueTask<QuicConnection> CreateConnectionAsync(QuicClientConnectionOptions options, CancellationToken cancellationToken = default);
}
```
## [QUIC] QuicListener

**Approved** | [#runtime/67560](https://github.com/dotnet/runtime/issues/67560#issuecomment-1155597405) | [Video](https://www.youtube.com/watch?v=eL_tQFSs54Q&t=0h30m26s)

* `QuicListenerOptions.ListenBacklog`. We shouldn't introduce an opinion on the buffer boundary, e.g. `512`
    - We shouldn't have unbounded buffers/queues on our end (due to OOMs) but equally we shouldn't add have a fixed opinion on the upper bound across all platforms
    - Ideally, we'd just map to the underlying implementation (e.g. MSQUIC) but that's difficult as MSQUIC for example doesn't throttle based on number of connection but on CPU load.
* `QuicListenerOptions.ListenEndPoint`
    - We should align this with `TcpListener` and `Socket` and call it `LocalEndPoint` 
* `QuicListenerOptions`
    - Consider having a constructor that forces the required properties
    - Also sync with @333fred to see whether we can use C# 11's `required` keyword; not sure how this works for collections
    - Also, if you go with properties and default constructor, think about nullable annotations
* We should seal the types
    - This means we don't have to implement the `Dispose` pattern
    - Also, we'd get away with not implementing `IDisposable`
* `ServerOptionsSelectionCallback`
    - Could just be a func, given that the types are sufficiently descriptive

```C#
namespace System.Net.Quic;

public sealed class QuicListener : IAsyncDisposable
{
    public static bool IsSupported { get; }
    public static ValueTask<QuicListener> ListenAsync(QuicListenerOptions options,
                                                      CancellationToken cancellationToken = default);

    public IPEndPoint ListenEndPoint { get; }
    public ValueTask<QuicConnection> AcceptConnectionAsync(CancellationToken cancellationToken = default);
    public ValueTask DisposeAsync();
}

public sealed class QuicListenerOptions
{
    public QuicListenerOptions();
    public IPEndPoint ListenEndPoint { get; set; }
    public List<SslApplicationProtocol> ApplicationProtocols { get; }
    public int ListenBacklog { get; set; }
    public Func<QuicConnection, SslClientHelloInfo, CancellationToken, ValueTask<QuicServerConnectionOptions>> ConnectionOptionsCallback { get; set; }
}
```
## [QUIC] QuicConnection

**NeedsWork** | [#runtime/68902](https://github.com/dotnet/runtime/issues/68902#issuecomment-1155613212) | [Video](https://www.youtube.com/watch?v=eL_tQFSs54Q&t=1h45m12s)

* `QuicConnection.RemoteCertificate`
    - Should be `X509Certificate2`; nothing in our libraries should ever allocate `X509Certificate`.
* `QuicConnection.OpenStreamAsync`
    - Should probably be `OpenOutboundStream`; otherwise `OpenStreamAsync` and `AcceptStream` seem too similar
    - We may want to consider renaming `AcceptStreamAsync` to `AcceptInboundStreamAsync`
* ***Note**: we haven't reviewed the other types yet*

```C#
namespace System.Net.Quic;

public sealed class QuicConnection : IAsyncDisposable
{
    public static bool IsSupported { get; }
    public static ValueTask<QuicConnection> ConnectAsync(QuicConnectionOptions options,
                                                         CancellationToken cancellationToken = default);

    public IPEndPoint RemoteEndPoint { get; }
    public IPEndPoint LocalEndPoint { get; }
    public X509Certificate2? RemoteCertificate { get; }
    public SslApplicationProtocol NegotiatedApplicationProtocol { get; }
    public ValueTask<QuicStream> OpenStreamAsync(QuicStreamType type, CancellationToken cancellationToken = default);
    public ValueTask<QuicStream> AcceptStreamAsync(CancellationToken cancellationToken = default);
    public ValueTask CloseAsync(long errorCode, CancellationToken cancellationToken = default);
    public void DisposeAsync();
}
```
