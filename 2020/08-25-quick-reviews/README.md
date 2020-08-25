# Quick Reviews 08/25/2020

## Add ConnectionId to ConnectionBase

**Approved** | [#runtime/41110](https://github.com/dotnet/runtime/issues/41110#issuecomment-680176004) | [Video](https://www.youtube.com/watch?v=j8HB7GczxNQ&t=0h0m0s)

* The connection id shouldn't change. Some protocols, such as QUIC, have changing ids but the connection id of `ConnectionBase` won't be using that. Otherwise, it's impossible for users to track instances over time, which is the goal for this property.
* To guarantee this, we should the template method pattern where the property is non-virtual and it calls a virtual method to lazily initialize itself.
* The base implementation will use an auto generated GUID or a monotonically increasing integer
* We should allow passing the connection ID to `FromPipe` and `FromStream` 

```C#
namespace System.Net.Connections
{
    public partial class ConnectionBase
    {
        public string ConnectionId { get; }
        protected virtual string CreateConnectionId();
    }
    public partial class Connection
    {
        // Existing method, add connectionId
        public static Connection FromPipe(
            IDuplexPipe pipe,
            bool leaveOpen = false,
            IConnectionProperties? properties = null,
            EndPoint? localEndPoint = null,
            EndPoint? remoteEndPoint = null,
            string? connectionId = null);


        // Existing method, add connectionId
        public static Connection FromStream(
            Stream stream,
            bool leaveOpen = false,
            IConnectionProperties? properties = null,
            EndPoint? localEndPoint = null,
            EndPoint? remoteEndPoint = null,
            string? connectionId = null);
    }
}
```

## get full path of current process executable

**Approved** | [#runtime/40862](https://github.com/dotnet/runtime/issues/40862#issuecomment-680189514) | [Video](https://www.youtube.com/watch?v=j8HB7GczxNQ&t=0h41m48s)

* Let's separate the issue of loading content files (#41341) the path of the OS process.
* This looks good.

```C#
namespace System
{
    public partial class Environment
    {
        public static string? ProcessPath { get; }
    }
}
```

## BackgroundService TaskStatus

**Approved** | [#runtime/35991](https://github.com/dotnet/runtime/issues/35991#issuecomment-680203238) | [Video](https://www.youtube.com/watch?v=j8HB7GczxNQ&t=1h8m42s)

* Should be a property ;-)
* Let's rename it to `ExecuteTask` to make it clear that's the task returned by `ExecuteAsync` rather than being some transient task.

```C#
namespace Microsoft.Extensions.Hosting
{
    public abstract class BackgroundService
    {
        public virtual Task ExecuteTask { get; }
    }
}
```

## Make char.IsAscii public

**Approved** | [#runtime/40936](https://github.com/dotnet/runtime/issues/40936#issuecomment-680205179) | [Video](https://www.youtube.com/watch?v=j8HB7GczxNQ&t=1h34m44s)

* Looks reasonable

```C#
namespace System
{
    public readonly struct Char
    {
        public static bool IsAscii(char ch);
    }
}
```

## Add Environment.Is32BitProcess

**Rejected** | [#runtime/41188](https://github.com/dotnet/runtime/issues/41188#issuecomment-680208361) | [Video](https://www.youtube.com/watch?v=j8HB7GczxNQ&t=1h39m40s)

* We already have `Is64BitProcess` and `Is64BitOperatingSystem`. Given that we're extremely unlikely to support either 16bit or 128bit, how would this be different from negating those properties?

```C#
namespace System
{
    public partial class Environment
    {
        public static bool Is32BitProcess  { get; }
        // public static bool Is32BitOperatingSystem { get; }
    }
}
```

## Allow ConnectionListener.AcceptAsync() to return null

**Approved** | [#runtime/41304](https://github.com/dotnet/runtime/issues/41304#issuecomment-680213099) | [Video](https://www.youtube.com/watch?v=j8HB7GczxNQ&t=1h46m19s)

* Looks good as proposed

```C#
namespace System.Net.Connections
{
    public partial class ConnectionListener
    {
        public abstract ValueTask<Connection?> AcceptAsync(
            IConnectionProperties? options = null,
            CancellationToken cancellationToken = default);
    }
}
```

