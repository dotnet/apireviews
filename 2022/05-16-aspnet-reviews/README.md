# API Review 05/16/2022

## Allow Use of IConfigureOptions Consistently Everywhere

**Rejected** | [#aspnetcore/39251](https://github.com/dotnet/aspnetcore/issues/39251#issuecomment-1127955896)

API Review Notes:

> Options can be configured separately without the callback, but this is uncommon. We're worried that introducing this overload will create confusion only to save seven characters of code in the uncommon case.

We are worried that adding the non-options-configuring `AddStackExchangeRedisCache(this IServiceCollection services)` will create too much confusion by making people think configuring options isn't necessary. If options are configured seperately, calling `AddStackExchangeRedisCache(this IServiceCollection services, Action<RedisCacheOptions> configure)` with `_ => { }` is easy enough.
## RequestDecompression middleware

**Approved** | [#aspnetcore/40080](https://github.com/dotnet/aspnetcore/issues/40080#issuecomment-1127967089)

API Review:

We think this is filling a gap. People would be surprised if the `RequestSizeLimitAttribute` works but `DisableRequestSizeLimitAttribute` doesn't. Approved.

```diff
namespace Microsoft.AspNetCore.Http.Metadata;

public interface IRequestSizeLimitMetadata
{
-    public long MaxRequestBodySize { get; }
+    public long? MaxRequestBodySize { get; }
}

namespace Microsoft.AspNetCore.Mvc;

- public class DisableRequestSizeLimitAttribute : Attribute, IFilterFactory, IOrderedFilter
+ public class DisableRequestSizeLimitAttribute : Attribute, IFilterFactory, IOrderedFilter, IRequestSizeLimitMetadata
```

We should update our docs to make it clear that disabling request size limits can be a security concern, particularly if the request body is being buffered. @david-acker Do you mind updating the doc comments for `DisableRequestSizeLimitAttribute` with a remark to this effect in your PR when you add the `IRequestSizeLimitMetadata` implementation to `DisableRequestSizeLimitAttribute`?
