# UTF8 String

Status: **Needs more work** | 
[API](https://github.com/dotnet/corefx/issues/30503) |
[Video](https://www.youtube.com/watch?v=KZPwCDVFM6s)

Major types:

* `Utf8String`
    - Immutable reference type, but we're still talking about whether it can or
      should be a struct.
    - Is a designed to be a user-visible exchange type that can store data to
      the heap (as opposed to `Span<Char8>`) but there is still some debate
      whether that's the correct design.
    - We should default all `StringComparison` arguments to `Ordinal`.
    - In fact, we should use optional parameters across the board to reduce
      number of overloads.
* `UnicodeScalar`
    - We probably want to change `String` and `StringBuilder` to also accept and
      return `UnicodeScalar`
* `Char8`
    - Not reviewed today, but logically an 8-bit representation for a code point
      in `Utf8String`

## Notes

* Constructors will copy data and validate
* Equality is ordinal (like `String`)
* Comparisons are ordinal (unlike `String` which is culture specific)
    - `CompareOrdinal` might be confusing as it implies the default isn't
      ordinal. Maybe we should just drop the `Ordinal` suffix. Instead of making
      it an overload to `Compare` we should just default the comparison
      parameter.
* **Open issue**: how do we do interpolated strings with `Utf8String`? Do we
  need `FormattableUtf8String`?
* **Open issue**: it looks like we need `Utf8StringBuilder` as the method like
  `Concat` cannot handle the variety of types we have (spans, memories,
  strings).
* Remove `CreateFromBytes` and `CreateFromBytesWithoutValidation`. Should be
  handled by a cast from `Span<Char8>` to `Span<byte>`. It's an advanced
  scenario and doesn't warrant an API
* APIs that take `UnicodeScalar` should also accept a `StringComparison`.
* The instance `Equals` should probably accept a `StringComparison`
    - We should think about doing for `Equals(string)` but this might be a bad
      idea as we might need to allocate.
    - We need to think about how we do cross-encoding equality/comparisons
* `IsNullOrEmpty` / `IsNullOrWhitespace` should accept a `Span<Char8>`
* Remove `PadLeft`/`PadRight` as it's unclear what the number is supposed to
  mean
* **Open issue**: Many methods right now only take scalars and strings. Should
  we add overloads that spans of `Char8`?
* **Open issue**: We need a way to convert `Utf8String` to
  `ReadOnlyMemory<byte>` and it shouldn't hurt performance of other
  `ReadOnlyMemory<>` usages.



