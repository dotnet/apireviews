# API Review 05/03/2023

## Add `AdditionalTextValueProvider<TValue>`

**Approved** | [#roslyn/67611](https://github.com/dotnet/roslyn/issues/67611#issuecomment-1533887132)

### API Review

* Worked around it for existing usages, do we really need it?
    * Is it the type of thing that we'd tell people not to use?
    * Not really. It's not super ergonomic, but it's not unsafe or dangerous.
    * Morally kind of wrong though?
* Approved, marked help wanted. If it gets done it gets done.
## Add ReportDiagnostic API to all initial transform APIs

**NeedsWork** | [#roslyn/63776](https://github.com/dotnet/roslyn/issues/63776#issuecomment-1533887892)

### API Review

* Are the `Select` methods a source-breaking change for lambdas?
    * Maybe not, exist Selects are two-args?
    * Not a source-breaking change
* Do we want `Where` as well?
* Are diagnostics a side-channel?
* How would we actually implement this? We'd have a lot of the same issues of tracking and invalidating those locations
    * We could do it the same way users can already, but that's no more efficient
        * Slight optimizations possible, but no meaningful optimizations
    * Possible for more efficient impl, but it's a very big architecture change.
* The pattern we've seen so far has been a separate analyzer
    * Lots of duplication between analyzer and generator
    * Analyzers can be disabled
* These extensions could be written as a 3rd party without needing to be in Roslyn.
    * Could do it as extensions for now and collect info on how expensive these things are?

Conclusion: We will start with 3rd party extension methods and see what data we get about how expensive these are.
