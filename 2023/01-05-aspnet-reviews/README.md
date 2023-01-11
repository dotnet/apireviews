# API Review 01/05/2023

## Remove/replace virtual method in UserManager.cs constructor

**Rejected** | [#aspnetcore/45358](https://github.com/dotnet/aspnetcore/issues/45358#issuecomment-1371768982)

Yep. Sounds like CallBase is the right solution here. Was an interesting interlude into the mockability or lack thereof of `UserManager<TUser>` and did cause us to have a bit of a discussion on how we might do things differently in the future. I'll go ahead and close this issue for now.

## Adding Http(Request/Response) extension methods overloads accepting non-generic JsonTypeInfo

**Approved** | [#aspnetcore/45568](https://github.com/dotnet/aspnetcore/issues/45568#issuecomment-1372913890)

API Review Notes:

1. This matches the pattern used by JsonSerializer.DeserializeAsync/SerializeAsync
2. We're curious how JsonSerializer handles null, but we figure it just treats the type as `object`.

API Approved as proposed!

## Add tag trough OutputCacheAttribute

**Approved** | [#aspnetcore/45842](https://github.com/dotnet/aspnetcore/issues/45842#issuecomment-1372918369)

API Review Notes:

1. This follows a very similar pattern to other `string[]?` properties on the `OutputCacheAttribute`.
2. @sebastienros omitted this originally because tag data is often dynamic, but even in our samples, we sometimes use static sets of tags for a given endpoint.

API approved as proposed!

## [Analyzer] Warn on ambiguous route patterns

**Approved** | [#aspnetcore/45223](https://github.com/dotnet/aspnetcore/issues/45223#issuecomment-1372930590)

API Review Notes:

- How conservative is this check? Very. Any unknown extension method on the IEndpointConventionBuilder causes this not to fire. Same if the builder is stored in the variable.
- Why not error level? We're a little worried about false positives still. If we don't get feedback about false positives, we can consider increasing the severity. Also, other endpoints can still work when there are conflicting routes.
- We consider any false positives a bug. We want to error on the side of false negatives in all cases.
- Does this fire once for each conflict, or each endpoint that conflicts? It will fire for each endpoint that conflicts.
- What do we think about the titles and messages?
  - Should we show what patterns it conflicts with? No. It could get really noisy, and each conflict is already highlighted.
  - "route's path" vs "route's pattern"? "route's pattern"
- What about the Usage category? Makes sense.

Analyzer approved as proposed with s/path/pattern.

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
