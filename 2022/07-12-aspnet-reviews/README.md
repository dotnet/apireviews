# API Review 07/12/2022

## Rename filter extension methods and type name to be more agnostic

**Approved** | [#aspnetcore/42589](https://github.com/dotnet/aspnetcore/issues/42589#issuecomment-1181212978)

API review notes:

There were a few emails (from me included) that probably could have been GitHub comments, so I'm pasting them here.

- @DamianEdwards: "Can we make this name more descriptive? Perhaps `EndpointFilterFactoryContext`?"
  - @captainsafia: "Good point! EndpointFilterFactoryContext​ seems fine to me. Other thoughts?"
  - @halter73 (me): "I think we missed RouteHandlerInvocationContext (the base type for DefaultRouteHandlerInvocationContext). To align these with the just-proposed EndpointFilterFactoryContext name, I think these should be renamed to Endpoint*Filter*InvocationContext and DefaultEndpoint*Filter(InvocationContext respectively."
- @halter73 (me): "If we decided to properly apply “EndpointFilters” to SignalR Hub method invocations in the future, the word “endpoint” might be a little out of place, but I don’t have a better name. Maybe just “Filter”, but that’s too nondescript."
  - @davidfowl: "I thought about this too, but I think it might be out of place. I was looking at razor pages as well, but it might not work well in all cases. I do think we need to decide on how these apply before we come up with the final name. We know we can't change things after the fact."
  - @captainsafia: "Thoughts on going back to the unmarked "AddFilters"/"FilterDelegate"? On the plus side, it's the least specific and allows us to pick a more specific name at a later time. One the other hand, it's the least specific and is likely vague enough to confuse users. Then again, it's probably prudent to start with a more specific name for the API then expand to a more specific one at a later time then eventually deprecate the old names?"
  - @DamianEdwards: "I still vote for EndpointFilter given that Hubs are the exception (and still invoked with a separate request in the cases of non-WebSocket transports anyway so the mapping isn’t too egregious)."

We want this for preview7 that closes tomorrow, so we can rename `EndpointFilter` again in an RC if we really need to, but I agree with @DamianEdwards that's what we should go with for now.

API Approved!

```diff
namespace Microsoft.AspNetCore.Http;

- public static class RouteHandlerFilterExtensions
+ public static class EndpointFilterExtensions
{
-  public static TBuilder AddRouteHandlerFilter<TBuilder>(this TBuilder builder, IRouteHandlerFilter filter);
-  public static TBuilder AddRouteHandlerFilter<TBuilder, TFilterType>(this TBuilder builder);
-  public static RouteHandlerBuilder AddRouteHandlerFilter<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)] TFilterType>(this RouteHandlerBuilder builder);
-  public static RouteGroupBuilder AddRouteHandlerFilter<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)] TFilterType>(this RouteGroupBuilder builder);
-  public static TBuilder AddRouteHandlerFilter<TBuilder>(this TBuilder builder, Func<RouteHandlerInvocationContext, RouteHandlerFilterDelegate, ValueTask<object?>> routeHandlerFilter);
-  public static TBuilder AddRouteHandlerFilter<TBuilder>(this TBuilder builder, Func<RouteHandlerContext, RouteHandlerFilterDelegate, RouteHandlerFilterDelegate> filterFactory); 

+  public static TBuilder AddEndpointFilter<TBuilder>(this TBuilder builder, IEndpointFilter filter);
+  public static TBuilder AddEndpointFilter<TBuilder, TFilterType>(this TBuilder builder);
+  public static RouteHandlerBuilder AddEndpointFilter<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)] TFilterType>(this RouteHandlerBuilder builder);
+  public static RouteGroupBuilder AddEndpointFilter<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)] TFilterType>(this RouteGroupBuilder builder);
+  public static TBuilder AddEndpointFilter<TBuilder>(this TBuilder builder, Func<EndpointExecutionContext, EndpointFilterDelegate, ValueTask<object?>> endpointFilter);
+  public static TBuilder AddEndpointFilterFactory<TBuilder>(this TBuilder builder, Func<EndpointContext, EndpointFilterDelegate, EndpointFilterDelegate> filterFactory); 
}

- public interface IRouteHandlerFilter {}
+ public interface IEndpointFilter {}

- public delegate ValueTask<object?> RouteHandlerFilterDelegate(RouteHandlerInvocationContext context);
+ public delegate ValueTask<object?> EndpointFilterDelegate(EndpointExecutionContext context);

-  public class RouteHandlerContext {}
+  public class EndpointFilterFactoryContext {}

- public abstract class RouteHandlerInvocationContext {}
+  public class EndpointFilterInvocationContext {}

-  public class DefaultRouteHandlerInvocationContext {}
+  public class DefaultEndpointFilterInvocationContext {}
```

