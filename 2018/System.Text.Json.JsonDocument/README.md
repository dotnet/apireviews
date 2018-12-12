# System.Text.Json.JsonDocument

Status: **Needs more work** | 
[Spec](https://github.com/dotnet/corefx/issues/33968)
[Video](https://www.youtube.com/watch?v=6QOv4c1O6ME)

## Notes

* The default nesting limit should be a number "between what's reasonable and
  somebody is attacking me".
* Should the `Type` enum be a distinct type for the DOM? This might be easier to
  decide what the consumer has to handle. Right now, it's hard to know which
  APIs are valid in what context. Maybe we should also have more methods that
  with different names that makes clearer (like splitting `GetChildren()` into
  `GetProperties()` and `GetArrayValues()`).
* The indexer currently throws an exception. That's not very convenient.
  Returning a `JsonElement` whose kind is `null` wouldn't be helpful either as
  this doesn't work with null-propagation operators. It would be more convenient
  to have an API that returns `JsonElement?` so that people can write
  `root["foo"]?["bar"]?[fiz]"` which will return null if `foo.bar.biz` cannot be resolved or the actual value. Without this, the developer would have to write this:
    ```C#
    var exists = root.TryGetValue("foo", out var foo) &&
                 foo.TryGetValue("bar", out var bar) &&
                 bar.TryGetValue("fiz", out var fiz) &&
                 fiz.TryGetInt32(out var value);
    ```
* We need think about how we're going to evolve the indexer for UTF8 strings.
  Right now, the thinking on the language side is to use target typing (i.e. not
  providing special syntax for UTF8 string literals). This would make it error
  prone as adding an overload that takes a UTF8 string would never be called
  with regular literals, unless they are cast to UTF8 string.
* The type itself isn't specific to UTF8 encoding, except for
    - The `Parse` method, but this can provide an overload for any encoding.
    - The `TryGetRawValue`/`TryCopyRawValue` which assumes a UTF8 encoding.
    - For now, we'll remove `TryGetRawValue` and `TryCopyRawValue`.
* The DOM currently assumes all data to be in one contiguous buffer, which
  doesn't work for Kestrel/pipes where everything is expressed as
  `ReadOnlySequence<byte>`.
* Nullable values are common JSON. We should provide overloads that allow
  returning nullable primitives (e.g. `bool? GetNullableBoolean()`).
* `TryGetValue()` should be aligned with `GetXxx()`, i.e. it should be
  `TryGetInt32()`
* Should we allow people to easily build XLinq like APIs? This would require
  `JsonElement` to implement `IEnumerable<JsonElement>`. However, what is problematic right not that this method throws.