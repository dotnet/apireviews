# UTF8 JSON Writer

Status: **Needs more work** | 
[API](https://github.com/dotnet/corefx/issues/33552)

## Notes

* It's a ref struct, so there is no `async` support
    - Can we mirror the reader? Have a state struct, let the caller unwind the
      stack and provider a bigger buffer.
* `WriteNameValue()` -> `Write<JsonType>()`
* `WriteNull(ReadOnlySpan<byte> name)`
    - Should have overloads for both `ReadOnlySpan<byte>` and `string`
* Should the writer expose an API that allows writing a really long string (e.g.
  comma separated list)?
    - Raises the question how we handle escaping/validation
* Should we support primitive types?
    - It seems desirable to have symmetry between the reader/writer
* Should we expose an API like `WriteNamedValue<T>(string name, T value) where T: ISpanFormattable`?
    - Could support virtually all primitive types
* We should always escape the values
* We're escaping the value, should we escape the property name too?
    - We believe no, as they are most likely fixed strings/literals
    - We should, however, offer an escaping API
* Expose the an options API (like pretty printing)
* We should probably support writing comments
    - Needs to follow our escaping policy
* Should we remove the overloads taking `string`?
    - UTF8 string literals will solve that.

Rough sketch of the Writer methods:

```C#
public void WriteNull();
public void WriteNull(string propertyName);
public void WriteBoolean(string propertyName, bool value);
public void WriteNumber(string propertyName, long value);
public void WriteNumber(string propertyName, ulong value);
public void WriteString(string propertyName, string value);
public void WriteString(string propertyName, ReadOnlySpan<byte> value);
public void WriteString(string propertyName, ReadOnlySpan<char> value);
public void WriteString(string propertyName, DateTime value);
public void WriteString(string propertyName, DateTimeOffset value);
public void WriteString(string propertyName, Guid value);

public void WriteNull(ReadOnlySpan<byte> propertyName);
public void WriteBoolean(ReadOnlySpan<byte> propertyName, bool value);
public void WriteNumber(ReadOnlySpan<byte> propertyName, long value);
public void WriteNumber(ReadOnlySpan<byte> propertyName, ulong value);
public void WriteString(ReadOnlySpan<byte> propertyName, string value);
public void WriteString(ReadOnlySpan<byte> propertyName, ReadOnlySpan<byte> value);
public void WriteString(ReadOnlySpan<byte> propertyName, ReadOnlySpan<char> value);
public void WriteString(ReadOnlySpan<byte> propertyName, DateTime value);
public void WriteString(ReadOnlySpan<byte> propertyName, DateTimeOffset value);
public void WriteString(ReadOnlySpan<byte> propertyName, Guid value);
```

We didn't expose the Base64 versions. Instead, we should expose a more low-level
API that allows writing raw bytes. These could be extension methods in user
code:

```C#
public void WriteBase64(string propertyName, ReadOnlySpan<byte> bytes);
public void WriteBase64(ReadOnlySpan<byte> propertyName, ReadOnlySpan<byte> bytes);
```
