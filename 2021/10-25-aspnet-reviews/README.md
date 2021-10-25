# API Review 10/25/2021

## GetEndpointAttribute<T> in EndpointHttpContextExtensions.cs

**NeedsWork** | [#aspnetcore/37425](https://github.com/dotnet/aspnetcore/issues/37425#issuecomment-951163011)

API review:

👍🏽 all the points @JamesNK raises. It's a bit difficult to reason about what to do when `GetEndpoint()` returns null. A alternative we might consider is to bump the `GetMetadata` and `GetOrderedMetadata` to hang off of `Endpoint`:

```diff
+ public static class EndpointExtensions
+ {
+    public T? GetMetadata<T>(this Endpoint endpoint) where T : class
+    public IReadOnlyList<T> GetOrderedMetadata<T>(this Endpoint endpoint) where T : class
+ }
```
