# API Review 02/24/2025

## Support registering OpenApiOperationTransformer via extension method for Minimal APIs

**Approved** | [#aspnetcore/59180](https://github.com/dotnet/aspnetcore/issues/59180#issuecomment-2679300462)

* Should the existing WithOpenApi methods be marked as Obsolete? Not at this time.
* Looks good as proposed

```csharp
// Assembly: Microsoft.AspNetCore.OpenApi
namespace Microsoft.AspNetCore.Builder;

public static class OpenApiEndpointConventionBuilderExtensions
{
    public static TBuilder AddOpenApiOperationTransformer<TBuilder>(this TBuilder builder, Func<OpenApiOperation, OpenApiOperationTransformerContext, CancellationToken, Task> transformer) where TBuilder : IEndpointConventionBuilder { }
}
```
## Add support for emitting ServerSentEvents from minimal APIs

**Approved** | [#aspnetcore/56172](https://github.com/dotnet/aspnetcore/issues/56172#issuecomment-2679391759)


* `ServerSentEventResult` got pluralized to `ServerSentEventsResult`
* The `value` parameters all got pluralized to `values`
* Should "ServerSentEvents" be reduced to "Sse" to match "SseItem"?
  * It was felt that the scoping of this type does not lead well to "Sse" being known to be ServerSentEvents, so it's better to expand it here.
* Should ServerSentEventsResult also implement IStatusCodeHttpResult?  Yes
* Should ServerSentEventsResult also implement IValueHttpResult? No
* The overload accepting SseItems changed its signature.  Verify that does not cause an ambiguous invocation.
* There may be other overloads in the future taking JSON serialization information.
* The new methods on TypedResults should be mirrored to Results, per the Results types pattern.

```csharp
// Assembly: Microsoft.AspNetCore.Http.Results
namespace Microsoft.AspNetCore.Http;

public static partial class TypedResults 
{
    public static ServerSentEventsResult<string> ServerSentEvents(
        IAsyncEnumerable<string> values, 
        string? eventType = null);

    public static ServerSentEventsResult<T> ServerSentEvents<T>(
        IAsyncEnumerable<T> values, 
        string? eventType = null);

    public static ServerSentEventsResult<SseItem<T>> ServerSentEvents<T>(
        IAsyncEnumerable<SseItem<T>> values);
}

public static partial class Results 
{
    public static IResult ServerSentEvents(
        IAsyncEnumerable<string> values, 
        string? eventType = null);

    public static IResult ServerSentEvents<T>(
        IAsyncEnumerable<T> values, 
        string? eventType = null);

    public static IResult ServerSentEvents<T>(
        IAsyncEnumerable<SseItem<T>> values);
}

public sealed class ServerSentEventsResult<T> :
    IResult,
    IEndpointMetadataProvider,
    IStatusCodeHttpResult
 {
    static void IEndpointMetadataProvider.PopulateMetadata(MethodInfo method, EndpointBuilder builder);
    public Task ExecuteAsync(HttpContext httpContext);
    public int? StatusCode { get; }
 }
```
