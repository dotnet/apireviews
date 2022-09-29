# API Review 09/29/2022

## Update `SymbolDisplay` to include `scoped` for parameters and locals

**Approved** | [#roslyn/63208](https://github.com/dotnet/roslyn/issues/63208#issuecomment-1262825808)

### API Review

* Like reusing
* Should we just call it `IncludeModifiers`?
    * Yes
* What about implicit cases? `out` is implicitly `scoped` in some cases, for example.
    * Shouldn't include in symbol display if it's not legal.
    * Rather than trying to really determine this, maybe we just say we don't put `scoped` on `out` parameters.
* Can we punt it to 17.5, as we're running up to the deadline for 17.4?
    * Yes, this is fine.

**Conclusion**: Accepted, renaming the options to `IncludeModifiers`.
## Add support for reporting analyzer diagnostics in editorconfig files

**NeedsWork** | [#roslyn/64213](https://github.com/dotnet/roslyn/issues/64213#issuecomment-1262826398)

### API Review

* Analyzer config files are a hierarchy, but this operates on a single file. Is that good enough for analyzers?
    * Analysis might be "not enough keys set", which could be non-observable from an individual file.
* The analysis context gets plain text, can we expose the stuff we've already parsed?
    * Is the user just going to have to reparse the file?
    * Our parser doesn't keep track of location today, so keeping ourselves and users who are attempting to give diagnostics in sync for understanding the editorconfig file will be challenging with this API.
* If we don't want to provide any more structure, can we just add them to the things `RegisterAdditionalFileAction` is called back for?

**Conclusion**: Needs more work to address the above questions.
## Add a new Location.Create overload for external file location with a different mappedLineSpan

**Approved** | [#roslyn/64236](https://github.com/dotnet/roslyn/issues/64236#issuecomment-1262826660)

### API Review

* FileLinePositionSpan has a boolean that indicates if it's a mapped path, which isn't settable externally.
    * This is implicit information in proposal 2, so we're leaning towards proposal 1.

**Conclusion**: Proposal 1 is approved.
## Deprecate usage of Workspace in TextLoader (breaking change)

**Approved** | [#roslyn/63888](https://github.com/dotnet/roslyn/issues/63888#issuecomment-1262827007)

### API Review

* Using the IDE indicated that `LoadTextOptions` would be good to include as well.
* Calling the new API on a loader that has not updated will drop the options. Is that ok?
    * Can also happen if the loader can't reload the original binary representation
    * Could we make `IsReloadable` public, and have it return `false` by default to determine whether or not the options can be dropped?
    * Maybe we just document that the `option`s can be ignored by the loader you're working with?

**Conclusion**: We'll flesh out the documentation for the options parameter.
