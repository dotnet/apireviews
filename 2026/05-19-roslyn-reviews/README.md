# API Review 05/19/2026

## New public API for Unions feature

**Approved** | [#roslyn/83222](https://github.com/dotnet/roslyn/issues/83222#issuecomment-4490447330)

### API Review 
 
* It's a little weird that `CommonConversion.IsUnion` and `Conversion.IsUnion` will have different behavior (the former is only valid when the method is there), but that is the same behavior as user-defined conversions, so we're ok with it.

**Conclusion**: Approved
## Syntax model representation for a Union Declaration

**NeedsWork** | [#roslyn/83223](https://github.com/dotnet/roslyn/issues/83223#issuecomment-4490466350)

### API Review

* What happens if we introduce `class union`, or we expand `union` declarations in such a way that it will only apply to `union` types, not `struct`s?
* Will cause more work in the IDE because every `StructDeclarationSyntax` will need to be special cased for "is this a `struct` or a `union`"
    * Will cause more work anyway because of needing to audit every `StructDeclarationSyntax` usage
* `ParameterList` is defined on `TypeDeclarationSyntax`, so it will be there no matter what we do

**Conclusion**: We will switch to a `UnionDeclarationSyntax`, and reuse `ParameterListSyntax` as the type list since it is already present
## Use params collections and ORPA for APIs with both `params T[]` and `ImmutableArray<T>` overloads.

**NeedsWork** | [#roslyn/83155](https://github.com/dotnet/roslyn/issues/83155#issuecomment-4490469211)

### API Review

* We're unsure of what the behavior of our .NET Standard 2.0 users on C# 7.3 will be in the presence of this change. Will need to investigate this before we approve.
* Likely should be `ORPA(-1)` on the existing params, not `ORPA(1)` on new params.

**Conclusion**: Approved in principle, barring behavior testing
