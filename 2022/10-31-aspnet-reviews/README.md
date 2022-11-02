# API Review 10/31/2022

## AuthZ: Add IAuthorizationRequirementData

**Approved** | [#aspnetcore/44551](https://github.com/dotnet/aspnetcore/issues/44551#issuecomment-1297451848)

API Review Notes:

1. The motivation is that writing code to add new auth requirements to an endpoint (e.g. min age) requires writing a lot of code today in many classes.
2. Discussed how the `IAuthorizationRequirement` marker interface works. All existing logic.
3. Are we okay with logic in attributes? Yes. It's super convenient.
4. Method vs property. We fee that a method better matches expectations. We don't know much about what the implementation could be. It's probably usually cached by our built-in policy providers, so we don't really require this is inexpensive.

API Approved!

```diff
namespace Microsoft.AspNetCore.Authorization;

+ public interface IAuthorizationRequirementData
+ {
+    IEnumerable<IAuthorizationRequirement> GetRequirements();
+ }
```
