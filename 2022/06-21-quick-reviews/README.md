# API Review 06/21/2022

## [QUIC] QuicConnection

**NeedsWork** | [#runtime/68902](https://github.com/dotnet/runtime/issues/68902#issuecomment-1162128560) | [Video](https://www.youtube.com/watch?v=-tJL3MKcuoM&t=0h0m0s)

* `QuicConnection`
    - What's the lifetime expectation of `CloseAsync` vs `DisposeAsync`?
* `QuicConnectionOptions`
    - Should we have a default values for `MaxBidirectionalStreams` and `MaxUnidirectionalStreams`. Consider renaming those to make it clear that this is about accepted/inbound streams, never about outbound.
    - You may want to move the initialization to the derived classes to allow for different defaults for client/server, e.g. `0`/`0` for client and `100`/`10` for server.
    - `IdleTimeout` can we expose the underlying timeout? This way we don't expose our own policy. If we can't, we can also make it nullable and just say "platform specific timeout".
    - Depending on where `QuicConnection`'s `CloseAsync`/`DisposeAsync` lands, you may want to add `DefaultConnectionErrorCode`
* This is going to either use constructors to enforce configuring required values or the new C# `required` modifier

```C#
namespace System.Net.Quic;

public abstract class QuicConnectionOptions
{
    public int MaxBidirectionalStreams { get; set; } = 100;
    public int MaxUnidirectionalStreams { get; set; } = 100;
    public TimeSpan IdleTimeout { get; set; } = TimeSpan.FromMinutes(2);
    public long DefaultStreamErrorCode { get; set; }
    public long DefaultConnectionErrorCode { get; set; }
    public Action<QuicConnection>? EndPointChanged { get; set; }
}
public sealed class QuicClientConnectionOptions : QuicConnectionOptions
{
    public QuicClientConnectionOptions();
    public SslClientAuthenticationOptions ClientAuthenticationOptions { get; set; }
    public EndPoint RemoteEndPoint { get; set; }
    public IPEndPoint? LocalEndPoint { get; set; }
}
public sealed class QuicServerConnectionOptions : QuicConnectionOptions
{
    public QuicServerConnectionOptions();
    public SslServerAuthenticationOptions ServerAuthenticationOptions { get; set; }
}
```
