# API Review 06/06/2022

## Add access to services for route handler filter factories to RouteHandlerContext

**Approved** | [#aspnetcore/41900](https://github.com/dotnet/aspnetcore/issues/41900#issuecomment-1147722210)

API Review Note:

- No further feedback. Approved!
## Add `IAuthenticationConfigurationProvider.GetAuthenticationConfiguration`

**Approved** | [#aspnetcore/41989](https://github.com/dotnet/aspnetcore/issues/41989#issuecomment-1147739810)

API Review Notes:

- Do we like properties instead of methods?
  - Yes.
- Should we suffix the property names with "Configure"?
  - No. It's in the interface name already. This only shows up if you resolve it from DI, so you should know what the type does.


```diff
public interface IAuthenticationConfigurationProvider
{
-    public IConfiguration GetAuthenticationConfiguration();
+    public IConfiguration Authentication { get; }
+    public IConfiguration AuthenticationSchemes { get; }
}
```
## Kestrel support for WebSockets over HTTP/2

**Approved** | [#aspnetcore/7801](https://github.com/dotnet/aspnetcore/issues/7801#issuecomment-1147754711)

API Review notes:

- Do we need another way to check if the request is using the "CONNECT" verb?
  - We only set this to true for "extended" CONNECT requests.
  - Let's rename to `IsExtendedConnect`.
- Should we rename the interface to `IHttpExtendedConnectFeature`.
  - Why not? It won't be referenced much directly by apps.
- When `IsExtendedConnect` is true, `Protocol` is not null. Let's use the attribute for that.

```diff
namespace Microsoft.AspNetCore.Http.Features;

+ public interface IHttpExtendedConnectFeature
+ {
+    bool IsExtendedConnect { get; }
+    [NotNullWhenYadaYada]
+    string? Protocol { get; }
+    ValueTask<Stream> AcceptAsync();
+ }
```
## Add HttpContext ot ITicketStore methods (CookieAuthenticationHandler)

**Approved** | [#aspnetcore/41908](https://github.com/dotnet/aspnetcore/issues/41908#issuecomment-1147747376)

API Review Notes:

- Can we do the AuthenticationTicket thing?
  - No. The ticket is stored in the cache, and we don't want to root the HttpContext.
- Should the new parameter come last?
  - Last before the CancellationToken to keep the parameter orders as consistent as possible.

Approved!

```diff
namespace Microsoft.AspNetCore.Authentication.Cookies;

public interface ITicketStore
{
    Task<string> StoreAsync(AuthenticationTicket ticket);
    Task<string> StoreAsync(AuthenticationTicket ticket, CancellationToken cancellationToken) => StoreAsync(ticket);
+    Task<string> StoreAsync(AuthenticationTicket ticket, HttpContext httpContext, CancellationToken cancellationToken) => StoreAsync(ticket, cancellationToken);
    Task RenewAsync(string key, AuthenticationTicket ticket);
    Task RenewAsync(string key, AuthenticationTicket ticket, CancellationToken cancellationToken) => RenewAsync(key, ticket);
+    Task RenewAsync(string key, AuthenticationTicket ticket, HttpContext httpContext, CancellationToken cancellationToken) => RenewAsync(key, ticket, cancellationToken);
    Task<AuthenticationTicket?> RetrieveAsync(string key);
    Task<AuthenticationTicket?> RetrieveAsync(string key, CancellationToken cancellationToken) => RetrieveAsync(key);
+    Task<AuthenticationTicket?> RetrieveAsync(string key, HttpContext httpContext, CancellationToken cancellationToken) => RetrieveAsync(key, cancellationToken);
    Task RemoveAsync(string key);
    Task RemoveAsync(string key, CancellationToken cancellationToken) => RemoveAsync(key);
+    Task RemoveAsync(string key, HttpContext httpContext, CancellationToken cancellationToken) => RemoveAsync(key, cancellationToken);
}
```
## Adding a new Enhanced Connect :protocol pseudo-header

**NeedsWork** | [#aspnetcore/42002](https://github.com/dotnet/aspnetcore/issues/42002#issuecomment-1147718108)

API Review Notes:

1. Can this be used by code outside of Kestrel?
  - No.
2. Can we make it internal?
  - Yes if we move the class and assembly for the `":protocol"` psuedo-header name.
3. Should we obsolete other pseudo header names?
  - Yes. Will need new API proposal.
  - At least make them non-browsable in the editor.
## Consider adding Text overloads to support UTF8 text

**NeedsWork** | [#aspnetcore/41919](https://github.com/dotnet/aspnetcore/issues/41919#issuecomment-1147761583)

If we're going to have to copy either way which it seems like we will because there's no way for the developer returning `Results.Text(...)` to know when they can reclaim pooled memory, maybe we should just take `ReadOnlySpan<byte>`.
