# API Review 09/23/2021

## Consider making Operation.ChildOperations public

**Approved** | [#roslyn/49475](https://github.com/dotnet/roslyn/issues/49475#issuecomment-926170479)

## Review Notes

Significant discussion was had on what consistency principles we wanted to have here:

* We have existing `SyntaxList` APIs that expose indexers with similar performance characteristics. It would be valuable to have existing knowledge of our API shapes carry over to this API. At the same time, we're not getting requests for the ability to index a child collection of IOperation nodes, and every new piece of code adds maintenance work.
* Our existing public `List`-suffixed nodes implement `IReadOnlyList`.
* If we go with a minimal API now, but later want to add indexer and list implementations to it, some of the members of the design group would regret a non-list suffixed name, and some would not.

We decided to compromise by naming the type `OperationList`, but _not_ implementing an indexer on it at this time. If we get requests for an indexer later, we can add it then.
## Introduce IParameterSymbol.AssociatedSymbol to allow one to relate a primary constructor parameter to it's associated prop 

**Approved** | [#roslyn/54286](https://github.com/dotnet/roslyn/issues/54286#issuecomment-926171135)

## Review Notes

Updated proposal looks good. We have a singular rule we can say for all `AssociatedSymbol` properties: `AssociatedSymbol`s point from the most synthesized thing to the least synthesized thing, not the other way around.
