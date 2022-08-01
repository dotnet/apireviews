# API Review 08/01/2022

## API additions for RateLimitingMiddleware

**Approved** | [#aspnetcore/42667](https://github.com/dotnet/aspnetcore/issues/42667#issuecomment-1201544374)

API Review Notes:

- Do we need interfaces and non-attribute classes for metadata.
  - Not technically, but we have interfaces for both CORS and Auth, so we're breaking the pattern here if we don't.
  - That said, it's not clear whether we prefer an interface hierarchy or a bool property to indicate what's disabled.
  - If we decide how the interfaces should work, we *could* add them later to the attributes.
  - DisableRateLimitingAttribute always wins if present
  - Let us seal the attributes as well.
- `RateLimiterPolicyMetadata<TPartitionKey>` attribute?
  - Attributes don't make sense when you're specifying an instance.
  - Let us remove `RateLimiterPolicyMetadata<TPartitionKey>` for now and give the policy an unspeakable name and reference it using `EnableRateLimitingAttribute`.
- We like the IServiceCollection Add* extension method for configuring options
  - We still allow configuring options via `UseRateLimiting(options)`. We will prefer the more local options if specified.
- We changed the rate limiting options type to be mutable so they can follow the more conventional pattern of configuring options in ASP.NET
- Is `RequireRateLimiting<TBuilder, TPartitionKey>` going to require specifying both generic parameters all the time?
  - Since the `TPartitionKey` can be inferred from the policy instance we pass in, this shouldn't be necessary.

```diff
namespace Microsoft.AspNetCore.RateLimiting
+    [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = false, Inherited = true)]
+    public sealed class EnableRateLimitingAttribute : Attribute
+    {
+        public EnableRateLimitingAttribute(string policyName);
+        string PolicyName { get; }
+    }
+
+    [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = false, Inherited = true)]
+    public sealed class DisableRateLimitingAttribute : Attribute
+    {
+        public DisableRateLimitingAttribute();
+    }

namespace Microsoft.AspNetCore.Builder

+   public static class RateLimiterServiceCollectionExtensions
+    {
+        public static IServiceCollection AddRateLimiter(this IServiceCollection services, Action<RateLimiterOptions> configureOptions);
+    }

    public static class RateLimiterOptionsExtensions
    {
-       public static RateLimiterOptions AddNoLimiter(this RateLimiterOptions options, string policyName)
-       public static RateLimiterOptions AddTokenBucketLimiter(this RateLimiterOptions options, string policyName, TokenBucketRateLimiterOptions tokenBucketRateLimiterOptions)
+       public static RateLimiterOptions AddTokenBucketLimiter(this RateLimiterOptions options, string policyName, Action<TokenBucketRateLimiterOptions> configureOptions)
-       public static RateLimiterOptions AddFixedWindowLimiter(this RateLimiterOptions options, string policyName, FixedWindowRateLimiterOptions fixedWindowRateLimiterOptions)
+       public static RateLimiterOptions AddFixedWindowLimiter(this RateLimiterOptions options, string policyName, Action< FixedWindowRateLimiterOptions> configureOptions)
-       public static RateLimiterOptions AddSlidingWindowLimiter(this RateLimiterOptions options, string policyName, SlidingWindowRateLimiterOptions slidingWindowRateLimiterOptions)
+       public static RateLimiterOptions AddSlidingWindowLimiter(this RateLimiterOptions options, string policyName, Action< SlidingWindowRateLimiterOptions> configureOptions)
-       public static RateLimiterOptions AddConcurrencyLimiter(this RateLimiterOptions options, string policyName, ConcurrencyLimiterOptions concurrencyLimiterOptions)
+       public static RateLimiterOptions AddConcurrencyLimiter(this RateLimiterOptions options, string policyName, Action< ConcurrencyLimiterOptions> configureOptions)
    }

    public static class RateLimiterEndpointConventionBuilderExtensions
    {
+       public static TBuilder DisableRateLimiting<TBuilder>(this TBuilder builder) where TBuilder : IEndpointConventionBuilder
+       public static TBuilder RequireRateLimiting<TBuilder, TPartitionKey>(this TBuilder builder, IRateLimiterPolicy<TPartitionKey> policy) where TBuilder : IEndpointConventionBuilder
    }
```
## Add APIs to make it possible for users of RouteEndpointBuilder to use filters

**Approved** | [#aspnetcore/42592](https://github.com/dotnet/aspnetcore/issues/42592#issuecomment-1201550979)

API Review Notes:

- Yes. This does force an extra list allocation for every `EndpointBuilder` instance.
  - We do not care too much about this considering this is already the case for the `Metadata` list and is consistent.
- We seem generally happy with the shorter name and moving the property to the base class.

```diff
namespace Microsoft.AspNetCore.Builder;

public abstract class EndpointBuilder 
{
+    public IList<Func<EndpointFilterFactoryContext, EndpointFilterDelegate, EndpointFilterDelegate>> FilterFactories { get; }
}
```
## Expose entire EndpointBuilder to filter factories

**Approved** | [#aspnetcore/43000](https://github.com/dotnet/aspnetcore/issues/43000#issuecomment-1201566235)

API Review Notes:

- Exposing the entire `EndpointBuilder` to filters seems cleaner. It's less properties on `RequestDelegateFactoryOptions` and `EndpointFilterFactoryContext` and everything that is required on `EndpointBuilder`is required for filters.
- Will callers expect `EndpointBuilder.RequestDelegate` to be set upon completion?
  - Metadata is filled in upon completion, so why not the `RequestDelegate`?
  - We think we should set `EndpointBuilder.RequestDelegate` if RDF succeeds. At the very least if it is null going in.

API Approved!

```diff
namespace Microsoft.AspNetCore.Http;

public sealed class RequestDelegateFactoryOptions
{
-    public IReadOnlyList<Func<EndpointFilterFactoryContext, EndpointFilterDelegate, EndpointFilterDelegate>>? EndpointFilterFactories { get; init; }
-    public IList<object>? EndpointMetadata { get; init; }
+    public EndpointBuilder? EndpointBuilder { get; init; }
}

public sealed class EndpointFilterFactoryContext
{
-    public EndpointFilterFactoryContext(MethodInfo methodInfo, IList<object> endpointMetadata, IServiceProvider applicationServices);
+    public EndpointFilterFactoryContext(MethodInfo methodInfo, EndpointBuilder endopintBuilder);

-    public IList<object> EndpointMetadata { get; }
-    public IServiceProvider ApplicationServices { get; }
+    public EndpointBuilder EndpointBuilder { get; }
}
```
## Add alt-used to known headers

**NeedsWork** | [#aspnetcore/43013](https://github.com/dotnet/aspnetcore/issues/43013#issuecomment-1201570692)

API Review Notes:

- Given "The primary user is Kestrel string reuse." does this really need to be public API? Can we keep these internal yet still reuse them?
  - This reminds me of how we just mad a bunch of psudo-header names obsolete and decided not to add any more of them to `HeaderNames`.
  - If something like YARP actually needs to read this header, then I can see the point of making this public. I do want to keep `HeaderNames` limited to headers commonly used by application-level code.
## [Blazor] Enable cancellation of navigation events

**Approved** | [#aspnetcore/42877](https://github.com/dotnet/aspnetcore/issues/42877#issuecomment-1201722121)

> The possible API listed in the previous comment looks good to me. Does this need another formal API review before this issue can be `api-approved`?

That sounds good to me. We were just waiting on your feedback. API Approved!

```diff
namespace Microsoft.AspNetCore.Components;

public abstract class NavigationManager
{
+    public IDisposable RegisterLocationChangingHandler(Func<LocationChangingContext, ValueTask> locationChangingHandler);
+    protected ValueTask<bool> NotifyLocationChangingAsync(string uri, string? state, bool isNavigationIntercepted);
+    protected virtual void HandleException(Exception ex);
+    protected virtual void SetNavigationLockState(bool value);
}

namespace Microsoft.AspNetCore.Components.Routing;

+ public class LocationChangingContext
+ {
+     public string TargetLocation { get; }
+     public string? HistoryEntryState { get; }
+     public bool IsNavigationIntercepted { get; }
+     public CancellationToken CancellationToken { get; }
+     public void PreventNavigation();
+ }

+ public class NavigationLock : IComponent, IAsyncDisposable
+ {
+     public EventCallback<LocationChangingContext> OnBeforeInternalNavigation { get; set; }
+     public bool ConfirmExternalNavigation { get; set; }
+ }
```
