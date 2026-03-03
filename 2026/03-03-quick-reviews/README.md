# API Review 03/03/2026

## File: low-level API for creating pipe

**Approved** | [#runtime/122806](https://github.com/dotnet/runtime/issues/122806#issuecomment-3992948464) | [Video](https://www.youtube.com/watch?v=Yf-ibrthOOQ&t=0h0m0s)

* Added "Handle" suffix to the `out`s
* Discussed FileType vs FileKind, and kept it as Type, since we used that in Json
* FileType probably wants to be exposed from FileSystemInfo in a followup proposal
* The options bag may be added in the future.  When added, it needs to have consideration for AnonymousPipeServerStream, and similarly for Named pipes.
* Late edit: FileType => FileHandleType, and changed GetFileType() to a Type property.

```csharp
namespace System.IO
{
    public enum FileHandleType
    {
        Unknown = 0,
        RegularFile,
        Pipe,
        Socket,
        CharacterDevice,
        Directory,
        SymbolicLink,
        BlockDevice,
    }
}

namespace Microsoft.Win32.SafeHandles
{
    public sealed partial class SafeFileHandle
    {
        public FileHandleType Type { get; }
        public static void CreateAnonymousPipe(out SafeFileHandle readHandle, out SafeFileHandle writeHandle, bool asyncRead = false, bool asyncWrite = false);
    }
}
```
## Metrics for M.E.Caching.MemoryCache

**Approved** | [#runtime/124140](https://github.com/dotnet/runtime/issues/124140#issuecomment-3993004086) | [Video](https://www.youtube.com/watch?v=Yf-ibrthOOQ&t=0h56m42s)

* Recommend naming the meter Microsoft.Extensions.Caching.Memory.MemoryCache (match the type)
* We think it's goodness to make the ILoggerFactory parameter in the 2-arity ctor null-accepting instead of null-rejecting.

```csharp
namespace Microsoft.Extensions.Caching.Memory;

public partial class MemoryCacheStatistics
{
    long TotalEvictions { get; init; }
}

public partial class MemoryCacheOptions
{
    public string Name { get; set; } = "Default";
}

public class MemoryCache
{
    public MemoryCache(IOptions<MemoryCacheOptions> optionsAccessor, ILoggerFactory? loggerFactory, IMeterFactory? meterFactory)
}
```
## Add Deflate, ZLib and GZip encoder/decoder APIs

**Approved** | [#runtime/62113](https://github.com/dotnet/runtime/issues/62113#issuecomment-3993147258) | [Video](https://www.youtube.com/watch?v=Yf-ibrthOOQ&t=1h2m5s)

Looks good as proposed.

```csharp
namespace System.IO.Compression
{
    public sealed partial class ZLibCompressionOptions
    {
        public static int DefaultWindowLog { get; }
        public static int MaxWindowLog { get; }
        public static int MinWindowLog { get; }

        public int WindowLog { get; set; }
   }
    public sealed partial class DeflateDecoder : System.IDisposable
    {
        public DeflateDecoder();
        public System.Buffers.OperationStatus Decompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesConsumed, out int bytesWritten);
        public void Dispose();
        public static bool TryDecompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten);
    }
    public sealed partial class DeflateEncoder : System.IDisposable
    {
        public DeflateEncoder();
        public DeflateEncoder(int quality);
        public DeflateEncoder(int quality, int windowLog);
        public DeflateEncoder(System.IO.Compression.ZLibCompressionOptions options);
        public System.Buffers.OperationStatus Compress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesConsumed, out int bytesWritten, bool isFinalBlock);
        public void Dispose();
        public System.Buffers.OperationStatus Flush(System.Span<byte> destination, out int bytesWritten);
        public static long GetMaxCompressedLength(long inputLength);
        public static bool TryCompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten);
        public static bool TryCompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten, int quality);
        public static bool TryCompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten, int quality, int windowLog);
    }
    public sealed partial class GZipDecoder : System.IDisposable
    {
        public GZipDecoder();
        public System.Buffers.OperationStatus Decompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesConsumed, out int bytesWritten);
        public void Dispose();
        public static bool TryDecompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten);
    }
    public sealed partial class GZipEncoder : System.IDisposable
    {
        public GZipEncoder();
        public GZipEncoder(int quality);
        public GZipEncoder(int quality, int windowLog);
        public GZipEncoder(System.IO.Compression.ZLibCompressionOptions options);
        public System.Buffers.OperationStatus Compress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesConsumed, out int bytesWritten, bool isFinalBlock);
        public void Dispose();
        public System.Buffers.OperationStatus Flush(System.Span<byte> destination, out int bytesWritten);
        public static long GetMaxCompressedLength(long inputLength);
        public static bool TryCompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten);
        public static bool TryCompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten, int quality);
        public static bool TryCompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten, int quality, int windowLog);
    }
    public sealed partial class ZLibDecoder : System.IDisposable
    {
        public ZLibDecoder();
        public System.Buffers.OperationStatus Decompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesConsumed, out int bytesWritten);
        public void Dispose();
        public static bool TryDecompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten);
    }
    public sealed partial class ZLibEncoder : System.IDisposable
    {
        public ZLibEncoder();
        public ZLibEncoder(int quality);
        public ZLibEncoder(int quality, int windowLog);
        public ZLibEncoder(System.IO.Compression.ZLibCompressionOptions options);
        public System.Buffers.OperationStatus Compress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesConsumed, out int bytesWritten, bool isFinalBlock);
        public void Dispose();
        public System.Buffers.OperationStatus Flush(System.Span<byte> destination, out int bytesWritten);
        public static long GetMaxCompressedLength(long inputLength);
        public static bool TryCompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten);
        public static bool TryCompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten, int quality);
        public static bool TryCompress(System.ReadOnlySpan<byte> source, System.Span<byte> destination, out int bytesWritten, int quality, int windowLog);
    }
}
```
