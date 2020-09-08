# Quick Reviews 09/08/2020

## Add SocketsHttpHandler.ConnectCallback

**Approved** | [#runtime/41949](https://github.com/dotnet/runtime/issues/41949#issuecomment-689032184) | [Video](https://www.youtube.com/watch?v=SyCQa-125uQ&t=0h0m0s)

* We should add a context class that passes in the parameters so that we can add more values over time
* We originally had `SocketsHttpConnectionContext` as get/set POCO but that would mean that all properties would have to be nullable, which would be super annoying to use

```C#
namespace System.Net.Http
{
    public sealed class SocketsHttpConnectionContext
    {
        public DnsEndPoint DnsEndPoint { get; }
        public HttpRequestMessage RequestMessage { get; }
    }
    public sealed class SocketsHttpHandler : HttpMessageHandler
    {
        public Func<SocketsHttpConnectionContext, CancellationToken, ValueTask<Stream>>? ConnectCallback { get; set; }
    }
}
```

## Mark System.Threading.Thread ResetAbort as Obsolete

**Approved** | [#runtime/41925](https://github.com/dotnet/runtime/issues/41925#issuecomment-689040775) | [Video](https://www.youtube.com/watch?v=SyCQa-125uQ&t=0h12m58s)

* We believe it makes sense to reuse the existing diagnostic ID

```C#
namespace System.Threading
{
    public sealed class Thread
    {
        // Obsoleted in .NET 5:
        //[Obsolete(DiagnosticId = "SYSLIB0006")]
        //public void Abort();
        //[Obsolete(DiagnosticId = "SYSLIB0006")]
        //public void Abort(object stateInfo);
        [Obsolete(DiagnosticId = "SYSLIB0006")]
        public static void ResetAbort();
    }
}
```

