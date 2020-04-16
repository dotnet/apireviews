# Quick Reviews 4/16/2020

## Don't forget about JsonSerializerOptions.CreateForWeb()

**Approved** | [#runtime/34626](https://github.com/dotnet/runtime/issues/34626)

## OSPlatform support for WASM/Blazor

**Approved** | [#runtime/33328](https://github.com/dotnet/runtime/issues/33328#issuecomment-614802442)

* We considered expanding `Wasm` to `WebAssembly`, but the RIDs will use `wasm`, so it makes more sense to aling the names.
* Should `OSPlatform.IsOSPlatform(OSPlatform.Windows)` return `true` when running inside a browser on Windows. We concluded no, because that would likely break more people. Parties who care, need to use a more specific API that returns, for example, the user agent string.
* @marek-safar what are the plans for the RIDs? It would be logical to call the RID `browser-wasm`, akin to `win-x86`.

```C#
namespace System.Runtime.InteropServices
{
    public partial enum Architecture
    {
         Wasm
    }
    public partial struct OSPlatform
    {
         public static OSPlatform Browser { get; } = new OSPlatform(nameof(Browser));
    }
}
```
## Guarding calls to platform-specific APIs

**Needs Work** | [#runtime/33331](https://github.com/dotnet/runtime/issues/33331#issuecomment-614826740)

Some notes:

* Right now `OSPlatform` compares `Ordinal`, not `OrdinalIgnoreCase`. Unfortuantely, the field names don't match the strings passed to the constructor, which means `nameof(OSPlaform.Windows)` and `OSPlatform.Windows.ToString()` return different values. We should make the comparisons `OrdinalIgnoreCase`.
* We shouldn't add `macOS` and reuse `OSX`
* We should allow multiple attributes to be applied
* Maybe we should move `Url` to `OSPlatformVersionAttribute`?
## New API: `InitOnlyAttribute`

**Approved** | [#runtime/34978](https://github.com/dotnet/runtime/issues/34978#issuecomment-614845405)

* Seems like we should think about how reflections work with this feature
    - Reflection doesn't honor `modreq.
    - Should we have a reflection feature for init only? It would construct, init, and call the validator. `PropertyInfo.SetValue` could block setting init only.
* Serializers probably need to handle `init` setters (as in, they shoud be able to call them)
* ModReqs and ModOpts are usually plain marker types, not attributes. The reason this was made an attribute was to make reflection easier, but it seems we should have a reflection API anyway in which case it doesn't matter. We should be consistent with existing modifier types and not derive from attribute.
* We should prefix it with `Is` to be consistent with the other modifier types
* It was asked whether it only applies to properties and the answer is no, it might apply to fields and methods in the future, so the name should be generic

```C#
namespace System.Runtime.CompilerServices
{
    public sealed class IsInitOnly
    {
    }
}
```
