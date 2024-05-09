# Quick Reviews 7/9/2020

## System.Formats.Cbor: Add Reader and Writer methods for System.Half.

**Approved** | [#runtime/38467](https://github.com/dotnet/runtime/issues/38467) | [Video](https://www.youtube.com/watch?v=Uu-vtMorrC0&t=0h0m0s)

## Add PlatformID.Unknown value

**Approved** | [#runtime/38984](https://github.com/dotnet/runtime/issues/38984#issuecomment-656250578) | [Video](https://www.youtube.com/watch?v=Uu-vtMorrC0&t=0h1m32s)

API review suggests that "Other" is a better name, since it is "known" to not be one of the other values.

```diff
namespace System
{
    public enum PlatformID
    {
        MacOSX = 6,
+       Other = 7,
    }
```

Additionally, if we haven't already done obsoletion plans for this enum and the things related to it, it's probably a good idea.
## Request for Logging Internals to be public again

**Approved** | [#runtime/35060](https://github.com/dotnet/runtime/issues/35060#issuecomment-656257152) | [Video](https://www.youtube.com/watch?v=Uu-vtMorrC0&t=0h5m57s)

Adding the DefineScope T4-T6 for parity is approved.  If you want to add further ones, that's also approved, but in the review the comments were "why stop at 8?" and "do we need 7 and 8?".  If there's need for this, then they're approved.  If not, then maybe hold off.


```C#
    public static partial class LoggerMessage
    {
        public static System.Func<Microsoft.Extensions.Logging.ILogger, T1, T2, T3, T4, System.IDisposable> DefineScope<T1, T2, T3, T4>(string formatString) { throw null; }
        public static System.Func<Microsoft.Extensions.Logging.ILogger, T1, T2, T3, T4, T5, System.IDisposable> DefineScope<T1, T2, T3, T4, T5>(string formatString) { throw null; }
        public static System.Func<Microsoft.Extensions.Logging.ILogger, T1, T2, T3, T4, T5, T6, System.IDisposable> DefineScope<T1, T2, T3, T4, T5, T6>(string formatString) { throw null; }
    }
```

## Http header value Encoding selection

**Approved** | [#runtime/38711](https://github.com/dotnet/runtime/issues/38711#issuecomment-656265904) | [Video](https://www.youtube.com/watch?v=Uu-vtMorrC0&t=0h23m21s)

* ResponseHeaderEncodingSelector changed to using the request message, not the response, because of complexities of mutation across calls.

```C#
namespace System.Net.Http
{
    // "RequestHeaderEncodingSelector" in Kestrel

    public sealed class SocketsHttpHandler
    {
        public Func<string, HttpRequestMessage, Encoding?>? RequestHeaderEncodingSelector { get; set; }
        public Func<string, HttpRequestMessage, Encoding?>? ResponseHeaderEncodingSelector { get; set; }
    }
    
    public class MultipartContent
    {
        public Func<string, HttpContent, Encoding?>? HeaderEncodingSelector { get; set; }
    }
}
```

Since the `string` argument in the Func is not obviously a headerName value, consider using a custom delegate to name the parameter:

```C#
public delegate Encoding? HeaderEncodingSelector<TContext>(string headerName, TContext context);
```
## Add static hash helper methods

**Approved** | [#runtime/17590](https://github.com/dotnet/runtime/issues/17590#issuecomment-656269790) | [Video](https://www.youtube.com/watch?v=Uu-vtMorrC0&t=0h42m29s)

Approved as proposed

```C#
public partial class MD5
{
    public static byte[] Hash(byte[] source) => throw null;
    public static byte[] Hash(ReadOnlySpan<byte> source) => throw null;
    public static int Hash(ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
    public static bool TryHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
}
public partial class SHA1
{
    public static byte[] Hash(byte[] source) => throw null;
    public static byte[] Hash(ReadOnlySpan<byte> source) => throw null;
    public static int Hash(ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
    public static bool TryHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
}
public partial class SHA256
{
    public static byte[] Hash(byte[] source) => throw null;
    public static byte[] Hash(ReadOnlySpan<byte> source) => throw null;
    public static int Hash(ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
    public static bool TryHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
}
public partial class SHA384
{
    public static byte[] Hash(byte[] source) => throw null;
    public static byte[] Hash(ReadOnlySpan<byte> source) => throw null;
    public static int Hash(ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
    public static bool TryHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
}
public partial class SHA512
{
    public static byte[] Hash(byte[] source) => throw null;
    public static byte[] Hash(ReadOnlySpan<byte> source) => throw null;
    public static int Hash(ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
    public static bool TryHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
}
```
## Add EnumMemberAttribute constructor

**Rejected** | [#runtime/27655](https://github.com/dotnet/runtime/issues/27655#issuecomment-656275959) | [Video](https://www.youtube.com/watch?v=Uu-vtMorrC0&t=0h50m49s)

Since the property is already settable, our guidelines say that it can't also be a constructor parameter -- largely because it makes reading attributes harder via metadata readers.

> DO NOT provide constructor parameters to initialize properties corresponding to the optional arguments.

## Inject existing object into MEF2

**Approved** | [#runtime/29400](https://github.com/dotnet/runtime/issues/29400#issuecomment-656282604) | [Video](https://www.youtube.com/watch?v=Uu-vtMorrC0&t=1h3m59s)

After some discussion on the name of the methods, the use of defaults parameters, and the name of the generic, it's approved as

```C#
namespace System.Composition.Hosting
{
    public class ContainerConfiguration
    {
        public ContainerConfiguration WithExport<TExport>(TExport exportedInstance);
        public ContainerConfiguration WithExport<TExport>(TExport exportedInstance, string? contractName = null, IDictionary<string, object>? metadata = null);

        public ContainerConfiguration WithExport(Type contractType, object exportedInstance);
        public ContainerConfiguration WithExport(Type contractType, object exportedInstance, string? contractName = null, IDictionary<string, object>? metadata = null);
    }
} 
```

We also think it would be valueable to add an analyzer that warns when generic inference was used.  (e.g. `WithExport(new SomeExportedType())`)
## SequenceReader.TryReadTo(out ReadOnlySpan<T> sequence, ReadOnlySpan<T> delimiter)

**Approved** | [#runtime/31355](https://github.com/dotnet/runtime/issues/31355#issuecomment-656291313) | [Video](https://www.youtube.com/watch?v=Uu-vtMorrC0&t=1h18m40s)

After much debate, approved as proposed:

```C#
public partial ref struct SequenceReader
{
    public bool TryReadTo(out ReadOnlySpan<T> span, ReadOnlySpan<T> delimiter, bool advancePastDelimiter = true);
}
```
