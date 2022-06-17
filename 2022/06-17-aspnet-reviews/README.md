# API Review 06/17/2022

## Allow endpoint filter factories to manipulate endpoint metadata

**Approved** | [#aspnetcore/41722](https://github.com/dotnet/aspnetcore/issues/41722)

API review notes from @captainsafia :

[this proposal] makes sense given the other changes made around EndpointMetadata in the route groups PR.

## EndpointBuilder.ServiceProvider should be non-nullabe and renamed to ApplicationServices

**Approved** | [#aspnetcore/42137](https://github.com/dotnet/aspnetcore/issues/42137)

API review notes from @captainsafia:

[this proposal] is a sensible change that reflects our efforts to make use of IServiceProvider​ in contexts more consistent.

## Add Output Caching middleware

**Approved** | [#aspnetcore/41955](https://github.com/dotnet/aspnetcore/issues/41955))

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
