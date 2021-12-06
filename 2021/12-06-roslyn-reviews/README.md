# API Review 12/06/2021

## IOperation support for implicit Index/Range indexer over non-array types

**Approved** | [#roslyn/58079](https://github.com/dotnet/roslyn/issues/58079#issuecomment-987324439)

### API Review

* Naming: we have both Reference and Access in existing names
    * We'll keep it as is to keep in line IPropertyReferenceOperation
* We'll rename Index to Argument to line up with IPropertyReferenceOperation

#### Conclusion

Rename Index to Argument. Otherwise, API is approved.
## Source Generators: Graceful cancellation for timing

**NeedsWork** | [#roslyn/58080](https://github.com/dotnet/roslyn/issues/58080#issuecomment-987324780)

### API Review

* Cancellation makes this complicated
* Should we split the timing info and run results into separate method calls?
* AnalyzerDriver precedent?
* Should we instead pipe in a bag that holds timing info, always adds regardless of cancellation, if cancelled then timing data is consistent, but not complete.
    * Would mean we can't reuse work.
    * No public precedent for this type of mutable API.
* Existing API is more a suspend/resume design, not necessarily a cancellation design. We should sit down with some runtime folks and see if they have general patterns for this area.
    * We might want a real partial results view in the future for partial frozen semantics in the IDE.
* Should we pass timing data in the case of cancellation through the exception itself?
    * Would AggregateException flatten these things correctly and preserve data?

#### Conclusion

We'll split this up into "general timing info" and "timing info after cancellation". We'll write up a proposal for the `GetTimingInfo()` method and approve it over email. At a later time, we can revisit how to get info from a cancelled driver run, including both timing info and partial run results.
