# Quick Reviews 09/22/2020

## Allow ignoring unhandled exceptions in UnhandledException event handler

**Approved** | [#runtime/42275](https://github.com/dotnet/runtime/issues/42275#issuecomment-696895538) | [Video](https://www.youtube.com/watch?v=Kk9Fv8m0Ksw&t=0h0m0s)

* Looks good as proposed

```C#
namespace System
{
    public partial class UnhandledExceptionEventArgs : EventArgs
    {
        // Existing property.
        // public bool IsTerminating { get; }
        
        public bool Ignore { get; set; }
    }
}
```
## Add SocketsHttpHandler.PlaintextStreamFilter

**Approved** | [#runtime/42557](https://github.com/dotnet/runtime/issues/42557#issuecomment-696907531) | [Video](https://www.youtube.com/watch?v=Kk9Fv8m0Ksw&t=0h9m45s)

* Looks good as proposed, modulo:
* `HttpVersion` we should consider a term that conveys that it's the version after policy, such as `NegotiatedHttpVersion`
* `RequestMessage` should convey that this is the initial message only, such as `InitialRequestMessage`, because the callback is called once per connection, not per request. We should also rename the property on `SocketsHttpConnectionContext`.

```C#
namespace System.Net.Http
{
    public sealed class SocketsHttpPlaintextStreamFilterContext
    {
        public Stream PlaintextStream { get; }
        public Version NegotiatedHttpVersion { get; }
        public HttpRequestMessage InitialRequestMessage { get; }
    }
    public sealed class SocketsHttpHandler : HttpMessageHandler
    {
        public Func<SocketsHttpPlaintextStreamFilterContext, CancellationToken, ValueTask<Stream>>? PlaintextStreamFilter { get; set; }
    }
}
```
## Add Task-based async API for Socket.SendFile

**Approved** | [#runtime/42591](https://github.com/dotnet/runtime/issues/42591#issuecomment-696912247) | [Video](https://www.youtube.com/watch?v=Kk9Fv8m0Ksw&t=0h32m11s)

* Makes sense, but we should take a cancellation tokena and memories

```C#
namespace System.Net.Sockets
{
    public partial class Socket
    {
        // Existing APM APIs
        // public IAsyncResult BeginSendFile(string fileName, AsyncCallback? callback, object? state);
        // public IAsyncResult BeginSendFile(string? fileName, byte[]? preBuffer, byte[]? postBuffer, TransmitFileOptions flags, AsyncCallback? callback, object? state);
        public ValueTask SendFileAsync(string? fileName, CancellationToken cancellationToken = default);
        public ValueTask SendFileAsync(string? fileName, ReadOnlyMemory<byte> preBuffer, ReadOnlyMemory<byte> postBuffer, TransmitFileOptions flags, CancellationToken cancellationToken = default);
    }
}
```

