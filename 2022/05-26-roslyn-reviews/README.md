# API Review 05/26/2022

## API Additions for Required Members

**Approved** | [#roslyn/61444](https://github.com/dotnet/roslyn/issues/61444#issuecomment-1139053906)

### API Review

`WithRequired` -> `WithIsRequired`. Otherwise, this looks good for the initial API. Future APIs can be added as needed.
## Higher Level SyntaxProvider APIs for incremental generators

**NeedsWork** | [#roslyn/54725](https://github.com/dotnet/roslyn/issues/54725#issuecomment-1139062813)

### API Review

Overall the approach has promise, but we have some notes to look at before finalizing.

* AttributeData is provided in multiple places.
* GeneratorAttributeSyntaxContext might want to provide the target symbol
* If we keep the syntax generic parameter, a better name might be `TAttributeTarget`
* Our analyzers don't use generics, they use SyntaxKinds. Maybe we should do the same: it will allow filtering to multiple declaration kinds, and potentially make calling more ergonomic by allowing the user to leave off the generic arguments.
  * We might also be able use SymbolKinds instead, to allow language agnosticism?
* The non-transform overload should be removed. We don't think it's necessary, and could lead users down a pit of failure by causing more recalculation later in the pipeline.
