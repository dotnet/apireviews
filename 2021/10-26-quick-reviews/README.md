# API Review 10/26/2021

## Expose the field `_stringHasEscaping` of the `Utf8JsonReader` struct

**Approved** | [#runtime/45167](https://github.com/dotnet/runtime/issues/45167#issuecomment-952220732) | [Video](https://www.youtube.com/watch?v=MrStTiZcpuU&t=0h0m0s)

* It should return `false` when the current token isn't a string.
* It would be beneficial to align the name with `ValueSpan`. This also solves the issue if we ever have any other token types that are escaped, so let's rename this to `ValueIsEscaped`
* Do we need an `Unescape` API that allows unescaping in a non-allocating way. We can treat this a separate API request.

```C#
namespace System.Text.Json
{
    public partial ref struct Utf8JsonReader
    {
        public bool ValueIsEscaped { get; }
    }
}
```

