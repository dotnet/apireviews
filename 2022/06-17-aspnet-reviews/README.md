# API Review 06/17/2022

## Allow endpoint filter factories to manipulate endpoint metadata

**Approved** | [#aspnetcore/41722](https://github.com/dotnet/aspnetcore/issues/41722#issuecomment-1158421830)

API review notes from @captainsafia :

[this proposal] makes sense given the other changes made around EndpointMetadata in the route groups PR.

## EndpointBuilder.ServiceProvider should be non-nullabe and renamed to ApplicationServices

**Approved** | [#aspnetcore/42137](https://github.com/dotnet/aspnetcore/issues/42137#issuecomment-1158421583)

API review notes from @captainsafia:

[this proposal] is a sensible change that reflects our efforts to make use of IServiceProvider​ in contexts more consistent.

## Add Output Caching middleware

**Approved** | [#aspnetcore/41955](https://github.com/dotnet/aspnetcore/issues/41955#issuecomment-1158484702)

Final preview6 API review notes:

- Use `ValueTask` everywhere.
- `IOutputCacheStore` is a bit weird. It's not clear how the entries get tagged.
- Lets approve for preview6 and get some feedback and make more breaking changes in preview7

```csharp
namespace Microsoft.AspNetCore.Builder;

public static class OutputCacheServiceCollectionExtensions
{
    public static IServiceCollection AddOutputCache(this IServiceCollection services);
    public static IServiceCollection AddOutputCache(this IServiceCollection services, Action<OutputCacheOptions> configureOptions);
}
 
public static class OutputCacheApplicationBuilderExtensions
{
    public static IApplicationBuilder UseOutputCache(this IApplicationBuilder app);
}
 
namespace Microsoft.Extensions.DependencyInjection;

public static class OutputCacheConventionBuilderExtensions
{
    public static TBuilder CacheOutput<TBuilder>(this TBuilder builder) where TBuilder : IEndpointConventionBuilder;
    public static TBuilder CacheOutput<TBuilder>(this TBuilder builder, IOutputCachePolicy policy) where TBuilder : IEndpointConventionBuilder;
    public static TBuilder CacheOutput<TBuilder>(this TBuilder builder, Action<OutputCachePolicyBuilder> policy) where TBuilder : IEndpointConventionBuilder;
    public static TBuilder CacheOutput<TBuilder>(this TBuilder builder, string policyName) where TBuilder : IEndpointConventionBuilder;
}

namespace Microsoft.AspNetCore.OutputCaching;

public sealed class OutputCacheOptions
{
    public long SizeLimit { get; set; }
    public long MaximumBodySize { get; set; }
    public TimeSpan DefaultExpirationTimeSpan { get; set; }
    public bool UseCaseSensitivePaths{ get; set; }
    public IList<IOutputCachePolicy> BasePolicies { get; }
    public IServiceProvider ApplicationServices { get; }
    public void AddPolicy(string name, IOutputCachePolicy policy);
    public void AddPolicy(string name, Action<OutputCachePolicyBuilder> build);
 }
 
 public interface IOutputCachePolicy
 {
    ValueTask CacheRequestAsync(OutputCacheContext context, CancellationToken token);
    ValueTask ServeFromCacheAsync(OutputCacheContext context, CancellationToken token);
    ValueTask ServeResponseAsync(OutputCacheContext context, CancellationToken token);
 }
 
public sealed class OutputCachePolicyBuilder
{
    public OutputCachePolicyBuilder();
    public OutputCachePolicyBuilder AddPolicy(Type policyType);
    public OutputCachePolicyBuilder AddPolicy<T>() where T : IOutputCachePolicy;
    public OutputCachePolicyBuilder AddPolicy(string policyName);
    public OutputCachePolicyBuilder With(Func<OutputCacheContext, bool> predicate);
    public OutputCachePolicyBuilder With(Func<OutputCacheContext, CancellationToken, ValueTask<bool>> asyncPredicate);
    public OutputCachePolicyBuilder Tag(params string[] tags);
    public OutputCachePolicyBuilder Expire(TimeSpan expiration);
    public OutputCachePolicyBuilder AllowLocking(bool lockResponse = true);
    public OutputCachePolicyBuilder Clear();
    public OutputCachePolicyBuilder NoCache();
    public OutputCachePolicyBuilder VaryByQuery(params string[] queryKeys);
    public OutputCachePolicyBuilder VaryByHeader(params string[] headers);
    public OutputCachePolicyBuilder VaryByValue(Func<HttpContext, string> varyBy);
    public OutputCachePolicyBuilder VaryByValue(Func<HttpContext, CancellationToken, ValueTask<string>> varyByAsync);
    public OutputCachePolicyBuilder VaryByValue(Func<HttpContext, KeyValuePair<string, string>> varyBy);
    public OutputCachePolicyBuilder VaryByValue(Func<HttpContext, CancellationToken, ValueTask<KeyValuePair<string, string>>> varyByAsnc);
}
 
public sealed class OutputCacheContext
{
    public OutputCacheContext();
    public bool EnableOutputCaching { get; set; }
    public bool AllowCacheLookup { get; set; }
    public bool AllowCacheStorage { get; set; }
    public bool AllowLocking { get; set; }
    public HttpContext HttpContext { get; }
    public DateTimeOffset? ResponseTime { get; }
    public CachedVaryByRules CachedVaryByRules { get; set; }
    public HashSet<string> Tags { get; }
    public TimeSpan? ResponseExpirationTimeSpan { get; set; }
}
 
public sealed class CacheVaryByRules
{
    public CacheVaryByRules();
    public IDictionary<string, string> VaryByCustom { get; }
    public StringValues Headers { get; init; }
    public StringValues QueryKeys { get; init; }
    public StringValues VaryByPrefix { get; init; }
}
 
public sealed class OutputCacheAttribute : Attribute
{
    public OutputCacheAttribute();
    public int Duration { get; init; }
    public bool NoStore { get; init; }
    public string[]? VaryByQueryKeys { get; init; }
    public string[]? VaryByHeaders { get; init; }
    public string? PolicyName { get; init; }
}
 
public interface IOutputCacheFeature
{
    OutputCacheContext Context { get; }
}

public interface IOutputCacheStore
{
    ValueTask EvictByTagAsync(string tag, CancellationToken token);
    ValueTask<byte[]> GetAsync(string key, CancellationToken token);
    ValueTask SetAsync(string key, byte[] value, TimeSpan validFor, CancellationToken token);
}
```

API Approved!

## Make RateLimiterMiddleware endpoint-aware
**Approved** | [#aspnetcore/41667](https://github.com/dotnet/aspnetcore/issues/41667#issuecomment-1159267825)

API Review Notes:

- `RateLimitPartition.CreateNoLimiter()` Makes it sound like a limiter or partition is being created every time the method is called.
  - This is already approved runtime API, but it should change.
  - We should communicate this is not creating a limiter or a partition necessarily. Most of the time it's just crating a key to look up an existing partition. It also happens to include information about how to create a partition if need be, but this happens less frequently.
  -  We should submit a runtime API proposal for this. `RateLimitPartition.Choose()`?
- Do we like "Custom" in `CustomRejectionStatusCode` and `CustomOnRejected`?
    - Should the "Custom" properties be nullable?
    - Yes to both.
- Should `DefaultRejectionStatusCode` be `RejectionStatusCode`?
  - Yes.
- Can we just get rid of `IRateLimiterPolicy<TKey>.RejectionStatusCode`.
- I can inject DI services into MyRateLimiterPolicy's constructors right?
- `AddTokenBucketRateLimiter` ->  `AddTokenBucketLimiter`
  - Remove `Rate` from all of these.
- Should `HttpContext`, `RateLimitLease` be added to a context class for future proofing
  - `OnRejectedContext` class?
  - Yes. Let's use `required init` here too.
- `IRateLimitMetadata` (not in the proposal currently, but in the PR) should be made internal for now.
- `Task` to `ValueTask`.
- `PartitionedRateLimiter<HttpContext> Limiter` to `PartitionedRateLimiter<HttpContext>? GlobalLimiter`
- Add CancellationToken
- `TKey` -> `TPartitionKey`

```diff
namespace Microsoft.AspNetCore.RateLimiting
{
-  public interface IRateLimitMetadata
- {
- }
-
+   public interface IRateLimiterPolicy<TPartitionKey>
+   {
+       public Func<OnRejectedContext, CancellationToken, ValueTask>? OnRejected { get; }
+       public RateLimitPartition<TPartitionKey> GetPartition(HttpContext httpContext);
+   }

    public sealed class RateLimiterOptions
    {
-        public PartitionedRateLimiter<HttpContext> Limiter { get; set; }
+        public PartitionedRateLimiter<HttpContext>? GlobalLimiter { get; set; }
-        public Func<HttpContext, RateLimitLease, Task> OnRejected { get; set; }
+        public Func<OnRejectedContext, CancellationToken, ValueTask>? OnRejected { get; set; }
-        public int DefaultRejectionStatusCode { get; set; }
+        public int RejectionStatusCode { get; set; }
+        public RateLimiterOptions AddPolicy<TPartitionKey>(string policyName, Func<HttpContext, RateLimitPartition<TPartitionKey>> partitioner)
+        public RateLimiterOptions AddPolicy<TPartitionKey, TPolicy>(string policyName) where TPolicy : IRateLimiterPolicy<TPartitionKey>
+        public RateLimiterOptions AddPolicy<TPartitionKey>(string policyName, IRateLimiterPolicy<TPartitionKey> policy);
    }

+ // We could add the policy name to this in the future if we want.
+ public sealed class OnRejectedContext
+ {
+       public HttpContext HttpContext { get; required init; }
+       public RateLimitLease Lease { get; required init; }
+ }
+
+   public static class RateLimiterOptionsExtensions
+   {
+       public static RateLimiterOptions AddTokenBucketLimiter(this RateLimiterOptions options, string policyName, TokenBucketRateLimiterOptions tokenBucketRateLimiterOptions)
+       public static RateLimiterOptions AddFixedWindowLimiter(this RateLimiterOptions options, string policyName, FixedWindowRateLimiterOptions fixedWindowRateLimiterOptions)
+       public static RateLimiterOptions AddSlidingWindowLimiter(this RateLimiterOptions options, string policyName, SlidingWindowRateLimiterOptions slidingWindowRateLimiterOptions)
+       public static RateLimiterOptions AddConcurrencyLimiter(this RateLimiterOptions options, string policyName, ConcurrencyLimiterOptions concurrencyLimiterOptions)
+       public static RateLimiterOptions AddNoLimiter(this RateLimiterOptions options, string policyName)
+   }

-   public static class RateLimitingApplicationBuilderExtensions
-   {
-       //...
-   }
}
+ namespace  Microsoft.AspNetCore.Builder
+ {
+   public static class RateLimitingApplicationBuilderExtensions
+ {
+     //...
+ }
+
+   public static class RateLimiterEndpointConventionBuilderExtensions
+   {
+       public static TBuilder RequireRateLimiting<TBuilder>(this TBuilder builder, string policyName) where TBuilder : IEndpointConventionBuilder
+   }
}
```

API Approved!
