# API Review 02/15/2024

## Add public API to determine if a call is being intercepted

**Approved** | [#roslyn/72093](https://github.com/dotnet/roslyn/issues/72093#issuecomment-1947415530)

### API Review

* Do we need to point at the entire `IInvocationOperation` of the return?
    * Differences in parameter or return?
    * Currently there are none allowed, but would that change in the future, and should we try to plan for that?
    * This would effectively be exposing a new lowered form of the tree, and we don't do that at all today.
* Do we want to make `GetInterceptorMethod` an extension method in the `Microsoft.CodeAnalysis.CSharp` namespace?
* Why would we want to just put it on `IOperation`?
    * Implementing this as a method on `SemanticModel`
    * Does an API shape that puts the method on `IOperation` lock us into having lazy state on `IInvocationOperation`?
* Could we invert the API and provide "here's the list of locations that are being intercepted"?
    * Might be the wrong approach for analyzer-based approaches
* We only have one specific customer scenario for this at the moment, a narrowly-focused API seems appropriate
* What symbol should be returned here?
    * Should it have type substitution performed, for example?
        * Type substitution is effectively a form of lowering
    * We should adjust the documentation to make it clear that you're getting the definition of the method, not a substituted version
* What if we had the method on `Compilation`, rather than `SemanticModel`?
    * The input `SemanticModel` is somewhat irrelevant: the actual intercepting method is likely to be in a different tree
    * `Compilation` doesn't have any public APIs for getting semantic information from a specific syntax element.

**Conclusion**: We approved the `SemanticModel`-based `GetInterceptorMethod` API that takes an `InvocationExpressionSyntax`. We need to be careful to be very clear in the documentation for the method that it returns the original definition of the intercepting method, not the fully-substituted version.
