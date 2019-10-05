# Quick Reviews 12/19/2017

## Add Brotli Compression to CoreFX

**Approved** | [#corefx/25785](https://github.com/dotnet/corefx/issues/25785#issuecomment-352921246) | [Video](https://www.youtube.com/watch?v=IIKqOagRdWA&t=-2h-12m-7s)

We reviewed the API and made some minor tweaks:

```csharp
public partial class BrotliStream : Stream
{
    public BrotliStream(Stream stream, CompressionLevel compressionLevel);
    public BrotliStream(Stream stream, CompressionLevel compressionLevel, bool leaveOpen);
    public BrotliStream(Stream stream, CompressionMode mode);
    public BrotliStream(Stream stream, CompressionMode mode, bool leaveOpen);
    public Stream BaseStream { get; }

    // We don't think we need those for now. Not having aligns the type with DeflateStream/GzipStream
    // public BrotliStream(Stream stream, CompressionLevel compressionLevel, bool leaveOpen, int bufferSize);
    // public BrotliStream(Stream stream, CompressionMode mode, bool leaveOpen, int bufferSize);

    // Overrides omitted for clarity
}
// We discussed making these guys classes and derive from CFO ourselves instead of wrapping a SafeHandle
// but we decided against it due to complexity and only minor savings (like when these are boxed)
public struct BrotliDecoder : IDisposable
{
    public OperationStatus Decompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);
    public void Dispose();

    public static bool TryDecompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
}
public struct BrotliEncoder : IDisposable
{
	BrotliEncoder(int quality, int window);

    public OperationStatus Compress(ReadOnlySpan<byte> source,
    	                            Span<byte> destination,
    	                            out int bytesConsumed,
    	                            out int bytesWritten,
    	                            bool isFinalBlock);
    public OperationStatus Flush(Span<byte> destination, out int bytesWritten);

    public void Dispose();

    public static bool TryCompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
    public static bool TryCompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten, int quality, int window);
    public static int GetMaxCompressedLength(int length);
}
```
