# API Review 03/31/2022

## Initial sketch of exposing imports from the semantic model

**NeedsWork** | [#roslyn/59533](https://github.com/dotnet/roslyn/pull/59533#issuecomment-1085098582)

## API Review

* `IAliasSymbol`s already have a `DeclaringSyntaxReference` property, which should be sufficient for discovering where they were defined. That property does not work for two other nodes, but we can remove the syntax references from the two aliases apis.
* We should introduce a dedicated `struct` for each of the other two API returns: `ImportedNamespaceOrType` and `ImportedName` are suggestions for the names of them.
* We shouldn't return the synthesized syntax reference for VB global imports. We make them behind the scenes in the compiler layer for code sharing, but no syntax is passed from the user for these things so we shouldn't expose it.
