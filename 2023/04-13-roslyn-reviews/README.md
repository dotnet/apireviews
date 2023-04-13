# API Review 04/13/2023

## [Lightbulb Perf] Expose an optional FilterSpan public API on SyntaxTreeAnalysisContext and SemanticModelAnalysisContext

**Approved** | [#roslyn/67257](https://github.com/dotnet/roslyn/issues/67257#issuecomment-1507605065)

### API Review

* Purely informative, analyzers can still report outside of it
* Already proven internally.
* Should this be internal only?
    * Already used internal only, and we need it further, such as in roslyn-analyzers
* Call it `FilterSpan`
* Should we add it to _all_ contexts?
    * Some contexts might start being called from a different file than is being filtered. SymbolStart/Stop for a partial field, for example

Approved for these two apis. Further additions will need to consider the different tree and span scenario.
## Deprecate CompilationWithAnalyzers.IsDiagnosticAnalyzerSuppressed public API

**NeedsWork** | [#roslyn/67592](https://github.com/dotnet/roslyn/issues/67592#issuecomment-1507646644)

### API Review

* Could we pull the existing impl out so it doesn't have to be updated in the future?
    * This could be pretty complex, it calls into some gnarly code.
    * What's the timeline for deprecation?
* Put it on the BannedAPI list for Roslyn users?

We'll move forward with obsoletion, and investigate how difficult freezing the current implementation will be. If it's easy, then we can remove at the next major version. If it's not, then we'll need to discuss when we want to break the functionality of the API.
