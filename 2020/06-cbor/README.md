# CBOR

**Approved** |
[Spec](https://github.com/dotnet/designs/pull/128)

## Round 1 (Writer)

[Video](https://www.youtube.com/watch?v=W_fBGxryww4)

* The namespace is `System.Formats.Cbor`
* No support for `IDisposable` for `BeginMap`. This was considered but it seems
  odd to call `EndMap` in failure cases.
* No support for `Stream`, but this would complicate the design and we expect
  payloads to be small-ish (less thant 2kb)
* `CborConformanceLevel`
    - `Lax` seems an odd choice. How about `WellFormed`.
    - Should change `Level` to `Mode` to make it clear the members aren't
      ordered
* `CborTag` has `ulong` base type. Can the resulting enum be used in VB?
    - Is it CLS compliant?
* `CborWriter`
    - Should the conformance mode be a required constructor parameter?
    - Add `public int Encode(Span<byte> destination)`
    - Should we collapse the `WriteXxx` methods for integers into
      `WriteInteger`?
    - We should have an overload for `WriteTextString()` should accept `string`
    - `WriteStartArray()` should take a nullable value for `definiteLength`
      instead of having two overloads.
    - Same for `WriteStartMap`
    - It might be confusing for consumers whether they pass in number of pairs
      or number of pairs times two.
    - We probably want to add `WriteHalf()`
    - `WriteEncodedValue` should take a `ReadOnlySpan<byte>` instead

## Round 2 (Writer)

[Video](https://www.youtube.com/watch?v=Ot8PTydQi2k)

* `CborReaderState`
    - `None` should be `Undefined`
    - Consider making `PeekState()` a property called `State` or `NextState`
    - Should `EndOfData` and `FormatError` be removed in favor of an exception* `CborReader`
    - `BytesRead` and `HasData` should be collapsed into `ByteRemaining`
* `CborReader`
    - `BytesRead` and `HasData` should be collapsed into `ByteRemaining`
    - `ReadCborNegativeIntegerEncoding` we should replacing `Encoding` with
      `Representation` (same on writer)
    - Consider a general "get the bytes" API similar to
      `AsnReader.PeekEncodedValue()` (Note: it's there and it's called
      `ReadEncodedValue()`)
    - `TryReadByteString` for definite-sized-byte-strings it would be nice to
      expose the size to that one doesn't have to guess the size of the span
    - Consider exposing `ReadDefiniteLengthByteString(): ReadOnlyMemory<byte>`
    - Consider exposing `ReadDefiniteTextStringUtf8(): ReadOnlyMemory<byte>`.
      We'll validate that each chunk is a valid UTF8 string. If we ever need a
      non-validating one we can still add `ReadDefiniteTextStringBytes()`.
    - We probably want to add `ReadHalf()`
    - `ReadEncodedValue()` should take a `bool disableConformanceModeChecks =
      false` as well.
    - `ReadEncodedValue` should return `ReadOnlyMemory<byte>`
