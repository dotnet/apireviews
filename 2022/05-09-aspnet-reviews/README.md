# API Review 05/09/2022

## Add CancellationToken overloads for reading multipart form sections

**Approved** | [#aspnetcore/41532](https://github.com/dotnet/aspnetcore/issues/41532#issuecomment-1121391331)

API Review Notes:

- ValueTask? Yes, for when the data is already buffered. Stream already varies between Task and ValueTask.
- FileMultipartSection doesn't have a `GetValueAsync`

Approved:

```diff
namespace Microsoft.AspNetCore.WebUtilities;

public class FormMultipartSection
{
+   public ValueTask<string> GetValueAsync(CancellationToken cancellationToken);
}

public static class MultipartSectionStreamExtensions
{
+   public static async ValueTask<string> ReadAsStringAsync(this MultipartSection section, CancellationToken cancellationToken);
}
```
## Add support for argument list surrogates in minimal APIs

**Approved** | [#aspnetcore/40712](https://github.com/dotnet/aspnetcore/issues/40712#issuecomment-1121430995)

API review notes:

- What should we target?
  - Parameters? For sure.
  - Types? Not for now. It might make it appear that nesting should work. It's not supported by other parameter attributes in MVC.
  - Properties? No nesting! 😞 
- Name?
  - Between keeping `[Parameters]` and `[AsParameters]`. The `[AsParameters]` feels more like the `[From...]` attributes.
  - `[AsParameters]` wins.
- Should the attribute be inheritable?
  - What does this even mean for parameters? Are parameter attributes inherited from the overridden method on the base type?
  - This seems pretty niche for minimal route handlers which are normally delegates or local methods. I don't think I've even seen someone use an overridden method as a route handler yet.
  - Starting out with `Inherited = false` seems simpler and safer. If we change our mind, I doubt it'd break many apps if we switch the attribute to be inheritable.
- Should `[FromServices]` be updated to support properties?
  - Yes, but be careful that this doesn't have weird MVC side effects.
- Are we happy with the `Microsoft.AspNetCore.Http` namespace? It's globally included by default.
  - Yes we're happy especially now that the attribute is `[AsParameters]` instead of `[Parameters]`.

API Approved!

```diff
namespace Microsoft.AspNetCore.Http;

+[AttributeUsage(AttributeTargets.Parameter, Inherited = false, AllowMultiple = false)]
+public sealed class AsParametersAttribute : Attribute
+{ }

namespace Microsoft.AspNetCore.Mvc;

-[AttributeUsage(AttributeTargets.Parameter, AllowMultiple = false, Inherited = true)]
+[AttributeUsage(AttributeTargets.Parameter | AttributeTargets.Property, AllowMultiple = false, Inherited = true)]
public class FromServicesAttribute : Attribute, IBindingSourceMetadata, IFromServiceMetadata
```
