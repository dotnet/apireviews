# System.Buffers.SequenceReader

Status: **Needs more work** | 
[API Ref](System.Buffers.SequenceReader.md)
[Issue](https://github.com/dotnet/corefx/issues/32588) |
[Video](https://www.youtube.com/watch?v=0WN1OXKBMl8)

## Session 1

### Notes

* Two types are being proposed here:
    - `SequenceReader<T>`: wraps a `ReadOnlySequence<T>`
    - `SpanReader<T>`: wraps a `ReadOnlySpan<T>`
* The reason being that `SpanReader<T>` can be faster as `SequenceReader<T>`
  because it has to do extra work when it runs out of the span. The mere
  existing of an extra call can cause the compiler to stack spill locals which
  in turn has regressed some micro benchmarks by 80%.
* The downside of having two types is that the API surface bifurcates, i.e. some
  APIs will take `SequenceReader<T>`, some will take `SpanReader<T>`. An author
  of a parsing API will have to decide. Supporting both would cause duplication
  as you can't funnel them (easily) through the same code path.
* We could do these things:
    - We can ship a third type that can wrap either a sequence or a span
    - We can ship two types (one can wrap either a sequence or a span) and one
      that only wraps span.
    - We can only ship a single type that wraps either a sequence or a span and
      ship a span-only one later if we discover we need the performance.
* None of this is accessible to languages that do not support `ref structs`,
  such as Visual Basic.

### API feedback

* `Rewind` seems a bit weird
* Can we unify `Advance` and `Rewind` into `Seek()` where the offset can be
  positive or negative.
* `Advance` is perf critical because the actual reader methods that need to
  advance are extension methods.
* `Peek(Span<T>)` might be a performance trap as callers would want to avoid the
  cost of zeroing out the passed in scratch buffer as it's rarely used. Counter
  argument from @stephentoub:
  > If `Peek` returned the number of `T`s actually peeked, why would the span
  > need to be zeroed out beforehand? (It looks like it's currently defined to
  > return a `ReadOnlySpan<T>`, and presumably that logically incorporates that
  > by being of the correct length?)
* `IsNext` is a bit weird. Maybe we should we use `StartsWith` and
  `StartsWithAny`. Also, remove the `advancePast` bool parameter.
* Skip method:
  - `SkipPast` -> `AdvancePast`
  - `TrySkipTo` -> `AdvanceTo`
  - We shouldn't name them `Advance` as they could conflict with the existing
    `Advance(long)` method when `T` is `long`.

