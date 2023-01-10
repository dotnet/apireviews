# API Review 01/09/2023

## enable configuring connection timeouts in HubConnectionBuilder

**Approved** | [#aspnetcore/44742](https://github.com/dotnet/aspnetcore/issues/44742#issuecomment-1376135226)

API Review Notes:

- This is particularly important to blazor which doesn't expose the connection. This could allow unwanted interference with the blazor app
- Do we need the C# client changes? No, but it's more consistent.
- Should we use a new option type? Or methods to set these properties? Let's use methods. It does not allow reading the setting back of the builder, but we don't think this is important. It can still be read off of the HubConnection.
- Is there more config we want to add the the builders that are currently on the HubConnection type? HandshakeTimeout? We can add HandshakeTimeout later.


### TS

```diff
export class HubConnectionBuilder
{
+    public withServerTimeout(milliseconds: number): HubConnectionBuilder;
+    public withKeepAliveInterval(milliseconds: number): HubConnectionBuilder;
}
```

Example Usage:

```js
Blazor.start({
    configureSignalR: function (builder) {
        builder.withServerTimeout(60000).withKeepAliveInterval(3000);
    };
});
```

### C#

```diff
namespace Microsoft.AspNetCore.SignalR.Client;

public static class HubConnectionBuilderHttpExtensions
{
+   public static IHubConnectionBuilder WithServerTimeout(this IHubConnectionBuilder hubConnectionBuilder, TimeSpan timeout);
+   public static IHubConnectionBuilder WithKeepAliveInterval(this IHubConnectionBuilder hubConnectionBuilder, TimeSpan interval);
```

### Java

We also approve the equivalent change to Java's HttpHubConnectionBuilder.

API Approved!
## Request timeouts middleware

**Approved** | [#aspnetcore/45732](https://github.com/dotnet/aspnetcore/issues/45732#issuecomment-1376168614)

API Review Notes:

- Is `AddRequestTimeouts(this IServiceCollection services)` necessary? Maybe not. Will it reset anything? No. It's idempotent.
- What happens if you set a policy that's already set? It probably throws. Or it overwrites. Let's make it consistent auth middleware.
- Do we want the policies to be enumerable? Can we make it a dictionary?
- Are seconds granular enough for request timeouts? Maybe not. TimeSpan doesn't work with attributes. Let's make it milliseconds.
- Let's leave the IConfigurationSection binding for a later API review, but we should consider it in the shape of our options type, but a straightforward bind is unlikely to work.
- Is this going to be a new project/assembly? No. Put it in Microsoft.AspNetCore.Http.
- DisableRequestTimeoutAttribute always wins no matter the order, then policy instance, then `RequestTimeoutAttribute`, then global default.


```diff
namespace Microsoft.Extensions.DependencyInjection;

+ public static class RequestTimeoutsIServiceCollectionExtensions
+ {
+     public static IServiceCollection AddRequestTimeouts(this IServiceCollection services) { }
+     public static IServiceCollection AddRequestTimeouts(this IServiceCollection services, Action<RequestTimeoutOptions> configure) { }
+ }

namespace Microsoft.AspNetCore.Builder;

+ public static class RequestTimeoutsIApplicationBuilderExtensions
+ {
+     public static IApplicationBuilder UseRequestTimeouts(this IApplicationBuilder builder) { }
+ }

+ public static class RequestTimeoutsIEndpointConventionBuilderExtensions
+ {
+     public static IEndpointConventionBuilder WithRequestTimeout(this IEndpointConventionBuilder builder, TimeSpan timeout)  { }
+     public static IEndpointConventionBuilder WithRequestTimeout(this IEndpointConventionBuilder builder, string policyName) { }
+     public static IEndpointConventionBuilder WithRequestTimeout(this IEndpointConventionBuilder builder, RequestTimeoutPolicy policy) { }
+     public static IEndpointConventionBuilder DisableRequestTimeout(this IEndpointConventionBuilder builder) { }
+ }

+ namespace Microsoft.AspNetCore.Http.Timeouts;

+ public sealed class RequestTimeoutOptions
+ {
+     // Applied to any request without a policy set. No value by default.
+     public TimeSpan? DefaultTimeout { get; set; }
+     public RequestTimeoutOptions AddPolicy(string policyName, TimeSpan timeout) { }
+     public RequestTimeoutOptions AddPolicy(string policyName, RequestTimeoutPolicy policy) { }
+     public IDictionary<string, RequestTimeoutPolicy> Policies { get; }
+ }

+ public sealed class RequestTimeoutPolicy
+ {
+     public TimeSpan? Timeout { get; }
+     public RequestTimeoutPolicy(TimeSpan? timeout) { }
+ }

+ // Overrides the global timeout, if any.
+ [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class)]
+ public sealed class RequestTimeoutAttribute : Attribute
+ {
+    public TimeSpan? Timeout { get; }
+    public string? PolicyName { get; }
+    public RequestTimeoutAttribute(int milliseconds) { }
+    public RequestTimeoutAttribute(string policyName) { }
+ }

+ // Disables all timeouts, even the global one.
+ [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class)]
+ public sealed class DisableRequestTimeoutAttribute : Attribute
+ {
+    public DisableRequestTimeoutAttribute() { }
+ }
```

API Approved!
