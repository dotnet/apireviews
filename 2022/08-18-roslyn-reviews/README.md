# API Review 08/18/2022

## Add an overload of `CSharpSyntaxTree.Create` that allows to specify checksum algorithm

**NeedsWork** | [#roslyn/62923](https://github.com/dotnet/roslyn/issues/62923#issuecomment-1219956928)

### API Review

* This seems like it would be a constant value of the input text, rather than needing to be recalculated by the tree.
* We need more background info on where the checksum is needed and when it needs to be calculated.
## GetDeclaredSymbol for lambdas

**NeedsWork** | [#roslyn/63051](https://github.com/dotnet/roslyn/issues/63051#issuecomment-1219957735)

### API Review

* Should we just have GetSymbolInfo be the API you use here?
* We need to investigate the behavior of GetSymbolInfo/GetDeclaredSymbol for:
  * Local functions
  * Anonymous types
  * Tuples
  * Other GetDeclaredSymbol examples that are not suffixed with "DeclarationSyntax"
* Do any of them return info from both of these APIs?
## Deprecate all public constructors of various diagnostic analysis context types

**Approved** | [#roslyn/63440](https://github.com/dotnet/roslyn/issues/63440#issuecomment-1219958684)

### API Review

* We're ok with the general concept, and we don't think we want to expose public constructors with `IsGeneratedCode` on them
* We think we should wait until .NET 8 to mark them deprecated, however, so we get more review time.
## Add CSharpSyntaxVisitor<TArgument, TResult> and CSharpSyntaxRewriter<TArgument>

**Approved** | [#roslyn/62647](https://github.com/dotnet/roslyn/issues/62647#issuecomment-1219960086)

### API Review

* APIs are approved
* We should also add a SyntaxWalker variant with `<TArgument>`, to match `OperationWalker<TArgument>`
* We need VB versions of these as well.
