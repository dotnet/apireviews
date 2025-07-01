# API Review 07/01/2025

## New WorkspaceChanged APIs

**Approved** | [#roslyn/79007](https://github.com/dotnet/roslyn/issues/79007#issuecomment-3024926074)

### API Review

* If these are async, then it might encourage people to do such operations like switching to/from the UI thread, rather than letting us batch all UI thread callbacks.
* These are more of a system primitive API, that batching is then built on. We can add batching in a public API and async handlers for such things later.
* Do we want to be in the business of handling the main thread switch for these handlers?
    * We have enough of our own events in this space that it's a perf pit if we don't batch the main thread options
* For batching, it's likely that the existing signatures wouldn't work. We'd need new args structs to represent the batch
* Could we just design these for batching now?
    * While we have plenty of examples of how _we_ do batching, we haven't done the research on how 3rd parties batch these events. Would require more research.
* Make the old events obsolete (warning) and nonbrowsable
* Make sure that WorkspaceEventRegistration has a private constructor to prevent.

**Conclusion**: Approved, with `Obsolete/EditorBrowsable` applied to the old events, and a private constructor for `WorkspaceEventRegistration`
