# Quick Reviews 1/12/2018

## Add MemoryExtension APIs to get parity with array APIs

**Approved** | [#corefx/25850](https://github.com/dotnet/corefx/issues/25850#issuecomment-357358606) | [Video](https://www.youtube.com/watch?v=1t_a9fbB3jY&t=0h0m0s)

Let's only add `Reverse` and not the rest. Reason being that delegate invocations could potentially become a perf trap; the have obvious 1 - 5 line replacements for the caller. `Reverse` can be optimized and seems generally useful.
## String-like extension methods to ReadOnlySpan<char> Epic

**Approved** | [#corefx/21395](https://github.com/dotnet/corefx/issues/21395#issuecomment-357363944) | [Video](https://www.youtube.com/watch?v=1t_a9fbB3jY&t=0h20m28s)

We don't think this is ready yet. @ahsonkhan, please redesign the APIs to avoid allocations by returning new spans. Instead, they should follow our `TryXxx` pattern. But we have to keep in mind that many times folks will start with `ReadOnlySpan<char>` from a `string` where in-place isn't possible. It would help to have some sample code that shows how these APIs are used.
## Add StringBuilder.Equals(string) to efficiently compare a StringBuilder with a string.

**Approved** | [#corefx/25846](https://github.com/dotnet/corefx/issues/25846) | [Video](https://www.youtube.com/watch?v=1t_a9fbB3jY&t=0h43m51s)

