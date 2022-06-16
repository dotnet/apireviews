# API Review 06/16/2022

## Add Output Caching middleware

**NeedsWork** | [#aspnetcore/41955](https://github.com/dotnet/aspnetcore/issues/41955#issuecomment-1157099975)

@sebastienros Can you explain `OutputCachePolicyBuilder.AllowLocking()` and `OutputCacheContext.AllowLocking` a little bit more? It looks like setting this to false disables serving from the response cache. Is that it? Will most users of this API understand that?

Also, if the default to true, wouldn't we want `AllowLocking()` to change the default? So shouldn't it be `AllowLocking(bool lockResponse = false)` instead of `AllowLocking(bool lockResponse = true)`?
## [Route Groups] Support AddFilter, WithOpenApi and other additive conventions

**Approved** | [#aspnetcore/41427](https://github.com/dotnet/aspnetcore/issues/41427#issuecomment-1158177166)

API Review Notes:

- Demo time!
- `GetGroupedEndpoints` name?
  - What about `GetEndpointGroup`?
  - I think I prefer this.
- What about `AddRouteHandlerFilter`?
  - Is it too long?
  - We don't think `AddRouteHandlerFilter` could ever work with other kinds of endpoints.
- Is everyone okay with the `RouteHandlerBuiler` and `RouteGroupBuilder` overloads for the typed `AddRouteHandlerFilter` method?
  - I hear no objections.
- What about `WithOpenApi`?
  - Is it too generic?
  - I did make it work if *anything* adds an `OpenApiOperation` to the metadata.

```diff
namespace Microsoft.AspNetCore.Http;

public sealed class RequestDelegateFactoryOptions
{
-    public IEnumerable<object>? InitialEndpointMetadata { get; init; }
+    public IList<object>? EndpointMetadata { get; init; }
}

namespace Microsoft.AspNetCore.Routing;

public sealed class RouteGroupBuilder : IEndpointRouteBuilder, IEndpointConventionBuilder
{
-    // It was too easy for this to get out of sync with RouteGroupContext.Prefix in real life given RouteGroupBuilder decorators.
-    public RoutePattern GroupPrefix { get; }
}

+public sealed class RouteGroupContext
+{
+    public RouteGroupContext(RoutePattern prefix, IReadOnlyList<Action<EndpointBuilder>> conventions, IServiceProvider applicationServices);
+
+    public RoutePattern Prefix { get; }
+    public IReadOnlyList<Action<EndpointBuilder>> Conventions { get; }
+    public IServiceProvider ApplicationServices { get; }
+}

public abstract class EndpointDataSource
{
+    public virtual IReadOnlyList<Endpoint> GetEndpointGroup(RouteGroupContext context);
}

-public sealed class CompositeEndpointDataSource : EndpointDataSource
+public sealed class CompositeEndpointDataSource : EndpointDataSource, IDisposable
{
+    public override IReadOnlyList<Endpoint> GetEndpointGroup(RouteGroupContext context);
+    public void Dispose();
}

public static class RouteHandlerFilterExtensions
{
-    public static RouteHandlerBuilder AddFilter(this RouteHandlerBuilder builder, IRouteHandlerFilter filter);
+    public static TBuilder AddRouteHandlerFilter<TBuilder>(this TBuilder builder, IRouteHandlerFilter filter) where TBuilder : IEndpointConventionBuilder;

-    public static RouteHandlerBuilder AddFilter<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)] TFilterType>(this RouteHandlerBuilder builder)
-        where TFilterType : IRouteHandlerFilter;
+    public static RouteHandlerBuilder AddRouteHandlerFilter<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)] TFilterType>(this RouteHandlerBuilder builder)
+        where TFilterType : IRouteHandlerFilter;

+    public static TBuilder AddRouteHandlerFilter<TBuilder, [DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)] TFilterType>(this TBuilder builder)
+        where TBuilder : IEndpointConventionBuilder
+        where TFilterType : IRouteHandlerFilter;

+    public static RouteGroupBuilder AddRouteHandlerFilter<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)] TFilterType>(this RouteGroupBuilder builder)
+        where TFilterType : IRouteHandlerFilter;

-    public static RouteHandlerBuilder AddFilter(this RouteHandlerBuilder builder, Func<RouteHandlerInvocationContext, RouteHandlerFilterDelegate, ValueTask<object?>> routeHandlerFilter);
+    public static TBuilder AddRouteHandlerFilter<TBuilder>(this TBuilder builder, Func<RouteHandlerInvocationContext, RouteHandlerFilterDelegate, ValueTask<object?>> routeHandlerFilter)
+        where TBuilder : IEndpointConventionBuilder;

-    public static RouteHandlerBuilder AddFilter(this RouteHandlerBuilder builder, Func<RouteHandlerContext, RouteHandlerFilterDelegate, RouteHandlerFilterDelegate> filterFactory);
+    public static TBuilder AddRouteHandlerFilter<TBuilder>(this TBuilder builder, Func<RouteHandlerContext, RouteHandlerFilterDelegate, RouteHandlerFilterDelegate> filterFactory)
+        where TBuilder : IEndpointConventionBuilder;
}

namespace Microsoft.AspNetCore.OpenApi;

public static class OpenApiRouteHandlerBuilderExtensions
{
-    public static RouteHandlerBuilder WithOpenApi(this RouteHandlerBuilder builder);
+    public static TBuilder WithOpenApi<TBuilder>(this TBuilder builder) where TBuilder : IEndpointConventionBuilder;

-    public static RouteHandlerBuilder WithOpenApi(this RouteHandlerBuilder builder, Func<OpenApiOperation, OpenApiOperation> configureOperation);
+    public static TBuilder WithOpenApi<TBuilder>(this TBuilder builder, Func<OpenApiOperation, OpenApiOperation> configureOperation);
}
```

API Approved!
