# API Review 02/24/2022

## Initial sketch of exposing imports from the semantic model

**NeedsWork** | [#roslyn/59533](https://github.com/dotnet/roslyn/pull/59533)

## IOperation tree change for target-typed new is breaking analyzers in the wild

**Rejected** | [#roslyn/59171](https://github.com/dotnet/roslyn/issues/59171#issuecomment-1050338084)

## API Review

This conversion node was added to the tree to allow the compiler to better do nullability analysis, but it's also semantically correct to have. The specification states that there exists a conversion from target-typed new nodes to the final type, which is what is now reflected by the IOperation tree. We should definitely look into infrastructure improvements in the roslyn-analyzers repo to ensure that future changes get caught earlier, but in general this change is in line with what we expect to happen with the IOperation tree as the language grows.
## Source Generators: Timing APIs

**Approved** | [#roslyn/59398](https://github.com/dotnet/roslyn/issues/59398#issuecomment-1050338472)

## API Review

This API is approved.
## Expose IMethodSymbolInternal.IsIterator

**Approved** | [#roslyn/20179](https://github.com/dotnet/roslyn/issues/20179#issuecomment-1050339084)

## API Review

This API is approved. It will return true from source symbols, when that symbol is an iterator (sync or async).
