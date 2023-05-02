# API Review 05/02/2023

## Introduce a code generator to handle option validation

**Approved** | [#runtime/85475](https://github.com/dotnet/runtime/issues/85475#issuecomment-1531930293) | [Video](https://www.youtube.com/watch?v=tfCkoEqoVgI&t=0h0m0s)

* `OptionsValidatorAttribute`
    - This belongs with `IValidateOptions<T>` which is `Microsoft.Extensions.Options`
* The other two attributes seem to belong to `System.ComponentModel.DataAnnotations`
* Can we make the code generation a bit simpler so that people don't have to write stubs for the generated types?
* `ValidateTransitivelyAttribute`
    - Most customers use the term *recursively*
    - `ValidateObjectMembersAttribute`
* `ValidateEnumerableAttribute`
    - We're not validating the container.
    - `ValidateEnumeratedItemsAttribute`
* The source generator should live in two places
    - The ref pack where `Microsoft.Extensions.Options` is in-box
    - The package `Microsoft.Extensions.Options` for people consuming it as such
* Remove the conditionals

```C#
namespace Microsoft.Extensions.Options;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Struct)]
public sealed class OptionsValidatorAttribute : Attribute
{
}

[AttributeUsage(AttributeTargets.Property | AttributeTargets.Field)]
public sealed class ValidateEnumerableAttribute : ValidationAttribute
{
    public ValidateEnumerableAttribute();
    public ValidateEnumerableAttribute(Type validator);
    public Type? Validator { get; }
}

[AttributeUsage(AttributeTargets.Property | AttributeTargets.Field)]
public sealed class ValidateTransitivelyAttribute : ValidationAttribute
{
    public ValidateTransitivelyAttribute();
    public ValidateTransitivelyAttribute(Type validator);
    public Type? Validator { get; }
}
```
## Add System.Web.IHtmlString and System.Web.HtmlString to System.Web.HttpUtility

**Approved** | [#runtime/83477](https://github.com/dotnet/runtime/issues/83477#issuecomment-1531949313) | [Video](https://www.youtube.com/watch?v=tfCkoEqoVgI&t=1h6m44s)

* This interface would need to ship in-box and some existing type will need to implement the interface
    - @halter73 will investigate which type(s) this would be
    - `HttpUtility.HtmlEncode(object)` will need to be updated

```C#
namespace System.Web;

public interface IHtmlString
{
    string ToHtmlString();
}
```
## Rename IndexOfAnyValues

**Approved** | [#runtime/82622](https://github.com/dotnet/runtime/issues/82622#issuecomment-1531962052) | [Video](https://www.youtube.com/watch?v=tfCkoEqoVgI&t=1h22m2s)

We'd prefer `SearchValues`/`SearchValues<T>` as the type name.
## `IndexOfAnyValues<string>`

**Approved** | [#runtime/85573](https://github.com/dotnet/runtime/issues/85573#issuecomment-1532006760) | [Video](https://www.youtube.com/watch?v=tfCkoEqoVgI&t=1h31m56s)

We had a long discussion about the implications of just returning the `int` match and felt it was OK (we could extend it later to out a match length or the matching term).

Looks good as proposed.

```C#
namespace System.Buffers;

public static partial class IndexOfAnyValues
{
    public static IndexOfAnyValues<string> Create(ReadOnlySpan<string> values, StringComparison comparisonType);
}

namespace System;

public static partial class MemoryExtensions
{
    public static int IndexOfAny(this ReadOnlySpan<char> span, IndexOfAnyValues<string> values);
}
```
