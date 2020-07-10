# Quick Reviews 7/10/2020

## Mark Windows-specific APIs

**Needs Work** | [#designs/138](https://github.com/dotnet/designs/pull/138) | [Video](https://www.youtube.com/watch?v=PlhWVNciJm4&t=0h0m0s)

## Add static hash helper methods

**Approved** | [#runtime/17590](https://github.com/dotnet/runtime/issues/17590#issuecomment-656829666) | [Video](https://www.youtube.com/watch?v=PlhWVNciJm4&t=1h42m25s)

We discussed this again, and accepted HashData.

```C#
public partial class MD5
{
    public static byte[] HashData(byte[] source) => throw null;
    public static byte[] HashData(ReadOnlySpan<byte> source) => throw null;
    public static int HashData(ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
    public static bool TryHashData(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
}
public partial class SHA1
{
    public static byte[] HashData(byte[] source) => throw null;
    public static byte[] HashData(ReadOnlySpan<byte> source) => throw null;
    public static int HashData(ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
    public static bool TryHashData(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
}
public partial class SHA256
{
    public static byte[] HashData(byte[] source) => throw null;
    public static byte[] HashData(ReadOnlySpan<byte> source) => throw null;
    public static int HashData(ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
    public static bool TryHashData(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
}
public partial class SHA384
{
    public static byte[] HashData(byte[] source) => throw null;
    public static byte[] HashData(ReadOnlySpan<byte> source) => throw null;
    public static int HashData(ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
    public static bool TryHashData(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
}
public partial class SHA512
{
    public static byte[] HashData(byte[] source) => throw null;
    public static byte[] HashData(ReadOnlySpan<byte> source) => throw null;
    public static int HashData(ReadOnlySpan<byte> source, Span<byte> destination) => throw null;
    public static bool TryHashData(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten) => throw null;
}
```
## Expose a ValueRef property on LinkedListNode<T>

**Approved** | [#runtime/37681](https://github.com/dotnet/runtime/issues/37681#issuecomment-656831777) | [Video](https://www.youtube.com/watch?v=PlhWVNciJm4&t=1h42m35s)

Looks good as proposed

```C#
namespace System.Collections.Generic
{
    public partial class LinkedListNode<T>
    {
        public ref T ValueRef { get; }
    }
}
```
## Make IncrementalHash.CreateHMAC ReadOnlySpan<byte> key public

**Approved** | [#runtime/35869](https://github.com/dotnet/runtime/issues/35869#issuecomment-656833490) | [Video](https://www.youtube.com/watch?v=PlhWVNciJm4&t=1h46m48s)

Looks good as proposed.

```C#
namespace System.Security.Cryptography
{
     public partial class IncrementalHash
     {
        public static IncrementalHash CreateHMAC(HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> key);
     }
 }
