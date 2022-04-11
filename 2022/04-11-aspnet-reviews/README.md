# API Review 04/11/2022

## Change EndpointMetadataContext/EndpointParameterMetadataContext classes to use constructors to set property values

**Approved** | [#aspnetcore/41117](https://github.com/dotnet/aspnetcore/issues/41117#issuecomment-1095399192)

Approved. Constructors seem like the best way to avoid uninitialized properties for context types.
## Results<T1, TN> union types should implement IEndpointMetadataProvider

**Approved** | [#aspnetcore/41097](https://github.com/dotnet/aspnetcore/issues/41097#issuecomment-1095389928)

API review:

This is filling a gap. Approved.
## Improve minimal api routing with grouping

**Approved** | [#aspnetcore/36007](https://github.com/dotnet/aspnetcore/issues/36007#issuecomment-1095375518)

API Review:

- Let's rework on `OfBuilder<BuilderType>(builder => ...)` so it doesn't require a callback
- Let's remove `IGroupEndpointDataSource` since that just there to support `OfBuilder`.

```diff
// Microsoft.AspNetCore.Routing.dll

namespace Microsoft.AspNetCore.Builder;

public static class EndpointRouteBuilderExtensions
{
+     public static GroupRouteBuilder MapGroup(this IEndpointRouteBuilder endpoints, RoutePattern pattern);
+     public static GroupRouteBuilder MapGroup(this IEndpointRouteBuilder endpoints, string pattern);
}

namespace Microsoft.AspNetCore.Routing;

+ public sealed class GroupRouteBuilder : IEndpointRouteBuilder, IEndpointConventionBuilder
+ {
+     public RoutePattern GroupPrefix { get; }
+
+     IServiceProvider IEndpointRouteBuilder.ServiceProvider { get; }
+     IApplicationBuilder IEndpointRouteBuilder.CreateApplicationBuilder();
+     ICollection<EndpointDataSource> IEndpointRouteBuilder.DataSources { get; }
+     void IEndpointConventionBuilder.Add(Action<EndpointBuilder> convention);
+     //public void OfBuilder<T>(Action<T> configureBuilder) where T : IEndpointConventionBuilder;
+ }

// Microsoft.AspNetCore.Http.Abstractions.dll
// namespace Microsoft.AspNetCore.Builder;
+ //public interface IGroupEndpointDataSource
+ //{
+ //   IEnumerable<IEndpointConventionBuilder> ConventionBuilders { get; }
+ //}
```

We do not expect methods to target `GroupRouteBuilder` but instead `IEndpointRouteBuilder`, `IEndpointConventionBuilder` or `IApplicationBuilder` directly, so hopefully it won't be too important to construct or wrap a `GroupRouteBuilder`. We might want to add an interface with a `GroupPrefix` that methods can as-cast to as a possible future addition:

```C#
// public interface IGroupPrefixProvider
// {
//    public RoutePattern GroupPrefix { get; }
//    public IList<object> GroupMetadata { get; }
// }
```

We're going to approve the uncommented APIs above for preview4.
