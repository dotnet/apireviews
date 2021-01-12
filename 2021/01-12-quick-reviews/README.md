# Quick Reviews 01/12/2021

## Consider hiding legacy overloads for Math.DivRem

**Rejected** | [#runtime/44829](https://github.com/dotnet/runtime/issues/44829#issuecomment-758843410) | [Video](https://www.youtube.com/watch?v=3s2OAi9EbCA&t=0h0m0s)

This could confuse people multi-targeting for .NET Standard because neither side is now a superset in IntelliSense. The value of hiding also seems low, especially because developers will typically explore alternative overloads and the tuple overloads will show first.
## Provide an overload of System.Numerics.BigInteger.ToString()/TryFormat() that allows precision greater than 99

**Rejected** | [#runtime/46440](https://github.com/dotnet/runtime/issues/46440#issuecomment-758863444) | [Video](https://www.youtube.com/watch?v=3s2OAi9EbCA&t=0h9m41s)

* We approved similar APIs for `float` and `double`. But given that another type showed up, maybe we should change the parser and not add the APIs.
* The rule would be: a format modifier with any number of digits is interpreted a standard format + precision. If they value doesn't fit into an `int`, we throw.
* We should also un-approve the APIs for `float` and `double`.
* This is a potential breaking change. We should try to get it into .NET 6 Preview 1 so that we have the longest lead time.

```C#
namespace System.Numerics.BigInteger
{
    public struct BigInteger
    {
        public string ToString(char format, int precision);
        public string ToString(char format, int precision, IFormatProvider? provider);
        public bool TryFormat(Span<char> destination, out int charsWritten, char format, int precision, IFormatProvider? provider = default);
    }
}
```

## Async parallel foreach

**Approved** | [#runtime/1946](https://github.com/dotnet/runtime/issues/1946) | [Video](https://www.youtube.com/watch?v=3s2OAi9EbCA&t=0h45m57s)

## Add API to read, set and ensure capacity of Stack<T> and Queue<T>

**Approved** | [#runtime/44801](https://github.com/dotnet/runtime/issues/44801#issuecomment-758883879) | [Video](https://www.youtube.com/watch?v=3s2OAi9EbCA&t=1h6m42s)

* Looks good
* Let's also make the method on `List<T>` public

```C#
namespace System.Collections.Generic
{
    public partial class List<T>
    {
        public int EnsureCapacity(int capacity);
    }
    public partial class Stack<T>
    {
        public int EnsureCapacity(int capacity);
    }
    public partial class Queue<T>
    {
        public int EnsureCapacity(int capacity);
    }
}
```

## ChaCha20Poly1305 cryptographic algorithm

**Approved** | [#runtime/45130](https://github.com/dotnet/runtime/issues/45130#issuecomment-758897593) | [Video](https://www.youtube.com/watch?v=3s2OAi9EbCA&t=1h19m53s)

* We should remove the attribute and tell people they need to call an `IsSupported`
    - We should add an `IsSupported` property to `AesGcm` and `AesCcm` as well.

```C#
namespace System.Security.Cryptography
{
    public sealed partial class ChaCha20Poly1305 : IDisposable
    {
        public static bool IsSupported { get; }
        public ChaCha20Poly1305(byte[] key);
        public ChaCha20Poly1305(ReadOnlySpan<byte> key);
        public void Decrypt(byte[] nonce, byte[] ciphertext, byte[] tag, byte[] plaintext, byte[]? associatedData = null);
        public void Decrypt(ReadOnlySpan<byte> nonce, ReadOnlySpan<byte> ciphertext, ReadOnlySpan<byte> tag, Span<byte> plaintext, ReadOnlySpan<byte> associatedData = default(ReadOnlySpan<byte>));
        public void Dispose();
        public void Encrypt(byte[] nonce, byte[] plaintext, byte[] ciphertext, byte[] tag, byte[]? associatedData = null);
        public void Encrypt(ReadOnlySpan<byte> nonce, ReadOnlySpan<byte> plaintext, Span<byte> ciphertext, Span<byte> tag, ReadOnlySpan<byte> associatedData = default(ReadOnlySpan<byte>));
    }
}
```

