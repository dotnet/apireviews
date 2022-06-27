# API Review 06/27/2022

## Introduce interfaces to describe IResult types

**Approved** | [#aspnetcore/42187](https://github.com/dotnet/aspnetcore/issues/42187#issuecomment-1167668658)

API review notes:
- Should we keep `IValueHttpResult`?
  - Struct values don't work with covariance `result is IValueHttpResult<object?>` for example, so this allows generic code to look for any result value, struct or object
- Status code changing to non-nullable?
  - I think it makes sense, we don't want to apply the interface to a type if it might not have a status code
- Should the interfaces be in  Microsoft.AspNetCore.Http.HttpResults or  Microsoft.AspNetCore.Http?
  -  Microsoft.AspNetCore.Http is good, code with TypeResults will have the  Microsoft.AspNetCore.Http namespace already and so these will show up without introducing a new namespace.

```diff
namespace Microsoft.AspNetCore.Http
{
+ public interface IStatusCodeHttpResult
+ {
+     int StatusCode { get; }
+ }
 
+ public interface IValueHttpResult
+ {
+     object? RawValue { get; }
+ }
 
+ public interface IValueHttpResult<out TValue>
+ {
+     TValue? Value { get; }
+ }
 
+ public interface IContentTypeHttpResult
+ {
+     string? ContentType { get; }
+ }
 
+ public interface IFileHttpResult
+ {
+     string? ContentType { get; }
+     string? FileDownloadName { get; }
+ }
 
+ public interface INestedHttpResult
+ {
+     IResult Result { get; }
+ }
}

### Result Types

namespace Microsoft.AspNetCore.Http.HttpResults;
{

- public sealed class Accepted : IResult, IEndpointMetadataProvider
+ public sealed class Accepted : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult
 
- public sealed class AcceptedAtRoute : IResult, IEndpointMetadataProvider
+ public sealed class AcceptedAtRoute : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult
 
- public sealed class AcceptedAtRoute<TValue> : IResult, IEndpointMetadataProvider
+ public sealed class AcceptedAtRoute<TValue> : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult, IValueHttpResult, IValueHttpResult<TValue>
 
- public sealed class Accepted<TValue> : IResult, IEndpointMetadataProvider
+ public sealed class Accepted<TValue> : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult, IValueHttpResult, IValueHttpResult<TValue>
 
- public sealed class BadRequest : IResult, IEndpointMetadataProvider
+ public sealed class BadRequest : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult
 
- public sealed class BadRequest<TValue> : IResult, IEndpointMetadataProvider
+ public sealed class BadRequest<TValue> : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult, IValueHttpResult, IValueHttpResult<TValue>
 
- public sealed class Conflict : IResult, IEndpointMetadataProvider
+ public sealed class Conflict : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult
 
- public sealed class Conflict<TValue> : IResult, IEndpointMetadataProvider
+ public sealed class Conflict<TValue> : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult, IValueHttpResult, IValueHttpResult<TValue>
 
- public sealed partial class ContentHttpResult : IResult
+ public sealed partial class ContentHttpResult : IResult, IStatusCodeHttpResult, IContentTypeHttpResult
 
- public sealed class Created : IResult, IEndpointMetadataProvider
+ public sealed class Created : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult
 
- public sealed class CreatedAtRoute : IResult, IEndpointMetadataProvider
+ public sealed class CreatedAtRoute : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult
 
- public sealed class CreatedAtRoute<TValue> : IResult, IEndpointMetadataProvider
+ public sealed class CreatedAtRoute<TValue> : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult, IValueHttpResult, IValueHttpResult<TValue>
 
- public sealed class Created<TValue> : IResult, IEndpointMetadataProvider
+ public sealed class Created<TValue> : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult, IValueHttpResult, IValueHttpResult<TValue>
 
- public sealed partial class FileContentHttpResult : IResult
+ public sealed partial class FileContentHttpResult : IResult, IFileHttpResult, IContentTypeHttpResult
 
- public sealed class FileStreamHttpResult : IResult
+ public sealed class FileStreamHttpResult : IResult, IFileHttpResult, IContentTypeHttpResult
 
- public sealed partial class JsonHttpResult<TValue> : IResult
+ public sealed partial class JsonHttpResult<TValue> : IResult, IStatusCodeHttpResult, IValueHttpResult, IValueHttpResult<TValue>, IContentTypeHttpResult
 
- public class NoContent : IResult, IEndpointMetadataProvider
+ public class NoContent : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult
 
- public sealed class NotFound : IResult, IEndpointMetadataProvider
+ public sealed class NotFound : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult
 
- public sealed class NotFound<TValue> : IResult, IEndpointMetadataProvider
+ public sealed class NotFound<TValue> : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult, IValueHttpResult, IValueHttpResult<TValue>
 
- public sealed class Ok : IResult, IEndpointMetadataProvider
+ public sealed class Ok : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult
 
- public sealed class Ok<TValue> : IResult, IEndpointMetadataProvider
+ public sealed class Ok<TValue> : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult, IValueHttpResult, IValueHttpResult<TValue>
 
- public sealed partial class PhysicalFileHttpResult : IResult
+ public sealed partial class PhysicalFileHttpResult : IResult, IFileHttpResult, IContentTypeHttpResult
 
- public sealed class ProblemHttpResult : IResult
+ public sealed class ProblemHttpResult : IResult, IStatusCodeHttpResult, IContentTypeHttpResult, IValueHttpResult, IValueHttpResult<ProblemDetails>
{ 
-      public int? StatusCode { get; }
+      public int StatusCode { get; }
} 
 
- public sealed class PushStreamHttpResult : IResult
+ public sealed class PushStreamHttpResult : IResult, IFileHttpResult, IContentTypeHttpResult
 
- public sealed class Results<TResult1, TResultN> : IResult, IEndpointMetadataProvider
+ public sealed class Results<TResult1, TResultN> : IResult, INestedHttpResult, IEndpointMetadataProvider
 
- public sealed partial class StatusCodeHttpResult : IResult
+ public sealed partial class StatusCodeHttpResult : IResult, IStatusCodeHttpResult
 
- public sealed class UnauthorizedHttpResult : IResult
+ public sealed class UnauthorizedHttpResult : IResult, IStatusCodeHttpResult
 
- public sealed class UnprocessableEntity : IResult, IEndpointMetadataProvider
+ public sealed class UnprocessableEntity : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult
 
- public sealed class UnprocessableEntity<TValue> : IResult, IEndpointMetadataProvider
+ public sealed class UnprocessableEntity<TValue> : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult, IValueHttpResult, IValueHttpResult<TValue>
 
- public sealed class ValidationProblem : IResult, IEndpointMetadataProvider
+ public sealed class ValidationProblem : IResult, IEndpointMetadataProvider, IStatusCodeHttpResult, IContentTypeHttpResult, IValueHttpResult, IValueHttpResult<HttpValidationProblemDetails>
 
- public sealed class VirtualFileHttpResult : IResult
+ public sealed class VirtualFileHttpResult : IResult, IFileHttpResult, IContentTypeHttpResult
}
```

API Approved!
## Allow consistent `Problem Details` generation

**NeedsWork** | [#aspnetcore/42212](https://github.com/dotnet/aspnetcore/issues/42212#issuecomment-1167705108)

API review notes:
- Can we use a context for the WriteAsync methods? Avoid large number of default arguments :D
- Let's try to use `ValueTask` for `Task` returning methods
- Let's check the breaking-change-ness of adding a default value to the middleware ctors, or make a new public ctor with the additional arg and see if DI picks the longer one
- Rename `IProblemMetadata` and `IProblemTypes` to `IProblemDetailsMetadata` `IProblemDetailsTypes`
- Fix typo: `All = RoutingFailures | Exceptions | ClientErrors,`
- Is `IProblemDetailsService.IsEnabled(ProblemTypes type);` needed?
  - Don't think so, let's see what it's like without it
- Multiple writers can be registered and we'll iterate over them until one can write, let's make sure the doc comments are clear and if this is the pattern we want

@brunolins16 will update the proposal for either email or next review
## Move OpenAPI extension methods to M.A.Builder namespace

**Approved** | [#aspnetcore/42445](https://github.com/dotnet/aspnetcore/issues/42445#issuecomment-1167715052)

API review notes:
- Noticed two `OpenApiRouteHandlerBuilderExtensions` types, let's rename this one to something like `OpenApiEndpointConventionBuilderExtensions`

```diff
+ namespace Microsoft.AspNetCore.Builder;

- public static class OpenApiRouteHandlerBuilderExtensions
+ public static class OpenApiEndpointConventionBuilderExtensions
{
  ...
}
```

API approved!


## Allow configuring virtualization spacer element type

**Approved** | [#aspnetcore/42449](https://github.com/dotnet/aspnetcore/issues/42449#issuecomment-1167723214)

API review:
```diff
public class Virtualize<TItem>
{
+    [Parameter]
+    public string SpacerElement { get; set; } = "div";
}
```

API approved!
## Introduce WebApplicationBuilder.Authorization property that allows for AuthZ setup

**Approved** | [#aspnetcore/42235](https://github.com/dotnet/aspnetcore/issues/42235#issuecomment-1167741477)

API review notes:
- `public AuthorizationBuilder SetInvokeHandlersAfterFailure(bool invoke)`
  - Let's rename this to `InvokeHandlersAfterFailure`
- Not touching
```
public class WebApplicationBuilder
{
+    public AuthorizationBuilder Authorization { get; }
}
```
letting David and co. discuss this one.

```diff
namespace Microsoft.AspNetCore.Authorization;

public static class PolicyServiceCollectionExtensions
{
+   public static AuthorizationBuilder AddAuthorizationBuilder(this IServiceCollection services)
}

+public class AuthorizationBuilder
+{
+    AuthorizationBuilder(IServiceCollection services);
+    public virtual IServiceCollection { get; }
+    public virtual AuthorizationBuilder AddPolicy(
+        string name,
+        Action<AuthorizationPolicyBuilder> configurePolicy);
+    public virtual AuthorizationBuilder AddPolicy(
+        string name,
+        AuthorizationPolicy policy);
+    public virtual AuthorizationBuilder AddDefaultPolicy(
+        string name,
+        Action<AuthorizationPolicyBuilder> configurePolicy);
+    public virtual AuthorizationBuilder AddDefaultPolicy(
+        string name,
+        AuthorizationPolicy policy);
+    public virtual AuthorizationBuilder AddFallbackPolicy(
+        string name,
+        Action<AuthorizationPolicyBuilder> configurePolicy);
+    public virtual AuthorizationBuilder AddFallbackPolicy(
+        string name,
+        AuthorizationPolicy policy);
+    public virtual AuthorizationBuilder SetDefaultPolicy(string name);
+    public virtual AuthorizationBuilder SetFallbackPolicy(string name);
+    public virtual AuthorizationBuilder InvokeHandlersAfterFailure(bool invoke);
+}
```

API approved!
