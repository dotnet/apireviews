# API Review 10/07/2021

## GeneratedKind.SourceGenerator

**Approved** | [#roslyn/56810](https://github.com/dotnet/roslyn/issues/56810#issuecomment-938170140)

## API Review

We generally approved the idea. We think that the naming could be a bit better, but we'll address this during review when implemented. We have these candidates:

* SourceGenerator
* SourceGenerated
* GeneratedSource
* SourceGeneratorGenerated
## Expose API to signify if an `or-expression` (`|`) surprisingly widened a sign extended value.

**NeedsWork** | [#roslyn/56994](https://github.com/dotnet/roslyn/issues/56994#issuecomment-938179043)

## API Review

Initial review is concerned that this is too specific of a concept and is also never going to be change, so the existing code should have a low maintenance burden. We're going to start with a test of a simplification on the IDE side, where the simplifier will not remove widening, signed, numeric conversions, rather than trying to implement the full logic of the compiler here. We could also consider simply removing the compiler warning altogether. We'll explore these before revisiting this proposal.
