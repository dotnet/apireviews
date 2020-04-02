# Quick Reviews 4/1/2020

## Add a HTTP Message Handler to configure options in WASM Browser 

**Needs Work** | [#runtime/34168](https://github.com/dotnet/runtime/issues/34168#issuecomment-607439446) | [Video](https://www.youtube.com/watch?v=jQw5hiePju0&t=0h0m0s)

* **Shipping vehicle**. Which assembly should this go in? Should this be shipped out-of-band?
* **Namespace**. We should consider either having a prefix for all types or introduce a new namespace.
* **Scoping**. It seems those are really per request, having them on the handler seems problematic unless we say they are just defaults. But we'd still need a way to pass them in per request.
* **Mechanics**. It seems we all like the concept of a loose property bag over message that a handler can inspect to perform request specific operations. However, we don't like the loose typing of `HttpRequestMessage.Properties` which is `IDictionary<string, object>`. It seems the easiest short term solution is an extension methods over `HttpRequestMessage` like `SetRequestCache`/`GetRequestCache` that will set/get the value in the property bag. Alternatively, we could expose a new type, such as `HttpMessageOptions` that people can pass around and that we can be stored in `HttpRequestMessage.Properties`. This way, libraries can accept an `HttpMessageOptions` and use that in case they are constructing HTTP messages themselves. @scalablecory also had [this idea](https://github.com/dotnet/runtime/issues/1793#issuecomment-607085294).

```C#
#nullable enable
namespace System.Net.Http
{
    public class WebAssemblyHttpHandler : HttpMessageHandler
    {
        public WebAssemblyHttpHandler();
        public string? Integrity { get; set; }
        public RequestCache? RequestCache { get; set; }
        public RequestCredentials? RequestCredentials { get; set; }
        public RequestMode? RequestMode { get; set; }
        public bool StreamingEnabled { get; set; }
    }
    public enum RequestCache
    {
        Default = 0,
        NoStore = 1,
        Reload = 2,
        NoCache = 3,
        ForceCache = 4,
        OnlyIfCached = 5,
    }
    public enum RequestCredentials
    {
        Omit = 0,
        SameOrigin = 1,
        Include = 2,
    }
    public enum RequestMode
    {
        SameOrigin = 0,
        NoCors = 1,
        Cors = 2,
        Navigate = 3,
    }
}
```

