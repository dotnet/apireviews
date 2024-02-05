# API Review 02/05/2024

## [Blazor] Public API changes to enable compression on Interactive components for Blazor Web

**Approved** | [#aspnetcore/53821](https://github.com/dotnet/aspnetcore/issues/53821#issuecomment-1927860025)

API Review Notes:

- Setting the `ConfigureWebsocketOptions` to null disables compression. ~This is the only way to disable compression.~
  - The default is non-null which is unusual. And it's not obvious that the nullness of the callback disables compression.
  - There is a second way. Setting `WebSocketAcceptContext.DangerousEnableCompression = false` in `ConfigureWebsocketOptions`. If you override the Func, you need to manually set it to true again.
  - Can `ConfigureWebsocketOptions` be an `Action` rather than a `Func`?
  - Does `ConfigureWebsocketOptions` need to return a `Task`?
- `WebSocket` should have a capital "S".
- `"'self'"` is the default value for `ContentSecurityFrameAncestorsPolicy` and maps to a `Content-Security-Policy: frame-ancestors {ContentSecurityFrameAncestorsPolicy};` header.
  - "frame-ansestors" is plural, so let's make the property match.
- We have "FrameAncestors" in the property name to indicate you can set other CSP headers manually for other things.
- Let's make `ConfigureWebSocketOptions` non-nullable and make `(httpContext, webSocketAcceptContext) => Task.CompletedTask` the default value.

API Approved!

```diff
+ namespace Microsoft.AspNetCore.Components.Server;

+ public class ServerComponentsEndpointOptions()
+ {
+  public Func<HttpContext, WebSocketAcceptContext, Task> ConfigureWebSocketOptions { get; set; }
+  public string? ContentSecurityFrameAncestorsPolicy { get; set; }
+  public bool DisableWebSocketCompression { get; set; }
+ }

+ name Microsoft.AspNetCore.Builder;

+ public static class ServerRazorComponentsEndpointConventionBuilderExtensions
+ {
+  RazorComponentsEndpointConventionBuilder AddInteractiveServerRenderMode(this RazorComponentsEndpointConventionBuilder builder, Action<ServerComponentsEndpointOptions>? callback = null);
+ }
```
