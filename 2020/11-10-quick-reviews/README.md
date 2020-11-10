# Quick Reviews 11/10/2020

## WinHttpHandler.TcpKeepAlive

**Approved** | [#runtime/44025](https://github.com/dotnet/runtime/issues/44025#issuecomment-724879708) | [Video](https://www.youtube.com/watch?v=hhwOM5MzoTA&t=0h0m0s)

API Review:

* Flatten things to just the one type
* Add a "Tcp" prefix since it's TCP KeepAlive, not HTTP KeepAlive
* SupportedOSPlatform attributes might not be required, depending on how you handle older OS interaction

```C#
public partial class WinHttpHandler
{
#if NET5
    [SupportedOSPlatform("windows10.0.2004")]
#endif
    public bool TcpKeepAliveEnabled { get; set; }

#if NET5
    [SupportedOSPlatform("windows10.0.2004")]
#endif
    public TimeSpan TcpKeepAliveTime { get; set; }

#if NET5
    [SupportedOSPlatform("windows10.0.2004")]
#endif
    public TimeSpan TcpKeepAliveInterval { get; set; }
}
```
## One-shot AES CBC and ECB

**Approved** | [#runtime/2406](https://github.com/dotnet/runtime/issues/2406#issuecomment-724886705) | [Video](https://www.youtube.com/watch?v=hhwOM5MzoTA&t=0h15m57s)

Looks good as proposed:

```C#
namespace System.Security.Cryptography
{
    public abstract class SymmetricAlgorithm
    {
        // ECB (Block Aligned, no default padding, no IV)
        public byte[] DecryptEcb(byte[] ciphertext, PaddingMode paddingMode);
        public byte[] DecryptEcb(ReadOnlySpan<byte> ciphertext, PaddingMode paddingMode);
        public int DecryptEcb(ReadOnlySpan<byte> ciphertext, Span<byte> destination, PaddingMode paddingMode);
        public byte[] EncryptEcb(byte[] plaintext, PaddingMode paddingMode);
        public byte[] EncryptEcb(ReadOnlySpan<byte> plaintext, PaddingMode paddingMode);
        public int EncryptEcb(ReadOnlySpan<byte> plaintext, Span<byte> destination, PaddingMode paddingMode);
        public bool TryDecryptEcb(
            ReadOnlySpan<byte> ciphertext, Span<byte> destination, PaddingMode paddingMode, out int bytesWritten);
        protected virtual bool TryDecryptEcbCore(
            ReadOnlySpan<byte> ciphertext, Span<byte> destination, PaddingMode paddingMode, out int bytesWritten);
        public bool TryEncryptEcb(
            ReadOnlySpan<byte> plaintext, Span<byte> destination, PaddingMode paddingMode, out int bytesWritten);
        protected virtual bool TryEncryptEcbCore(
            ReadOnlySpan<byte> plaintext, Span<byte> destination, PaddingMode paddingMode, out int bytesWritten);

        // CBC (Block Aligned, default to PKCS#7 padding, IV required)
        public byte[] DecryptCbc(byte[] ciphertext, byte[] iv, PaddingMode paddingMode = PaddingMode.PKCS7);
        public byte[] DecryptCbc(
            ReadOnlySpan<byte> ciphertext, ReadOnlySpan<byte> iv, PaddingMode paddingMode = PaddingMode.PKCS7);
        public int DecryptCbc(
            ReadOnlySpan<byte> ciphertext, ReadOnlySpan<byte> iv, Span<byte> destination,
            PaddingMode paddingMode = PaddingMode.PKCS7);
        public byte[] EncryptCbc(byte[] plaintext, byte[] iv, PaddingMode paddingMode = PaddingMode.PKCS7);
        public byte[] EncryptCbc(
            ReadOnlySpan<byte> plaintext, ReadOnlySpan<byte> iv, PaddingMode paddingMode = PaddingMode.PKCS7);
        public int EncryptCbc(
            ReadOnlySpan<byte> plaintext, ReadOnlySpan<byte> iv, Span<byte> destination,
            PaddingMode paddingMode = PaddingMode.PKCS7);
        public bool TryDecryptCbc(
            ReadOnlySpan<byte> ciphertext, ReadOnlySpan<byte> iv, Span<byte> destination, out int bytesWritten,
            PaddingMode paddingMode = PaddingMode.PKCS7);
        protected virtual bool TryDecryptCbcCore(
            ReadOnlySpan<byte> ciphertext, ReadOnlySpan<byte> iv, Span<byte> destination, PaddingMode paddingMode,
            out int bytesWritten);
        public bool TryEncryptCbc(
            ReadOnlySpan<byte> plaintext, ReadOnlySpan<byte> iv, Span<byte> destination, out int bytesWritten,
            PaddingMode paddingMode = PaddingMode.PKCS7);
        protected virtual bool TryEncryptCbcCore(
            ReadOnlySpan<byte> plaintext, ReadOnlySpan<byte> iv, Span<byte> destination, PaddingMode paddingMode,
            out int bytesWritten);
    }

    public abstract class SymmetricAlgorithm
    {
        // CFB (Feedback-size aligned, default to No padding, IV required, adds feedback size (default 8))
        public byte[] DecryptCfb(
            byte[] ciphertext, byte[] iv, PaddingMode paddingMode = PaddingMode.None, int feedbackSizeBits = 8);
        public byte[] DecryptCfb(
            ReadOnlySpan<byte> ciphertext, ReadOnlySpan<byte> iv, PaddingMode paddingMode = PaddingMode.None,
            int feedbackSizeBits = 8);
        public int DecryptCfb(
            ReadOnlySpan<byte> ciphertext, ReadOnlySpan<byte> iv, Span<byte> destination,
            PaddingMode paddingMode = PaddingMode.None, int feedbackSizeBits = 8);

        public byte[] EncryptCfb(
            byte[] plaintext, byte[] iv, PaddingMode paddingMode = PaddingMode.None, int feedbackSizeBits = 8);
        public byte[] EncryptCfb(
            ReadOnlySpan<byte> plaintext, ReadOnlySpan<byte> iv, PaddingMode paddingMode = PaddingMode.None,
            int feedbackSizeBits = 8);
        public int EncryptCfb(
            ReadOnlySpan<byte> plaintext, ReadOnlySpan<byte> iv, Span<byte> destination,
            PaddingMode paddingMode = PaddingMode.None, int feedbackSizeBits = 8);

        public bool TryDecryptCfb(
            ReadOnlySpan<byte> ciphertext, ReadOnlySpan<byte> iv, Span<byte> destination, out int bytesWritten,
            PaddingMode paddingMode = PaddingMode.None, int feedbackSizeBits = 8);

        public bool TryEncryptCfb(
            ReadOnlySpan<byte> plaintext, ReadOnlySpan<byte> iv, Span<byte> destination, out int bytesWritten,
            PaddingMode paddingMode = PaddingMode.None, int feedbackSizeBits = 8);

        protected virtual bool TryDecryptCfbCore(
            ReadOnlySpan<byte> ciphertext, ReadOnlySpan<byte> iv, Span<byte> destination, PaddingMode paddingMode,
            int feedbackSizeBits, out int bytesWritten);

        protected virtual bool TryEncryptCfbCore(
            ReadOnlySpan<byte> plaintext, ReadOnlySpan<byte> iv, Span<byte> destination, PaddingMode paddingMode,
            int feedbackSizeBits, out int bytesWritten);
    }

    public abstract class SymmetricAlgorithm
    {
       public int GetCiphertextLengthEcb(int plaintextLength, PaddingMode paddingMode);
       public int GetCiphertextLengthCbc(int plaintextLength, PaddingMode paddingMode = PaddingMode.PKCS7);
       public int GetCiphertextLengthCfb(
           int plaintextLength, PaddingMode paddingMode = PaddingMode.None, int feedbackSizeBits = 8);
    }
}
```
## Do not use OfType<T>() with impossible types

**Approved** | [#runtime/33770](https://github.com/dotnet/runtime/issues/33770#issuecomment-724892803) | [Video](https://www.youtube.com/watch?v=hhwOM5MzoTA&t=0h29m27s)

Approved as proposed

Category: Reliability
Suggested severity: Warning
## Use span-based string.Concat

**Approved** | [#runtime/33777](https://github.com/dotnet/runtime/issues/33777#issuecomment-724900704) | [Video](https://www.youtube.com/watch?v=hhwOM5MzoTA&t=0h41m28s)

* Find calls to `str1 + str2.Substring(...) + ...` (but not `string.Concat(str1, str2.Substring(...), ...)`, since that's in a different analyzer)
* If there's an applicable overload of string.Concat that accepts ReadOnlySpan&lt;char> with the correct number of arguments, suggest changing the Substring call to AsSpan and calling string.Concat
* Don't arbitrarily suggest changing str1 + str2 to string.Concat(str1, str2).

Category: Performance
Suggested severity: Info
## Prefer string.AsSpan() over string.Substring() when parsing

**Approved** | [#runtime/33784](https://github.com/dotnet/runtime/issues/33784#issuecomment-724901611) | [Video](https://www.youtube.com/watch?v=hhwOM5MzoTA&t=0h56m49s)

* Find argument expressions that are str.Substring(...) and there's an overload (or the same overload) that accepts ReadOnlySpan&lt;char> in that position
* Suggest changing Substring to AsSpan for the call

Category: Performance
Suggested severity: Info
## Add Type.GetMethod overload that takes Name, BindingFlags, and Parameter Types

**Approved** | [#runtime/42753](https://github.com/dotnet/runtime/issues/42753#issuecomment-724906242) | [Video](https://www.youtube.com/watch?v=hhwOM5MzoTA&t=0h58m35s)

Looks good as proposed

```C#
namespace System
{
    public partial class Type
    {
        public System.Reflection.MethodInfo? GetMethod(string name, System.Reflection.BindingFlags bindingAttr, System.Type[] types) { throw null; }

        public System.Reflection.ConstructorInfo? GetConstructor(System.Reflection.BindingFlags bindingAttr, System.Type[] types) { throw null; }
     }
}
```
## Move SocketTaskExtensions methods to Socket class itself

**Approved** | [#runtime/43901](https://github.com/dotnet/runtime/issues/43901#issuecomment-724910395) | [Video](https://www.youtube.com/watch?v=hhwOM5MzoTA&t=1h4m11s)

Looks good as proposed

* Copy all members on SocketTaskExtensions to instance methods on Socket (removing the `this` parameter)
* Mark existing members as EditorBrowsable(Never)
* Remove any members on SocketTaskExtensions that have been introduced for .NET 6.  Be nimble with regard to approved, but not-implemented ones.
## Add Span overload for Socket.ReceiveMessageFrom

**Approved** | [#runtime/43933](https://github.com/dotnet/runtime/issues/43933#issuecomment-724912569) | [Video](https://www.youtube.com/watch?v=hhwOM5MzoTA&t=1h11m41s)

Looks good as proposed

```C#
namespace System.Net.Sockets
{
    public partial class Socket : IDisposable
    {
        public int ReceiveMessageFrom(Span<byte> buffer, ref System.Net.Sockets.SocketFlags socketFlags, ref System.Net.EndPoint remoteEP, out System.Net.Sockets.IPPacketInformation ipPacketInformation);
    }
}
```
## System.Threading.ThreadPool.RegisterWorkItemHandler method

**NeedsWork** | [#runtime/44213](https://github.com/dotnet/runtime/issues/44213#issuecomment-724914826) | [Video](https://www.youtube.com/watch?v=hhwOM5MzoTA&t=1h15m53s)

@stephentoub believes that this is not in a ready state, transitioning to needs work.
## Add static method to create an IEnumerable with one element

**Rejected** | [#runtime/15136](https://github.com/dotnet/runtime/issues/15136#issuecomment-724922312) | [Video](https://www.youtube.com/watch?v=hhwOM5MzoTA&t=1h20m1s)

API Review:

We're not really clear how this would be used in practice, and why something other than `new[] { item }` would be better in that context.  It also seems like the best name for a single item is `Single`, but that has a bad collision with the existing `Single` semantics.  (Yield doesn't seem like it's discoverable, just easily explained.)

With new data (e.g. sample usage) we might come to different conclusions, but it doesn't seem like there's enough utility and there's a dearth of good names.
## Allow for specify return value on System.Linq.Enumerable.*OrDefault methods

**Approved** | [#runtime/20064](https://github.com/dotnet/runtime/issues/20064#issuecomment-724928896) | [Video](https://www.youtube.com/watch?v=hhwOM5MzoTA&t=1h33m54s)

Looks good as proposed, but there was a concern raised that the Queryable methods might have an impact on Linq-to-SQL, which would be good to verify (@ajcvickers do you have insight or contacts here?).

```C#
namespace System.Linq
{
    public static partial class Enumerable
    {
        public static TSource SingleOrDefault<TSource>(this IEnumerable<TSource> source, TSource defaultValue);
        public static TSource SingleOrDefault<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate, TSource defaultValue);

        public static TSource FirstOrDefault<TSource>(this IEnumerable<TSource> source, TSource defaultValue);
        public static TSource FirstOrDefault<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate, TSource defaultValue);

        public static TSource LastOrDefault<TSource>(this IEnumerable<TSource> source, TSource defaultValue);
        public static TSource LastOrDefault<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate, TSource defaultValue);
    }

    public static class Queryable
    {
        public static TSource SingleOrDefault<TSource>(this IQueryable<TSource> source, TSource defaultValue);
        public static TSource SingleOrDefault<TSource>(this IQueryable<TSource> source, Func<TSource, bool> predicate, TSource defaultValue);

        public static TSource FirstOrDefault<TSource>(this IQueryable<TSource> source, TSource defaultValue);
        public static TSource FirstOrDefault<TSource>(this IQueryable<TSource> source, Func<TSource, bool> predicate, TSource defaultValue);

        public static TSource LastOrDefault<TSource>(this IQueryable<TSource> source, TSource defaultValue);
        public static TSource LastOrDefault<TSource>(this IQueryable<TSource> source, Func<TSource, bool> predicate, TSource defaultValue);
    }
}
```
## Add ReadOnlySpan<char> overloads to StringInfo APIs

**Approved** | [#runtime/28704](https://github.com/dotnet/runtime/issues/28704#issuecomment-724933993) | [Video](https://www.youtube.com/watch?v=hhwOM5MzoTA&t=1h46m52s)

* Returning the length instead of a span allows the method to be more versatile (ReadOnlySpan, Span, ReadOnlyMemory, Memory, etc)
* GetNextTextElementLength allows for better association in IntelliSense

```C#
namespace System.Globalization
{
    public partial class StringInfo
    {
        public static int GetNextTextElementLength(string str);
        public static int GetNextTextElementLength(string str, int index);
        public static int GetNextTextElementLength(ReadOnlySpan<char> str);
    }
}
```
