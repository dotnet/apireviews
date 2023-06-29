# API Review 06/29/2023

## Pass Meter to HttpClientHandler and SocketsHttpHandler

**Approved** | [#runtime/86961](https://github.com/dotnet/runtime/issues/86961#issuecomment-1613616354) | [Video](https://www.youtube.com/watch?v=H8P2wTqBtKM&t=0h0m0s)

For future extensibility it seems like the best answer is to take something like `IMeterFactory`, which has made us reconsider the notion that the factory belongs in the Microsoft.Extensions layer and move it down.

```C#
namespace System.Net.Http
{
    public class HttpClientHandler
    {
        public IMeterFactory? MeterFactory { get; set; }
    }

    public class SocketsHttpHandler
    {
        public IMeterFactory? MeterFactory { get; set; }
    }
}

namespace System.Diagnostics.Metrics
{
    public interface IMeterFactory : IDisposable
    {
        Meter Create(MeterOptions options);
    }

    public static class MeterFactoryExtensions
    {
        public static Meter Create(this IMeterFactory, string name, string? version = null, IEnumerable<KeyValuePair<string,object?>> tags = null,  object? scope = null);
    }
}
```
## pass custom tags for metrics enrichment in HttpClientHandler

**Approved** | [#runtime/86281](https://github.com/dotnet/runtime/issues/86281#issuecomment-1613667260) | [Video](https://www.youtube.com/watch?v=H8P2wTqBtKM&t=1h34m7s)

We moved the extension method to a static `AddCallback` on the context type.  Otherwise, looks good as proposed.


```C#
namespace System.Net.Http.Metrics;

public sealed class HttpMetricsEnrichmentContext
{
    public HttpRequestMessage Request { get; }
    public HttpResponseMessage? Response { get; } // null when an exception is thrown by the innermost handler
    public Exception? Exception { get; } // null when no exception is thrown
    public void AddCustomTag(string name, object? value);

    public static void AddCallback(HttpRequestOptions options, Action<HttpMetricsEnrichmentContext> callback);
}
```

