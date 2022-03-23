# API Review 03/21/2022

## Add additional overloads for RouteValueDictionary ctor

**Approved** | [#aspnetcore/40438](https://github.com/dotnet/aspnetcore/issues/40438#issuecomment-1074210703)

https://github.com/dotnet/aspnetcore/pull/40430
## SignalR SourceGenerator attribute API

**Approved** | [#aspnetcore/39973](https://github.com/dotnet/aspnetcore/issues/39973)

## Support returning values from client invocations

**Approved** | [#aspnetcore/5280](https://github.com/dotnet/aspnetcore/issues/5280#issuecomment-1074238170)

## Review Notes

- The suggestion of using `new` `Caller` and `Client` methods `ISingleClientProxy` would be source breaking with existing `IHubClients` and `IHubCallerClients` implementations, decorators etc... if someone replace the `IHubContext<>` in DI, so we'll stick with `Single`
- We should reach out to the ASRS team soon to verify they're happy with this design.

## Approved API

**Client side:**
```diff
class HubConnection
{
    public virtual IDisposable On(string methodName, Type[] parameterTypes, Func<object?[], object, Task> handler, object state)
+   public virtual IDisposable On(string methodName, Type[] parameterTypes, Func<object?[], object, Task<object?>> handler, object state);
}

public static partial class HubConnectionExtensions
{
    public static IDisposable On(this HubConnection hubConnection, string methodName, Type[] parameterTypes, Func<object?[], Task> handler);
    public static IDisposable On<T1>(this HubConnection hubConnection, string methodName, Func<T1, Task> handler);
+   public static IDisposable On<TResult>(this HubConnection hubConnection, string methodName, Type[] parameterTypes, Func<object?[], Task<T>> handler);
+   public static IDisposable On<TResult>(this HubConnection hubConnection, string methodName, Func<Task<T>> handler);
+   public static IDisposable On<TResult>(this HubConnection hubConnection, string methodName, Func<T> handler);
+   public static IDisposable On<T1, TResult>(this HubConnection hubConnection, string methodName, Func<T1, T> handler);
+   public static IDisposable On<T1, TResult>(this HubConnection hubConnection, string methodName, Func<T1, Task<T>> handler);
+   public static IDisposable On<T1, T2, TResult>(this HubConnection hubConnection, string methodName, Func<T1, T2, Task<T>> handler);
    // etc...
}
```

**TS client side:**
```diff
+ public on(methodName: string, newMethod: (...args: any[]) => any): void
public on(methodName: string, newMethod: (...args: any[]) => void): void {
    // ...
}
```

**Server side:**
```diff
namespace Microsoft.AspNetCore.SignalR;

+ public interface ISingleClientProxy : IClientProxy
+ {
+     Task<T> InvokeCoreAsync<T>(string method, object?[] args, CancellationToken cancellationToken = default) => throw new NotImplementedException();
+ }

public interface IHubClients<T>
{
+   T Single(string connectionId) => throw new NotImplementedException();
    T All { get; }
    T AllExcept(IReadOnlyList<string> excludedConnectionIds);
    T Client(string connectionId);
    T Clients(IReadOnlyList<string> connectionIds);
    T Group(string groupName);
    T Groups(IReadOnlyList<string> groupNames);
    T GroupExcept(string groupName, IReadOnlyList<string> excludedConnectionIds);
    T User(string userId);
    T Users(IReadOnlyList<string> userIds);
}

public interface IHubClients : IHubClients<IClientProxy>
{
+   new ISingleClientProxy Single(string connectionId) => throw new NotImplementedException();
}

public interface IHubCallerClients : IHubCallerClients<IClientProxy>
{
+   new ISingleClientProxy Single(string connectionId) => throw new NotImplementedException();
}

public static class ClientProxyExtensions
{
    public static Task SendAsync(this IClientProxy clientProxy, string method, CancellationToken cancellationToken = default);
    public static Task SendAsync(this IClientProxy clientProxy, string method, object? arg1, 
CancellationToken cancellationToken = default);
    // etc...

+   public static Task<T> InvokeAsync<T>(this ISingleClientProxy clientProxy, string method, CancellationToken cancellationToken = default);
+   public static Task<T> InvokeAsync<T>(this ISingleClientProxy clientProxy, string method, object? arg1, CancellationToken cancellationToken = default);
    // etc...
}

public abstract class HubLifetimeManager<THub> where THub : Hub
{
    public abstract Task SendAllAsync(string methodName, object?[] args, CancellationToken cancellationToken = default);
    public abstract Task SendConnectionAsync(string connectionId, string methodName, object?[] args, CancellationToken cancellationToken = default);

+   public virtual Task<T> InvokeConnectionAsync<T>(string connectionId, string methodName, object?[] args, CancellationToken cancellationToken = default) => throw new NotImplementedException();

+   public virtual Task SetConnectionResultAsync(string connectionId, CompletionMessage result) => throw new NotImplementedException();

+   public virtual bool TryGetReturnType(string invocationId, [NotNullWhen(true)] out Type? type)
+   {
+       type = null;
+       return false;
+   }
}

namespace Microsoft.AspNetCore.SignalR.Protocol;
+ public class RawResult
+ {
+     public RawResult(ReadOnlySequence<byte> rawBytes);
+     public ReadOnlySequence<byte> RawSerializedData { get; private set; }
+ }
```
## gRPC transcoding public API

**Approved** | [#aspnetcore/40792](https://github.com/dotnet/aspnetcore/issues/40792#issuecomment-1074516780)

Api review:

- Change `*Transcoding*` -> `*JsonTranscoding*` in all APIs, namespaces and package names.
- Can `TypeRegistry` go on the main options type?
- Can we expose `JsonSerializerOptions` on `JsonSettings`?
    - Not for now
- `JsonSettings` -> `GrpcJsonSettings`
- `WriteDefaultValues` -> `IgnoreDefaultValues` (or `SkipDefaultValues`)
- Make extension method on `IGrpcServerBuilder` instead of `IServiceCollection`

```diff
+ namespace Microsoft.AspNetCore.Grpc.JsonTranscoding;
+
+ public sealed class GrpcJsonTranscodingOptions
+ {
+     public GrpcJsonSettings JsonSettings { get; set; }
+     public Google.Protobuf.Reflection.TypeRegistry TypeRegistry { get; set; }
+ }
+ 
+ public sealed class GrpcJsonSettings
+ {
+     public bool WriteIndented { get; set; } = false;
+     public bool WriteEnumsAsIntegers { get; set; } = false;
+     public bool WriteInt64AsStrings { get; set; } = false;
+     public bool IgnoreDefaultValues { get; set; } = false;
+ }
+ 
+ public sealed class GrpcJsonTranscodingMetadata
+ {
+     public GrpcTranscodingMetadata(Google.Protobuf.Reflection.MethodDescriptor methodDescriptor, Google.Api.HttpRule httpRule);
+ 
+     public Google.Api.HttpRule HttpRule { get; }
+     public Google.Protobuf.Reflection.MethodDescriptor MethodDescriptor { get; }
+ }
+ 
+ namespace Microsoft.Extensions.DependencyInjection;
+
+ public static class GrpcTranscodingServiceExtensions
+ {
+     public static IGrpcServerBuilder AddJsonTranscoding(this IGrpcServerBuilder builder);
+     public static IGrpcServerBuilder AddJsonTranscoding(this IGrpcServerBuilder builder, Action<GrpcJsonTranscodingOptions> configureOptions);
+ }

```
