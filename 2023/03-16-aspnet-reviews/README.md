# API Review 03/16/2023

## Blazor Sections API Proposal

**Approved** | [#aspnetcore/46937](https://github.com/dotnet/aspnetcore/issues/46937#issuecomment-1472404282)

Thanks for the API update. I agree that `SectionId="@("topbar")"` is ugly.

I marked both `SectionName` and `SectionId` as nullable. I imagine if the `SectionId` is unset, it can return the `SectionName` from the getter, but the same isn't true in reverse. And there's the transient state before either is set when both *could* be null. Let me know if this doesn't make sense.

I'm not sure if it's obvious that `SectionName` and `SectionId` are essentially equivalent, but no one has proposed better names. In the interest of unblocking the work for preview3, I'll mark this `api-approved` again ahead of our normal API review meeting.

API Approved!

```diff
namespace Microsoft.AspNetCore.Components.Sections;

+ public sealed class SectionOutlet : IComponent, IDisposable
+ {
+     [Parameter] public string? SectionName { get; set; }
+     [Parameter] public object? SectionId { get; set; }

+     // <inheritdoc/>
+     void Attach(RenderHandle renderHandle);

+     // <inheritdoc/>
+     Task SetParametersAsync(ParameterView parameters);

+     // <inheritdoc/>
+     public void Dispose();
+ }

+ public sealed class SectionContent : IComponent, IDisposable
+ {
+     [Parameter] public string? SectionName { get; set; }
+     [Parameter] public object? SectionId { get; set; }

+     [Parameter] public RenderFragment? ChildContent { get; set; }

+     // <inheritdoc/>
+     void Attach(RenderHandle renderHandle);

+     // <inheritdoc/>
+     Task SetParametersAsync(ParameterView parameters);

+     // <inheritdoc/>
+     public void Dispose();
+ }
```
## Add Extensions to Http.TypedResults

**Approved** | [#aspnetcore/47050](https://github.com/dotnet/aspnetcore/issues/47050#issuecomment-1472834993)

API Review Notes:

- Why did we add `Results.Extensions` in the first place? Mostly discoverability of custom `IResult` types.
- The workaround is just continue using `Results.Extensions` and return strong types from custom `IResult` types.
- Is it confusing to include an `Extensions` in both places? It might less confusing because it's the only member of `Results` that's not in `TypedResults`.
- Do we like the alternative design? It provides more flexibility. You could have one `IResult`-returning method better for multiple return points in a lambda, and one that's type. It could be confusing if extensions that showed up on one didn't on the other though. And it's a lot more ceremony for custom `IResult` authors.

API Approved as proposed!

```diff
namespace Microsoft.AspNetCore.Http;

public static class TypedResults
{
+    public static IResultExtensions Extensions { get; } = new ResultExtensions();
}
```
## HttpLoggingMiddleware could allow custom code to decide whether to log or not

**Approved** | [#aspnetcore/39200](https://github.com/dotnet/aspnetcore/issues/39200#issuecomment-1472864099)

API Review Notes:

- Would the `FieldSelector` be called at the beginning of the request? Yes.
  - Wouldn't this be too early to "log only failed requests"? Yes.
  - Are we okay with this? Sounds like yes. We could possibly add an option to delay logs and also the call to the `FieldSelector` until after the response is produced at a later date.
- Should we register a service rather than add a settable `Func` to options? This would make it a little easier to resolve services and maintain. It's possible to resolve services using the options pattern, so this is a matter of style.
- What does the example usage look like?

```csharp
builder.Services.AddHttpLogging(options =>
{
    //options.LoggingFields = FieldSelector.RequestPropertiesAndHeaders;

    options.FieldSelector = context =>
    {
          if (<some condition that means the request should not be logged>)
          {
               return FieldSelector.None;
          }

          return null;
    };
});
```

- How do we combine it with `HttpLoggingOptions.LoggingFields`? Does it override it always? Should we make the return type nullable if we want the default behavior? Let's do that.
- Do we want to support multiple `FieldSelector`s? We think manually chaining the funcs is sufficient for this.

API Approved with nullable return type!

```diff
namespace Microsoft.AspNetCore.HttpLogging
{
    public sealed class HttpLoggingOptions
    {
+       public Func<HttpContext, HttpLoggingFields?>? FieldSelector { get; set; } 
    }
}
```
## Make Http Logging Middleware Endpoint aware

**NeedsWork** | [#aspnetcore/43222](https://github.com/dotnet/aspnetcore/issues/43222#issuecomment-1472876130)

API Review Notes:

- If you add `[EnableLoggingAttribute]` to the method, what happens? Is everything enabled? The default set of logging fields specified by options?
- Why is `EnableLoggingAttribute.LoggingFields` get-only?
- How does this combine with the just-approved `HttpLoggingOptions.FieldSelector`? Does it override it?
  - When people use the attribute, do they always want to override? Or do they want a default that could still be disabled by the func which is potentially more flexible?
- Should we go back and change #39200 to make the field selector func and its return value non-nullable? Its initial value would be a func that reads the endpoint metadata and returns that or `options.LoggingFields`.

We can revisit this when we have answers to these questions and the people working on the feature are available for API review.

