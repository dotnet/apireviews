# API Review 12/21/2023

## Make ProgrammaticSuppressionInfo public

**Approved** | [#roslyn/69000](https://github.com/dotnet/roslyn/issues/69000#issuecomment-1866977149)

### API Review

* Should `Suppressions` be an `ImmutableArray` instead?
    * Doesn't seem like there are any blockers, just comment that order isn't guaranteed?
    * Unless we're already using it as a HashSet internally and it's a perf hit to use an array.
    * We'll let this be driven by the implementation; whichever is better for it.
* Change the property name to `ProgrammaticSuppressions`

**Approved with name/return type change**
## Remove IPersistentStorageService, ITemporaryStorageService, ITemporaryTextStorage and ITemporaryStreamStorage.

**NeedsWork** | [#roslyn/71361](https://github.com/dotnet/roslyn/issues/71361#issuecomment-1866978232)

### API Review

* Is there any benefit to removing them?
    * Could just mark them EditorBrowsable.Never?
    * Doesn't cost us anything to keep?
    * Might be useful for us to ensure that we're actually moving off of these 32-bit apis
* Bring it up with the compat council
    * Are we able to look at the marketplace and see if we actually have users of this API?
    * Should we put in telemetry?

**Needs work. Will work with compat council to clarify our policy here.**
## Enhance the Rename API to allow existing roslyn-internal features to work solely using the public API.

**NeedsWork** | [#roslyn/71177](https://github.com/dotnet/roslyn/issues/71177#issuecomment-1866978626)

### API Review

* Rename `IgnoreLocations` to `IgnoreSpans`?
    * `DocumentSpan` doesn't seem like it's in a good shape to make public.
* For `IgnoreConflictResolutionSymbols`, the general idea is understandable once explained
    * Could we more directly express it as an API? Have a rename method that goes from symbol A to symbol B to make it clear, instead of A to B.Name, pass in B through a side channel?

**Needs work**
