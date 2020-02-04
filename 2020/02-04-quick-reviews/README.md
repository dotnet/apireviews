# Quick Reviews 2/4/2020

## One-shot PEM reader

**Approved** | [#runtime/29588](https://github.com/dotnet/runtime/issues/29588#issuecomment-582069797) | [Video](https://www.youtube.com/watch?v=nD5kZ2aXjdM&t=0h0m0s)

* The constructor on `PemFields` isn't public to avoid having to validate well-formedness
* Should `PemFields` implement IEquatable<PemFields>`?

```C#
namespace System.Security.Cryptography
{
    public static class PemEncoding
    {
        public static bool TryFind(ReadOnlySpan<char> pemData, out PemFields fields);
        public static PemFields Find(ReadOnlySpan<char> pemData);

        public static int GetEncodedSize(int labelLength, int dataLength);
        public static bool TryWrite(ReadOnlySpan<char> label, ReadOnlySpan<byte> data, Span<char> destination, out int charsWritten);
        public static char[] Write(ReadOnlySpan<char> label, ReadOnlySpan<byte> data);
    }

    public readonly struct PemFields
    {
        public Range Location { get; }
        public Range Label { get; }
        public Range Base64Data { get; }
        public int DecodedDataLength { get; }
    }
 }
 ```
