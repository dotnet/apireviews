# Quick Reviews 8/20/2019

## Add a Socket.Listen() overload

**Approved** | [#corefx/9680](https://github.com/dotnet/corefx/issues/9680#issuecomment-523130997) | [Video](https://www.youtube.com/watch?v=dfzXrQCINHM&t=0h0m0s)

Makes sense.
## Add ImmutableInterlocked.Update for ImmutableArray

**Approved** | [#corefx/19408](https://github.com/dotnet/corefx/issues/19408#issuecomment-523140923) | [Video](https://www.youtube.com/watch?v=dfzXrQCINHM&t=0h5m45s)

We should make sure that the new overloads don't cause ambiguities but recompiling CoreFx will probably be sufficient.
## Add support for getting current stack into Exception

**Approved** | [#corefx/19416](https://github.com/dotnet/corefx/issues/19416#issuecomment-523144443) | [Video](https://www.youtube.com/watch?v=dfzXrQCINHM&t=0h30m11s)

Based on the scenario, it seems `SetCurrentStackTrace` would more sense; if we ever need an append, we can add an overload with a Boolean. @stephentoub suggested to make it fail for exceptions with existing stack traces.


## Add 'split' support for ReadOnlySpan<char> similar to string

**Approved** | [#corefx/26528](https://github.com/dotnet/corefx/issues/26528#issuecomment-523152939) | [Video](https://www.youtube.com/watch?v=dfzXrQCINHM&t=0h39m55s)

Based on the conversation, we believe we would prefer something more scoped:

```C#
partial class MemoryExtensions
{
    public static SpanSplitEnumerator<char> Split(this ReadOnlySpan<char> span);
    public static SpanSplitEnumerator<char> Split(this ReadOnlySpan<char> span, char separator);
    public static SpanSplitEnumerator<char> Split(this ReadOnlySpan<char> span, string separator);

    // This API seems possible, but we can defer until we see scenarios
    // public static SpanSplitEnumerator<char> SplitAny(this ReadOnlySpan<char> span, ReadOnlySpan<char> separators);

    // This API seems possible, but we should defer until we see scenarios. It wouldn't be good make this work for UTF8, for example.
    // public static SpanSplitEnumerator<T> Split<T>(this ReadOnlySpan<T> span, T separator);
}
```

The enumerator returns ranges so you cans split a memory via this API and use the span to slice the memory and to unify span and read-only spans. It's unclear whether we need different enumerator types for different `Split` behaviors, but we don't expect people to exchange those, which is why they are currently proposed to be nested types too.
