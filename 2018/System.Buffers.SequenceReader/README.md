# System.Buffers.SequenceReader

Status: **Needs more work** | 
[API Ref](System.Buffers.SequenceReader.md) |
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

## Session 2

### SequenceReader\<T>

* Should we remove the `unamanged` constrained and replace with a `.ctor` check?
    - Done for pinning and being able to take a pointer?
* Can we have some sample code that uses a `ReadOnlySpan<T>`-based reader vs. a
  `ReadOnlySequence<T>`-based reader?
    - We can then compare the per gains
    - We can then compare the usability gains
    - From [@stephentoub](https://github.com/stephentoub):
    > And specifically (since I'm not sure I was clear in the meeting), here's
    > what I was hoping to see. Given some common scenario that reads from /
    > parses data from a pipeline:
    > 
    > * Use the BufferReader that only supports span, and have a single method
    >   (written as we would in production code as best we can) with all of the
    >   relevant boilerplate that handles interaction both with the pipeline and
    >   with managing sequences/calls to the span-based reader.
    > * Use the BufferReader that supports sequence, and have a single method
    >   (written as we would in production code as best we can) with all of the
    >   relevant boilerplate that handles interaction both with the pipeline and
    >   with the sequence-based reader.
    > 
    > I'd like to us to be able to look at the code the provides the same
    > functionality and the benchmarks on it and compare side-by-side both
    > usability and performance. If we could do the test as part of kestrel
    > (e.g. two different runnable commits off of master) and look at plaintext
    > benchmarks and what the kestrel code looks like both ways, even better.
    > 
    > If the version with the span-based reader is totally incomprehensible and
    > provides only modest gains, then that leads us one way. If the version
    > with the span-based reader is only mildly more cumbersome and provides
    > significant gains, then that leads us another way. Etc. I just want to
    > make sure we're all in agreement on the goals, seeing the same data, and
    > then coming to a joint conclusion.
* `AtLastSegment`. The logic outlined in the comment is wrong, you need to honor
  position, and not check whether there is next segment.
* `Allocator` seems odd. If we need it, should we promote the delegate or move
  it elsewhere? We can't use a `Func` due to the generic instantiation issue
  with `Span<T>`.
    - Should the allocator be passed in as argument? Storing it as a field might
      cause additional memory barrier
* `AreAnyNext` -> `IsNextAny`
* Suggestion from last time was to rename `IsNext` to `StartsWith` but the
  latter isn't clear whether it's relative to the current position vs. the
  buffer's start.
* We may want to make `advancePastDelimiter` different methods to avoid the perf
  issue due to the extra branches.
* The struct has to be passed by `ref` or you have bugs. Also, since it's large,
  passing it by value would also tank perf.

### SequenceReaderExtensions

* All methods have a limit of reading 128 `T`s
    - This allows stack allocated buffers to be used and seemed rationale
    - Might be too small for specialized scenarios, such as many leading zeros
    - We cannot really expose APIs with sizes as this would likely destroy perf
* No APIs can produce strings, we should probably add that as dealing with
  things going across buffers is hard and we don't want folks to go through an
  intermediary array.

## BufferExtensions

* Can't we make this an instance method?

## UnsafeGetPositionWithinSegment

* Move to `SequenceMarshal`