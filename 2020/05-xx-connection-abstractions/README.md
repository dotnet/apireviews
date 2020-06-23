# Connection Abstractions

**In-progress** |
[Issue](https://github.com/dotnet/runtime/issues/1793)

## Round 1

[Video](https://www.youtube.com/watch?v=a9WxNvINTmU&list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju)

* There was an unresolved discussion that most, if not all, of these interfaces
  should be abstract classes.
* `ConnectionPropertyCollection` shouldn't use the name `Collection` since it's
  not enumerable
* `(I)ConnectionStream` should not use the name `Stream`, since it does not
  derive from `Stream`.
* There was an unresolved discussion around splitting the connection factory
  into two types, one for `Connect` and one for `Bind`.
  * The main question was about what that would look like for implementations,
    and if it becomes limiting for overload resolution--including extension
    method resolution.
  * If they're on the same type, is it worth a `CanBind`/`CanAccept`?
* There was a long, unresolved, debate of whether or not the connection factory
  should be disposable.
  * What should the ownership model of factory composition be? If the factory
    type abstraction isn't disposable but a specific factory is, who owns it
    now?
* Consider an `Abort()` method on `IConnectionStream`.
  * Presumably that requires implementers to have a way to abort their stream or
    pipe?
  * Does that make `Dispose()` graceful, or do we need some kind of
    `CloseGracefully()` to disambiguate that as well?
* Combine `IConnectionStream` and IConnection, the endpoint properties are already
  nullable so the separation isn't providing a lot of value.
* There was a long, unresolved, debate on using `Type` or `object` as the key on
  the property lookup.

One potential alternative for the property lookup (in no way agreed upon, but
took elements from the discussion):

```C#
public abstract class Connection
{
    public bool TryGetProperty<T>(out T value) where T : IConnectionPropertySet
    {
        object valueObject;

        if (!TryGetProperty(typeof(T), out valueObject))
        {
            return _parent != null && _parent.TryGetProperty(typeof(T), out valueObject);
        }

        value = (T)valueObject;
        return true;
    }

    public T GetProperty<T>() where T : IConnectionPropertySet
    {
        if (!TryGetProperty<T>(out T value))
        {
            throw Something;
        }

        return value;
    }

    protected virtual bool TryGetProperty(Type key, out T value);
}
```

## Round 2

[Video](https://www.youtube.com/watch?v=C5W7f10eX3Q&list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju)

* We should use base classes for connection, listener, and connection factory.
    - Specifically the property bag should remain as an interface
      (`IConnectionPropertyBag`) but we need a simple default like
      `ConnectionPropertyBag`.
* Rough shape:
    ```C#
    public enum ConnectionCloseMethod
    {
        GracefulShutdown,
        Abort,
        Immediate
    }
    public abstract class ConnectionBase : IAsyncDisposable, IDisposable
    {
        public IConnectionProperties ConnectionProperties { get; }
        public EndPoint? LocalEndPoint { get; }
        public EndPoint? RemoteEndPoint { get; }

        public ValueTask CloseAsync(ConnectionCloseMethod method = default, CancellationToken cancellationToken = default);
    }
    public class Connection : ConnectionBase
    {
        public Stream Stream { get; }
        public IDuplexPipe Pipe { get; }
    }
    public class MultiplexedConnection : ConnectionBase
    {
        public ValueTask<Connection> OpenStream(IConnectionProperties? options = null, CancellationToken cancellationToken = default);
        public ValueTask<Connection> AcceptStream(IConnectionProperties? options = null, CancellationToken cancellationToken = default);
    }
    ```

## Round 3

[Video](https://www.youtube.com/watch?v=vEUndbRMOjQ)

```C#
public enum ConnectionCloseMethod
{
    GracefulShutdown,
    Abort,
    Immediate
}

public abstract class ConnectionBase : IAsyncDisposable, IDisposable
{
    public virtual IConnectionProperties ConnectionProperties { get; }
    public virtual EndPoint? LocalEndPoint { get; }
    public virtual EndPoint? RemoteEndPoint { get; }

    public abstract ValueTask CloseAsync(ConnectionCloseMethod method = default, CancellationToken cancellationToken = default);
}

public abstract class Connection : ConnectionBase
{
    public Stream Stream { get; }
    public IDuplexPipe Pipe { get; }

    protected virtual Stream CreateStream();
    protected virtual IDuplexPipe CreatePipe();

    public static Connection FromStream(Stream stream, IConnectionProperties? properties = null, EndPoint? localEndPoint = null, EndPoint? remoteEndPoint = null);
    public static Connection FromPipe(IDuplexPipe pipe, IConnectionProperties? properties = null, EndPoint? localEndPoint = null, EndPoint? remoteEndPoint = null);
}

public abstract class ConnectionFactory : IAsyncDisposable, IDisposable
{
	public abstract ValueTask<Connection> ConnectAsync(EndPoint? endPoint, IConnectionProperties? options = null, CancellationToken cancellationToken = default);

	public void Dispose();
	public ValueTask DisposeAsync();

	protected abstract void Dispose(bool disposing);
	protected abstract ValueTask DisposeAsyncCore();
}

public abstract class ConnectionListenerFactory : IAsyncDisposable, IDisposable
{
	public abstract ValueTask<ConnectionListener> BindAsync(EndPoint? endPoint, IConnectionProperties? options = null, CancellationToken cancellationToken = default);

	public void Dispose();
	public ValueTask DisposeAsync();

	protected abstract void Dispose(bool disposing);
	protected abstract ValueTask DisposeAsyncCore();
}

public abstract class ConnectionListener : IAsyncDisposable, IDisposable
{
	public IConnectionProperties ListenerProperties { get; }
	public EndPoint? LocalEndPoint { get; }

	public void Dispose();
	public ValueTask DisposeAsync();

	protected abstract void Dispose(bool disposing);
	protected abstract ValueTask DisposeAsyncCore();

	public abstract ValueTask<Connection> AcceptAsync(IConnectionProperties? options = null, CancellationToken cancellationToken = default);
}

public class SocketsHttpConnectionFactory : ConnectionFactory
{
    // For users of the above two APIs, they can call this if they just want to wrap the defaults.
    public virtual Socket CreateSocket(HttpRequestMessage message, DnsEndPoint endPoint, IConnectionProperties options);
    public virtual ValueTask<IConnection> EstablishConnection(HttpRequestMessage message, DnsEndPoint endPoint, IConnectionProperties options, CancellationToken cancellationToken);
}
```
