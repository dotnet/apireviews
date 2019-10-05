# Quick Reviews 11/21/2017

## Support casting ReadOnlyMemory<T> to Memory<T>

**Approved** | [#corefx/23879](https://github.com/dotnet/corefx/issues/23879#issuecomment-346117097) | [Video](https://www.youtube.com/watch?v=o5JFZtgaJLs&t=-12h-4m-25s)

API review: Let's separate @jkotas's rename change into separate issue -- #25412.
Marking as api-approved again.
## Move (ReadOnly)Span.DangerousGetPinnableReference to MemoryMarshal

**Approved** | [#corefx/25412](https://github.com/dotnet/corefx/issues/25412#issuecomment-346119552) | [Video](https://www.youtube.com/watch?v=o5JFZtgaJLs&t=0h8m18s)

Looks good as proposed. We should also remove the corresponding GetDangerousXxxx() instance methods.

@jkotas why are they marked as in? Does it make a difference?
## [ExcludeFromCodeCoverageAttribute] should be applicable to assemblies

**Approved** | [#corefx/24694](https://github.com/dotnet/corefx/issues/24694#issuecomment-346119984) | [Video](https://www.youtube.com/watch?v=o5JFZtgaJLs&t=0h16m46s)

As long as we're working with the code coverage tool providers to make it's honored, I sounds good (and it seems we are).
## String.Contains(char)

**Approved** | [#corefx/25094](https://github.com/dotnet/corefx/issues/25094) | [Video](https://www.youtube.com/watch?v=o5JFZtgaJLs&t=0h19m25s)

## Expose UnixDomainSocket

**Approved** | [#corefx/10981](https://github.com/dotnet/corefx/issues/10981) | [Video](https://www.youtube.com/watch?v=o5JFZtgaJLs&t=0h20m49s)

## TryFormat with format strings as ReadOnlySpan<char> or string

**Approved** | [#corefx/25337](https://github.com/dotnet/corefx/issues/25337#issuecomment-346126703) | [Video](https://www.youtube.com/watch?v=o5JFZtgaJLs&t=0h25m43s)

We seem to gravitate to (2) because it doesn't require an implicit conversion, but also doesn't prevent us from adding it in the future.

@stephentoub has filed a separate issue to discuss (3) #25413.
## "ItemRef" - Ref element accessor for types that already have an indexer.

**Rejected** | [#corefx/25189](https://github.com/dotnet/corefx/issues/25189#issuecomment-346139011) | [Video](https://www.youtube.com/watch?v=o5JFZtgaJLs&t=0h43m37s)

For data types like `Stack<T>` and `Queue<T>` the proposed name `ItemRef` wouldn't work. Thus, this would be more like a pattern where the name depends on the concept you add a `ref`-returning method for:

* Indexers -> `ItemRef()`
* `Peek()` -> `PeekRef()`
