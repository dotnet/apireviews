# API Review 09/23/2025

## Add public API implementation for getting await info from await using and await foreach

**Approved** | [#roslyn/80409](https://github.com/dotnet/roslyn/issues/80409#issuecomment-3325189633)

### API Review

* Looks good as written
* We don't think a more narrow approach makes sense.

**Conclusion**: API approved
## WithAnalyzers Compilation extensions throw on empty analyzers array

**Approved** | [#roslyn/80395](https://github.com/dotnet/roslyn/issues/80395#issuecomment-3325191328)

### API Review

* It seems irregular that behavior changes from 1 to 0
* IDE may keep its perf optimizations though.
* Need to confirm that `CompilationWithAnalyzers` will behave correctly in this case.
    * Needs good unit tests and a full audit of `CompilationWithAnalyzers` to ensure we understand what will happen.

**Conclusion**: Change approved
## Need way to check if a particular (modern) extension member is applicable to a receiver type.

**NeedsWork** | [#roslyn/80273](https://github.com/dotnet/roslyn/issues/80273#issuecomment-3325667853)

### API Review

* Likely skipped if cyrus isn't here
* What would happen if it's called on something that needs no substitution?
    * Why would it throw?
    * Does this mean the name isn't great?
* Naming suggestions
    * `ExtendType`
    * `ReduceExtensionMethod`
    * `WithExtensionParameterType`
        * Wouldn't change a non-generic interface to the implementing type
* Still have an open question as to whether `ISymbol` is the right place for such an api: should it perhaps be an the method/property level, rather than on all symbols?
