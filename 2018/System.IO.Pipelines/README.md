# System.IO.Pipelines Review: `ReadOnlyBuffer` and `IBufferList`

Status: **Needs more work** | 
[API](System.IO.Pipelines.md) |
[Presentation](System.IO.Pipelines.pptx) |
[Video](https://www.youtube.com/watch?v=tLnQsNTfl9Q)

## Intro

* There are multiple categories of types (see columns in slide #2)
* This review we're focusing on (part) of the first column, specifically:
    - `ReadOnlyBuffer`
    - `Position`
    - `IBufferList`
* `Span<T>`/`Memory<T>` friends are for contiguous memory; pipelines add
  primitives that handles discontinuous memory. The analogy that
  [@Krzysztof](https://github.com/KrzysztofCwalina) provided was:
    - `ReadOnlyBuffer` ~ `Memory<byte>` (but discontinuous)
    - `Pipe` ~ `System.Threading.Channel<byte>`
* Writing is generally easier and thus has fewer types; thus full API-by-API
  symmetry between reading and writing might be hard.
* .NET has multiple layers of buffers, many of which are implementation details
  of a particular class. Pipelines makes makes buffers a first-class concern to
  avoid redundant (and hidden) buffers and copies.

## API Feedback

### `Position`

* `Position` (used to be `ReadCursor`) is a very generic name, maybe we should
  consider prefixing it with something, e.g. `BufferPosition`, in order to avoid
  naming collisions.

### `ReadOnlyBuffer`

* Should be generic
* `Move` should be called `Seek` or `GetPosition`. It seems to cause some
  confusion as it implies mutation.
* Lots of `Position` parameters are still called cursor
* We should add `Slice` overloads that take `int` in addition to `long`
* `ReadOnlyBuffer` shouldn't take `OwnedMemory<T>`. It should take
  `ReadOnlyMemory<T>` (will be easier after `ReadOnlyBuffer` is moved to
  `System.Memory.dll`) and array overloads.
* `IsSingleSpan` should be called `IsSingleSegment` (span happens to be a type
  and a single contiguous buffer can logically contain many spans).

### `IBufferList`

* Should be generic
* `IBufferList` is really a `IMemoryList`, so we should either change the
  property name or the type name (changing the type name seems more logical).
* `VirtualIndex` seems to be a name folks don't understand. Maybe it should be
  `GlobalIndex` or maybe even just `Index` or `Offset`.
* Should `IBufferList` be read-only? Downside is that we couldn't reuse it for
  a (hypothetical) type `ReadWriteBuffer` without adding a mutable version of
  `IBufferList` too.