# API Review 02/24/2026

## Expose HttpRouteFormatter and HttpRouteParser classes

**NeedsWork** | [#extensions/6940](https://github.com/dotnet/extensions/issues/6940#issuecomment-3954019060) | [Video](https://www.youtube.com/watch?v=7rCQ1_jOdTU&t=0h0m0s)

The API has several issues...
* Segment is an overly general name.
* Segment exceeds our normal struct size guideline
* HttpRouteParser.TryExtractParameters feels very clunky
* The implementation is currently InternalsVisibleTo, which makes doing something right now complicated for versioning.

The idea seems desirable, but rather than just exposing current implementation details, a solution needs to be properly scoped out and designed to solve particular problems.
## Add `Stream` wrappers for memory and text-based types

**NeedsWork** | [#runtime/82801](https://github.com/dotnet/runtime/issues/82801#issuecomment-3954518151) | [Video](https://www.youtube.com/watch?v=7rCQ1_jOdTU&t=0h37m32s)

* The discussion seemed to land at the notion that we feel that there are types more primitive than Stream (e.g. String, Memory) and types that are less primitive (e.g. BinaryData).
  * The more primitive ones shouldn't know how to convert themselves to a Stream, because they're lower level, so the "From" approach applies.
  * The less primitive ones should, so "To" or "As" apply.
* We didn't really discuss the specific methods here, just how we feel about the "From" direction and the "As"/"To" direction
* We don't have solid consensus as to whether ReadOnlySequence is higher level, lower level, or peer to Stream, so don't take the "AsStream" here as decided.
* We also didn't _really_ discuss Stream.From- vs just exposing, e.g. StringStream.

```csharp
namespace System.IO
{
    public partial class Stream
    {
        public static Stream FromText(string text, Encoding? encoding = null);
        public static Stream FromText(ReadOnlyMemory<char> text, Encoding? encoding = null);

        public static Stream FromReadOnlyData(ReadOnlyMemory<byte> data);
        public static Stream FromWritableData(Memory<byte> data);
    }
}

namespace System.Buffers
{
    public static class ReadOnlySequence
    {
        public static Stream AsStream(this ReadOnlySequence<byte> sequence);
    }
}
```
