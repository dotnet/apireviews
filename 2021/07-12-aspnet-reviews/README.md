# API Review 07/12/2021

## [SignalR] Close connection on auth expiration

**Approved** | [#aspnetcore/32431](https://github.com/dotnet/aspnetcore/pull/32431)

## RequestDelegateFactory should take an optional set of route pattern names or route parameter names

**Approved** | [#aspnetcore/33700](https://github.com/dotnet/aspnetcore/issues/33700#issuecomment-878484429)

```diff
+ public class RequestDelegateFactoryOptions
+ {
+     public IServiceProvider? ServiceProvider { get; init; }
-     public IReadOnlyList<string>? RouteParameterNames { get; init; }
+    public IEnumerable<string>? RouteParameterNames { get; init; }
+ }
## Capture Endpoint and RouteValues  on ExceptionHandlerFeature

**Approved** | [#aspnetcore/34205](https://github.com/dotnet/aspnetcore/pull/34205)

