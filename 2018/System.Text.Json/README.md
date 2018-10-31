# System.Text.Json.JsonReader

Status: **Approved with Feedback** | 
[API Ref](System.Text.Json.JsonReader.md) |
[Additional Notes](Additional.Notes.md)

## Session 1

### Notes

* Do we have (or need) a component which takes in a Stream and does the
  boilerplate to wrap the reader in an asynchronous fashion? Or do we expect
  developers to write that loop manually? Should the guidance to developers
  be to use the high-level serializer type if you want the abstraction of async?
    - If we do need it, we can provide a JsonStreamReader class that is built on
      top of internal static methods on Utf8JsonReader. See [Additional Notes](Additional.Notes.md).
      This decision doesn't have significant impact on the JsonReader API design.
* Is there a way to remove the requirement that you need to manually persist
  the state across await boundaries? But if this is an advanced API then perhaps
  we should expect developers to use it carefully.
    - There are pros and cons of this approach and further deliberation is
       necessary to see if its a good trade-off. Also, if you look at the sample
      code, there is a some boilerplate code that is necessary when dealing with
      `Pipe` anyway (isFinalBlock, advancing, etc.). Persisting the state object
      is only part of the code required for correctness.
* Do we support multiple JSON objects inside a single payload - one right after
  the other? What about a JSON object followed by (some arbitrary garbage)?
  Do we just stop reading once we hit the end of the object?
    - Whether the JSON is followed by arbitrary characters, or another JSON
      object, we consider the input invalid and throw. We do not explicitly stop
      after reading a single JSON object and leave it to the user to detect and
      track this. If there is a compelling enough scenario, we could add support
      for reading multiple JSON object by extending `JsonReaderOptions`.
      Alternatively, we could change the current implementation to stop reading
      after the first object. Regardless, the caller can deal with multiple JSON
      objects themselves in their reader loop.
* Error tracking: API provides line number, does it also need column number? Or
  perhaps an exception that says:
    - `The error occurred here: <BAD CHAR><next 50 chars>.`
  Related: Do we need to add "whitespace" as an enum value from the Read method?
  This might allow callers (serializers) to provide better error messages.
    - We **do not** provide column number in UTF-8 code points (or UTF-16 chars).
      We **do** provide number of bytes processed on the current line so far.
* We may have issues where the JSON reader returns a Span, but the Span may
  point to a buffer which has been returned to the pool. Additionally, we need
  to make sure we're not allocating when we cross segment boundaries.
  Related: How do we avoid double returns to the pool on dispose?
* We may need to exist in two worlds:
    - a ValueSpan property(returns `ReadOnlySpan<byte>`)
    - a ValueSegment property (returns `ReadOnlySequence<byte>`)

### API feedback

* Remove `Dispose` and use of `ArrayPool`
* Add `ValueSequence` property to avoid allocating when data straddle boundary

## Session 2

### Utf8JsonReader

* Namespace and standalone assembly: `System.Text.Json`.
* The current design based around a single ref struct type is fine.
* Consider renaming to `JsonUtf8Reader` so all types sort together.
    - Alternatives: `Utf8JsonReader`, `JsonReaderUtf8`, `JsonReader`
    - Having Utf8 as the prefix draws attention to the characteritic of the
      reader instead of its functionality
* Remove the single argument constructors to avoid confusion and improve
  discoverability of the ctor that requires passing state. Alternatively remove
  the default arguments from the ctor that takes `JsonReaderState` to make it
  mandatory for the caller to pass it in.
* Rename `Consumed` to `BytesConsumed`
* Add `GetValueAsBool()`
* Consider adding `GetValueAsBigIntgerer()`
* Remove `GetValueAsNumber()` which boxes and returns an object.
* Change the `GetValueAsX()` APIs to return false following the Try pattern.
* `GetValueAsString()` should throw when TokenType is not a string or
  property name.

### JsonReaderException

* Change both `LineNumber` and `BytePosition` to be 0-based counters rather than
  1-based. Either way, both should be consistent.
* Rename `BytePosition` to `LineBytePosition` to be explicit it is the number of
  bytes on the current line.

### JsonReaderOptions

* Change it to a struct for extensibility and add an enum `CommentHandling`
  with regular enum values rather than having them as flags.

### JsonTokenType

* Change the `TokenType` enum to be backed by an int rather than a byte.

### JsonReaderState

* Keep the `Position` and `BytesConsumed` properties on both `Utf8JsonReader`
  and on `JsonReaderState` since only one survives the async boundary.