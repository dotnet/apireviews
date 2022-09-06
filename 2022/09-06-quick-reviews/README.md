# API Review 09/06/2022

## Authenticated ciphers should distigush between mismatched tag and failure

**Approved** | [#runtime/67082](https://github.com/dotnet/runtime/issues/67082#issuecomment-1238442843) | [Video](https://www.youtube.com/watch?v=xsWMkQ5QNjM&t=0h0m0s)

Looks good as proposed

```C#
namespace System.Security.Cryptography
{
    // Note for discussion: Should this be sealed or unsealed?
    public sealed class AuthenticationTagMismatchException : CryptographicException
    {
        public AuthenticationTagMismatchException();
        public AuthenticationTagMismatchException(string? message);
        public AuthenticationTagMismatchException(string? message, Exception? innerException);
    }
}
```
## Poolable / Resettable CBOR Readers

**Approved** | [#runtime/67868](https://github.com/dotnet/runtime/issues/67868#issuecomment-1238451966) | [Video](https://www.youtube.com/watch?v=xsWMkQ5QNjM&t=0h6m42s)

Looks good as proposed

```C#
namespace System.Formats.Cbor;

partial class CborReader
{
    //resets reader state
    void Reset(ReadOnlyMemory<byte> data);
}
```
## `ImmutableArray<T>.Builder.ToImmutableAndClear()`

**Approved** | [#runtime/72625](https://github.com/dotnet/runtime/issues/72625#issuecomment-1238477596) | [Video](https://www.youtube.com/watch?v=xsWMkQ5QNjM&t=0h16m20s)

* ToImmutableAndClear, as proposed, has a problem where when Capacity==Count Capacity changes to 0, and otherwise it remains.
* We instead think the "do the cheapest thing possible" would be DrainToImmutable, always making Capacity 0 as a result of being called.
  * We debated many verbs (Drain, Extract, Empty) and prefix vs post-fix, but prefix matched the existing MoveToImmutable vs ToImmutable choice.

```C#
namespace System.Collections.Immutable;

public struct ImmutableArray<T>
{
    public partial class Builder
    {
        public ImmutableArray<T> DrainToImmutable();
    }
}
```
## Add Marvin32 to System.IO.Hashing

**Rejected** | [#runtime/68616](https://github.com/dotnet/runtime/issues/68616#issuecomment-1238485217) | [Video](https://www.youtube.com/watch?v=xsWMkQ5QNjM&t=0h44m17s)

Based on low current demand, and there being arguably better alternatives already implemented (such as xxHash32), this doesn't seem like a thing we should be adding at this time.

(If this makes you sad and you really wanted this, then please speak up.)
## Modify Float/Double Equals and CompareTo to reflect -0.0 and +0.0 difference and ordering

**Approved** | [#runtime/73583](https://github.com/dotnet/runtime/issues/73583#issuecomment-1238509104) | [Video](https://www.youtube.com/watch?v=xsWMkQ5QNjM&t=0h52m31s)

* Rather than take a breaking change here, it seems less confusing to just expose a new comparer for the concept.

```C#
namespace System.Numerics;

public struct TotalOrderIeee754Comparer<T> : IComparer<T>, IEqualityComparer<T>
    where T : IFloatingPointIeee754<T>
{
    // all the methods required by those interfaces
}
```
## Utf8Formatter/Utf8Parser should have UInt128 and Int128 overloads

**Approved** | [#runtime/73842](https://github.com/dotnet/runtime/issues/73842#issuecomment-1238512335) | [Video](https://www.youtube.com/watch?v=xsWMkQ5QNjM&t=1h18m42s)

Looks good as proposed

```C#
namespace System.Buffers.Text;

public static partial class Utf8Formatter
{
    public static bool TryFormat(UInt128 value, System.Span<byte> destination, out int bytesWritten, System.Buffers.StandardFormat format = default(System.Buffers.StandardFormat)) { throw null; }
    public static bool TryFormat(Int128 value, System.Span<byte> destination, out int bytesWritten, System.Buffers.StandardFormat format = default(System.Buffers.StandardFormat)) { throw null; }
}

public static partial class Utf8Parser
{
    public static bool TryParse(System.ReadOnlySpan<byte> source, out UInt128 value, out int bytesConsumed, char standardFormat = '\0') { throw null; }
    public static bool TryParse(System.ReadOnlySpan<byte> source, out Int128 value, out int bytesConsumed, char standardFormat = '\0') { throw null; }
}
```
## password bytes PKCS8 PEM exports

**Approved** | [#runtime/74085](https://github.com/dotnet/runtime/issues/74085#issuecomment-1238517501) | [Video](https://www.youtube.com/watch?v=xsWMkQ5QNjM&t=1h21m55s)

Looks good as proposed

```C#
namespace System.Security.Cryptography {
    public partial class AsymmetricAlgorithm {
        public string ExportEncryptedPkcs8PrivateKeyPem(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters);
        public bool TryExportEncryptedPkcs8PrivateKeyPem(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters, Span<char> destination, out int charsWritten);
    }
}
```
## Add "Create" method to EqualityComparer<> class

**Approved** | [#runtime/24460](https://github.com/dotnet/runtime/issues/24460#issuecomment-1238531234) | [Video](https://www.youtube.com/watch?v=xsWMkQ5QNjM&t=1h29m12s)

Looks good as proposed

```C#
namespace System.Collections.Generic
{
    public partial class EqualityComparer<T> : IEqualityComparer<T>, IEqualityComparer
    {
        public static EqualityComparer<T> Create(Func<T?, T?, bool> equals, Func<T, int> getHashCode);
    }
}
```
## Detect non-cancelable Task.Delay passed to Task.WhenAny

**Approved** | [#runtime/33805](https://github.com/dotnet/runtime/issues/33805#issuecomment-1238541268) | [Video](https://www.youtube.com/watch?v=xsWMkQ5QNjM&t=1h42m39s)

Seems good as proposed.  Steve and Stephen pointed out that it's eventually a problem beyond performance, so the category is now Reliability.

* Analyzer to detect when Task.Delay with no cancellation token is used in Task.WhenAny, using the next available diagnostic ID.
* A fixer which suggests to use WaitAsync with a timeout over the WhenAny approach, in the cases where it is safe to do so (and the TFM has WaitAsync available).
* The docs page for the diagnostic should demonstrate with "with cancellation token" pattern for older TFMs, and perhaps talk about when some other token is available.

Severity: Info
Category: Reliability
