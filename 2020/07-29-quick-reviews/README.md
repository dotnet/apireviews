# Quick Reviews 07/29/2020

## configurable HTTP/2 PING timeouts in HttpClient

**Approved** | [#runtime/31198](https://github.com/dotnet/runtime/issues/31198#issuecomment-665783930)

```C#
namespace System.Net.Http
{
    public class SocketsHttpHandler
    {
        public TimeSpan KeepAlivePingDelay { get; set; }
        public TimeSpan KeepAlivePingTimeout { get; set; }
        public HttpKeepAlivePingPolicy KeepAlivePingPolicy { get; set; }
    }
    public enum HttpKeepAlivePingPolicy
    {
        WithActiveRequests,
        Always
    }
}
```
