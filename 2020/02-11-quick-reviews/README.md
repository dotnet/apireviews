# Quick Reviews 2/11/2020

## HttpRequestException w/ Status Code

**Approved** | [#runtime/911](https://github.com/dotnet/runtime/issues/911#issuecomment-584780389) | [Video](https://www.youtube.com/watch?v=kGIsqCyWqa0&t=0h0m0s)

* Looks good
* At the minimum we'll need to remove the nullable annotation fromt he first constructor, otherwise passing null as the second argument would be a source breaking change
* However, we concluded we can just remove it.

```C#
public class HttpRequestException : Exception
{
    // Existing:
    // public HttpRequestException(string message);
    // public HttpRequestException(string message, Exception inner);

    public HttpRequestException(string message, Exception inner, HttpStatusCode? statusCode);
    public HttpStatusCode? StatusCode { get; }
}
## Make System.Security.Cryptography.Oid write-once (initializable read-only)

**Approved** | [#runtime/2043](https://github.com/dotnet/runtime/issues/2043#issuecomment-584786046) | [Video](https://www.youtube.com/watch?v=kGIsqCyWqa0&t=0h12m10s)

* Looks good. It's a runtime breaking change but we believe the value is high and the risk is low (enough)

```C#
public partial class Oid
{
    public string Value
    {
        get => _value;
        set
        {
            if (_value != null && !_value.Equals(value))
                throw new PlatformNotSupportedException(...);

            _value = value;
        }
    }

    public string FriendlyName
    {
        get => _friendlyName ??= LookupFriendlyName(_value);
        set
        {
            if (_friendlyName != null && !_friendlyName.Equals(value))
                throw new PlatformNotSupportedException(...);

            _friendlyName = value;
        }
    }
}
```
## Expose the match length when using String.IndexOf in culture specific mode

**Approved** | [#runtime/27935](https://github.com/dotnet/runtime/issues/27935#issuecomment-584799296) | [Video](https://www.youtube.com/watch?v=kGIsqCyWqa0&t=0h24m32s)

* We shoulnd't invent new names for `IndexOf` and `LastIndexOf`
* While our guidance says we should be adding `string`-based version of the new range returning APIs, we don't believe they are common enough to be warranted. We can add them later if necessary.
* Given the programming model for `IndexOf`, we decided against using the `Try`-pattern.

```C#
namespace System.Globalization
{
    public partial class CompareInfo
    {
        public bool IsPrefix(ReadOnlySpan<char> source, ReadOnlySpan<char> prefix, CompareOptions options, out Range match);
        public bool IsSuffix(ReadOnlySpan<char> source, ReadOnlySpan<char> suffix, CompareOptions options, out Range match);
        public int IndexOf(ReadOnlySpan<char> source, ReadOnlySpan<char> value, CompareOptions options, out int matchLength);
        public int LastIndexOf(ReadOnlySpan<char> source, ReadOnlySpan<char> value, CompareOptions options, out int matchLength);
    }
}
```
## Incremental Hash, allowing to continue to hash

**Approved** | [#runtime/1968](https://github.com/dotnet/runtime/issues/1968#issuecomment-584806878) | [Video](https://www.youtube.com/watch?v=kGIsqCyWqa0&t=0h53m47s)

* There don't seem to be any scenarios for `DuplicateHash`, so let's remove it for now. If we were to do it, we should probably name it `Clone` though.
* Rest looks good as proposed.
* We should add a `HashSizeInBytes` property so that callers don't have to call `TryGetCurrentHash` in a loop
* While we're at it, we should also add overloads for the new `TryGetCurrentHash` and the existing `TryGetHashAndReset` to return the length instead of returning a `bool`.

```C#
namespace System.Security.Cryptography
{
    public partial class IncrementalHash
    {
        public bool TryGetCurrentHash(Span<byte> destination, out int bytesWritten);
        public int GetCurrentHash(Span<byte> destination);
        // public bool TryGetHashAndReset(Span<byte> destination, out int bytesWritten);
        public int GetHashAndReset(Span<byte> destination);
        public byte[] GetCurrentHash();
        public int HashLengthInBytes { get; }
    }
}
```
## Async parallel foreach

**Needs Work** | [#runtime/1946](https://github.com/dotnet/runtime/issues/1946) | [Video](https://www.youtube.com/watch?v=kGIsqCyWqa0&t=1h11m25s)

## BinaryPrimitives.ToLittleEndian / FromLittleEndian

**Rejected** | [#runtime/29222](https://github.com/dotnet/runtime/issues/29222#issuecomment-584820663) | [Video](https://www.youtube.com/watch?v=kGIsqCyWqa0&t=1h22m43s)

We need a more compelling reason. Sorry Levi :-)
