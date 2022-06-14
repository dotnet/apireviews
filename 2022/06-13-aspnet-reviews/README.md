# API Review 06/13/2022

## Replace Single(connectionId) with Client(connectionId) and Caller

**Approved** | [#aspnetcore/41994](https://github.com/dotnet/aspnetcore/issues/41994#issuecomment-1154202973)

API Review Notes:

- Update docs!
  - Breaking change notes
  - [Existing docs](https://github.com/dotnet/AspNetCore.Docs/pull/25796)
- Verify that this is not binary breaking by first building a dll with a custom `IHubCallerClients` implementation against .NET 6 and then using the dlls with .NET 7 previews.

Approved assuming no binary break!

```diff
namespace Microsoft.AspNetCore.SignalR;

public interface IHubClients<T>
{
-   T Single(string connectionId) => throw new NotImplementedException();
}


public interface IHubClients : IHubClients<IClientProxy>
{
-   new ISingleClientProxy Single(string connectionId) => throw new NotImplementedException();
+   new ISingleClientProxy Client(string connectionId) => new NonInvokingSingleClientProxy(((IHubClients<IClientProxy>)this).//...
}

public interface IHubCallerClients : IHubCallerClients<IClientProxy>
{
-   new ISingleClientProxy Single(string connectionId) => throw new NotImplementedException();
+   new ISingleClientProxy Client(string connectionId) => new NonInvokingSingleClientProxy(((IHubCallerClients<IClientProxy>)this).//...
+   new ISingleClientProxy Caller => new NonInvokingSingleClientProxy(((IHubCallerClients<IClientProxy>)this).//...
}
```
## Add methods for registering both authentication and authorization middlewares/services

**NeedsWork** | [#aspnetcore/42047](https://github.com/dotnet/aspnetcore/issues/42047#issuecomment-1154221681)

API Review Notes:
- Do we need `AddAuthenticationAndAuthorization` given you can call `AddAuthentication` and `AddAuthorization` in either order? And that there are two different options objects to configure?
  - No
- What about `UseAuthenticationAndAuthorization`? Do we need it?
  - Yes. It doesn't make sense to use authz without authn.
- Can we come up with a better name?
  - UseAuthNAndAuthZ
  - UseAuth
  - UseAuthStar 😆 
  - UseAuths
- `UseAuth` wins. We think it implies both authn and authz.
- What assembly should `UseAuth` live in?
  - We can't find a good one

API would be approved if we can find a good assembly for it. Until then, it needs work.

```diff
namespace Microsoft.AspNetCore.Builder;

public static class AuthAppBuilderExtensions
{
+  public static IApplicationBuilder UseAuth(this IApplicationBuilder app);
}
```

## Consider adding Text overloads to support UTF8 text

**Approved** | [#aspnetcore/41919](https://github.com/dotnet/aspnetcore/issues/41919#issuecomment-1154208366)

API Review:

- Do we need Memory overloads too?
  - Not for now. Span is easy enough and we want to copy anyway.
- Do we want to add an overload to `Results.Stream()` supporting `leaveOpen:`?
  - Maybe. Not yet though.

Api approved!


```diff
namespace Microsoft.AspNetCore.Http;

public static class Results
{
+    public static IResult Text(ReadOnlySpan<byte> utf8Content, string? contentType = null, Encoding? contentEncoding = null, int? statusCode = null) 
}

public static class TypedResults
{
+    public static ContentHttpResult Text(ReadOnlySpan<byte> utf8Content, string? contentType = null, Encoding? contentEncoding = null, int? statusCode = null);
}
```
## Introduce top-level API on WebApplicationBuilder for configuring Authorization

**NeedsWork** | [#aspnetcore/42172](https://github.com/dotnet/aspnetcore/issues/42172#issuecomment-1154469145)

FYI @HaoK @Tratcher @davidfowl @captainsafia @jcjiang @blowdart 
## Make RateLimiterMiddleware endpoint-aware

**NeedsWork** | [#aspnetcore/41667](https://github.com/dotnet/aspnetcore/issues/41667#issuecomment-1154556045)

API Review Notes:

- We need to see some samples.
- This should be a diff so we can see what API is new vs which API exists.
- Is there a way to define a policy inline for the rate limiter?
  - Not yet. See https://github.com/dotnet/aspnetcore/issues/39840 for an auth example.
  - This can be done in a later API review if we feel this is worthwhile
- In `AddTokenBucketRateLimiter` and similar, the `string name` parameter should be a `string policyName` parameter.
  - All `string name` parameters should be `string policyName`.
- What about "global = false" what is that about?
    ```csharp
    services.AddLimiter("myRolePolicy", context =>
    {
        if (context.IsAdmin()) // Pretend this extension method is defined
        {
            return RateLimitPartion.CreateNoLimiter("admin");
        }
        else
        {
            return RateLimitPartion.CreateTokenBucketLimiter("pleb", //...
        }
    });

    services.AddLimiter("myUserNamePolicy", context =>
        RateLimitPartion.Create(httpContext.User.Identity.Name, //...
    ```
  - Non-global should be the only option. We don't think it's realistic for different policies to want to share RateLimitPartionKey.
- Do we want to be able to specify `OnRejected` per policy? We can already set `CustomRejectionStatusCode` with `IRateLimiterPolicy`.
  - Didn't get to this.
- Can `RateLimiterOptions.Limiter` be merged with the endpoint-specific stuff? If you don't want to use the endpoint-specific stuff, you just don't define policies or call `RequireRateLimiting`.
- Can I use a something on the HttpContext to select a policy name if the request is not hitting an endpoint?
  - Maybe a feature would let you set a `string? RateLimterPolicyName`. Who would set the feature considering middleware hasn't run yet? We had a similar issue with output caching cc @sebastienros 
  - Maybe we could have an `Func<HttpContext, string?> RateLimiterOptions.PolicyNameCallback`
