# Quick Reviews 08/07/2020

## Span<char> from null-terminated char*

**Approved** | [#runtime/40202](https://github.com/dotnet/runtime/issues/40202#issuecomment-670625737) | [Video](https://www.youtube.com/watch?v=ub-XaCUH1n0&t=0h0m0s)

* Let's make it clear in the name what this method does.
* Let's have an overload that deals with UTF8
* Let's hold back UTF32 until we need it.
* We should also provide `strlen`-like APIs

```C#
namespace System.Runtime.InteropServices
{
    public partial class MemoryMarshal
    {
        public static Span<char> CreateFromNullTerminated(char* value);
        public static Span<byte> CreateFromNullTerminated(byte* value);
    }
}
namespace System
{
    public partial class Buffer
    {
        public unsafe static nuint GetStringLength(char* source);
        public unsafe static nuint GetStringLength(char* source, nuint maxLength);
        public unsafe static nuint GetStringLength(byte* source);
        public unsafe static nuint GetStringLength(byte* source, nuint maxLength);
    }
}
```
## Support generating random 64-bit values.

**Approved** | [#runtime/26741](https://github.com/dotnet/runtime/issues/26741#issuecomment-670633590) | [Video](https://www.youtube.com/watch?v=ub-XaCUH1n0&t=0h20m14s)

* Makes sense
* Let's also add `NextSingle()`

```C#
namespace System
{
    public class Random
    {
        public virtual long NextInt64();
        public virtual long NextInt64(long maxValue);
        public virtual long NextInt64(long minValue, long maxValue);
        public virtual float NextSingle();
    }
}
```

## Add File class method overloads for ReadOnlyMemory and ReadOnlySpan

**Rejected** | [#runtime/35054](https://github.com/dotnet/runtime/issues/35054#issuecomment-670642453) | [Video](https://www.youtube.com/watch?v=ub-XaCUH1n0&t=0h40m49s)

* It seems odd to combine a low-level concept like span/memory with very high-level APIs like "write all this data to file".
* The goal is to make it very convenient to write some data to a file, not performance.
* My concern is that these additions would "pollute" a core API that is frequently used by beginners. The API is designed to grow to expose users to more concepts, such as `StreamReader` and `FileStream`.
* If the arugment is that creating a file stream is too hard, for example, opening it for async, I'd rather we add dedicated methods for that, such as `OpenForAsync`.
* Unless there is a compelling reason for those APIs, we're inclined to say no.

```C#
namespace System.IO
{
    public static class File
    {
        public static ValueTask AppendAllLinesAsync(string path, IEnumerable<ReadOnlyMemory<char>> contents, CancellationToken cancellationToken = default);
        public static ValueTask AppendAllLinesAsync(string path, IEnumerable<ReadOnlyMemory<char>> contents, Encoding encoding, CancellationToken cancellationToken = default);
        public static void AppendAllText(string path, ReadOnlySpan<char> contents);
        public static void AppendAllText(string path, ReadOnlySpan<char> contents, Encoding encoding);
        public static ValueTask AppendAllTextAsync(string path, ReadOnlyMemory<char> contents, CancellationToken cancellationToken = default);
        public static ValueTask AppendAllTextAsync(string path, ReadOnlyMemory<char> contents, Encoding encoding, CancellationToken cancellationToken = default);
        public static ValueTask WriteAllLinesAsync(string path, IEnumerable<ReadOnlyMemory<char>> contents, CancellationToken cancellationToken = default);
        public static ValueTask WriteAllLinesAsync(string path, IEnumerable<ReadOnlyMemory<char>> contents, Encoding encoding, CancellationToken cancellationToken = default);
        public static void WriteAllText(string path, ReadOnlySpan<char> contents);
        public static void WriteAllText(string path, ReadOnlySpan<char> contents, Encoding encoding);
        public static ValueTask WriteAllTextAsync(string path, ReadOnlyMemory<char> contents, CancellationToken cancellationToken = default);
        public static ValueTask WriteAllTextAsync(string path, ReadOnlyMemory<char> contents, Encoding encoding, CancellationToken cancellationToken = default);
        public static void WriteAllBytes(string path, ReadOnlySpan<byte> bytes);
        public static ValueTask WriteAllBytesAsync(string path, ReadOnlyMemory<byte> bytes, CancellationToken cancellationToken = default);
    }
}
```

## [Uri] Add UriScheme Properties: SSH, FTPS, SFTP, WS, WSS

**Approved** | [#runtime/35180](https://github.com/dotnet/runtime/issues/35180#issuecomment-670645017) | [Video](https://www.youtube.com/watch?v=ub-XaCUH1n0&t=1h1m41s)

* The names and API shape makes sense.
* The only concern that we have is whether supporting those schemes has any implications on other parts of URI, such as in `UriParser` and populating defaults, such as ports. Is there any extra work we'd be signing up to support those? If so, we may want to be more selective what we support. @dotnet/ncl, could you double check?

```C#
namespace System
{
    public partial class Uri
    {
        public static readonly string UriSchemeWs;
        public static readonly string UriSchemeWss;
        public static readonly string UriSchemeSsh;
        public static readonly string UriSchemeTelnet;
        public static readonly string UriSchemeSftp;
        public static readonly string UriSchemeFtps;
    }
}
```

## Exposing plumbing for single instance application

**NeedsWork** | [#runtime/35827](https://github.com/dotnet/runtime/issues/35827#issuecomment-670647368) | [Video](https://www.youtube.com/watch?v=ub-XaCUH1n0&t=1h7m54s)

* The namespace is an odd choice
* It seems folks in this thread have concerns with the shape.
* @KathleenDollard @cston if this is something we want to pursue for .NET 6/MAUI we should sit down and design this a bit further.
## Add unsigned overloads for Unsafe Add/Subtract and Add/SubtractByteOffset

**Approved** | [#runtime/36232](https://github.com/dotnet/runtime/issues/36232#issuecomment-670651996) | [Video](https://www.youtube.com/watch?v=ub-XaCUH1n0&t=1h11m27s)

* This logically creates `UIntPtr` overloads, but prefer `nuint` for APIs like these.
* We won't be adding `uint`
* Looks good as proposed
* @tannergooding would like to support other cases, such as refs to things that contain pointers and `ref struct`, could you file a separate issue for that?

```C#
namespace System.Runtime.CompilerServices
{
    public static partial class Unsafe
    {
        public static ref T AddByteOffset<T>(ref T source, nuint byteOffset);
        public static ref T Add<T>(ref T source, nuint elementOffset);

        public static ref T SubtractByteOffset<T>(ref T source, nuint byteOffset);
        public static ref T Subtract<T>(ref T source, nuint elementOffset);
    }
}
```

