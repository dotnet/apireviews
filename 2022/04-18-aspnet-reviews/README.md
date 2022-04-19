# API Review 04/18/2022

## Introduce interface with static abstract BindAsync method for custom bound parameters of route handler delegates

**Approved** | [#aspnetcore/40927](https://github.com/dotnet/aspnetcore/issues/40927#issuecomment-1101622622)

API Review Notes:

- Let's use the Http.Abstractions namespace
- Add class constraint from PR

``` c#
namespaces Microsoft.AspNetCore.Http.Abstractions;

public interface IBindableFromHttpContext<TSelf>
    where TSelf : class, IBindableFromHttpContext<TSelf>
{
    static abstract ValueTask<TSelf?> BindAsync(HttpContext context, ParameterInfo parameter);
}
```
## Implement Rate Limiting Middleware in aspnetcore

**Approved** | [#aspnetcore/37384](https://github.com/dotnet/aspnetcore/issues/37384#issuecomment-1101615329)

API Review:

- In the future Limiter will be aggregated with default behavior
- Remove IServiceCollection extension method for now
- RateLimiting -> RateLimiter in method and class names

```C#
namespace Microsoft.AspNetCore.RateLimiting
{
    public sealed class RateLimiterOptions
    {
        public PartitionedRateLimiter<HttpContext> Limiter { get; set; }
        public Func<HttpContext, RateLimitLease, Task> OnRejected { get; set; }
        public int DefaultRejectionStatusCode { get; set; }
    }

    public static class RateLimitingApplicationBuilderExtensions
    {
        public static IApplicationBuilder UseRateLimiter(this IApplicationBuilder app);
        public static IApplicationBuilder UseRateLimiter(this IApplicationBuilder app, RateLimitingOptions options);
    }
}
```
## IContractResolver must be passed to ConversionResultProvider methods in JSON Patch library

**Approved** | [#aspnetcore/16690](https://github.com/dotnet/aspnetcore/issues/16690#issuecomment-1101629292)

API review:

Looks good as proposed.
## Introduce Results.Typed factory methods for creating results whose types properly reflect the response type & shape

**Approved** | [#aspnetcore/41009](https://github.com/dotnet/aspnetcore/issues/41009#issuecomment-1101648889)

API Review:

- It was nice using verbs vs nouns to determine if we should use the `HttpResult` suffix for the type, but using whether or not the type implements IEndpointMetadataProvider is easier.
- Like the new `HttpResults` subnamespace for all `IResult` implementations.
- Looks good as proposed. Will await community feedback to decide if any of the type names are too generic.

```diff
namespace Microsoft.AspNetCore.Http;

+public static class TypedResults
+{
+    public static ChallengeHttpResult Challenge(
+        AuthenticationProperties? properties = null,
+        IList<string>? authenticationSchemes = null) {}
       
+    public static ForbidHttpResult Forbid(
+        AuthenticationProperties? properties = null, 
+        IList<string>? authenticationSchemes = null) {}
       
+    public static SignInHttpResult SignIn(
+        ClaimsPrincipal principal,
+        AuthenticationProperties? properties = null,
+        string? authenticationScheme = null) {}
       
+    public static SignOutHttpResult SignOut(
+        AuthenticationProperties? properties = null, 
+        IList<string>? authenticationSchemes = null) {}
       
+    public static ContentHttpResult Content(
+        string content, 
+        string? contentType = null, 
+        Encoding? contentEncoding = null) {}
       
+    public static ContentHttpResult Text(
+        string content, 
+        string? contentType = null, 
+        Encoding? contentEncoding = null) {}
       
+    public static ContentHttpResult Content(
+        string content, 
+        MediaTypeHeaderValue contentType) {}
       
+    public static JsonHttpResult<TValue> Json<TValue>(
+        TValue? data, 
+        JsonSerializerOptions? options = null, 
+        string? contentType = null, 
+        int? statusCode = null)  {}
       
+    public static FileContentHttpResult File(
+        byte[] fileContents,
+        string? contentType = null,
+        string? fileDownloadName = null,
+        bool enableRangeProcessing = false,
+        DateTimeOffset? lastModified = null,
+        EntityTagHeaderValue? entityTag = null)  {}
       
+    public static FileContentHttpResult Bytes(
+        byte[] contents,
+        string? contentType = null,
+        string? fileDownloadName = null,
+        bool enableRangeProcessing = false,
+        DateTimeOffset? lastModified = null,
+        EntityTagHeaderValue? entityTag = null)
+        => new(contents, contentType)  {}
       
+    public static FileContentHttpResult Bytes(
+        ReadOnlyMemory<byte> contents,
+        string? contentType = null,
+        string? fileDownloadName = null,
+        bool enableRangeProcessing = false,
+        DateTimeOffset? lastModified = null,
+        EntityTagHeaderValue? entityTag = null) {}
       
+    public static FileStreamHttpResult File(
+        Stream fileStream,
+        string? contentType = null,
+        string? fileDownloadName = null,
+        DateTimeOffset? lastModified = null,
+        EntityTagHeaderValue? entityTag = null,
+        bool enableRangeProcessing = false)  {}
       
+    public static FileStreamHttpResult Stream(
+        Stream stream,
+        string? contentType = null,
+        string? fileDownloadName = null,
+        DateTimeOffset? lastModified = null,
+        EntityTagHeaderValue? entityTag = null,
+        bool enableRangeProcessing = false) {}
       
+    public static FileStreamHttpResult Stream(
+        PipeReader pipeReader,
+        string? contentType = null,
+        string? fileDownloadName = null,
+        DateTimeOffset? lastModified = null,
+        EntityTagHeaderValue? entityTag = null,
+        bool enableRangeProcessing = false) {}
       
+    public static PushStreamHttpResult Stream(
+        Func<Stream, Task> streamWriterCallback,
+        string? contentType = null,
+        string? fileDownloadName = null,
+        DateTimeOffset? lastModified = null,
+        EntityTagHeaderValue? entityTag = null)  {}
       
+    public static PhysicalFileHttpResult PhysicalFile(
+        string path,
+        string? contentType = null,
+        string? fileDownloadName = null,
+        DateTimeOffset? lastModified = null,
+        EntityTagHeaderValue? entityTag = null,
+        bool enableRangeProcessing = false)  {}
       
+    public static VirtualFileHttpResult VirtualFile(
+        string path,
+        string? contentType = null,
+        string? fileDownloadName = null,
+        DateTimeOffset? lastModified = null,
+        EntityTagHeaderValue? entityTag = null,
+        bool enableRangeProcessing = false) {}
       
+    public static RedirectHttpResult Redirect(
+        string url, 
+        bool permanent = false, 
+        bool preserveMethod = false) {}
       
+    public static RedirectHttpResult LocalRedirect(
+        string localUrl, 
+        bool permanent = false, 
+        bool preserveMethod = false) {}
       
+    public static RedirectToRouteHttpResult RedirectToRoute(
+        string? routeName = null, 
+        object? routeValues = null, 
+        bool permanent = false, 
+        bool preserveMethod = false, 
+        string? fragment = null) {}
       
+    public static StatusHttpResult StatusCode(int statusCode) {}
       
+    public static NotFound NotFound() {}
       
+    public static NotFound<TValue> NotFound<TValue>(TValue? value) {}
       
+    public static UnauthorizedHttpResult Unauthorized() {}
       
+    public static BadRequest BadRequest() {}
       
+    public static BadRequest<TValue> BadRequest<TValue>(TValue? error) {}
       
+    public static Conflict Conflict() {}
       
+    public static Conflict<TValue> Conflict<TValue>(TValue? error) {}
       
+    public static NoContent NoContent() {}
       
+    public static Ok Ok() {}
       
+    public static Ok<TValue> Ok<TValue>(TValue? value) {}
       
+    public static UnprocessableEntity UnprocessableEntity() {}
       
+    public static UnprocessableEntity<TValue> UnprocessableEntity<TValue>(TValue? error) {}
       
+    public static ProblemHttpResult Problem(
+        string? detail = null,
+        string? instance = null,
+        int? statusCode = null,
+        string? title = null,
+        string? type = null,
+        IDictionary<string, object?>? extensions = null) {}
       
+    public static ProblemHttpResult Problem(ProblemDetails problemDetails) {}
       
+    public static ValidationProblem ValidationProblem(
+        IDictionary<string, string[]> errors,
+        string? detail = null,
+        string? instance = null,
+        int? statusCode = null,
+        string? title = null,
+        string? type = null,
+        IDictionary<string, object?>? extensions = null) {}
              
+    public static Created Created(string uri) {}
       
+    public static Created<TValue> Created<TValue>(
+        string uri, 
+        TValue? value) {}       

+    public static Created Created(Uri uri) {}
       
+    public static Created<TValue> Created<TValue>(
+        Uri uri, 
+        TValue? value) {}
       
+    public static CreatedAtRoute CreatedAtRoute(
+        string? routeName = null, 
+        object? routeValues = null) {}
       
+    public static CreatedAtRoute<TValue> CreatedAtRoute<TValue>(
+        TValue? value, 
+        string? routeName = null, 
+        object? routeValues = null) {}
       
+    public static Accepted Accepted(string uri) {}
       
+    public static Accepted<TValue> Accepted<TValue>(
+        tring uri, 
+        TValue? value) {}
       
+    public static Accepted Accepted(Uri uri) {}
       
+    public static Accepted<TValue> Accepted<TValue>(
+        Uri uri, 
+        TValue? value) {}
       
+    public static AcceptedAtRoute AcceptedAtRoute(
+        string? routeName = null, 
+        object? routeValues = null) {}
       
+    public static AcceptedAtRoute<TValue> AcceptedAtRoute<TValue>(
+        TValue? value, 
+        string? routeName = null, 
+        object? routeValues = null) {}       
+
+    public static EmptyHttpResult Empty { get; }
+}

+namespace Microsoft.AspNetCore.Http.HttpResults;

-public sealed class AcceptedHttpResult : IResult
+public sealed class Accepted : IResult, IEndpointMetadataProvider
{
-    public object? Value { get; }
+    static void IEndpointMetadataProvider.PopulateMetadata(EndpointMetadataContext context)
}

-public sealed class AcceptedAtRouteHttpResult : IResult
+public sealed class AcceptedAtRoute : IResult, IEndpointMetadataProvider
{
-    public object? Value { get; }
+    static void IEndpointMetadataProvider.PopulateMetadata(EndpointMetadataContext context)
}

-public sealed class BadRequestObjectHttpResult : IResult
+public sealed class BadRequest : IResult, IEndpointMetadataProvider
{
-    public object? Value { get; }
+    static void IEndpointMetadataProvider.PopulateMetadata(EndpointMetadataContext context)
}

-public sealed class ConflictObjectHttpResult : IResult
+public sealed class Conflict : IResult, IEndpointMetadataProvider
{
-    public object? Value { get; }
+    static void IEndpointMetadataProvider.PopulateMetadata(EndpointMetadataContext context)
}

-public sealed class CreatedHttpResult : IResult
+public sealed class Created : IResult, IEndpointMetadataProvider
{
-    public object? Value { get; }
+    static void IEndpointMetadataProvider.PopulateMetadata(EndpointMetadataContext context)
}

-public sealed class CreatedAtRouteHttpResult : IResult
+public sealed class CreatedAtRoute : IResult, IEndpointMetadataProvider
{
-    public object? Value { get; }
+    static void IEndpointMetadataProvider.PopulateMetadata(EndpointMetadataContext context)
}

-public sealed class NoContentHttpResult : IResult
+public sealed class NoContent : IResult, IEndpointMetadataProvider {   }

-public sealed class NotFoundObjectHttpResult : IResult
+public sealed class NotFound : IResult, IEndpointMetadataProvider
{
-    public object? Value { get; }
+    static void IEndpointMetadataProvider.PopulateMetadata(EndpointMetadataContext context)
}

-public sealed class OkObjectHttpResult : IResult
+public sealed class Ok : IResult, IEndpointMetadataProvider
{
-    public object? Value { get; }
+    static void IEndpointMetadataProvider.PopulateMetadata(EndpointMetadataContext context)
}

-public sealed class UnprocessableEntityObjectHttpResult : IResult
+public sealed class UnprocessableEntity : IResult, IEndpointMetadataProvider
{
-    public object? Value { get; }
+    static void IEndpointMetadataProvider.PopulateMetadata(EndpointMetadataContext context)
}

+public sealed class ValidationProblem : IResult, IEndpointMetadataProvider
+{
+    public HttpValidationProblemDetails ProblemDetails { get; }
+    public string ContentType => "application/problem+json";
+    public int StatusCode => StatusCodes.Status400BadRequest;

+    public Task ExecuteAsync(HttpContext httpContext){}
+    public static void PopulateMetadata(EndpointMetadataContext context)
+}

+public sealed class AcceptedAtRoute<TValue> : IResult
+{
+    public TValue? Value { get; }
+    public string? RouteName { get; }
+    public RouteValueDictionary RouteValues { get; }
+    public int StatusCode => StatusCodes.Status202Accepted;

+    public Task ExecuteAsync(HttpContext httpContext) {}
+}

+public sealed class Accepted<TValue> : IResult
+{
+    public TValue? Value { get; }
+    public int StatusCode => StatusCodes.Status202Accepted;
+    public string? Location { get; }

+    public Task ExecuteAsync(HttpContext httpContext) {}
+}

+public sealed class BadRequest<TValue> : IResult
+{
+    public TValue? Value { get; }
+    public int StatusCode => StatusCodes.Status400BadRequest;
+
+    public Task ExecuteAsync(HttpContext httpContext){}
+}

+public sealed class Conflict<TValue> : IResult
+{
+    public TValue? Value { get; }
+    public int StatusCode => StatusCodes.Status409Conflict;
+
+    public Task ExecuteAsync(HttpContext httpContext){}
+}

+public sealed class CreatedAtRoute<TValue> : IResult
+{
+    public TValue? Value { get; }
+    public string? RouteName { get; }
+    public RouteValueDictionary RouteValues { get; }
+    public int StatusCode => StatusCodes.Status201Created;

+    public Task ExecuteAsync(HttpContext httpContext) {}
+}

+public sealed class Created<TValue> : IResult
+{
+    public TValue? Value { get; }
+    public int StatusCode => StatusCodes.Status201Created;
+    public string? Location { get; }

+    public Task ExecuteAsync(HttpContext httpContext) {}
+}

-public sealed partial class JsonHttpResult : IResult
+public sealed partial class Json<TValue> : IResult
{
-    public object Value { get; }
+    public TValue? Value { get; }
}

+public sealed class Ok<TValue> : IResult
+{
+    public TValue? Value { get; }
+    public int StatusCode => StatusCodes.Status200OK;
+
+    public Task ExecuteAsync(HttpContext httpContext){}
+}

+public sealed class UnprocessableEntity<TValue> : IResult
+{
+    public TValue? Value { get; }
+    public int StatusCode => StatusCodes.Status422UnprocessableEntity;
+
+    public Task ExecuteAsync(HttpContext httpContext){}
+}
```
