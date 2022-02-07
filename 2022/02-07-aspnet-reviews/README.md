# API Review 02/07/2022

## Improve endpoint metadata debugging by overriding ToString

**Approved** | [#aspnetcore/39792](https://github.com/dotnet/aspnetcore/issues/39792#issuecomment-1031796143)

API review: API approved. We're happy to consider using ToString instead of alternate ways such as interface implementation. We do not guarantee that the returned string would be consistent across releases. We should try and make the strings appear similar without  dictating a serialization format. Legibility is the primary motive here.


## Add RouteValuesAddress .ToString ()

**Approved** | [#aspnetcore/39708](https://github.com/dotnet/aspnetcore/issues/39708#issuecomment-1031796466)

Approved. See https://github.com/dotnet/aspnetcore/issues/39792#issuecomment-1031796143
## Add EndpointMetadataCollection.GetRequiredMetadata

**Approved** | [#aspnetcore/39921](https://github.com/dotnet/aspnetcore/issues/39921#issuecomment-1031817237)

API review: we discussed moving the method to `Endpoint`, but didn't think it was necessary. API approved as proposed.
The exception message and type should follow the same pattern `GetRequiredFeature` / `GetRequiredService`

## Add support for implicit inference of `FromServices` for types that appear in DI

**Approved** | [#aspnetcore/39667](https://github.com/dotnet/aspnetcore/issues/39667#issuecomment-1031827904)

API review:

Approved including the new constructor (note that it's has a non-null 2nd parameter)

``` diff
namespace Microsoft.AspNetCore.Mvc.ApplicationModels;

public class InferParameterBindingInfoConvention : IActionModelConvention
{
+    public InferParameterBindingInfoConvention(
+       IModelMetadataProvider modelMetadataProvider,
+       IServiceProviderIsService serviceProviderIsService){}
}
```



* We want to change the option to `DisableImplicitFromServicesParameters` in ApiBehaviorOptions.
* We also want to update SignalR HubOptions to use the new name (there's a small difference from the current name).
* Since this is a breaking behavior change, this should follow SignalR's announcement and also file a breaking change announcement.
