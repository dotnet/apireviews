# API Review 08/08/2022

## Add client result support to Java client

**Approved** | [#aspnetcore/41777](https://github.com/dotnet/aspnetcore/issues/41777#issuecomment-1208490261)

API Review Notes:

- Can we make `callback` an `object` and just reflection it up?
  - Maybe, but the docs would be terrible.
- What about `HubConnection.results.on` or `HubConnection.clientResults.on`?
  - It's weird to expose a field in Java and we don't want to do `getResults().on()` lol.
- Why do we need async callbacks for things that do not return?
  - It might be nice, but it's not necessary. We can add later.

API Approved!

```diff
package com.microsoft.signalr;

+ public interface FunctionSingle<TResult> {
+     Single<TResult> invoke();
+ }
+ public interface Function1Single<T1, TResult> {
+     Single<TResult> invoke(T1 param1);
+ }
// Up to 8
+ Function8Single<T1, ..., T8, TResult>

class HubConnection {
+    public <TResult> Subscription onWithResult(String target, FunctionSingle<TResult> callback);

+    public <T1, TResult> Subscription onWithResult(String target, Function1Single<T1, TResult> callback, Class<T1> param1);
// Up to 8
+    public <T1, ..., T8, TResult> Subscription onWithResult(String target, FunctionSingle<T1, ..., T8, TResult> callback, Class<T1> param1, ..., Class<T8> param8);
}
## Problem uploading large files with Kestrel + Docker

**NeedsWork** | [#aspnetcore/6711](https://github.com/dotnet/aspnetcore/issues/6711#issuecomment-1208537157)

API Review Notes:

- Let's make this an AppContext switch so we don't have to worry about obsoleting it in .NET 8.

API Rejected!
## OpenAPI convention on route group cannot examine endpoint-specific metadata

**Approved** | [#aspnetcore/42750](https://github.com/dotnet/aspnetcore/issues/42750#issuecomment-1208525058)

API Review Notes:

- There seems to be some interest in option 4, "Finally".
  - Does it run before the `RouteEndpointBuilder` is built?
    - Yes.
    - Let's add a `RouteEndopintBuilder` parameter to make it clear that it's not built yet.
- Can we add the method to `IEndpointConventionBuilder` instead?
  - Yes, but is this too visible?
  - It's already annoying that `IEndpointConventionBuilder.Add` has the same visibility as extension methods. Adding, `Finally` to the list could be sad.
- Can we come up with a better name?
  - `OnBuild`?
    - Aren't all conventions called during build?
      - Not during `Build()`, but basically, yeah.
  - @captainsafia votes for `Finally` because "during build" is ambiguous.

API Approved!

```diff
namespace Microsoft.AspNetCore.Builder;
 
public interface IEndpointConventionBuilder
{
    void Add(Action<EndpointBuilder> convention);
+    void Finally(Action<EndpointBuilder> finallyConvention);
}

namespace Microsoft.AspNetCore.Routing;
 
public sealed class RouteGroupContext
{
-    public RouteGroupContext(RoutePattern prefix, IReadOnlyList<Action<EndpointBuilder>> conventions, IServiceProvider applicationServices);

-    public RoutePattern Prefix { get; }
+    public required RoutePattern Prefix { get; init; }
-    public IReadOnlyList<Action<EndpointBuilder>> Conventions { get; }
+    public IReadOnlyList<Action<EndpointBuilder>> Conventions { get; init; }
-    public IReadOnlyList<Action<EndpointBuilder>> FinallyConventions { get; }
+    public IReadOnlyList<Action<EndpointBuilder>> FinallyConventions { get; init; }
-    public IServiceProvider ApplicationServices { get; }
+    public IServiceProvider ApplicationServices { get; init; }
}
```

## [Blazor][Wasm] Dynamic and extensible authentication requests

**NeedsWork** | [#aspnetcore/42580](https://github.com/dotnet/aspnetcore/issues/42580#issuecomment-1208442915)

API Review Notes:

- What about the static factory methods on `InteractiveRequestOptions`?
  - Maybe add "Create" to the beginning of the method names to make it clear they are factories.
  - And add "Options" to the end?
- Let's used required init for required options and null otherwise.
- What about this crazy "Redirect" method on an exception?
  - Another overload already exists
- Does "Redirect" need a `Action<InteractiveRequestOptions>` or can it take `InteractiveRequestOptions` directly?
  - Type and ReturnUrl cannot change, so it's nice to prepopulate.
- Will `AccessTokenResult.RedirectUrl` be set with the new constructor?
  - Unsure.
  - Do we need to obsolete `RedirectUrl` and the old ctor?
- Just no to the disposing pattern.
- Do we need to make an internal version of `RemoteAuthenticationService` so we can add a ctor without public API?
  - Not this time. It's a lot of work, and we don't expect it to happen again.
- `InteractiveRequestOptions` parameters should be named `requestOptions` instead of `request`.

```diff
+public enum InteractionType
+{
+  SignIn = 0
+  GetToken = 1
+  SignOut = 2
+}

+ public sealed class InteractiveRequestOptions
+ {
+   public InteractiveRequestOptions();
+   public required InteractionType Interaction { get; init; }
+   public required string ReturnUrl { get; init; }
+   public System.Collections.Generic.IEnumerable<string> Scopes { get; init; }
+   public System.Collections.Generic.IDictionary<string, object> AdditionalRequestParameters { get; set; }
+ 
+   public static InteractiveRequestOptions CreateGetTokenOptions(string returnUrl);
+   public static InteractiveRequestOptions CreateGetTokenOptions(string returnUrl, System.Collections.Generic.IEnumerable<string> scopes);
+   public static InteractiveRequestOptions CreateSignInOptions(string returnUrl);
+   public static InteractiveRequestOptions CreateSignInOptions(string returnUrl, System.Collections.Generic.IEnumerable<string> scopes);
+   public static InteractiveRequestOptions CreateSignOutOptions(string returnUrl);
+ }

public class AccessTokenNotAvailableException
{
+  void Redirect(System.Action<InteractiveRequestOptions> requestOptions);
}

public class AccessTokenResult
{
+  public AccessTokenResult(AccessTokenResultStatus status, AccessToken token, string interactiveRequestUrl, InteractiveRequestOptions interactiveRequest);
+  public InteractiveRequestOptions InteractiveRequest { get; }
+  public string InteractiveRequestUrl { get; }
}

public class RemoteAuthenticationContext<TRemoteAuthenticationState>
{
+  public InteractiveRequestOptions InteractiveRequestOptions { get; set; }
}

public class RemoteAuthenticationService<TRemoteAuthenticationState, TAccount, TProviderOptions>
{
  // Added an additional constructor overload with a logger parameter
+  public RemoteAuthenticationService(
+    Microsoft.JSInterop.IJSRuntime jsRuntime,
+    Microsoft.Extensions.Options.IOptionsSnapshot<RemoteAuthenticationOptions<TProviderOptions>> options,
+    Microsoft.AspNetCore.Components.NavigationManager navigation, AccountClaimsPrincipalFactory<TAccount> accountClaimsPrincipalFactory,
+    Microsoft.Extensions.Logging.ILogger<RemoteAuthenticationService<TRemoteAuthenticationState, TAccount, TProviderOptions>> logger)
}

+public static class NavigationManagerExtensions()
+{
+  static void NavigateToLogin(this Microsoft.AspNetCore.Components.NavigationManager manager, string loginPath);
+  static void NavigateToLogin(this Microsoft.AspNetCore.Components.NavigationManager manager, string loginPath, InteractiveRequestOptions requestOptions);
+  static void NavigateToLogout(this Microsoft.AspNetCore.Components.NavigationManager manager, string logoutPath);
+  static void NavigateToLogout(this Microsoft.AspNetCore.Components.NavigationManager manager, string logoutPath, string returnUrl);
+}
```
## Add Output Caching middleware

**Approved** | [#aspnetcore/41955](https://github.com/dotnet/aspnetcore/issues/41955#issuecomment-1208656295)

I made two minor adjustments to this API after the meeting.

> Edit: Let's not do the `OutputCacheAttribute.VaryByQueryKeys` to `VaryByQuery` rename because then it's not plural, and `VaryByQueries` would be needlessly confusing.
> Edit 2: Rename `headers` param to `headerNames`.
