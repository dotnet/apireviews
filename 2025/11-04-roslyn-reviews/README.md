# API Review 11/04/2025

## Expose 'With-Element' information in ICollectionExpressionOperation

**NeedsWork** | [#roslyn/80998](https://github.com/dotnet/roslyn/issues/80998#issuecomment-3487418133)

### API Review

* Will it be odd for the order of evaluation to change? The arguments will always be specified first.
    * We could instead expose a list of arguments, no `IInvocationOperation/IObjectCreationOperation` to wrap?
* We do want `IArgumentOperation` as the type, since there can be by-ref parameters.
* 
* On the other hand, having an object creation there could be useful to some analyzers?
* `IList` will be tricky, since you can set `capacity:`. `IDictionary` in the future will also be tricky because of `comparer:`. What is exposed as the parameter for these?
* Workshopped idea:
  ```cs
  namespace Microsoft.CodeAnalysis.Operations
  {
      public interface ICollectionExpressionOperation : IOperation
      {
  +        ImmutableArray<IArgumentOperation> WithElementArguments { get; }
      }
  }
  ```
* Will need more work on designing what to do for array/dictionary interface argument cases.

**Conclusion**: Needs work on a few design elements before approval.
