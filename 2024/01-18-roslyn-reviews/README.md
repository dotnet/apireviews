# API Review 01/18/2024

## Update roslyn syntax list constructs to support collection expressions.

**Approved** | [#roslyn/71596](https://github.com/dotnet/roslyn/issues/71596#issuecomment-1899302795)

### API Review

* We love it.
* It will allow `params` usage for these types.
* Let's make sure we get all of our constructible list types, not just `SyntaxList`.
* We'll confirm with the runtime team about recommended naming conventions to make sure that we're naming this inline with the BCL.

**Conclusion**: Approved.
## Nullability analysis perf investigation during razor semantic classification

**Approved** | [#roslyn/70609](https://github.com/dotnet/roslyn/issues/70609#issuecomment-1899328965)

### API Review

* If we don't intend to do sharing, the API design should probably not imply that sharing can occur, which is why it's structured as it is.
* Do we have data demonstrating that we will benefit from sharing?
    * Should we instead investigate doing sharing across _all_ semantic models?
        * May be less brittle in the long term?
* Can we lean on ExperimentalAttribute so we can do some A/B testing and see larger trends?
* We prefer the `SemanticModelOptions` approach to avoid a continuously-evolving list of flags

**Approved in experiment**: We'll approve the alternative version of this proposal in experiment for now, and revisit the final design when we have real-world A/B data.
