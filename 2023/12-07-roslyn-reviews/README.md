# API Review 12/07/2023

## Add `GetDeclaredSymbol` overload for `LocalFunctionStatementSyntax`

**Approved** | [#roslyn/71028](https://github.com/dotnet/roslyn/issues/71028#issuecomment-1846178102)

### API Review

* Ship it.

**Conclusion**: Approved.
## Make code action's nested actions publicly accessible

**Approved** | [#roslyn/71050](https://github.com/dotnet/roslyn/issues/71050#issuecomment-1846178920)

### API Review

* We'll want to remove the OmniSharp EA API for this now that it's public.
* Let's make sure that the naming matches what we do for creation.

**Conclusion**: Approved
## Consider an alternative representation for binary expressions in the syntax model

**Rejected** | [#roslyn/70865](https://github.com/dotnet/roslyn/issues/70865#issuecomment-1846179632)

### API Review

* Massive breaking change for basically every roslyn api user
* Can we introduce a new API for helping doing the flattening?
* Would mean that the shape of the syntax tree wouldn't match the language specification
    * What if I wanted to ask about the type of a nested binary expression?
    * Where would we report errors about specific operators?
* This is most commonly seen on binary operators, but can also happen in other areas
    * Limited nested collection expressions to 64 nesting
    * Long fluent call chains
* Could we provide a "this is all the C# syntax" file for users?
    * Include things like deeply nested binary expressions so users know about it

**Conclusion**: Rejected. We would consider specific helpers for flattening these types if brought, or ways to help educate users that this can happen, but changing the shape of the tree is not a route we will pursue.
