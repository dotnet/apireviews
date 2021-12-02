# API Review 12/02/2021

## Source Generators: RegisterReferenceSourceOutput

**NeedsWork** | [#roslyn/57589](https://github.com/dotnet/roslyn/issues/57589#issuecomment-985038558)

## API Review

* Most of the generator examples we've seen as motivating examples are v1 generators that, when factored into incremental generators, have perfectly fine performance.
* Doesn't help with Hot Reload or EnC scenarios, which need full implementation results
    * Only for the application of edits, not for things like intellisense
* Workaround is available: register 2 generators, one that registers partials with empty bodies, one that provides the real implementation with our existing API
    * If you did it this way, it would require partial consistency, whereas the ReferenceSourceOnly version could make it harder to ensure that these things line up.
    Would have errors
* Might have been nice if we had done a slightly different design. What if we just did RegisterSourceOutput, and put a flag on it?
    * Would that cause us to have to run generators twice, even if they didn't check that flag?
* Can RegisterReferenceSourceOutput actually change the reference assembly output?
    * They probably can do that, so we're ok
* Might need a slightly better name
    * We'll bikeshed over this when we have perf data.

### Conclusion

We think the API shape is reasonable (modulo the name), but we would like data on whether it actually helps our motivating generators before shipping the API for real.
