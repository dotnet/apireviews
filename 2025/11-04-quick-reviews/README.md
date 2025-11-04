# API Review 11/04/2025

## Kubernetes specialized Resource Monitoring

**Approved** | [#extensions/6496](https://github.com/dotnet/extensions/issues/6496#issuecomment-3487462021) | [Video](https://www.youtube.com/watch?v=7MoDnvIwtiE&t=0h0m0s)

* Guaranteed => Baseline
* Otherwise, looks good as proposed.

```csharp

// Microsoft.Extensions.Diagnostics

namespace Microsoft.Extensions.Diagnostics.ResourceMonitoring
{
    public abstract class ResourceQuotaProvider
    {
        public abstract ResourceQuota GetResourceQuota();
    }
}

// Microsoft.Extensions.Diagnostics

namespace Microsoft.Extensions.Diagnostics.ResourceMonitoring
{
    public sealed class ResourceQuota
    {
        public ulong MaxMemoryInBytes { get; set; }
        public double MaxCpuInCores { get; set; }
        public ulong BaselineMemoryInBytes { get; set; }
        public double BaselineCpuInCores { get; set; }
    }
}

// Microsoft.Extensions.Diagnostics.Kubernetes

namespace Microsoft.Extensions.DependencyInjection
{
    public static class KubernetesResourceQuotaServiceCollectionExtensions
    {
        public static IServiceCollection AddKubernetesResourceMonitoring(
            this IServiceCollection services,
            string? environmentVariablePrefix = default)
    }
}
```
## Add support for Zstandard to System.IO.Compression

**Approved** | [#runtime/59591](https://github.com/dotnet/runtime/issues/59591#issuecomment-3487837206) | [Video](https://www.youtube.com/watch?v=7MoDnvIwtiE&t=0h15m22s)


* ZstandardDecoder.GetMaxDecompressedLength: Changed return to long, and removed the Get in favor of the Try-Get.
  * Changed related methods on ZstandardEncoder to also be long.
* ZstandardDictionary.TrainFromSamples: Cut the `ReadOnlyMemory<ReadOnlyMemory<byte>>` overload, renamed to Train, updated `lengths` to `sampleLengths`
* ZstandardDictionary.Create: Removed the overload using `ZstandardDictionaryType`.  If we need it in the future, we can do it with method groups instead of an enum (Create, CreateRaw, Deserialize)
* Changed ZstandardEncoder and ZstandardDecoder to classes
* Changed ReferencePrefix to SetPrefix to show the overwrite semantics.
* ZstandardStream, decided against `ownsEncoder` parameters.
* Added SetSourceSize to ZstandardStream
* Decided isFinalBlock should be the last parameter to Compress even though it's not defaulted and after the `out`s (consistent with Brotli)

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
        public static int DefaultWindow { get; } // 23 
        public static int MinWindow { get; } // 10
        public static int MaxWindow { get; } // 31

        public static int DefaultQuality { get; } // defined by zstd implementation as 3
        public static int MaxQuality { get; } // ZSTD_maxCLevel
        public static int MinQuality { get; } // ZSTD_minCLevel

        public int Quality { get; set; };
        public ZstandardDictionary? Dictionary { get; set; }
        public int Window { get; set; };

        public int TargetBlockSize { get; set; }

        public bool AppendChecksum { get; set; }

        public bool EnableLongDistanceMatching { get; set; }
    }

    // standalone decoder implementation, closely copies BrotliDecoder design
    public partial class ZstandardDecoder : System.IDisposable
    {
        public ZstandardDecoder();
        public ZstandardDecoder(ZstandardDictionary dictionary);
        public ZstandardDecoder(int maxWindow);
        public ZstandardDecoder(ZstandardDictionary dictionary, int maxWindow);

        public OperationStatus Decompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);

        public void Dispose();
        public void Reset();

        public void ReferencePrefix(ReadOnlyMemory<byte> prefix);

        public static bool TryGetDecompressedLength(ReadOnlySpan<byte> data, out long length);

        public static bool TryDecompress(ReadOnlySpan<byte> source, ZstandardDictionary dictionary, Span<byte> destination, out int bytesWritten);
        public static bool TryDecompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
    }

    public partial class ZstandardEncoder : System.IDisposable
    {
        private object _dummy;
        private int _dummyPrimitive;

        public ZstandardEncoder();
        public ZstandardEncoder(int quality);
        public ZstandardEncoder(ZstandardDictionary dictionary);
        public ZstandardEncoder(int quality, int window);
        public ZstandardEncoder(ZstandardDictionary dictionary, int window);

        public ZstandardEncoder(ZstandardCompressionOptions options);

        public OperationStatus Compress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten, bool isFinalBlock);
        public System.Buffers.OperationStatus Flush(Span<byte> destination, out int bytesWritten);

        public void Dispose();

        public void Reset();

        public void SetPrefix(ReadOnlyMemory<byte> prefix);

        public void SetSourceSize(long size);

        public static long GetMaxCompressedLength(long inputSize);

        // one-off compression functions
        // note that `quality` and `dictionary` are mutually exclusive
        public static bool TryCompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
        public static bool TryCompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten, int quality, int window);

        public static bool TryCompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten, ZstandardDictionary dictionary, int window);
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

        public void SetSourceSize(long size);

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
