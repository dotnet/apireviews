# API Review 03/30/2023

## Add direct APIs for Http.Sys extensions currently using IHttpSysRequestInfoFeature

**NeedsWork** | [#aspnetcore/35012](https://github.com/dotnet/aspnetcore/issues/35012#issuecomment-1491067005)

API Review Notes:

 - What does the existing code look like to read these timestamps?
   - TBD
 - What assembly and namespace will `IHttpSysRequestTimingFeature` live in?
   - We want to copy what we did for `IHttpSysRequestInfoFeature` and `IHttpSysRequestDelegationFeature` which is to put it in Microsoft.AspNetCore.Server.HttpSys.dll in the Microsoft.AspNetCore.Server.HttpSys namespace.
   - IIS in-proc (`UseIIS()`) will probably have to take a dependency on Microsoft.AspNetCore.Server.HttpSys.dll.
 - Do we want an HttpSys feature that covers all the server-specific functionality? It might help with discoverability, but we like interface segregation to separate concerns.
 - These two methods follow the try-pattern. When will they return false?
   - When there are events that didn't happen so the timespan is zero.
 - How do you know which timestamp is which using `ReadOnlySpan<ulong> Timestamps { get; }`? It's the same order as the enum.
   - Could it be an `IEnumerable<ulong>`? Maybe.
   - Should we include the enum value in the span/enumerable? That would make it more expensive.
   - Is it needed at all given the `Try*` methods exist? You could loop yourself, but that would also be more expensive.
- Can `TryGetTimestamp` return return a `DateTime` or `DateTimeOffset`?
- Can `TryGetElapsedTime` be an extension method? Probably, but it just add more types for little value.
- What happens to `TryGetElapsedTime` with ConnectionStart?
  - Returns false? Let's remove it if TryGetTimestamp is giving us a DateTimeOffset
- NOTE: Order matters for the `HttpSysRequestTimingType` enum!  `ConnectionStart` starts at 0 and increments by 1.

Draft update to API:

```c#
// Microsoft.AspNetCore.Server.HttpSys.dll

namespace Microsoft.AspNetCore.Server.HttpSys;

public interface IHttpSysRequestTimingFeature
{
    ReadOnlySpan<ulong> Timestamps { get; }
    bool TryGetTimestamp(HttpSysRequestTimingType timingType, out ulong timestamp);
    bool TryGetElapsedTime(HttpSysRequestTimingType startTiming, HttpSysRequestTimingType endTiming, out TimeSpan elapsed);
}

public enum HttpSysRequestTimingType
{
    ConnectionStart = 0,
    DataStart,
    TlsCertificateLoadStart,
    TlsCertificateLoadEnd,
    TlsHandshakeLeg1Start,
    TlsHandshakeLeg1End,
    TlsHandshakeLeg2Start,
    TlsHandshakeLeg2End,
    TlsAttributesQueryStart,
    TlsAttributesQueryEnd,
    TlsClientCertQueryStart,
    TlsClientCertQueryEnd,
    Http2StreamStart,
    Http2HeaderDecodeStart,
    Http2HeaderDecodeEnd,
    RequestHeaderParseStart,
    RequestHeaderParseEnd,
    RequestRoutingStart,
    RequestRoutingEnd,
    RequestQueuedForInspection,
    RequestDeliveredForInspection,
    RequestReturnedAfterInspection,
    RequestQueuedForDelegation,
    RequestDeliveredForDelegation,
    RequestReturnedAfterDelegation,
    RequestQueuedForIO,
    RequestDeliveredForIO,
    Http3StreamStart,
    Http3HeaderDecodeStart,
    Http3HeaderDecodeEnd,
}
```

API needs work.

We need to better understand what a timestamp is. We want to know if it even can be represented as `DateTime` and whether the frequency is dynamic and needs to be exposed via API. It would also help if we could fill out the API proposal issue template: https://github.com/dotnet/aspnetcore/issues/new?assignees=&labels=api-suggestion&template=30_api_proposal.md&title=
## Add IRouteDiagnosticsMetadata

**Approved** | [#aspnetcore/47492](https://github.com/dotnet/aspnetcore/issues/47492#issuecomment-1491079849)

API Review Note:

- Would we ever consider moving `RouteEndpoint` and `RoutePattern` down into Http.Abstractions.dll?
  - `RoutePattern` might be too big when all we need is the string for stuff like diagnostics and logging.
- [Microsoft.AspNetCore.Http.Metadata](https://learn.microsoft.com/en-nz/dotnet/api/microsoft.aspnetcore.http.metadata?view=aspnetcore-670) or Microsoft.AspNetCore.Routing? Let's hide it in Microsoft.AspNetCore.Http.Metadata.
- Can we just have Routing.dll add metadata with the actual `RoutePattern`? If you have a reference to routing, you can just cast the `Endpoint` to a `RouteEndpoint`.
  - `var route = ((RouteEndpoint)httpContext.GetEndpoint()).RoutePattern`
  - Hosting needs it.
- Do we like the name `Route` for the property? What about `RoutePattern`?
  - We don't want to get peoples hopes up that they're getting the actual `RoutePattern` type.
- What about the `IRouteDiagnosticsMetadata` type name? There could be a lot of "Diagnostics". Would we add more to it in the future? Probably not.

API Approved!

```diff
// Microsoft.AspNetCore.Http.Abstractions.dll

namespace Microsoft.AspNetCore.Http.Metadata;

+ public interface IRouteDiagnosticsMetadata
+ {
+     string Route { get; }
+ }
```
