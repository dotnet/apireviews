# Quick Reviews 3/24/2020

## HTTP version selection

**Approved** | [#runtime/987](https://github.com/dotnet/runtime/issues/987#issuecomment-603384663) | [Video](https://www.youtube.com/watch?v=IcE-wGQC_eM&t=0h0m0s)

* Looks good as proposed
    - We may want to change `HttpVersionPolicy.RequestXxx` to `HttpVersionPolicy.RequestedXxx`

```C#
namespace System.Net.Http
{
    public partial class HttpClient
    {
        // Exists today - will be interpreted as Min or Max based on new API below.
        // Version DefaultVersion { get; set; }
        public HttpVersionPolicy DefaultVersionPolicy { get; set; } = HttpVersionPolicy.RequestVersionOrLower;
    }

    public partial class HttpRequestMessage
    {
        // Exists today - will be interpreted as Min or Max based on new API below.
        // Version Version { get; set; }
        public HttpVersionPolicy VersionPolicy { get; set; } = HttpVersionPolicy.RequestVersionOrLower;
    }

    public enum HttpVersionPolicy
    {
        RequestVersionOrLower,
        RequestVersionOrHigher,
        RequestVersionExact
    }
}
```
## Create APIs to deal with processing ASCII text (as bytes)

**Approved** | [#runtime/28230](https://github.com/dotnet/runtime/issues/28230#issuecomment-603439786) | [Video](https://www.youtube.com/watch?v=IcE-wGQC_eM&t=0h16m50s)

* `Equals` ignores non-ASCII characters. Should it return `false` or even throw?
* `IndexOf` et. al. should return `-1` if the "needle" contains non-ASCII characters; it's acceptable to return a successful match if the "needle" is found in the "haystack" and the haystack has non-ASCII anywhere (to the left or right of the match). IOW, only the "needle" is validated, not the "haystack".
* It seems a bit odd that some APIs have both `ReadOnlySpan<char>` and `string` and some only have `ReadOnlySpan<char>`. We should make it consistent.
* `IsAllAscii`, `IsAsciiChar`, `IsAsciiByte` should be an overloaded `IsAscii` method
* We should drop `Invariant` suffix (for the same reason we dropped `Ordinal`, it's all ASCII here)
* `ToLower()` / `ToUpper()` should throw when destination is too small (returning the `int` is still fine)
* `GetHashCodeOrdinalIgnoreCase()` should be `GetHashCodeIgnoreCase()`
* `ToString()` should be dropped in favor of `Encoding.Ascii.GetString()`
* `Trim`, `TrimStart`, `TrimEnd` should return `Range` so that callers can do the right thing
* `GetTrimmedRange()` suggest that it returns the whitespace
* Several parameters were renamed from `buffer` to `value`

```C#
namespace System.Buffers.Text
{
    public static class Ascii
    {
        // Compares two ASCII buffers for equality, optionally treating [A-Z] and [a-z] as equal.
        // All non-ASCII bytes / chars are compared for pure binary equivalence.

        public static bool Equals(ReadOnlySpan<byte> left, ReadOnlySpan<byte> right);
        public static bool Equals(ReadOnlySpan<char> left, ReadOnlySpan<char> right);
        public static bool EqualsIgnoreCase(ReadOnlySpan<byte> left, ReadOnlySpan<byte> right);
        public static bool EqualsIgnoreCase(ReadOnlySpan<char> left, ReadOnlySpan<char> right);

        // Compares an ASCII byte buffer and an ASCII char buffer for equality, optionally treating
        // [A-Z] and [a-z] as equal. Returns false if the ASCII byte buffer contains any non-ASCII
        // data or if the char buffer contains any element in the range [ 0080 .. FFFF ], as we
        // wouldn't know what encoding to use to perform the transcode-then-compare operation.

        public static bool Equals(ReadOnlySpan<byte> left, ReadOnlySpan<char> right);
        public static bool Equals(ReadOnlySpan<byte> left, string right);
        public static bool EqualsIgnoreCase(ReadOnlySpan<byte> left, ReadOnlySpan<char> right);
        public static bool EqualsIgnoreCase(ReadOnlySpan<byte> left, string right);

        // Searches for the first occurrence of the target substring within the search space,
        // optionally treating [A-Z] and [a-z] as equal. All non-ASCII bytes are compared for pure
        // binary equivalence. Returns the index of where the first match is found, else returns -1.
        // ADDENDUM: Also assume there are *Last equivalents of the following.

        public static int IndexOf(ReadOnlySpan<byte> text, ReadOnlySpan<byte> value);
        public static int IndexOfIgnoreCase(ReadOnlySpan<byte> text, ReadOnlySpan<byte> value);

        // Searches for the first occurrence of the target substring within the search space,
        // optionally treating [A-Z] and [a-z] as equal. Returns the index of where the first match
        // is found, else returns -1. If the target string contains any non-ASCII chars ([ 0080 .. FFFF ]),
        // the search is assume to have failed, and the method returns -1.
        // ADDENDUM: Also assume there are *Last equivalents of the following.

        public static int IndexOf(ReadOnlySpan<byte> text, ReadOnlySpan<char> value);
        public static int IndexOf(ReadOnlySpan<byte> text, string value);
        public static int IndexOfIgnoreCase(ReadOnlySpan<byte> text, ReadOnlySpan<char> value);
        public static int IndexOfIgnoreCase(ReadOnlySpan<byte> text, string value);

        // Given a buffer, returns the index of the first element in the buffer which
        // is a non-ASCII byte, or -1 if the buffer is empty or all-ASCII. The bool-
        // returning method is a convenience shortcut to perform the same check.

        public static int GetIndexOfFirstNonAsciiByte(ReadOnlySpan<byte> buffer);
        public static int GetIndexOfFirstNonAsciiChar(ReadOnlySpan<char> buffer);
        public static bool IsAscii(ReadOnlySpan<byte> value);
        public static bool IsAscii(ReadOnlySpan<char> value);

        // Returns true iff the provided byte is an ASCII byte; i.e., in the range [ 00 .. 7F ];
        // or if the provided char is in the range [ 0000 .. 007F ].

        public static bool IsAscii(byte value);
        public static bool IsAscii(char value);

        // Copies source to destination, converting [A-Z] -> [a-z] or vice versa during
        // the copy. All values outside [A-Za-z] - including non-ASCII values - are unchanged
        // during the copy.
        //
        // If source.Length <= destination.Length, succeeds and returns source.Length (# bytes copied).
        // If source.Length > destination.Length, returns -1.

        public static int ToLower(ReadOnlySpan<byte> source, Span<byte> destination);
        public static int ToLower(ReadOnlySpan<char> source, Span<char> destination);
        public static int ToUpper(ReadOnlySpan<byte> source, Span<byte> destination);
        public static int ToUpper(ReadOnlySpan<char> source, Span<char> destination);

        // Performs case conversion ([A-Z] -> [a-z] or vice versa) in-place. All values
        // outside [A-Za-z] - including non-ASCII values - are unchanged.

        public static void ToLowerInPlace(Span<byte> value);
        public static void ToLowerInPlace(Span<char> value);
        public static void ToUpperInPlace(Span<byte> value);
        public static void ToUpperInPlace(Span<char> value);

        // Performs case conversion on a single value, converting [A-Z] -> [a-z] or vice versa.
        // All values outside [A-Za-z] - including non-ASCII values - are unchanged.

        public static byte ToLower(byte value);
        public static byte ToLower(char value);
        public static byte ToUpper(byte value);
        public static byte ToUpper(char value);

        // Returns a hash code for the provided buffer suitable for use in a dictionary or
        // other keyed collection. For the OrdinalIgnoreCase method, the values [A-Z] and [a-z]
        // are treated as equivalent during hash code computation. All non-ASCII values
        // are treated as opaque data. The hash code is randomized but is not guaranteed to
        // implement any particular algorithm, nor is it guaranteed to be a member of the same
        // PRF family as other GetHashCode routines in the framework.

        public static int GetHashCode(ReadOnlySpan<byte> value);
        public static int GetHashCode(ReadOnlySpan<char> value);
        public static int GetHashCodeIgnoreCase(ReadOnlySpan<byte> value);
        public static int GetHashCodeIgnoreCase(ReadOnlySpan<char> value);

        // Widens an ASCII buffer to UTF-16 or narrows a UTF-16 buffer to ASCII.
        // Returns OperationStatus.InvalidData if the source buffer contains a non-ASCII byte
        // or a char in the range [ 0080 .. FFFF ].
        // OPEN QUESTION: Should we have an equivalent with Latin-1 semantics? Probably doesn't
        // belong on the ASCII class if we do that.

        public static OperationStatus WidenToUtf16(ReadOnlySpan<byte> source, Span<char> destination, out int bytesConsumed, out int charsWritten);
        public static OperationStatus NarrowFromUTf16(ReadOnlySpan<char> source, Span<byte> destination, out int charsConsumed, out int bytesWritten);

        // Trims only ASCII whitespace values from the buffer, returning the trimmed buffer.

        public static Range Trim(ReadOnlySpan<byte> value);
        public static Range Trim(ReadOnlySpan<char> value);
        public static Range TrimStart(ReadOnlySpan<byte> value);
        public static Range TrimStart(ReadOnlySpan<char> value);
        public static Range TrimEnd(ReadOnlySpan<byte> value);
        public static Range TrimEnd(ReadOnlySpan<char> value);
    }
}
```

## Move appropriate parts of Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment into the shared framework

**Approved** | [#runtime/26780](https://github.com/dotnet/runtime/issues/26780#issuecomment-603448231) | [Video](https://www.youtube.com/watch?v=IcE-wGQC_eM&t=1h42m7s)

* Looks good
    - We like that we don't duplicate code between the native and the managed implementation
    - However, we should make sure that other hosts can access the logic in the hosting APIs, so it seems they should expose an easy API to compute the current RID and set the `AppContext` value (or make sure they are automatically set)
* We should make it a property
* It shouldn't return a nullable string, rather it should do a sensible fallback (either in the host or the managed side)

```C#
#nullable enabled
namespace System.Runtime.InteropServices
{
    public static class RuntimeInformation
    {
        // The current OS RID, e.g.: win7-x64, osx.10.11-x64, ubuntu.18.04-arm64, rhel.7-x64
        public static string RuntimeIdentifier { get; }
    }
}
```

## Add mechanism to handle circular references when serializing

**Needs Work** | [#runtime/30820](https://github.com/dotnet/runtime/issues/30820#issuecomment-603451353) | [Video](https://www.youtube.com/watch?v=IcE-wGQC_eM&t=1h55m15s)

It sounds like you need to design an extension point by making it class instead. That sounds reasonable, so we bumped it back to `api-needs-work`. Once you have a proposal, we can re-review.
## Add a copy constructor to JsonSerializerOptions

**Approved** | [#runtime/30445](https://github.com/dotnet/runtime/issues/30445#issuecomment-603455704) | [Video](https://www.youtube.com/watch?v=IcE-wGQC_eM&t=2h4m50s)

* Looks good
* This will only copy settings, no cache information
* Converters that were added automatically (e.g. factories) should not be copied.
* Let's use `options` for the parameter name

```C#
namespace System.Text.Json
{
    public sealed partial class JsonSerializerOptions
    {
        public JsonSerializerOptions(JsonSerializerOptions options)
        {
            /* Copy all options from source to a new JsonSerializerOptions instance. */
        }
    }
}
```

## System.Text.Json ignore only when null API enhancement

**Approved** | [#runtime/30687](https://github.com/dotnet/runtime/issues/30687#issuecomment-603461964) | [Video](https://www.youtube.com/watch?v=IcE-wGQC_eM&t=2h15m16s)

* Looks good
* We should have a follow-up for `JsonSerializerOptions.IgnoreDefault` to support skipping value types.

```C#
namespace System.Text.Json.Serialization
{
    public enum JsonIgnoreCondition
    {
        Always,
        WhenNull,
        Never
    }

    public partial class JsonIgnoreAttribute
    {
        public JsonIgnoreCondition Condition { get; set; };
    }
}
```

