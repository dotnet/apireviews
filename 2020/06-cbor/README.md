# CBOR

**In Progress** |
[Spec](https://github.com/dotnet/designs/pull/128) |
[Video](https://www.youtube.com/watch?v=W_fBGxryww4)

## Notes

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
