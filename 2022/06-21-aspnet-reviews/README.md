# API Review 06/21/2022

## Add general property bag to cookies to support potential new cookie "standards:

**Approved** | [#aspnetcore/39968](https://github.com/dotnet/aspnetcore/issues/39968#issuecomment-1162037875)

API review notes:
- Should `CreateCookie` be renamed to `CreateCookieHeader`?
  - The name is a bit ambiguous, is it creating a new cookie header or a new cookie option?
  - Return type does make it more obvious that it's the header but we think it's better to not use the return type to disambiguate

```diff
namespace Microsoft.AspNetCore.Http;

public class CookieOptions
{
+    public CookieOptions(CookieOptions options); // Copy constructor, avoids manual copying when needing to change something.
+    public IList<string> Extensions { get; };
+    public SetCookieHeaderValue CreateCookieHeader(string name, string value); // Factory, avoids manual copying when creating the header
}

public class CookieBuilder
{
+    public IList<string> Extensions { get; };
}
```

API approved!
## Add generic method overloads to Microsoft.AspNetCore.Http.Results static class

**Approved** | [#aspnetcore/41724](https://github.com/dotnet/aspnetcore/issues/41724#issuecomment-1162050174)

API review notes:
- We are assuming all `object` accepting `Results` methods are having new APIs added here, lets double check when making the PR
- Lets make sure the `TypedResults` behavior matches

```diff
namespace Microsoft.AspNetCore.Http;

public static class Results
{
+    public static IResult Accepted<TValue>(string? uri = null, TValue? value = default);

+    public static IResult AcceptedAtRoute<TValue>(string? routeName = null, object? routeValues = null, TValue? value = default);

+    public static IResult BadRequest<TValue>(TValue? error);
+    public static IResult Conflict<TValue>(TValue? error);
+    public static IResult Created<TValue>(string uri, TValue? value);
+    public static IResult Created<TValue>(Uri uri, TValue? value);

+    public static IResult CreatedAtRoute<TValue>(string? routeName = null, object? routeValues = null, TValue? value = default);

+    public static IResult Json<TValue>(TValue value, JsonSerializerOptions? options = null, string? contentType = null, int? statusCode = null);
+    public static IResult NotFound<TValue>(TValue? value);
+    public static IResult Ok<TValue>(TValue? value);
+    public static IResult UnprocessableEntity<TValue>(TValue? error);
}
```

API approved!
## [Route Groups] Support AddFilter, WithOpenApi and other additive conventions

**Approved** | [#aspnetcore/41427](https://github.com/dotnet/aspnetcore/issues/41427#issuecomment-1162078486)

API review notes:
- `GetEndpointGroup` implies that an `EndpointGroup` type is being returned
- `GetGroupedEndpoints` has side-effects like running conventions and doing prefix pre-pending, can we come up with a name that suggests that?
  - No, not really without it being really ugly :D
- Let's just make sure the doc comment is really clear about what the method is supposed to be doing.

```diff
namespace Microsoft.AspNetCore.Http;

public abstract class EndpointDataSource
{
-    public virtual IReadOnlyList<Endpoint> GetEndpointGroup(RouteGroupContext context);
+    public virtual IReadOnlyList<Endpoint> GetGroupedEndpoints(RouteGroupContext context);
}
```

API approved!
## Introduce interfaces to describe IResult types

**NeedsWork** | [#aspnetcore/42187](https://github.com/dotnet/aspnetcore/issues/42187#issuecomment-1162128646)

API review notes:
- Does `public interface IValueHttpResult<TValue> : IValueHttpResult` need to be marked with `out` to be [covariant](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/out-generic-modifier)?
  - Probably, let's test it
  - How does a struct `T` work?
- Do we even need `public interface IValueHttpResult` now that all return types are generic?
  - No, let's remove it, checking for "does my result have an object?" can be done with checking for `IValueHttpResult<object>`
- Should the interface types inherit from other interfaces? (e.g. `public interface IFileHttpResult : IContentHttpResult`)
  - No, let's keep them separate
- `IMultiResult` is an odd name, it implies there are multiple results
  - `INestedResult` might be better, let's think about it

API needs update, @brunolins16 will update with the full diff once testing a couple things.
