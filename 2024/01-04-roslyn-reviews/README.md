# API Review 01/04/2024

## Update DocumentationCommentId.CreateDeclarationId to return null instead of throw.

**Approved** | [#roslyn/71460](https://github.com/dotnet/roslyn/issues/71460#issuecomment-1877810818)

### API Review

* Should we instead make a `TryCreate` version?
    * Can only impact cases that use exceptions for null arguments
    * Should a `Try` version throw on `null` input?
* What would we decide if we were to make the API today? Return `null` or `Try` pattern?
* We'll (narrowly) go with the pattern as proposed, altering the existing API. For new APIs, we'd probably prefer the `Try` pattern, but pragmatism drives us to modifying the existing API.

**Conclusion**: API request is approved
