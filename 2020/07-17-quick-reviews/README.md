# Quick Reviews 07/17/2020

## Connection Abstractions

**Approved** | [#runtime/1793](https://github.com/dotnet/runtime/issues/1793#issuecomment-660289247) | [Video](https://www.youtube.com/watch?v=n4Pq8e9OlpI&t=0h0m0s)

* Approved. We're done with it!

```C#
public abstract class ConnectionBase : IAsyncDisposable, IDisposable
{
    protected ConnectionBase();
    public abstract IConnectionProperties ConnectionProperties { get; }
    public abstract EndPoint? LocalEndPoint { get; }
    public abstract EndPoint? RemoteEndPoint { get; }
    public ValueTask CloseAsync(ConnectionCloseMethod method = ConnectionCloseMethod.GracefulShutdown, CancellationToken cancellationToken = default);
    protected abstract ValueTask CloseAsyncCore(System.Net.Connections.ConnectionCloseMethod method, CancellationToken cancellationToken);
}

public enum ConnectionCloseMethod
{
    GracefulShutdown,
    Abort,
    Immediate,
}

// needs review: FromPipe with disposable.
public abstract class Connection : ConnectionBase
{
    protected Connection();
    public IDuplexPipe Pipe { get; }
    public Stream Stream { get; }
    protected virtual IDuplexPipe CreatePipe();
    protected virtual Stream CreateStream();
    public static Connection FromPipe(IDuplexPipe pipe, bool leaveOpen = false, IConnectionProperties? properties = null, EndPoint? localEndPoint = null, EndPoint? remoteEndPoint = null);
    public static Connection FromStream(Stream stream, bool leaveOpen = false, IConnectionProperties? properties = null, EndPoint? localEndPoint = null, EndPoint? remoteEndPoint = null);
}

public static class ConnectionExtensions
{
    public static ConnectionFactory Filter(this ConnectionFactory factory, Func<Connection, IConnectionProperties?, CancellationToken, ValueTask<Connection>> filter);
    public static bool TryGet<T>(this IConnectionProperties properties, [MaybeNullWhenAttribute(false)] out T property);
}

public abstract class ConnectionFactory : IAsyncDisposable, IDisposable
{
    protected ConnectionFactory();
    public abstract ValueTask<Connection> ConnectAsync(EndPoint? endPoint, IConnectionProperties? options = null, CancellationToken cancellationToken = default);
    protected virtual void Dispose(bool disposing);
    protected virtual ValueTask DisposeAsyncCore();
}

public abstract class ConnectionListener : IAsyncDisposable, IDisposable
{
    protected ConnectionListener();
    public abstract IConnectionProperties ListenerProperties { get; }
    public abstract EndPoint? LocalEndPoint { get; }
    public abstract ValueTask<Connection> AcceptAsync(IConnectionProperties? options = null, CancellationToken cancellationToken = default);
    protected virtual void Dispose(bool disposing);
    protected virtual ValueTask DisposeAsyncCore();
}

public abstract class ConnectionListenerFactory : IAsyncDisposable, IDisposable
{
    protected ConnectionListenerFactory();
    public abstract ValueTask<ConnectionListener> BindAsync(EndPoint? endPoint, IConnectionProperties? options = null, CancellationToken cancellationToken = default);
    protected virtual void Dispose(bool disposing);
    protected virtual ValueTask DisposeAsyncCore();
}

public interface IConnectionProperties
{
    bool TryGet(Type propertyKey, [NotNullWhenAttribute(true)] out object? property);
}

// needs review. adding to existing class.
public class SocketsHttpHandler
{
    public ConnectionFactory? ConnectionFactory { get; set; }
    public Func<HttpRequestMessage, Connection, CancellationToken, ValueTask<Connection>>? PlaintextFilter { get; set; }
}

// needs review
public class SocketsHttpConnectionFactory : ConnectionFactory
{
    public SocketsHttpConnectionFactory();
    public sealed override ValueTask<Connection> ConnectAsync(EndPoint? endPoint, IConnectionProperties? options = null, CancellationToken cancellationToken = default);
    protected virtual Socket CreateSocket(HttpRequestMessage message, EndPoint? endPoint, IConnectionProperties options);
    protected virtual ValueTask<Connection> EstablishConnectionAsync(HttpRequestMessage message, EndPoint? endPoint, IConnectionProperties options, CancellationToken cancellationToken);
}

// needs review.
class SocketsConnectionFactory : ConnectionFactory
{
    // dual-mode IPv6 socket. See Socket(SocketType socketType, ProtocolType protocolType)
    public SocketsConnectionFactory(SocketType socketType, ProtocolType protocolType);

    // See Socket(AddressFamily addressFamily, SocketType socketType, ProtocolType protocolType)
    public SocketsConnectionFactory(AddressFamily addressFamily, SocketType socketType, ProtocolType protocolType);

    public override ValueTask<Connection> ConnectAsync(EndPoint? endPoint, IConnectionProperties? options = null, CancellationToken cancellationToken = default);

    // These exist to provide an easy way to shim the default behavior.
    // A more idiomatic (but more API-heavy) way to do this would be to pass some sort of ISocketConfiguration that has all the pre-connect socket options one could want.
    protected virtual Socket CreateSocket(AddressFamily addressFamily, SocketType socketType, ProtocolType protocolType, EndPoint? endPoint, IConnectionProperties? options);
    protected virtual Stream CreateStream(Socket socket, IConnectionProperties? options);
    protected virtual IDuplexPipe CreatePipe(Socket socket, IConnectionProperties? options);
}

// needs review.
class SocketsListenerFactory : ConnectionListenerFactory
{
    // dual-mode IPv6 socket. See Socket(SocketType socketType, ProtocolType protocolType)
    public SocketsListenerFactory(SocketType socketType, ProtocolType protocolType);

    // See Socket(AddressFamily addressFamily, SocketType socketType, ProtocolType protocolType)
    public SocketsListenerFactory(AddressFamily addressFamily, SocketType socketType, ProtocolType protocolType);

    public override ValueTask<ConnectionListener> BindAsync(EndPoint? endPoint, IConnectionProperties? options = null, CancellationToken cancellationToken = default);

    // These exist to provide an easy way for users to override default behavior.
    // A more idiomatic (but more API-heavy) way to do this would be to pass some sort of ISocketConfiguration that has all the pre-connect socket options one could want.
    protected virtual Socket CreateSocket(AddressFamily addressFamily, SocketType socketType, ProtocolType protocolType, EndPoint? endPoint, IConnectionProperties? options);
}
```
