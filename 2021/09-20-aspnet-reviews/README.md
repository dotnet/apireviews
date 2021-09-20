# API Review 09/20/2021

## Add MapPatch overloads to routing

**Approved** | [#aspnetcore/36198](https://github.com/dotnet/aspnetcore/issues/36198#issuecomment-923142748)

API approved. We discussed adding `MapHead` along with this, but it typically appears in a place where `GET` and `HEAD` need to be supported together. `MapMethods` is what we would recommend in that case. We should also add the RequestDelegate overload as part of this change:

```diff
namespace Microsoft.AspNetCore.Builder
{
    public static class DelegateEndpointRouteBuilderExtensions
    {
+       public static DelegateEndpointConventionBuilder MapPatch(this IEndpointRouteBuilder endpoints, string pattern, Delegate handler);
    }
}

namespace Microsoft.AspNetCore.Builder
{
    public static class EndpointRouteBuilderExtensions
    {
+        public static IEndpointConventionBuilder MapPatch(this IEndpointRouteBuilder endpoints, string pattern, RequestDelegate requestDelegate)
    }
}
```
  
