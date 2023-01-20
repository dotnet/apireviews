# API Review 01/20/2023

## Add `Map` implementation with overload for `InferMetadata` and `CreateRequestDelegate`

**Approved** | [#aspnetcore/46163](https://github.com/dotnet/aspnetcore/issues/46163#issuecomment-1397770617)

API Review Notes:

- What about the assembly and namespace?
    - The assembly probably needs to be in Microsoft.AspNetCore.Routing.dll to reference `RouteHandlerBuilder`.
    - The most obvious namespaces are `Microsoft.AspNetCore.Builder` and `Microsoft.AspNetCore.Routing`. We don't need to use the `Builder` namespace if it's not an extension method. All of these are a default global usings in the Web SDK, so it's not a great place to "hide" code only called from code generators.
    - Let's go with the `Microsoft.AspNetCore.Routing` namespace and hide it with attributes.
- `[EditorBrowsable(EditorBrowsableState.Never)]`?
    - We think so. It's a signal to customers that they're not intended to use it directly. @davidfowl
- What about using context options so we can add parameters in the future without creating new overloads?
    - We could always add stuff to `RequestDelegateFactoryOptions`.
    - Any change would probably require new overloads to RDF.InferMetadat
    - Create.
    - We also plan to hide this as it's only intended to be called from generated code.
- Should it be an extension method?
    - No. We should hide it.
- Can the funcs be an interface? We don't want to introduce unnecessary types, and it's cleaner from the callsite to define two funcs rather than an interface implementation.
- What do we think about `GeneratedRouteEndpointExtensions` as a name?
    - "Extensions" doesn't make sense any more.
    - S.T.J uses "Services" for some things called only by the source generator. This is a little overloaded in ASP.NET Core, but if it's hidden it might be fine.
    - We have `RouteEndpointDataSource` already exists. A "RouteEndpoint" prefix might make sense. It's also related to RequestDelegateFactory.
    - Do we like "Generated" in the name? No.
    - `RouteEndpointBuildServices`?
    - `RouteEndpointGeneratorServices`?
    - `RouteHandlerServices`? No one seems too excited about this name or the others, but this was the last suggestion and we plan to hide it. We took some inspiration from [JsonMetadataServices](https://apisof.net/catalog/d6f26391-e5b8-9c7e-cd7a-85141c1e3f21).

API Approved!

```diff
// Microsoft.AspNetCore.Routing.dll

namespace Microsoft.AspNetCore.Routing;

+ [EditorBrowsable(EditorBrowsableState.Never)]
+ public static class RouteHandlerServices
+ {
+  public static Microsoft.AspNetCore.Builder.RouteHandlerBuilder Map(
+    Microsoft.AspNetCore.Routing.IEndpointRouteBuilder routes,
+    string pattern,
+    System.Delegate handler,
+    IEnumerable<string> httpMethods,
+    Func<MethodInfo, RequestDelegateFactoryOptions?, RequestDelegateMetadataResult> populateMetadata,
+    Func<Delegate, RequestDelegateFactoryOptions, RequestDelegateMetadataResult?, RequestDelegateResult> createRequestDelegate);
+ }
```
