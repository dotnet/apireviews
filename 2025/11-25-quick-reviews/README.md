# API Review 11/25/2025

## Extend FakeLogCollector with waiting capabilities

**Approved** | [#extensions/5752](https://github.com/dotnet/extensions/issues/5752#issuecomment-3577021826) | [Video](https://www.youtube.com/watch?v=p857nTg9lTg&t=0h0m0s)


* Decided the built-in skip or take were unnecessary, just use Skip or Take, as appropriate.
* Specify `[EnumeratorCancellation]` if the implementation of this is based on `yield return`.

```csharp
namespace Microsoft.Extensions.Logging.Testing;

public partial class FakeLogCollector
{
    public IAsyncEnumerable<FakeLogRecord> GetLogsAsync(CancellationToken cancellationToken = default);
}
```
## Enhance ToDictionary with Duplicate Key Handling Options

**NeedsWork** | [#runtime/113096](https://github.com/dotnet/runtime/issues/113096#issuecomment-3577053162) | [Video](https://www.youtube.com/watch?v=p857nTg9lTg&t=0h27m1s)

API Review: This came up in discussion, but the feeling by the area owner was that it wasn't ready, so marking as needs-work.
## `TextWriter.WriteLineAsync` with `CancellationToken` overloads

**Approved** | [#runtime/106136](https://github.com/dotnet/runtime/issues/106136#issuecomment-3577108257) | [Video](https://www.youtube.com/watch?v=p857nTg9lTg&t=0h32m40s)

* Approved as non-virtual (they can defer to the ReadOnlyMemory implementation).  If we need virtual later we can do that.

```csharp
namespace System.IO;

public abstract class TextWriter
{
    public Task WriteAsync(string? value, CancellationToken token);
    public Task WriteLineAsync(CancellationToken token);
    public Task WriteLineAsync(string? value, CancellationToken token);
}
```
## Add support for Zstandard to System.IO.Compression

**Approved** | [#runtime/59591](https://github.com/dotnet/runtime/issues/59591#issuecomment-3577248883) | [Video](https://www.youtube.com/watch?v=p857nTg9lTg&t=0h47m36s)


* ZstandardDictionary.Train - change `ReadOnlySpan<int> sampleLengths` to `ReadOnlySpan<long>`
  * We didn't like this change.  sampleLengths.Sum() is less than or equal to samples.Length, so int is more natural.
    * The perf cost of int to size_t is likely insiginifcant compared to the operation, and this shouldn't be on a hotpath.
* `ZstandardDecoder.TryDecompress(ReadOnlySpan<byte> source, ZstandardDictionary dictionary, Span<byte> destination, out int bytesWritten)` move dictionary parameter to after the out
  * Approved
* `Zstandard(Encoder|Stream).SetSourceSize(long size)`
  * changed to SetSourceLength(long length)
* `ZstandardEncoder.GetMaxCompressedLength(int inputSize)`
  * Changed to `ZstandardEncoder.GetMaxCompressedLength(int inputLength)`
* `ZstandardDictionary.Train(ReadOnlySpan<byte> samples, ReadOnlySpan<int> sampleLengths, int maxDictionarySize)`
  * Left as-is.
* Seal Encoder/Decoder classes
  * So sealed.
* Window/WindowSize/WindowBits
  * The manpage for Zstandard calls the option `windowLog`, and it's ceil(log2(window))... so we recommend changing it to `WindowLog`

```csharp
namespace System.IO.Compression
{
    public sealed partial class ZstandardDictionary : System.IDisposable
    {
        internal ZstandardDictionary();

        public void Dispose();

        public static ZstandardDictionary Create(ReadOnlySpan<byte> buffer, int quality);
        public static ZstandardDictionary Create(ReadOnlySpan<byte> buffer);

        public static ZstandardDictionary Train(ReadOnlySpan<byte> samples, ReadOnlySpan<int> sampleLengths, int maxDictionarySize);

        public ReadOnlyMemory<byte> Data { get; }
    }

    public sealed partial class ZstandardCompressionOptions
    {
        public static int DefaultWindowLog { get; } // 23 
        public static int MinWindowLog { get; } // 10
        public static int MaxWindowLog { get; } // 31

        public static int DefaultQuality { get; } // defined by zstd implementation as 3
        public static int MaxQuality { get; } // ZSTD_maxCLevel
        public static int MinQuality { get; } // ZSTD_minCLevel

        public int Quality { get; set; };
        public ZstandardDictionary? Dictionary { get; set; }
        public int WindowLog { get; set; };

        public int TargetBlockSize { get; set; }

        public bool AppendChecksum { get; set; }

        public bool EnableLongDistanceMatching { get; set; }
    }

    // standalone decoder implementation, closely copies BrotliDecoder design
    public sealed class ZstandardDecoder : System.IDisposable
    {
        public ZstandardDecoder();
        public ZstandardDecoder(ZstandardDictionary dictionary);
        public ZstandardDecoder(int maxWindowLog);
        public ZstandardDecoder(ZstandardDictionary dictionary, int maxWindowLog);

        public OperationStatus Decompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);

        public void Dispose();
        public void Reset();

        public void ReferencePrefix(ReadOnlyMemory<byte> prefix);

        public static bool TryGetDecompressedLength(ReadOnlySpan<byte> data, out long length);

        public static bool TryDecompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten, ZstandardDictionary dictionary);
        public static bool TryDecompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
    }

    public sealed class ZstandardEncoder : System.IDisposable
    {
        private object _dummy;
        private int _dummyPrimitive;

        public ZstandardEncoder();
        public ZstandardEncoder(int quality);
        public ZstandardEncoder(ZstandardDictionary dictionary);
        public ZstandardEncoder(int quality, int windowLog);
        public ZstandardEncoder(ZstandardDictionary dictionary, int windowLog);

        public ZstandardEncoder(ZstandardCompressionOptions options);

        public OperationStatus Compress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten, bool isFinalBlock);
        public System.Buffers.OperationStatus Flush(Span<byte> destination, out int bytesWritten);

        public void Dispose();

        public void Reset();

        public void SetPrefix(ReadOnlyMemory<byte> prefix);

        public void SetSourceLength(long length);

        public static long GetMaxCompressedLength(long inputLength);

        // one-off compression functions
        // note that `quality` and `dictionary` are mutually exclusive
        public static bool TryCompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
        public static bool TryCompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten, int quality, int windowLog);

        public static bool TryCompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten, ZstandardDictionary dictionary, int windowLog);
    }

    // Wrapper around ZstandardEncoder/Decoder to provide Stream API
    public sealed partial class ZstandardStream : System.IO.Stream
    {
        // similar ctor members as BrotliStream
        // QUESTION: why don't we make leaveOpen always default to null?
        public ZstandardStream(Stream stream, CompressionMode mode);
        public ZstandardStream(Stream stream, CompressionMode mode, bool leaveOpen);
        // this ctor is needed to perform decompression with dictionary (but can be achieved by a ctor taking ZstandardDecoder listed below)
        public ZstandardStream(Stream stream, CompressionMode mode, ZstandardDictionary dictionary, bool leaveOpen = false);

        // these imply CompressionMode.Compress
        public ZstandardStream(Stream stream, CompressionLevel compressionLevel);
        public ZstandardStream(Stream stream, CompressionLevel compressionLevel, bool leaveOpen);
        public ZstandardStream(Stream stream, ZstandardCompressionOptions compressionOptions, bool leaveOpen = false);

        public ZstandardStream(System.IO.Stream stream, ZstandardDecoder decoder, bool leaveOpen = false);
        public ZstandardStream(System.IO.Stream stream, ZstandardEncoder encoder, bool leaveOpen = false);

        public void SetSourceLength(long length);

        public System.IO.Stream BaseStream { get; }
        public override bool CanRead { get; }
        public override bool CanSeek { get; }
        public override bool CanWrite { get; }
        public override long Length { get; }
        public override long Position { get; set; }
        public override System.IAsyncResult BeginRead(byte[] buffer, int offset, int count, System.AsyncCallback? callback, object? state);
        public override System.IAsyncResult BeginWrite(byte[] buffer, int offset, int count, System.AsyncCallback? callback, object? state);
        protected override void Dispose(bool disposing);
        public override System.Threading.Tasks.ValueTask DisposeAsync();
        public override int EndRead(System.IAsyncResult asyncResult);
        public override void EndWrite(System.IAsyncResult asyncResult);
        public override void Flush();
        public override System.Threading.Tasks.Task FlushAsync(System.Threading.CancellationToken cancellationToken);
        public override int Read(byte[] buffer, int offset, int count);
        public override int Read(Span<byte> buffer);
        public override System.Threading.Tasks.Task<int> ReadAsync(byte[] buffer, int offset, int count, System.Threading.CancellationToken cancellationToken);
        public override System.Threading.Tasks.ValueTask<int> ReadAsync(System.Memory<byte> buffer, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken));
        public override int ReadByte();
        public override long Seek(long offset, System.IO.SeekOrigin origin);
        public override void SetLength(long value);
        public override void Write(byte[] buffer, int offset, int count);
        public override void Write(ReadOnlySpan<byte> buffer);
        public override System.Threading.Tasks.Task WriteAsync(byte[] buffer, int offset, int count, System.Threading.CancellationToken cancellationToken);
        public override System.Threading.Tasks.ValueTask WriteAsync(System.ReadOnlyMemory<byte> buffer, System.Threading.CancellationToken cancellationToken = default(System.Threading.CancellationToken));
        public override void WriteByte(byte value);
    }
}
```
