# API Review 01/30/2023

## Introduce per-HealthCheck Delay and Period

**Approved** | [#aspnetcore/42645](https://github.com/dotnet/aspnetcore/issues/42645#issuecomment-1409120237)

API Review Notes:

- What about the new HealthCheckRegistration ctor? Why don't we take a factory instead? Let's hold off on the constructor since you can use existing ones and this is somewhat advanced.
- Should the new properties be nullable? Yes. Unlike `Timeout`, this falls back to the HealthCheckPublisherOptions when null.
- Do we need HealthStatus.Disabled? Maybe not. HealthStatus.Healthy might be sufficient to skip over things, and it's less likely to break existing code that does checks like `status != HealthStatus.Healthy`. We can keep https://github.com/dotnet/aspnetcore/issues/42939 open for now to track adding API for selectively enabling things.

```diff
namespace Microsoft.Extensions.Diagnostics.HealthChecks;

public sealed class HealthCheckRegistration
{
    public HealthCheckRegistration(string name, IHealthCheck instance, HealthStatus? failureStatus, IEnumerable<string>? tags);
    public HealthCheckRegistration(string name, IHealthCheck instance, HealthStatus? failureStatus, IEnumerable<string>? tags, TimeSpan? timeout);
    public HealthCheckRegistration(string name, Func<IServiceProvider, IHealthCheck> factory, HealthStatus? failureStatus, IEnumerable<string>? tags);
    public HealthCheckRegistration(string name, Func<IServiceProvider, IHealthCheck> factory, HealthStatus? failureStatus, IEnumerable<string>? tags, TimeSpan? timeout);

    public Func<IServiceProvider, IHealthCheck> Factory { get; set; }
    public HealthStatus FailureStatus { get; set; }
    public TimeSpan Timeout { get; set; }
    public string Name { get; set; }
    public ISet<string> Tags { get; }

+    public TimeSpan? Delay { get; set; }
+    public TimeSpan? Period { get; set; }
}
```

API Approved!
## Adding Json aot/trimmer-safe overloads to [Typed]Results

**Approved** | [#aspnetcore/46252](https://github.com/dotnet/aspnetcore/issues/46252#issuecomment-1409146653)

API Review Notes:

- Why is `JsonTypeInfo` not `JsonTypeInfo<TValue>` like it is for `WriteAsJsonAsync`. Let's stay consistent with `WriteAsJsonAsync` and `SerializeAsync` and include the generic parameter.

API Approved!

```diff
namespace Microsoft.AspNetCore.Http;

public static class TypedResults
{
     public static JsonHttpResult<TValue> Json<TValue>(TValue? data, JsonSerializerOptions? options = null, string? contentType = null, int? statusCode = null)
+    public static JsonHttpResult<TValue> Json<TValue>(TValue? data, JsonSerializerContext jsonContext, string? contentType = null, int? statusCode = null) {}
+    public static JsonHttpResult<TValue> Json<TValue>(TValue? data, JsonTypeInfo<TValue> jsonTypeInfo, string? contentType = null, int? statusCode = null)
}

public static class Results
{
     public static IResult Json<TValue>(TValue? data, JsonSerializerOptions? options = null, string? contentType = null, int? statusCode = null)
+    public static IResult Json<TValue>(TValue? data, JsonSerializerContext jsonSerializerContext, string? contentType = null, int? statusCode = null) {}
+    public static IResult Json<TValue>(TValue? data, JsonTypeInfo<TValue> jsonTypeInfo, string? contentType = null, int? statusCode = null){}

     public static IResult Json(object? data, JsonSerializerOptions? options = null, string? contentType = null, int? statusCode = null)

// EDIT: The commented API was originally approved. The line below with the Type type argument now replaces it.
// +    public static IResult Json(object? data, JsonSerializerContext jsonSerializerContext, string? contentType = null, int? statusCode = null){}
+    public static IResult Json(object? data, Type type, JsonSerializerContext jsonSerializerContext, string? contentType = null, int? statusCode = null){}
+    public static IResult Json(object? data, JsonTypeInfo jsonTypeInfo, string? contentType = null, int? statusCode = null){}
}
```
## Add NamedPipeTransportOptions.ListenerQueueCount property

**Approved** | [#aspnetcore/46268](https://github.com/dotnet/aspnetcore/issues/46268#issuecomment-1409153781)

API Review Notes

- This is similar to `SocketTransportOptions.IOQueueCount`. Do we have any better naming suggestions? `ListenerLoopCount`? `AcceptLoopCount`? No.
- Namespace looks good.

API Approved as proposed!

```diff
namespace Microsoft.AspNetCore.Server.Kestrel.Transport.NamedPipes;

public sealed class NamedPipeTransportOptions
{
+    public int ListenerQueueCount { get; set; } = Math.Min(Environment.ProcessorCount, 16);
}
```
## Analyzer to detect greater than 1 [FromBody] usage

**Approved** | [#aspnetcore/45382](https://github.com/dotnet/aspnetcore/issues/45382#issuecomment-1409159532)

API Review Notes:

- Usage category and error level makes sense.
- We think the message should be "At most one parameter can have a [FromBody] attribute."

API Approved!

```
Category: Usage
Level: Error
Message: At most one parameter can have a [FromBody] attribute.
```
## IsTfaEnabled

**Approved** | [#aspnetcore/45874](https://github.com/dotnet/aspnetcore/issues/45874#issuecomment-1409169100)

API Review Notes:

- Could this API be protected instead of public? Public usage is demonstrated in the example, so it looks like yes.
- Does this support more than two factors? Should it be called `IsMultiFactorEnabledAsync`? No. `TwoFactor` is already used in other APIs, so let's stick with that.
- Edit: I copied the wrong method name. We definitely want to suffix it with "Async".

API Approved!

```diff
namespace Microsoft.AspNetCore.Identity;

public class SignInManager<TUser> where TUser : class
{
-     private async Task<bool> IsTfaEnabled(TUser user) {}
+     public virtual async Task<bool> IsTwoFactorEnabledAsync(TUser user) {}
}
```
