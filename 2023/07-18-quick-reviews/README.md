# API Review 07/18/2023

## make SocketAddress more useful

**Approved** | [#runtime/86872](https://github.com/dotnet/runtime/issues/86872#issuecomment-1640682996) | [Video](https://www.youtube.com/watch?v=lw1ffNyUYWE&t=0h0m0s)

* We should remove the setter for `SocketBuffer` and rename it to just `Buffer`
* The setter for `Size` will throw if it's larger than `SocketBuffer.Length`.
* The `IEquatable<SocketAddress>` doesn't change the equality contract, it just makes it callable via the interface
* We discussed the helper methods but we don't like designing APIs for an internal consumer. Let's wait until we have a public consumer.

```diff
 namespace System.Net;
 
-public class SocketAddress
+public class SocketAddress : IEquatable<SocketAddress>
 {
-     public int Size { get; }
+     public int Size { get; set; }
+     public static int GetMaximumAddressSize(AddressFamily addressFamily);
+     public Memory<byte> Buffer { get; }
 }
```
## add overloads with SocketAddress to Socket's SendTo and ReceiveFrom

**Approved** | [#runtime/87397](https://github.com/dotnet/runtime/issues/87397#issuecomment-1640687676) | [Video](https://www.youtube.com/watch?v=lw1ffNyUYWE&t=0h44m3s)

* let's change the name of the parameter `socketAddress` to `receivedAddress` to make it clear that it will be mutated.

```C#
namespace System.Net.Sockets;

public partial class Socket
{
    public int ReceiveFrom(Span<byte> buffer, SocketFlags socketFlags, SocketAddress receivedAddress);
    public ValueTask<int> ReceiveFromAsync(Memory<byte> buffer, SocketFlags socketFlags, SocketAddress receivedAddress, CancellationToken cancellationToken = default);

    // Approved already
    // public int SendTo(ReadOnlySpan<byte> buffer, SocketFlags socketFlags, SocketAddress socketAddress);
    // public ValueTask<int> SendToAsync(ReadOnlyMemory<byte> buffer, SocketFlags socketFlags, SocketAddress socketAddress, CancellationToken cancellationToken = default);
}
```
## Revisit HttpRequestException.HttpRequestError type

**Approved** | [#runtime/89080](https://github.com/dotnet/runtime/issues/89080#issuecomment-1640710162) | [Video](https://www.youtube.com/watch?v=lw1ffNyUYWE&t=0h47m55s)

* Makes sense as proposed

```diff
 namespace System.Net.Http;
 
 public class HttpRequestException : Exception
 {
     // Previously approved
-    public HttpRequestException(string? message, Exception? inner = null, HttpStatusCode? statusCode = null, HttpRequestError? httpRequestError = null);
-    public HttpRequestError? HttpRequestError { get; }

     // Newly approved
+    public HttpRequestException(HttpRequestError httpRequestError, string? message = null, Exception? inner = null, HttpStatusCode? statusCode = null);
+    public HttpRequestError HttpRequestError { get; }
 }
```
## Make AddBinaryFormattedResource obsolete

**Approved** | [#runtime/88217](https://github.com/dotnet/runtime/issues/88217#issuecomment-1640714788) | [Video](https://www.youtube.com/watch?v=lw1ffNyUYWE&t=1h6m33s)

* Makes sense as proposed
* We should probably reuse an existing diagnostic ID for binary formatter related obsoletions

```C#
namespace System.Resources.Extensions;

public partial class PreserializedResourceWriter
{
     [Obsolete(...)]
     public void AddBinaryFormattedResource(string name, byte[] value, string? typeName = null);
}
```
## Ship IPNetwork downlevel for netstandard2.0

**NeedsWork** | [#runtime/88965](https://github.com/dotnet/runtime/issues/88965#issuecomment-1640726614) | [Video](https://www.youtube.com/watch?v=lw1ffNyUYWE&t=1h10m5s)

@GrabYourPitchforks mentioned he received partner requests and will follow up with @antonfirsov to see if those got lost somewhere in mail.
## Streaming APIs for the `System.Net.Http.Json` extensions

**Approved** | [#runtime/87577](https://github.com/dotnet/runtime/issues/87577#issuecomment-1640740628) | [Video](https://www.youtube.com/watch?v=lw1ffNyUYWE&t=1h17m26s)

* Makes sense as proposed
     - We believe the other verbs `POST`, `PUT`, `DELETE` are less useful. Users can dispatch to the extension methods on the `HttpContent`.

```C#
namespace System.Net.Http.Json;

public static partial class HttpClientJsonExtensions
{
    public static IAsyncEnumerable<TValue?> GetFromJsonAsAsyncEnumerable<TValue>(
        this HttpClient client,
        [StringSyntax(StringSyntaxAttribute.Uri)] string? requestUri,
        JsonSerializerOptions? options,
        CancellationToken cancellationToken = default);

    public static IAsyncEnumerable<TValue?> GetFromJsonAsAsyncEnumerable<TValue>(
        this HttpClient client,
        Uri? requestUri,
        JsonSerializerOptions? options,
        CancellationToken cancellationToken = default);

    public static IAsyncEnumerable<TValue?> GetFromJsonAsAsyncEnumerable<TValue>(
        this HttpClient client,
        [StringSyntax(StringSyntaxAttribute.Uri)] string? requestUri,
        JsonTypeInfo<TValue> jsonTypeInfo,
        CancellationToken cancellationToken = default);

    public static IAsyncEnumerable<TValue?> GetFromJsonAsAsyncEnumerable<TValue>(
        this HttpClient client,
        Uri? requestUri,
        JsonTypeInfo<TValue> jsonTypeInfo,
        CancellationToken cancellationToken = default);

    public static IAsyncEnumerable<TValue?> GetFromJsonAsAsyncEnumerable<TValue>(
        this HttpClient client,
        [StringSyntax(StringSyntaxAttribute.Uri)] string? requestUri,
        CancellationToken cancellationToken = default);

    public static IAsyncEnumerable<TValue?> GetFromJsonAsAsyncEnumerable<TValue>(
        this HttpClient client,
        Uri? requestUri,
        CancellationToken cancellationToken = default);
}
public static partial class HttpContentJsonExtensions
{
    public static IAsyncEnumerable<T?> ReadFromJsonAsAsyncEnumerable<T>(
        this HttpContent content, 
        JsonSerializerOptions? options, 
        CancellationToken cancellationToken = default);

    public static IAsyncEnumerable<T?> ReadFromJsonAsAsyncEnumerable<T>(
        this HttpContent content, 
        CancellationToken cancellationToken = default);

    public static IAsyncEnumerable<T?> ReadFromJsonAsAsyncEnumerable<T>(
        this HttpContent content,
        JsonTypeInfo<T> jsonTypeInfo,
        CancellationToken cancellationToken = default);
}
```
## Add an analyzer that warns on single-use JsonSerializerOptions instances

**Approved** | [#runtime/65396](https://github.com/dotnet/runtime/issues/65396#issuecomment-1640745980) | [Video](https://www.youtube.com/watch?v=lw1ffNyUYWE&t=1h27m46s)

Looks good as proposed.

**Category**: Performance
**Severity**: Info
## Implement IAsyncDisposable on `TextReader`

**Approved** | [#runtime/88244](https://github.com/dotnet/runtime/issues/88244#issuecomment-1640751241) | [Video](https://www.youtube.com/watch?v=lw1ffNyUYWE&t=1h32m22s)

* Looks good as proposed

```C#
namespace System.IO;

public partial class TextReader
{
    public virtual ValueTask DisposeAsync();
}

public partial class StreamReader : TextReader
{
    public override ValueTask DisposeAsync();
}
```
## Unsafe.IsAddressLessThanOrEqualTo / IsAddressGreaterThanOrEqualTo

**Approved** | [#runtime/88478](https://github.com/dotnet/runtime/issues/88478#issuecomment-1640754755) | [Video](https://www.youtube.com/watch?v=lw1ffNyUYWE&t=1h37m15s)

* Looks good as proposed

```C#
namespace System.Runtime.CompilerServices;

public static class Unsafe
{
    // Existing
    // public static bool IsAddressGreaterThan<T>([AllowNull] ref T left, [AllowNull] ref T right);
    // public static bool IsAddressLessThan<T>([AllowNull] ref T left, [AllowNull] ref T right);

    // New
    public static bool IsAddressGreaterThanOrEqualTo<T>([AllowNull] ref T left, [AllowNull] ref T right);
    public static bool IsAddressLessThanOrEqualTo<T>([AllowNull] ref T left, [AllowNull] ref T right);
}
```
## Add FileAttributes.None

**Approved** | [#runtime/83125](https://github.com/dotnet/runtime/issues/83125#issuecomment-1640772721) | [Video](https://www.youtube.com/watch?v=lw1ffNyUYWE&t=1h39m23s)

* Looks good as proposed
     - Users might be confused by `None` vs `Normal` but we believe it's Not A Big Deal™️.

```C#
namespace System.IO;

[System.Flags]
public enum FileAttributes
{
    None = 0,
    // Existing members omitted
}
```
## Warn when comparing spans to null

**NeedsWork** | [#runtime/84265](https://github.com/dotnet/runtime/issues/84265#issuecomment-1640819525) | [Video](https://www.youtube.com/watch?v=lw1ffNyUYWE&t=1h49m14s)

We should warn for any comparison against spans and memories (`==`, `!=`, `is`, `is not`):

* Comparisons against the default span should be written as `span.IsDefault`. Which doesn't exist but we could add one.
* Comparisons against content should be done as `span.SequenceEqual(...)`
* Comparisons against `null` don't make sense

It seems we don't have consensus on this view point. We should discuss this with once we have quorum again.
