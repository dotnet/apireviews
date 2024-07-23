# API Review 07/23/2024

## Standardize pattern for exposing advanced configuration for compression streams

**Approved** | [#runtime/42820](https://github.com/dotnet/runtime/issues/42820#issuecomment-2246012201) | [Video](https://www.youtube.com/watch?v=__J_nrAyRyc&t=0h0m0s)


* ZlibCompressionStrategy => ZlibCompressionStrategy (consistent casing with ZLibStream)
* ZlibCompressionStrategy.Rle => RunLengthEncoding
* ZlibCompressionStrategy.DefaultStrategy => Default
* ZLibFlushMode is cut pending a scenario
* ZLibCompressionLevel is cut, just using the raw integer.
* Created ZLibCompressionOptions to allow for expanding to more options in the future
* ZLibCompressionOptions is a required parameter, consistent with the existing overloads.
* BrotliCompressionQuality => int property on a new class

```C#
namespace System.IO.Compression
{
    public enum ZLibCompressionStrategy
    {
        Default = 0,
        Filtered = 1,
        HuffmanOnly = 2,
        RunLengthEncoding = 3,
        Fixed = 4,
    }
    public sealed class ZLibCompressionOptions
    {
        public int CompressionLevel { get; set; }
        public ZLibCompressionStrategy CompressionStrategy { get; set; }
    }
    public sealed class ZLibStream : Stream
    {
        // CompressionMode.Compress
        public ZLibStream(Stream stream, ZLibCompressionOptions compressionOptions, bool leaveOpen = false);
    }
    public partial class DeflateStream : Stream
    {
        public DeflateStream(Stream stream, ZLibCompressionOptions compressionOptions, bool leaveOpen = false);
    }
    public partial class GZipStream : Stream
    {
        public GZipStream(Stream stream, ZLibCompressionOptions compressionOptions, bool leaveOpen = false);
    }


    public sealed class BrotliCompressionOptions
    {
        public int Quality { get; set; }
    }
    public sealed partial class BrotliStream : System.IO.Stream
    {
        public BrotliStream(Stream stream, BrotliCompressionOptions compressionOptions, bool leaveOpen = false) { }
    }
}
```
