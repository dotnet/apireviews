# API Review 07/18/2023

## Move aspnetcore to leverage JsonWebToken and JsonWebTokenHandler

**Approved** | [#aspnetcore/49469](https://github.com/dotnet/aspnetcore/issues/49469#issuecomment-1640988259)

API Review Notes:

- There are no changes to Microsoft.AspNetCore.Authentication
- `SecurityTokenValidators` is the name of the existing `IList<ISecurityTokenValidator>`. This will now be disabled by default.
  - Can we make the disabled `IList` throw if it is modified for clearer errors?
    - Is it possible that old libraries use it? Wouldn't you want to know? But would it be hard to workaround.
    - We should make it throw.
  - Should we obsolete the old API?
    - It does still work, so we should obsolete it with a warning, not an error.
- There is no API change to `JwtBearerEvents` or other events, but some people do downcast `TokenValidatedContext.SecurityToken` to `JwtSecurityToken` while it will now be a `JsonWebToken` which does not implement, e.g. https://github.com/AzureAD/microsoft-identity-web/blob/07f896d427f52552c0fa5b18a463f1dc0c9dadce/src/Microsoft.Identity.Web/WebApiExtensions/MicrosoftIdentityWebApiAuthenticationBuilder.cs#L102. It will now be 
  - We would like it if there was an explicit conversion from `JwtSecurityToken` to `JsonWebToken`.
- What do we think about making `UseTokenHandlers` default to true?
  - Opting out should be possible to opt out by setting `UseTokenHandlers` via named options even if you didn't add the auth hander directly.
  - Defaulting to true potentially makes trimming easier, but if someone sets it to `true` manually, neither code path will be trimmed.
- Do we want a method that only allows opting out?
  - This would have a small benefit if someone manually resets UseTokenHandlers to true, but we don't think it's big because Newtonsoft will be removed from `JwtSecurityToken` logic.
  - Properties can be set from config which is nice, but anyone doing that won't get trimming.
  - @captainsafia assures that we don't bind to all of JwtBearerOptions automatically and it's a hand-rolled artisanal binding.
- Can we make the property false by default by calling it something else?
  - `UseSecurityTokenValidators`? Yes.
- Are we worried we will have to change the default back to the old API because it's too breaking?
  - We're not too worried, and we could change the default if necessary
- Do we need `MapInboundClaimsTokenHandler`? Can we continue to use `MapInboundClaims` and have it map to both? It's intended to do the same thing.
  - It could make trimming trickier, but hopefully not impossible.
 
API Approved!

```diff
namespace Microsoft.AspNetCore.Authentication.JwtBearer;

public class JwtBearerOptions : AuthenticationSchemeOptions
{
+   [Obsolete("SecurityTokenValidators is no longer used by default. Use TokenHandlers instead. To continue using SecurityTokenValidators, set UseSecurityTokenValidators to true."]
    public IList<ISecurityTokenValidator> SecurityTokenValidators { get; }

+   public IList<TokenHandler> TokenHandlers { get; }
+   public bool UseSecurityTokenValidators { get; set; }
}

namespace Microsoft.AspNetCore.Authentication.WsFederation;

public class WsFederationOptions : RemoteAuthenticationOptions
{
+   [Obsolete("SecurityTokenValidators is no longer used by default. Use TokenHandlers instead. To continue using SecurityTokenValidators, set UseSecurityTokenValidators to true."]
    public IList<ISecurityTokenValidator> SecurityTokenValidators { get; }

+   public IList<TokenHandler> TokenHandlers { get; }
+   public bool UseSecurityTokenValidators { get; set; }
}

namespace Microsoft.AspNetCore.Authentication.OpenIdConnect;

public class OpenIdConnectOptions : RemoteAuthenticationOptions
{
+   [Obsolete("SecurityTokenValidator is no longer used by default. Use TokenHandler instead. To continue using SecurityTokenValidators, set UseSecurityTokenValidator to true."]
    public ISecurityTokenValidator SecurityTokenValidator { get; set; }

+   public TokenHandler TokenHandler { get; set; }
+   public bool UseSecurityTokenValidator { get; set; }
}
```

This is not for ASP.NET Core, but in System.IdentityModel.Token.Jwt we'd like to see an explicit conversion.

```diff
namespace System.IdentityModel.Tokens.Jwt;

public class JwtSecurityToken : SecurityToken
{
+      public static explicit operator JsonWebToken(JwtSecurityToken token);
}
```
