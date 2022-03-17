# API Review 03/17/2022

## ILocalSymbol.IsReadOnly

**Approved** | [#roslyn/59709](https://github.com/dotnet/roslyn/issues/59709#issuecomment-1071450792)

## API Review

The alternative design is approved.

```diff
namespace Microsoft.CodeAnalysis
{
  public interface ILocalSymbol
  {
+     bool IsForEach { get; }
+     bool IsUsing { get; }
  }
}
## Make FixAll APIs for ContainingType/Member scope public

**NeedsWork** | [#roslyn/60029](https://github.com/dotnet/roslyn/issues/60029#issuecomment-1071458718)

## API Review

* Some test cases to add:
    * Top level statements
    * Mutli-cursor in VS
        * Will we need to make `triggerSpan` an ImmutableArray to deal with this?
* We should consider renaming `triggerSpan` to `diagnosticSpan`, to make it clear this is the span of the _diagnostic_ that triggered the code, not the user's selection span.
* What is the use case for `GetDocumentSpanDiagnosticsAsync` to be public? Is it for the testing framework? Otherwise, we're not sure if we should add this helper now, or wait for use cases.
