# API Review 11/22/2021

## Support SkipStatusCodePages on endpoints and authorized routes

**Approved** | [#aspnetcore/38573](https://github.com/dotnet/aspnetcore/issues/38573#issuecomment-975822858)

**API review**:

Approved. We can remove the `Enabled` property and make this a vanilla 

```diff
namespace Microsoft.AspNetCore.Http.Metadata;

+ public interface ISkipStatusCodePagesMetadata
+ {
+ }

namespace Microsoft.AspNetCore.Mvc;

- public class SkipStatusCodePagesAttribute : Attribute, IResourceFilter
+ public class SkipStatusCodePagesAttribute : Attribute, IResourceFilter, ISkipStatusCodePagesMetadata
{
}
```


