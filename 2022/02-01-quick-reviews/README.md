# API Review 02/01/2022

## Nullable.RefValue, RefValueOrDefault

**Approved** | [#runtime/1534](https://github.com/dotnet/runtime/issues/1534#issuecomment-1027214141) | [Video](https://www.youtube.com/watch?v=hE7KsqWPFlE&t=0h20m8s)

* This API might open a pit of failure where the compiler made a copy before passing in the value, thus exposing a ref to the copy, not the original
    - Example: `Nullable.GetValueRefOrDefaultRef(someObj.SomeProp)`
    - We could add an analyzer but it might be very difficult to write an analyzer that fully understands all the places where the compiler makes a copy. However, we might be able to construct an analyzer to checks for known-safe cases, e.g. when passing in locals or fields and warns on everything else or on known unsafe cases. Either way, the analyzer could add value without having to duplicate the logic of the compiler. Another option is force the call site to specify `in`, which is a safe pattern. However, we don't want to add a global analyzer because that would result in us pushing for a given language use or code style and we don't believe that's our role. We could, however, have targeted analyzer for specific APIs. This is something where having a conversion with @dotnet/LDM would be a good idea.

```C#
namespace System
{
    public static partial class Nullable
    {
        public static ref readonly T GetValueRefOrDefaultRef<T>(in T? nullable) where T : struct;
    }
}
```
## LibraryImportAttribute for p/invoke source generation

**NeedsWork** | [#runtime/46822](https://github.com/dotnet/runtime/issues/46822#issuecomment-1027242060) | [Video](https://www.youtube.com/watch?v=hE7KsqWPFlE&t=0h21m46s)

The main thing that came up in discussion is that the string encoding being an open `Type` parameter is not very good ease-of-use/user-experience.  We probably want something with the convenience of the `CharSet` enum, but perhaps a new one specific to this use case.  (So this would be a sibling property to the `MarshalStringsUsing` property, just accelerating the common cases)
