# API Review 04/09/2024

## Add the ability to specify line endings when serializing Json

**Approved** | [#runtime/84117](https://github.com/dotnet/runtime/issues/84117#issuecomment-2045730036) | [Video](https://www.youtube.com/watch?v=OSx-Hxpijac&t=0h0m0s)


* JsonSourceGenerationOptionsAttribute was missing, but should have parity with JsonSerializerOptions, so it was added.

```c#
namespace System.Text.Json
{
    public partial class JsonSerializerOptions
    {
        public string NewLine { get; set; } = Environment.NewLine; // only "\n" or "\r\n" are permitted values.
    }

    public partial struct JsonWriterOptions
    {
        public string NewLine { get; set; } = Environment.NewLine; // only "\n" or "\r\n" are permitted values.
    }
}

namespace System.Text.Json.Serialization
{
    public partial class JsonSourceGenerationOptionsAttribute
    {
        public string? NewLine { get; set; }
    }
}
```
## MetadataLoadContext.GetLoadContext(Assembly)

**Approved** | [#runtime/98756](https://github.com/dotnet/runtime/issues/98756#issuecomment-2045744541) | [Video](https://www.youtube.com/watch?v=OSx-Hxpijac&t=0h15m8s)


* We questioned whether it should be `TryGetLoadContext`, but decided it was OK as-is.

```c#
namespace System.Reflection;

public partial class MetadataLoadContext
{
    public static MetadataLoadContext? GetLoadContext(Assembly assembly);
}
```
## Replace `TFrom : struct` constraint on `Unsafe.BitCast` with a dynamic check

**Approved** | [#runtime/99205](https://github.com/dotnet/runtime/issues/99205#issuecomment-2045758779) | [Video](https://www.youtube.com/watch?v=OSx-Hxpijac&t=0h24m15s)


* Once we're removing one constraint we should remove both.
* We briefly entertained renaming the Generic Type Parameters (e.g. `TStructFrom`), but didn't feel it had sufficient value.

```diff
namespace System.Runtime.CompilerServices;

public static class Unsafe
{
    public static TTo BitCast<TFrom, TTo>(TFrom value)
-       where TFrom : struct
-       where TTo : struct;
}
```
## Support Base 64 URL

**Approved** | [#runtime/1658](https://github.com/dotnet/runtime/issues/1658#issuecomment-2045882076) | [Video](https://www.youtube.com/watch?v=OSx-Hxpijac&t=0h32m28s)


* GetMaxEncodedToUtf8Length(int length) => GetEncodedLength(int bytesLength)
* GetMaxDecodedFromUtf8Length(int length) => GetMaxDecodedLength(int base64Length)
* We improved the parity between UTF8 and char source/destination
* We added a lot of convenience/aesthetics overloads for the OperationStatus methods
* In IsValid for utf8, we moved the utf8 marker from a suffix to a prefix, to match IUtf8SpanFormattable

This type is needed for .NET Standard 2.0.  System.Memory.nupkg is the most obvious destination, but we may have problems with that which require a net new package.

```c#
namespace System.Buffers.Text;

public static class Base64Url
{
    public static int GetMaxDecodedLength(int base64Length);
    public static int GetEncodedLength(int bytesLength);

    public static OperationStatus EncodeToUtf8(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten, bool isFinalBlock = true);
    public static int EncodeToUtf8(ReadOnlySpan<byte> source, Span<byte> destination);
    public static bool TryEncodeToUtf8(ReadOnlySpan<byte> source, Span<byte> destination, out int charsWritten);
    public static byte[] EncodeToUtf8(ReadOnlySpan<byte> source);

    public static OperationStatus EncodeToChars(ReadOnlySpan<byte> source, Span<char> destination, out int bytesConsumed, out int charsWritten, bool isFinalBlock = true);
    public static int EncodeToChars(ReadOnlySpan<byte> source, Span<char> destination);
    public static bool TryEncodeToChars(ReadOnlySpan<byte> source, Span<char> destination, out int charsWritten);
    public static char[] EncodeToChars(ReadOnlySpan<byte> source);
    public static string EncodeToString(ReadOnlySpan<byte> source);

    public static bool TryEncodeToUtf8InPlace(Span<byte> buffer, int dataLength, out int bytesWritten);

    public static OperationStatus DecodeFromUtf8(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten, bool isFinalBlock = true);
    public static int DecodeFromUtf8(ReadOnlySpan<byte> source, Span<byte> destination);
    public static bool TryDecodeFromUtf8(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
    public static byte[] DecodeFromUtf8(ReadOnlySpan<byte> source);

    public static OperationStatus DecodeFromChars(ReadOnlySpan<char> source, Span<byte> destination, out int charsConsumed, out int bytesWritten, bool isFinalBlock = true);
    public static int DecodeFromChars(ReadOnlySpan<char> source, Span<byte> destination);
    public static bool TryDecodeFromChars(ReadOnlySpan<char> source, Span<byte> destination, out int bytesWritten);
    public static byte[] DecodeFromChars(ReadOnlySpan<char> source);

    public static int DecodeFromUtf8InPlace(Span<byte> buffer);

    public static bool IsValid(ReadOnlySpan<char> base64UrlText);
    public static bool IsValid(ReadOnlySpan<char> base64UrlText, out int decodedLength);
    public static bool IsValid(ReadOnlySpan<byte> utf8Base64UrlText);
    public static bool IsValid(ReadOnlySpan<byte> utf8Base64UrlText, out int decodedLength);
}
```
