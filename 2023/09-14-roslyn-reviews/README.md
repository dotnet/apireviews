# API Review 09/14/2023

## public API for roslyn features to expose the ability to report progress as they run.

**NeedsWork** | [#roslyn/69111](https://github.com/dotnet/roslyn/issues/69111#issuecomment-1720148479)

### API Review

* Simplified version after feedback from last time
* `IncompleteItems` -> `AddIncompleteItems`?
    * Confusing as to whether you're adding new items or adjusting the existing incomplete count
* Add description optional parameters to the other methods
* Add optional count of items that were completed
* Would be helpful to have usage examples of how a CodeAction would call the apis

**Conclusion**: A few minor updates to make, but the general shape is looking good.
## Include implicit `MyBase.New()` calls in VB Constructors

**Approved** | [#roslyn/64354](https://github.com/dotnet/roslyn/issues/64354#issuecomment-1720149119)

### API Review

* Is this a bugfix?
    * Really more of an API betterness feature
* Should we wait for 17.9 so that VB analyzers have more time to react?
    * 17.8 comes with a new major .NET version, and new analyzers. They may need more bake time?

**Conclusion**: Change is approved, will implement for 17.9
