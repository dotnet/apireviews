# API Review 05/31/2022

## [Route Groups] WithTags et al. should target IEndpointConventionBuilder

**Approved** | [#aspnetcore/41428](https://github.com/dotnet/aspnetcore/issues/41428#issuecomment-1142635825)

API Review Notes:

- Do we need to make the `this` generic or could it be `IEndpointConventionBuilder`?
  - Returning `TBuilder` for better chaining is the reason for not just using `IEndpointConventionBuilder` directly.
- `app.MapGet("/", () => {}).Produces()` now compiles because every argument has a default value (`typeof(void)` for the `responseType` and 200 for status). Previously, one of either the status code or response type was required. I did this so you wouldn't need to manually pass in null or typeof(void) just to add a produced status code to a group. Is this a problem?
  - Yes this is a problem. What are the alternatives?
    a. Leave defaults for all parameters. `Produces(responseType = null, statusCode = 200, …`
    b. Leave default for `statusCode` but not `responseType`. `Produces(responseType, statusCode = 200”, …`
    c. No default for `responseType` or `statusCode`. `Produces(responseType, statusCode, …`
    d. Remove the `MapGroup`-supporting `TBuilder` overloads for `Produces`/`ProducesProblem`/`ProducesValidationProblem`/`Accepts` because people don’t need them on groups.
  - Option "d" wins the vote over option "a" because:
    -  A group of endpoints having the same produces or accepts metadata doesn't seem super common compared to tags for example.
    - Produces and accepts metadata can be added manually with `WithMetadata` if you really need to.
    - Something like a common set a `ProblemDetails` for a group of endpoints might be common enough, but we can always add the `Produces`/`ProducesProblem`/`ProducesValidationProblem`/`Accepts` overloads or some subset back if we want to. Removing overloads is harder.
    
Approved API:

```diff
namespace Microsoft.AspNetCore.Http;

public static class OpenApiRouteHandlerBuilderExtensions
{
     public static RouteHandlerBuilder ExcludeFromDescription(this RouteHandlerBuilder builder);
+    public static TBuilder ExcludeFromDescription<TBuilder>(this TBuilder builder) where TBuilder : IEndpointConventionBuilder;

    public static RouteHandlerBuilder WithTags(this RouteHandlerBuilder builder, params string[] tags);
+    public static TBuilder WithTags<TBuilder>(this TBuilder builder, params string[] tags) where TBuilder : IEndpointConventionBuilder;

-    public static RouteHandlerBuilder WithDescription(this RouteHandlerBuilder builder, string description);
+    public static TBuilder WithDescription<TBuilder>(this TBuilder builder, string description) where TBuilder : IEndpointConventionBuilder;

-    public static RouteHandlerBuilder WithSummary(this RouteHandlerBuilder builder, string summary);
+    public static TBuilder WithSummary<TBuilder>(this TBuilder builder, string summary) where TBuilder : IEndpointConventionBuilder
```
