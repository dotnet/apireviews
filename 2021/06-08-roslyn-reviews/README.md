# API Review 06/08/2021

## Incremental Generators merge to main

**Approved** | [#roslyn/53948](https://github.com/dotnet/roslyn/issues/53948#issuecomment-857141143)

# Notes

## Combine vs Pair

* Combine is an action, Pair is not necessarily an action.
* Settled on Combine

## Should every function in the pipeline take 2 parameters?

* Pretty much any operation can be cancellable
* If we require CancellationTokens in lambdas, then our analyzers around not passing through CancellationTokens can help. If there's no CancellationToken in scope, then it can't.
* We'll go with CancellationTokens for all predicates.

## Moving GenerateSource

* Weird to end the pipeline that is fully pure with an explicitly side-effecting function.
* Rename to RegisterSourceOutput, put on the context

## Flattening the pipeline

* Pipeline is just a nested context, we could reduce a level of nesting.
* Users don't need the extra level of nesting.
* Approved: we'll remove the nested initialization contexts and just build pipelines in the initialize methods.

## AsSingleValue

* Not really analogous to `Single` from LINQ, but sounds similar
* Possibilities
    * Collect
    * ToArray
    * AsValueProvider
    * GetValue
* Collect it is, we can wait for user feedback if needed

