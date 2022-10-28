# API Review 10/27/2022

## Kestrel: Support for multiple transports, side-by-side

**Approved** | [#aspnetcore/44537](https://github.com/dotnet/aspnetcore/issues/44537#issuecomment-1294197033)

API Review Notes:

1. This seems useful considering the abstraction supports multiple Endpoint types many not implemented by the default socket transport
2. What if I want to have different transports listen on the same type of endpoint?
   - Can we have the endpoint pick the transport somehow?
   - Can we add an option to ListenOptions?
     - We could add a ListenOptions API later if this scenario is real. It's kinda hard to use. You don't always have a listen options callback.

> It targets netstandard2.0 and .NET Framework. The new DIM would only be available in .NET 8+. Is there precedence for doing this?

- We could `#ifdef` the API for .NET 8 and greater only.
- We could add a new a new interface like `ICanBindToEndpoint`.
  - `IConnectionListenerFactorySelector`?
- If the interface is missing, we assume the transport supports the endpoint.

```diff
namespace Microsoft.AspNetCore.Connections;

+ public interface IConnectionListenerFactorySelector
+ {
+     bool CanBind(EndPoint endpoint);
+ }
```
## Add API for Kestrel named pipes transport

**Approved** | [#aspnetcore/44612](https://github.com/dotnet/aspnetcore/issues/44612#issuecomment-1294216873)

API Review Notes:

1. Can we remove the Backlog option until we prove it's necessary?
   - Yes.
2. What about extending `IServiceCollection` instead of `IWebHost`?
   - Also seems fine.
   - `AddNamedPipes`? `AddNamedPipesTransport`? `AddKestrelNamedPipes`?
3. In the future, we could write something similar to `CreateBoundListenSocket` to allow per-endpoint acl config.

```diff
namespace Microsoft.AspNetCore.Connections.Features;

+ #if !NETSTANDARD
+ public interface IConnectionNamedPipeFeature
+ {
+    NamedPipeServerStream NamedPipe { get; }
+ }
+ #endif

+ namespace Microsoft.AspNetCore.Server.Kestrel.Transport.NamedPipes;

+ public sealed class NamedPipeTransportOptions
+ {
+    public long? MaxReadBufferSize { get; set; } = 1024 * 1024;
+    public long? MaxWriteBufferSize { get; set; } = 64 * 1024;

+    public bool CurrentUserOnly { get; set; } = true;
+    public PipeSecurity? PipeSecurity { get; set; }
+ }

namespace Microsoft.Extensions.DependencyInjection;

+ public static class ServiceCollectionNamedPipeExtensions
+ {
+    public static IServiceCollection AddNamedPipesTransport(this IServiceCollection services);
+    public static IServiceCollection AddNamedPipesTransport(this IServiceCollection services, Action<NamedPipeTransportOptions> configureOptions);
+ }

namespace Microsoft.AspNetCore.Server.Kestrel.Core;

public class KestrelServerOptions
{
+    public void ListenNamedPipe(string pipeName);
+    public void ListenNamedPipe(string pipeName, Action<ListenOptions> configure);
}
```


