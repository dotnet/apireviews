# API Review 01/06/2023

## Rate limiter context HttpContext feature API proposal

**Approved** | [#aspnetcore/45658](https://github.com/dotnet/aspnetcore/issues/45658#issuecomment-1372967290)

API Review Notes:

- Given that it might not be optimal to query statistics on every request, do we have a good way to query these statistics in the background? Putting the `PartitionedRateLimiter<HttpContext>` instances in a singleton service might help with getting the limiters outside a request context, but you still need an HttpContext to call GetStatistics. Could we parameters the EndpointRateLimiter on Endpoint instead of HttpContext?
  - In the future, the EndpointRateLimiter may partition within an endpoint. This doesn't happen today, but we'd be locking ourselves in a bit if we expose a `PartitionedRateLimiter<Endpoint>` instead of `PartitionedRateLimiter<HttpContext>`.
  - And in the case of the global rate limiter we cannot really use any key other than `HttpContext` because there could be a completely custom partitioning scheme.
- What about `RateLimitLeaseInfo` in Alternative Design - 1? We don't want people calling Dispose() on it themselves, but we're not sure this warrants a mostly redundant type.
- An issue with making the rate limiters services is there could be multiple instances of the middleware.
- Do we need the `IRateLimiterContextFeature.HttpContext` property? It could be convenient to save passing around an extra object sometimes, but it seem unnecessary. It could always be added later if it's too difficult to use without it.
- Do we want to make this opt-in? It could save an allocation if we don't pool. You don't have to opt in at the `PartitionRateLimiter` level, but `MemoryCache` has an opt-in API to avoid perf regressions. (https://github.com/dotnet/runtime/issues/50406#issuecomment-1074521694). We could pool `IRateLimiterContextFeature` if we really want to.
  - We could add an opt-out later in a non-breaking way, but that'd be ugly for the majority of users who might not need this feature.
  - Let's make it opt in.
- We'd like to provide `GetStatistics()` APIs rather than expose the entire `PartitionedRateLimiter<HttpContext>` yet. If it's the global one, it should be accessible to the user because they set it on the options.
- Can we come up with a better name than IRateLimiterContextFeature? Can we segregate the interface?
- Do we prefer `IRateLimitLeaseFeature` or `IRateLimiterLeaseFeature`? Let's stick with "RateLimiter" even though it doesn't appear in the `RateLimitLease` type.
- Should `IRateLimiterLeaseFeature.Lease` be nullable? `Endpoint` is nullable for the endpoint feature. And there won't be any lease if you disable rate limiting with the attribute, so yes.
- Are we concerned about exposing the the RateLimitLease after it's disposed? Yes. Let's unset it. This means it's only accessible in inner middleware and OnRejected, but then we can pool.

The following version the the API is approved!

```diff
+ namespace Microsoft.AspNetCore.RateLimiting.Features;

+ public interface IRateLimiterStatisticsFeature
+ {
+     RateLimiterStatistics? GetGlobalStatistics();
+     RateLimiterStatistics? GetEndpointStatistics();
+ }

+ public interface IRateLimiterLeaseFeature
+ {
+      RateLimitLease? Lease { get; } 
+ }

namespace Microsoft.AspNetCore.RateLimiting;

public sealed class RateLimiterOptions
{
    // IRateLimiterStatisticsFeature won't be set unless this is opted into, but will not be unset after the rate limiting middleware exits.
    // IRateLimiterLeaseFeature will always be set, but we should be able to pool it since we unset to avoid exposing a disposed lease.
+    public bool TrackStatistics { get; set; }
}
```
