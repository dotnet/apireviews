# API Review 07/06/2022

## Remove `Authentication` property from `WebApplicationBuilder`

**Approved** | [#aspnetcore/42577](https://github.com/dotnet/aspnetcore/issues/42577#issuecomment-1176532432)

API review notes:

1. It's less aesthetically pleasing to write `builder.Services.AddAuthentication()` instead of `builder.Authentication`, but it's not worth a top-level property on WAB at the moment. Maybe in the future.

```diff
namespace Microsoft.AspNetCore.Builder;

public sealed class WebApplicationBuilder
{
-  public AuthenticationBuilder Authentication { get; }
}
```

API Approved.
## Expose RoutePatternFactory.Combine

**Approved** | [#aspnetcore/42593](https://github.com/dotnet/aspnetcore/issues/42593#issuecomment-1176536655)

API review notes:

1. This is very nice to have when writing your own `GetGroupedEndpoints` implementation in an `EndpointDataSource`.
2. Do we think `left` should be nullable? Yes. It's convenient when called from `Endpoints`.

API Approved!

```
namespace Microsoft.AspNetCore.Routing.Patterns;

public static class RoutePatternFactory
{
+    public static RoutePattern Combine(RoutePattern? left, RoutePattern right);
}
```
## Support disabling auto-registering auth middlewares in WAB

**NeedsWork** | [#aspnetcore/42596](https://github.com/dotnet/aspnetcore/issues/42596#issuecomment-1176547352)

API Review Notes:

- Can this be a `WebHostDefaults` key that we read from config when constructing the WAB?
   - This feels more like something that is specific to how the application is written and not environmental.
- Do we need this?
  - If no endpoints require auth, is there really a problem?
  - It could break an app upgrading to 7 that used auth services but no middleware!?
  - This can be disabled with `IApplicationBuilder.Properties[SOME_UNDOCUMENTED_KEY]` if you really want.

## `CreatedResult` should accept null location

**Approved** | [#aspnetcore/39454](https://github.com/dotnet/aspnetcore/issues/39454#issuecomment-1176557059)

API Review:

- How do we feel about changing MVC's CreatedResult?
   - It could break some logic that previously relied on `Location` never being null, but we're okay with this. Make a breaking change announcement.
- We should remove the single-argument `TypedResults.Created(string uri)` and `TypedResults.Created(Uri uri)` for consistency with the `Created` methods in other classes.

API Approved.

```diff
namespace Microsoft.AspNetCore.Mvc
{
    public class CreatedResult : ObjectResult
    {
+        public CreatedResult()
    
-        public CreatedResult(string location, object? value)
+        public CreatedResult(string? location, object? value)

-        public CreatedResult(Uri location, object? value)
+        public CreatedResult(Uri? location, object? value)

-        public string Location { get; set; }
+        public string? Location { get; set; }
    }

    public abstract class ControllerBase
    {
+        public virtual CreatedResult Created()
    
-        public virtual CreatedResult Created(string uri, [ActionResultObjectValue] object? value)
+        public virtual CreatedResult Created(string? uri, [ActionResultObjectValue] object? value)

-        public virtual CreatedResult Created(Uri uri, [ActionResultObjectValue] object? value)
+        public virtual CreatedResult Created(Uri? uri, [ActionResultObjectValue] object? value)
    }
}

namespace Microsoft.AspNetCore.Http
{
    public static class Results
    {
+        public static IResult Created()
    
-        public static IResult Created(string uri, object? value)
+        public static IResult Created(string? uri, object? value)

-        public static IResult Created(Uri uri, object? value)
+        public static IResult Created(Uri? uri, object? value)

-        public static IResult Created<TValue>(string uri, TValue? value)
+        public static IResult Created<TValue>(string? uri, TValue? value)

-        public static IResult Created<TValue>(string uri, TValue? value)
+        public static IResult Created<TValue>(string? uri, TValue? value)
    }

    public static class TypedResults
    {
+        public static Created  Created()

-        public static Created Created(string uri)
-        public static Created Created(Uri uri)

-        public static Created<TValue> Created<TValue>(string uri, TValue? value)
+        public static Created<TValue> Created<TValue>(string? uri, TValue? value)

-        public static Created<TValue> Created<TValue>(Uri uri, TValue? value)
+        public static Created<TValue> Created<TValue>(Uri? uri, TValue? value)
    }
}
```
## WebTransport API

**Approved** | [#aspnetcore/42490](https://github.com/dotnet/aspnetcore/issues/42490#issuecomment-1176579308)

API Review Notes:

- Can `WebTransportStream` be a `ConnectionContext` to align with Kestrel and SignalR transports?
  - It seems like it lines up nicely
- The `CancellationToken` parameters should have default values.
- Should `public ValueTask<WebTransportStream> ...` be `public ValueTask<ConnectionContext>` or `public ValueTask<WebTransportConnectionContext>`?
  - You can use features if you really need to.
  - It is sad that you have to use the `IStreamAbortFeature` (or whatever it's really called).
  - We can have the derived type and back them with features.
- Can `IWebTransportSession` be a `MultiplexedConnectionContext`?
  - Maybe! But possibly as a follow up change.
- All the approved public types belong in the Microsoft.AspNetCore.Http.Features assembly.

API Approved!

```diff
namespace Microsoft.AspNetCore.Http.Features;

+ public interface IWebTransportSession
+ {
+   public long SessionId { get; }
+   public void Abort(int errorCode = (int)Http3ErrorCode.NoError);
+   public ValueTask<ConnectionContext?> AcceptStreamAsync(CancellationToken cancellationToken = default);
+   public ValueTask<ConnectionContext> OpenUnidirectionalStreamAsync(CancellationToken cancellationToken = default);
+ }

+ public interface IHttpWebTransportFeature
+ {
+   public bool IsWebTransportRequest { get; }
+   [RequiresPreviewFeatures("WebTransport is a preview feature")]
+   public ValueTask<IWebTransportSession> AcceptAsync(CancellationToken cancellationToken = default);
+ }
```
