# System.Text.Utf8Char8

Status: **Needs more work** | 
[API Ref](https://github.com/dotnet/corefx/issues/34094)
[Video](https://www.youtube.com/watch?v=TRKTNxVSzoA)

## JSON Source Package

We discussed several names. Best candidates right now:

* `Microsoft.Bcl.Json.Sources`
* `Microsoft.Text.Json.Sources`

## Char8

* We need to think about what a down-level port would look like. I don't believe
  we can rev `System.Memory` package without breaking compat.
* `ReadOnlySpan<byte>` isn't convenient for UTF8 text
    - We don't *really* use `ReadOnlySpan<byte>` for text in public APIs. We
      have a few overloads, but they are mostly in buffer land or on brand new
      APIs.
* While there is some validation for individual values, this type doesn't in
  itself guaranteeing that any `ReadOnlySpan<UtfChar>` is a valid UTF8 string
  (for example, it can break multi-byte sequences apart). So if UTF8 text
  well-formedness is critical, users still have validate. The primary benefit of
  having the type is to express to callers of an API what the data is supposed
  to represent. We can also improve debugging by making `ToString()` meaningful.
* This feature is useful without `Utf8String`. However, the feature is much less
  compelling without compiler support for UTF8 literals.
* `IsWellFormedSequence()` doesn't seem super useful. We'd want to perform
  validation as part of transcoding/fixup. If people don't care about the
  result, they can underscore the output parameter.