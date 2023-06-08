# API Review 06/08/2023

## Add BinaryData.ContentType

**Approved** | [#runtime/87068](https://github.com/dotnet/runtime/issues/87068#issuecomment-1583142991) | [Video](https://www.youtube.com/watch?v=EZJzm7iYKCM&t=0h0m0s)


* We feel like the content-type defaulting for the existing byte-based inputs is complicating things, especially since the new property cannot be serialized properly.  So the property should be marked nullable, and will default to `null` except for FromObjectAsJson (application/json) and FromString (text/plain)
* Added FromString with contentType to square off the scenario of the caller already receiving JSON as a String and being able to do both parts in one call.
* Based on usage, this seems to be more like the "Media Type" value from HTTP, vs the full "Content Type", so we've renamed it accordingly.

```C#
public class BinaryData {

    // new members
    public string? MediaType { get; } // MIME type, e.g. "application/json"
    public BinaryData WithMediaType(string? mediaType);

    // new overloads of existing constructors adding mediaType parameter
    public BinaryData(ReadOnlyMemory<byte> data, string? mediaType);
    public BinaryData(byte[] data, string? mediaType);
    public BinaryData(string data, string? mediaType);

    // new overloads of existing factory methods adding mediaType parameter
    public static BinaryData FromBytes(byte[] data, string? mediaType);
    public static BinaryData FromStream(Stream stream, string? mediaType);
    public static Task<BinaryData> FromStreamAsync(Stream stream, string? mediaType);
    public static BinaryData FromString(string data, string? mediaType);
}
```
## Add a JsonConverter.Type property.

**Approved** | [#runtime/63898](https://github.com/dotnet/runtime/issues/63898#issuecomment-1583148811) | [Video](https://www.youtube.com/watch?v=EZJzm7iYKCM&t=1h23m33s)

Looks good as proposed

```diff
public abstract class JsonConverter
{
     internal JsonConverter() { }
+    public abstract Type? Type { get; }
}

public abstract class JsonConverter<T> : JsonConverter
{
+    public sealed override Type Type => typeof(T);
}

public abstract class JsonConverterFactory : JsonConverter
{
+    public sealed override Type? Type => null;
}
```
## MemoryExtensions.ContainsAny{Except}

**Approved** | [#runtime/86528](https://github.com/dotnet/runtime/issues/86528#issuecomment-1583171729) | [Video](https://www.youtube.com/watch?v=EZJzm7iYKCM&t=1h28m11s)


* We need to overload across ReadOnlySpan and Span, since extension methods don't apply to implicit conversions.
* We decided to go ahead and square the circle and approve a Contains version of everything that has an IndexOfAny

```C#
namespace System;

public static partial class MemoryExtensions
{
    public static bool ContainsAny<T>(this ReadOnlySpan<T> span, SearchValues<T> values) where T : IEquatable<T>?;
    public static bool ContainsAnyExcept<T>(this ReadOnlySpan<T> span, SearchValues<T> values) where T : IEquatable<T>?;
    public static bool ContainsAny(this ReadOnlySpan<char> span, SearchValues<string> values);

    public static bool ContainsAny<T>(this Span<T> span, SearchValues<T> values) where T : IEquatable<T>?;
    public static bool ContainsAnyExcept<T>(this Span<T> span, SearchValues<T> values) where T : IEquatable<T>?;
    public static bool ContainsAny(this Span<char> span, SearchValues<string> values);

    // If any overloads of IndexOfAny{Except} are missing, consider them approved, too.
    // NOTE: Generic constraints are missing in these methods, because they were not reflected in the proposal.
    // The constraints should match the IndexOfAny versions
    public static bool ContainsAnyExcept<T>(this ReadOnlySpan<T> span, T value);
    public static bool ContainsAny<T>(this ReadOnlySpan<T> span, T value0, T value1);
    public static bool ContainsAny<T>(this ReadOnlySpan<T> span, T value0, T value1, T value2);
    public static bool ContainsAny<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> values);
    public static bool ContainsAnyInRange<T>(this ReadOnlySpan<T> span, T lowInclusive, T highInclusive);
    public static bool ContainsAnyWhiteSpace(this ReadOnlySpan<char> span);
    public static bool ContainsAnyExcept<T>(this ReadOnlySpan<T> span, T value0, T value1);
    public static bool ContainsAnyExcept<T>(this ReadOnlySpan<T> span, T value0, T value1, T value2);
    public static bool ContainsAnyExcept<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> values);
    public static bool ContainsAnyExceptInRange<T>(this ReadOnlySpan<T> span, T lowInclusive, T highInclusive);
    public static bool ContainsAnyExceptWhiteSpace(this ReadOnlySpan<char> span);

    public static bool ContainsAnyExcept<T>(this Span<T> span, T value);
    public static bool ContainsAny<T>(this Span<T> span, T value0, T value1);
    public static bool ContainsAny<T>(this Span<T> span, T value0, T value1, T value2);
    public static bool ContainsAny<T>(this Span<T> span, ReadOnlySpan<T> values);
    public static bool ContainsAnyInRange<T>(this Span<T> span, T lowInclusive, T highInclusive);
    public static bool ContainsAnyExcept<T>(this Span<T> span, T value0, T value1);
    public static bool ContainsAnyExcept<T>(this Span<T> span, T value0, T value1, T value2);
    public static bool ContainsAnyExcept<T>(this Span<T> span, ReadOnlySpan<T> values);
    public static bool ContainsAnyExceptInRange<T>(this Span<T> span, T lowInclusive, T highInclusive);

}
```
