# System.Text.Json.Utf8JsonWriter

Status: **Approved with Feedback** | 
[API Ref](https://github.com/dotnet/corefx/issues/33552)
[Video](https://www.youtube.com/watch?v=jFIS4v2H_4s)

## Notes

* `JsonWriterState`
    - Should the state be nested under the writer class?
    - Should the properties be internal? Nobody will read them but the writer.
* `JsonWriterOptions`
    - We don't expose indent-level customizations (2 spaces vs 4 spaces vs 1 tab)
    - We can add it later if needed
* Validation
    - We should give people a way to disable validation of property names as
      that's fairly expensive.
    - We may want to split validation into escaping and structure validation
    - In fact, we should probably not consider escaping part of the validation.
    - Instead of failing, the writer should auto-escape. That means we should
      have `SkipValidation` (which only handles structure) and
      `EncodePropertyNames`.
    - If `EncodePropertyNames` is off, we should write the property as-is, no
      validation.
* We should rename the `WriteValue` methods to match the `WriteNumber`,
  `WriteString` etc. methods.
    - However, we should suffix them with `Value`, so it would be
      `WriteNumberValue` and `WriteStringValue`.
* `WriteBytesUnchecked` needs more thought