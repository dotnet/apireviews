# API Review 05/27/2022

## Simplify Authentication and Authorization configuration when using WebApplicationBuilder

**Approved** | [#aspnetcore/39855](https://github.com/dotnet/aspnetcore/issues/39855#issuecomment-1139979470)

API Review:

- `IAuthenticationConfigurationProvider` doesn't need a `Configuration` property.
- `GetSection` name can be more descriptive. `GetAuthenticationSchemeConfiguration` is clearer. This won't be called frequently or by most apps, so we can live with a longer name.


```diff
namespace Microsoft.AspNetCore.Builder;
 
class WebApplicationBuilder
{
+  public AuthenticationBuilder Authentication { get; }
}
 
namespace Microsoft.AspNetCore.Authentication;
 
+ public class IAuthenticationConfigurationProvider
+ {
+  public IConfiguration GetAuthenticationSchemeConfiguration(string authenticationScheme)
+ }

 
namespace Microsoft.Extensions.DependencyInjection;
 
public static class JwtBearerExtensions
{
+ public static AuthenticationBuilder AddJwtBearer(this AuthenticationBuilder builder, string authenticationScheme)
}
```
