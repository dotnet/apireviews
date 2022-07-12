# API Review 07/11/2022

## Add APIs to make it possible for users of RouteEndpointBuilder to use filters

**Approved** | [#aspnetcore/42592](https://github.com/dotnet/aspnetcore/issues/42592#issuecomment-1180685716)

API review notes:

- Do we need the setter either?
  - No

API Approved!

```diff
namespace Microsoft.AspNetCore.Routing;

public static class RouteEndpointBuilder 
{
+    public IList<Func<RouteHandlerContext, RouteHandlerFilterDelegate, RouteHandlerFilterDelegate>> RouteHandlerFilterFactories { get; }
}
```
## Additional Extensibility of `CreateHandler()` on `TestServer` 

**Approved** | [#aspnetcore/42567](https://github.com/dotnet/aspnetcore/issues/42567#issuecomment-1180715542)

API Review Notes:

- Awesome proposal. Simple and elegant.

If you want to submit a PR with a test @justindbaur, we'd welcome it.

It might be possible to use this to clean up some of our existing tests that previously had to use lower level HttpContext feature-based TestServer APIs. This doesn't have to be done as part of the initial change though.
