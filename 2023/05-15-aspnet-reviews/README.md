# API Review 05/15/2023

## Make ProblemDetailsDefaults public

**NeedsWork** | [#aspnetcore/47978](https://github.com/dotnet/aspnetcore/issues/47978#issuecomment-1548279804)

API Review Notes:

- We still don't want to expose the type and title as a `ValueTuple`.
- You could just copy the dictionary yourself into your code.
- Would anyone other than @Tornhoof and @campbellwray use this? It's not clear that they would. Might be nice if you were serializing to something other than the HttpResponse using the writer though.
- Would this lock us into supporting something we want to change in the future? We don't think so, but you never know.

API needs more interest to warrant the public API change. We were the closest to approving the instance method without the dictionary, but we decided against approving any new API at this time.


## API Proposal RoutingStateProvider; RouteData breaking change

**Approved** | [#aspnetcore/47854](https://github.com/dotnet/aspnetcore/issues/47854#issuecomment-1548291482)

API Review Notes for `RootComponentMetadata` (we already approved the rest of the proposal):

- This helps distinguish between multiple invocations of `MapRazorComponents<TRootComponent>()` where `RootComponentMetata.Type` is `typeof(TRootComponent)`.
- Typical users would read it via `httpContext.GetEndpoint().Metadata.Get<RootComponentMetata>()`.
- What do we think about the `Microsoft.AspNetCore.Builder` namespace?
  - It seems to niche to be in a default global using namespace.
  - It does match `ComponentTypeMetadata`, but that's also new in .NET 8 and can be moved.
  - `Microsoft.AspNetCore.Components.Endpoints`. It's a new dll, but the namespace has already been used for some stuff: https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.components.endpoints?view=aspnetcore-8.0

API Approved! Make sure we update the namespace for both `RootComponentMetadata` and `RootComponentMetadata`.

```diff
- namespace Microsoft.AspNetCore.Builder;
+ namespace Microsoft.AspNetCore.Components.Endpoints;

public class ComponentTypeMetadata
{
    public ComponentTypeMetadata(Type componentType);
    public Type Type { get; }
}

+  public class RootComponentMetadata
+  {
+      public RootComponentMetadata(Type rootComponentType);
+      public Type Type { get; }
+  }
```
## Setting route order imperatively

**Approved** | [#aspnetcore/47579](https://github.com/dotnet/aspnetcore/issues/47579#issuecomment-1548351250)

API Review Notes:

- Is lower or higher order negative priority? Negative.
  - This isn't intuitive, but it's consistent with the property.
- Will this apply to groups of endpoints?
  - Of course, it targets  `IEndpointConventionBuilder`
  - Mvc has an attribute for this, could this be overridden by the group?
    - We don't think so, but we should definitely add tests to ensure that the more-local MVC attributes win over the group.
    - We should also verify that calling `WithOrder` on the `ControllerActionEndpointConventionBuilder` doesn't override attributes.
    - In the group case, we want the attribute to be preferred (so we will use `IEndpointConventionBuilder.Add` instead of `IEndpointConventionBuilder.Finally`.
		```csharp
		var group = app.MapGroup("/order").WithOrder(-1);
		group.MapControllers();
		```
    - In the `ControllerActionEndpointConventionBuilder`, we want the `WithOrder` to be preferred. This is different from groups. We assume this would be the case for `.Add(static builder => ((RouteEndpointBuilder)builder).Order = -1)`
		```csharp
		app.MapControllers().WithOrder(-1);
		```
- What do we do in `WithOrder` if `EndpointBuilder` is not a `RouteEndpointBuilder`?
  - Do we skip them or throw an `InvalidOperationException`?
  - Would it be problematic if some `InertEndpointBuilder`s from `MapDynamicControllerRoute` or other types are in the mix?
  - How would you troubleshoot if this does nothing?
  - We are going to throw InvalidOperationException for now, and see how bad it is. It's less breaking to skip later.
- Do we want to define a new interface for `IEndpointConventionBuilder` implementations implement to support order so things like `MapControllers` can choose not to opt-in?
  - Don't we want to be able to use this `MapController`?
- Does this API give you enough control on override behavior in groups? What if I wanted the order to be changed in a `IEndpointConventionBuilder.Finally` callback?
  - Do we add a new `bool` parameter called `overrideOrder` or something?
- What if we extended `EndpointBuilder` instead of `IEndpointConventionBuilder`, so you still wouldn't have to cast, but you would control weather you call `Add` or `Finally?
  - Ex: `app.Map("/api/v1/users", emptyDelegate).Add(static builder => builder.WithOrder(-1))`
  - This really hurts discoverability.
- Does `RoutingEndpointConventionBuilderExtensions` make sense?
  - It includes `WithHost`, `WithName` and other similar methods. Seems reasonable.

API Approved. Let's make sure we throw for non-`RouteEndpointBuilder` early on.

```diff
namespace Microsoft.AspNetCore.Builder;

public static class RoutingEndpointConventionBuilderExtensions
{
+    public static TBuilder WithOrder<TBuilder>(this TBuilder builder, int order) where TBuilder : IEndpointConventionBuilder;
}
```
