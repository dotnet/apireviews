# Quick Reviews 3/28/2019

## Build a JsonDocument from an already positioned Utf8JsonReader

**Approved** | [#corefx/36148](https://github.com/dotnet/corefx/issues/36148#issuecomment-477696501) | [Video](https://www.youtube.com/watch?v=_Uq0qG3KAzg&t=0h0m0s)

We should have a consistent naming with the `Parse` methods. We suggest to go with `ParseValue`:

```C#
public partial class JsonDocument
{
    public static bool TryParseValue(ref Utf8JsonReader reader, out JsonDocument document);
    public static JsonDocument ParseValue(ref Utf8JsonReader reader);
}
```
## Utf8JsonWriter and JsonElement, redux

**Approved** | [#corefx/36153](https://github.com/dotnet/corefx/issues/36153#issuecomment-477705735) | [Video](https://www.youtube.com/watch?v=_Uq0qG3KAzg&t=0h21m30s)

Usability wise, this proposal seems less than ideal. Not sure I personal buy the layering argument, but if we feel strongly we should rename the method and swap the arguments:

```C#
public partial struct JsonElement
{
    public void WriteAsValue(ref Utf8JsonWriter writer);
    public void WriteAsProperty(ReadOnlySpan<char> propertyName, ref Utf8JsonWriter writer);
    public void WriteAsProperty(ReadOnlySpan<byte> utf8PropertyName, ref Utf8JsonWriter writer);
}
```
