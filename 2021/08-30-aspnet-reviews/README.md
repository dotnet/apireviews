# API Review 08/30/2021

## WithTags, ITagsMetadata, TagsAttribute

**Approved** | [#aspnetcore/35693](https://github.com/dotnet/aspnetcore/issues/35693#issuecomment-908549583)

### API review:

Let us make sure the doc comments explicitly state that these tags contribute to the OpenAPI / Swagger document (and are not to be confused with the HTML tags).
## Prevent `InvokeVoidAsync` Return Value Serialization

**Approved** | [#aspnetcore/35929](https://github.com/dotnet/aspnetcore/issues/35929#issuecomment-908555048)

**API review**: Let's move `IJSVoidResult` in to a non-root namespace `Microsoft.JSInterop.Infrastructure`. 
## Capture Endpoint and RouteValues on IStatusCodeReExecuteFeature

**Approved** | [#aspnetcore/35652](https://github.com/dotnet/aspnetcore/issues/35652#issuecomment-908556799)

**API review**: Make the change non-breaking by making these default interface methods.

```diff
public interface IStatusCodeReExecuteFeature
    {
+       /// <summary>
+       /// Gets the selected <see cref="Http.Endpoint"/> for the original request.
+       /// </summary>
+       public Endpoint? Endpoint { get; }

+       /// <summary>
+       /// Gets the <see cref="RouteValueDictionary"/> associated with the original request.
+       /// </summary>
+       public RouteValueDictionary? RouteValues { get; }
    }
```
## Add support for stating accepts/consumes metadata is required or not

**Approved** | [#aspnetcore/35849](https://github.com/dotnet/aspnetcore/issues/35849#issuecomment-908565613)

API review:

Let's change `IsRequired` -> `IsOptional` so it's false by default:

```diff
+    public static DelegateEndpointConventionBuilder Accepts(this DelegateEndpointConventionBuilder builder, Type requestType, bool isOptional, string contentType, params string[] additionalContentTypes);
  }
}

namespace Microsoft.AspNetCore.Mvc
{
    public class ConsumesAttribute : Attribute, ...
    {
        public ConsumesAttribute(string contentType, params string[] otherContentTypes);
        public ConsumesAttribute(Type requestType, string contentType, params string[] otherContentTypes);
    
+       public bool IsOptional { get; set; }
    }
}

namespace Microsoft.AspNetCore.Http.Metadata
{
    public interface IAcceptsMetadata
    {
       IReadOnlyList<string> ContentTypes { get; }
       Type? RequestType { get; }
+     bool IsOptional { get; }
    }
}
```

