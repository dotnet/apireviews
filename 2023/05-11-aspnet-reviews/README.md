# API Review 05/11/2023

## EnableKernelResponseBuffering

**Approved** | [#aspnetcore/47777](https://github.com/dotnet/aspnetcore/issues/47777#issuecomment-1544304751)

API Review Notes:

1. The per-request feature is additive and could be added later. It sounds like it could be useful, but no customer has specifically asked for it.
2. As to the question about whether we should enable kernel response buffering by default, @davidni made the following comment on the PR pointing out that there are real downsides to enabling it, so it seems safest not to
    > Side-note for posterity -- when I played with HTTP.sys perf on Asp .NET Core back in the day, one of our experiments was to NOT enable HTTP.sys buffering, and instead to do overlapped writes, ensuring there were always at least N outstanding writes. This helped improve perf in our scenarios (proxying large responses from a destination service -- before YARP), but (surprisingly at the time) we were never able to achieve the same throughput that http.sys kernel buffering afforded.

API Approved!

```diff
namespace Microsoft.AspNetCore.Server.HttpSys;

public class HttpSysOptions
{
+    public bool EnableKernelResponseBuffering { get; set; } 
}
```
## Make Http Logging Middleware Endpoint aware

**Approved** | [#aspnetcore/43222](https://github.com/dotnet/aspnetcore/issues/43222#issuecomment-1544770052)

API Review Notes:

- WCF uses a pattern where attributes use `Nullable<int>` properties and no corresponding constructor parameter. You cannot set the property to `null`, but you could add bool properties like `IsRequestBodyLogLimitSet`.
  - @Tratcher and @mconnew prefer the extra properties because it's more explicit than `-1`.
  - The get-only properties will not show up in intellisense when defining the attribute.
  - The `RequestBodyLogLimit` and `ResponseBodyLogLimit` getters should throw an `InvalidOperationException` if unset.
- @BrennanConroy and @halter73 are poisoned by working on `System.IO.Pipelines`, so we think `-1` for "unlimited" makes sense

API Approved with IsSet properties!

```diff
namespace Microsoft.AspNetCore.HttpLogging;

+ [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = false, Inherited = true)]
+ public sealed class HttpLoggingAttribute : Attribute
+ {
+     public HttpLoggingAttribute(HttpLoggingFields loggingFields) { }
+     public HttpLoggingFields LoggingFields { get; }

+     public bool IsRequestBodyLogLimitSet { get; }
+     public int RequestBodyLogLimit { get; set; }

+     public bool IsResponseBodyLogLimitSet { get; }
+     public int ResponseBodyLogLimit { get; set; }
+ }

namespace Microsoft.AspNetCore.Builder;

+ public static class HttpLoggingEndpointConventionBuilderExtensions
+ {
+    public static TBuilder WithHttpLogging<TBuilder>(this TBuilder builder, HttpLoggingFields loggingFields, int? requestBodyLogLimit = null, int? responseBodyLogLimit = null) where TBuilder : IEndpointConventionBuilder;
+ }
```
## Support all error's status codes ProblemDetail types for generating swagger docs in MinimalApi

**Rejected** | [#aspnetcore/47705](https://github.com/dotnet/aspnetcore/issues/47705#issuecomment-1544808236)

API Review Notes:

- We do not want to duplicate all of our non-200-returning `IResult` types and `TypedResult` methods to have a `ProblemDetails` variation.
  -  We already have special types for many status codes like `UnauthorizedHttpResult` for non-ProblemDetails returning APIs that return a 401. Same goes for many other status codes.
  - We think 400 for `ValidationProblem` is a special case, but we don't want to extend this pattern for every status code since it requires a new type and method per status code.
- Others error status codes, like `InternalProbelm`, `UnAuthorizedProblem`, `NotFoundProblem`... is not specific enough for approving the API, but even if they were all listed out, we'd be unlikely to accept the API.

We encourage using `TypedResults.Problem` with the `ProducesProblem` `IEndpointConventionBuilder` extension method either on the endpoint or a `RouteGroup` returned by `MapGroup`. For example:

```csharp
var hasUnauthorizedProblemsGroup = endpoints.MapGroup("");

hasUnauthorizedProblemsGroup.MapPost("/products",
    Results<ProblemHttpResult, Ok<ProductDto>>(ProductDto product) =>
    {
        if ( some condition )
           return TypedResults.Problem(statusCode: 401);
         
         return TypedResults.Ok(product);
    });

hasUnauthorizedProblemGroup.ProducesProblem(401);
```

You can also use `Produces` directly on the `MapPost` call:

```csharp
endpoints.MapPost("/products", ...).ProducesProblem(401);
```

Thanks for submitting the API proposal, but we have decided to reject it.

