# System.Text.Json (Serialization)

Status: **Review not complete** | 
[API Ref](https://github.com/dotnet/corefx/issues/34372) |
[Video](https://www.youtube.com/watch?v=I_CNwnkNDNA)

## Notes

* We're shooting for an MVP in the .NET Core 3.0 timeframe, mostly due to
  time constraints but also because baking a serializer takes time
* We should compare & contrast usability
* "What's the percentage of customers we want to be happy with the built-in JSON
  serialization."
* We shouldn't return `Span<T>`; we should let the caller pass in the buffer to
  fill. This likely also requires `out` parameters to indicate how many bytes
  were written.
* The APIs returning `ValueTask` should probably return `Task`
* We should avoid the `To` and `From` prefixes, we should go with something simpler 
    - e.g. `JsonSerializer.Read`, `JsonSerializer.ReadAsync`, `JsonSerializer.Write`, `JsonSerializer.WriteAsync`
* Should we have `Utf8` in the name somewhere? How would we grow the serializer
  to support other encodings, such as UTF16?
* Should the methods like `ToJson` and `ToJsonString` that only take `object`
  also take a `Type` or be generic?
* The nullable properties on the options class probably shouldn't be nullable
  because there is only one global behavior.
    - We believe we should strive for the least confusion values, which would be
      these:
    - `SerializeNullValues`: `true`
    - `DeserializeNullValues`: `true`
    - `CaseInsensitivePropertyName`: `false`
* We should consider moving `MaxDepth` into reader/writer options