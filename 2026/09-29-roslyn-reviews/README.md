# API Review 09/29/2026

## Add experimental gladstone APIs for accessing Roslyn solutions

**Approved** | [#roslyn/85236](https://github.com/dotnet/roslyn/issues/85236#issuecomment-5894882772)

### API Review

* What does the gladstone API itself look like?
    * For documents, they use LSP, so whatever gladstone's representation of an LSP doc looks like.
    * What context is being passed? Will gladstone allow handling multitargeting?
* Should `IExtensionDocumentMessageHandler` take a `TextDocument` so they can make requests on `AdditionalFiles` as well?
    * Yes
* Should we have an `ExtensionDocumentMessageContext` instead of the double `ExtensionMessageContext`/`Document`?
    * Yes. Follow the existing context conventions:
        * `ExtensionWorkspaceMessageContext`
        * `ExtensionDocumentMessageContext`
        * Both are `struct`s.

**Conclusion**: Approved with modifications above.
## Add an option to include numeric type suffixes in TypedConstantExtensions.ToCSharpString

**Approved** | [#roslyn/84704](https://github.com/dotnet/roslyn/issues/84704#issuecomment-5895344693)

### API Review

* Alternative: https://github.com/dotnet/roslyn/issues/74304
* Do we want an enum flag with type suffixes, but also allows us to later add support for other formatting options?
* VB needs a version, options should be called TypeCharacter not TypeSuffix
* Do we want to provide a SymbolDisplayOptions, since TypedConstants can be types?
    * It's not a complicated task to get a `Type` out, and there's really only one way to format a type with our general API. There's no other API that could do type suffixes for you.
    * We would also have to add some option to SymbolDisplayFormat to let you set this, which would only be relevant to this API.
    * We could consider another overload that takes a SymbolDisplayFormat at a later point.

APIs:

```cs
namespace Microsoft.CodeAnalysis.CSharp;

[Flags]
enum TypedConstantFormattingOptions
{
    IncludeTypeSuffix = 1,
}

class TypedConstantExtensions
{
    public static string ToCSharpString(this TypedConstant, TypedConstantFormattingOptions options);
}
```

```vb
Namespace Microsoft.CodeAnalysis.VisualBasic
    <Flags>
    Enum TypedConstantFormattingOptions
        IncludeTypeCharacter = 1,
    End Enum

    Module
        <Extension>
        Public Shared String ToVisualBasicString(constant As TypedConstant, options as TypedConstantFormattingOptions)
End Namespace
```

**Conclusion**: Approved
