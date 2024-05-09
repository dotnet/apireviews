# API Review 05/09/2024

## SyntaxTokenParser API Additions

**Rejected** | [#roslyn/73002](https://github.com/dotnet/roslyn/issues/73002#issuecomment-2103415731)

### API Review

* Do we have a concern that SyntaxKind.None and IsMissing would cause code to misbehave?
    * This is a fairly specialized API, unlikely to be used in existing codepaths unintentionally.

**Conclusion**: Approved
## New APIs to support "Ref Struct Interfaces" feature 

**Approved** | [#roslyn/73330](https://github.com/dotnet/roslyn/issues/73330#issuecomment-2103416395)

### API Review

* Naming of `Allows` vs `Allow`?
    * English prefers allows here
* Shakespeare's famous question: "To By or not to By?"
    * For consistency with `ITypeSymbol.IsRefLikeType`, we'll name it `ITypeParameterSymbol.AllowsRefLikeType`

**Conclusion**: Approved, with the rename of to `ITypeParameterSymbol.AllowsRefLikeType`. The `RuntimeCapability` name is approved as-is.
