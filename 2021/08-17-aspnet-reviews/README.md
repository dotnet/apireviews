# API Review 08/17/2021

## Improve Minimal APIs support for request media types & body schema/shape

**Approved** | [#aspnetcore/35082](https://github.com/dotnet/aspnetcore/issues/35082#issuecomment-900639459)

```diff
 public static class OpenApiEndpointConventionBuilderExtensions
 {
+    public static MinimalActionEndpointConventionBuilder Accepts<TRequest>(
+        this MinimalActionEndpointConventionBuilder builder, 
+        string contentType, 
+        params string[] otherContentTypes);

+    public static MinimalActionEndpointConventionBuilder Accepts(
+        this MinimalActionEndpointConventionBuilder builder, 
+        Type requestType, 
+        string contentType, 
+        params string[] otherContentTypes);
 }
```


* Remove `init` property setters. 
* Remove `<returns />` doc comment for property

```diff
+   public sealed class RequestDelegateResult
+    {
+        /// <summary>
+        /// Creates a new instance of <see cref="RequestDelegateResult"/>.
+        /// </summary>
+       public RequestDelegateResult(RequestDelegate requestDelegate, IReadOnlyList<object> metadata);

+        /// <summary>
+        /// Gets the <see cref="RequestDelegate" />
+        /// </summary>
+        public RequestDelegate RequestDelegate { get; }

+        /// <summary>
+        /// Gets endpoint metadata inferred from creating the <see cref="RequestDelegate" />
+        /// </summary>
+        public IReadOnlyList<object> EndpointMetadata { get; }
+    }
```

* Move the extenstion methods in to MVC, add `ConsumesAttribute` instead.


```diff
- ConsumesAttribute : IApiRequestMetadataProvider
+ ConsumesAttribute : IApiRequestMetadataProvider, IAcceptMetadata
{
     ConsumesAttribute(string[] contentTypes);
+    ConsumesAttribute(Type requestType, string contentType, params string[] contentTypes);

+    Type IApiRequestMetadataProvider.RequestType { get; set; }
}
```

```diff
+namespace Microsoft.AspNetCore.Http.Metadata
+{
+    /// <summary>
+    /// Interface for accepting request media types.
+    /// </summary>
+    public interface IAcceptsMetadata
+    {
+        /// <summary>
+        /// Gets a list of the allowed request content types. If the incoming request does not have a <c>Content-Type</c> with one of these values, the request will be rejected with a 415 response.
+        /// </summary>
+        IReadOnlyList<string> ContentTypes { get; }

+        /// <summary>
+        /// Gets the type being read from the request. 
+        /// </summary>
+        Type? RequestType { get; }
+    }
+}
```

Make `AcceptsMetadata` non-public / `internal`.
