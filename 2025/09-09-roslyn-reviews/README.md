# API Review 09/09/2025

## Extensions: ExtensionGroupingName and ExtensionMarkerName API proposal

**Approved** | [#roslyn/80163](https://github.com/dotnet/roslyn/issues/80163#issuecomment-3271705832)

### API Review

* Should we move all 3 things to `INamedTypeSymbol`?
    * Yes, we want to move this. It needs to happen for .NET 10
    * Also `ExtensionParameter`

**Conclusion**: Approved, but we will move these 4 apis down to `INamedTypeSymbol`
## Extensions: AssociatedExtensionImplementation API proposal

**Approved** | [#roslyn/80167](https://github.com/dotnet/roslyn/issues/80167#issuecomment-3271706933)

### API Review

* Why not `AssociatedSymbol`?
    * Already used in this direction when looking at a property getter/setter
    * We could maybe go the other direction with `AssociatedSymbol` in the future if we need to.
* Currently doesn't work for constructed symbols
    * `AssociatedSymbol` does work for constructed symbols, we'd like to have consistent behavior
  
**Conclusion**: Approved, with support for constructed symbols as well
## Provide a way for specific DiagnosticAnalyzers to get specific AnalyzerOptions when running analyzers

**Approved** | [#roslyn/80143](https://github.com/dotnet/roslyn/issues/80143#issuecomment-3271707808)

### API Review

* This is an API for the host, not individual analyzers
* We may want to make the name more precise to avoid future conflicts
    * Let's go with `getAnalyzerConfigOptionsProvider`
* No public API property, not needed.

**Conclusion**: Approved, with the name change and no property.
## Add public APIs for FixAll functionality for CodeRefactorings

**Approved** | [#roslyn/60703](https://github.com/dotnet/roslyn/issues/60703#issuecomment-3271709406)

### API Review

* Both shapes are kinda unfortunate
    * But is the "what apis can I call" worse?
* Can we actually just change the name?
    * `RefactorAllContext` and `RefactorAllProvider`?
    * This makes a lot of sense
    * We'll duplicate FixAllScopes, easy to keep these enums in sync

**Conclusion**: Approved duplicate types, with new names
