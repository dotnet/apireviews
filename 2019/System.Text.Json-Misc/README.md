# Miscellaneous JSON items

## `JsonStringEnumConverter`

Approved |
[Video](https://youtu.be/1k-niYNMo4w?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=400)

This is a built-in converter from enum values to strings:

```C#
namespace System.Text.Json.Serialization
{
    /// <summary>
    /// Converter to convert enums to and from strings.
    /// </summary>
    public sealed class JsonStringEnumConverter : JsonConverterFactory
    {
        public JsonStringEnumConverter();
        /// <summary>
        /// Constructor.
        /// </summary>
        /// <param name="namingPolicy">Optional naming policy for enum values.</param>
        /// <param name="allowIntegerValues">
        /// True to allow undefined enum values. When true, if an enum value isn't
        /// defined it will output as a number rather than a string.
        /// </param>
        public JsonStringEnumConverter(JsonNamingPolicy namingPolicy = null, bool allowIntegerValues = true);
    }
}
```

**Comments:**

* It only supports case differences, but not additional characters like kebab
  cases, i.e. we can't map the strings `some-enum-value` to `SomeEnumValue`. In
  that case we throw a `JsonException` with an appropriate error cases.
* We match using `Enum.Parse()`. We first try case-sensitive matching and then
  fall back to case-insensitive. This is not exactly how deal with casing for
  properties (where we do case-insensitive comparison after applying policy).
    - The proposed policy for enums matches JSON.NET
    - We can, if there is a need, support case-insensitive matching for
      properties as well.
* If `allowIntegerValues` is `true`, we'll allow any number, so long it can be
  cast to the underlying enum representation. We don't, however, check whether
  an enum member with that value exists.
* Looks good as proposed.

## `Utf8JsonReader` with options but without final block & state

Approved |
[Video](https://youtu.be/1k-niYNMo4w?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=2141)

Current:

```C#
public ref partial struct Utf8JsonReader
{
    public Utf8JsonReader(in ReadOnlySequence<byte> jsonData, bool isFinalBlock, JsonReaderState state);
    public Utf8JsonReader(ReadOnlySpan<byte> jsonData, bool isFinalBlock, JsonReaderState state);
}
```

Proposed:

```C#
public ref partial struct Utf8JsonReader
{
    // Keep
    public Utf8JsonReader(in ReadOnlySequence<byte> jsonData, bool isFinalBlock, JsonReaderState state);
    public Utf8JsonReader(ReadOnlySpan<byte> jsonData, bool isFinalBlock, JsonReaderState state);

    // Add
    public Utf8JsonReader(System.ReadOnlySpan<byte> jsonData, JsonReaderOptions options = default);
    public Utf8JsonReader(in System.Buffers.ReadOnlySequence<byte> jsonData, JsonReaderOptions options = default);
}
```

**Review:**

* Looks good as proposed.

## `TextEncoder`

Approved |
[Video](https://youtu.be/1k-niYNMo4w?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=2498)

There is an existing type `TextEncoder` that serves as the base type for HTML / URL / JavaScript string encoding. Currently this type exposes only UTF-16 (char-based) APIs. However, the JSON stack needs it to support UTF-8 in addition, as the JSON stack operates on UTF-8 text natively. I propose the following API additions to the type to enable these scenarios.

```C#
namespace System.Text.Encodings.Web
{
    public abstract partial class TextEncoder
    {
        public virtual OperationStatus EncodeUtf8(ReadOnlySpan<byte> utf8Source, Span<byte> utf8Destination, out int bytesConsumed, out int bytesWritten, bool isFinalBlock = true);

        [EditorBrowsable(EditorBrowsableState.Never)]
        public virtual int FindFirstByteToEncodeUtf8(ReadOnlySpan<byte> utf8Text);
    }
}
```

**Review:**

* Looks good as proposed.
