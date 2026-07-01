# API Review 06/30/2026

## Add Configuration References

**Approved** | [#runtime/127852](https://github.com/dotnet/runtime/issues/127852#issuecomment-4846355880) | [Video](https://www.youtube.com/watch?v=oguJUzjKP1c&t=0h0m0s)

* We removed the access model as over-complication.
* Since the customization is not expected in .NET 11 we didn't talk about it.
* Without the access model there's no reason for ConfigurationReferenceBuilder now, so there's no need for the callback, so we removed the parameter.
* It was highly debated if this feature should be opt-in (via AllowReferences) or opt-out (via AppContext?).  If it ends up being AppContext-opt-out then the feature needs no API.

```csharp
namespace Microsoft.Extensions.Configuration
{
    public static partial class ReferenceConfigurationBuilderExtensions
    {
         public static IConfigurationBuilder AllowReferences(this IConfigurationBuilder builder);
    }
}
```

## Make ZipArchiveEntry ZipVersionMadeByPlatform byte public

**Approved** | [#runtime/124755](https://github.com/dotnet/runtime/issues/124755#issuecomment-4846550127) | [Video](https://www.youtube.com/watch?v=oguJUzjKP1c&t=0h40m53s)

* Rather than expose the enum (and need to name all the things), let's just expose the ushort as the spec calls it:

```csharp
namespace System.IO.Compression
{
   public partial class ZipArchiveEntry
   {
        public ushort VersionMadeBy { get; }
   }
}
```
## HTTP Compression - Request Compression

**Approved** | [#runtime/46944](https://github.com/dotnet/runtime/issues/46944#issuecomment-4846858385) | [Video](https://www.youtube.com/watch?v=oguJUzjKP1c&t=1h1m26s)

* Rather than an enum-based dispatcher, let's just expose each type as a distinct HttpContent type.
* Let's leave Deflate out since it is unlikely to be needed.

```csharp
namespace System.Net.Http
{
    public sealed class ZstandardCompressedContent : System.Net.Http.HttpContent
    {
        public ZstandardCompressedContent(HttpContent content) { }
        public ZstandardCompressedContent(HttpContent content, ZstandardCompressionOptions compressionOptions) { }
    }

    public sealed class BrotliCompressedContent : System.Net.Http.HttpContent
    {
        public BrotliCompressedContent(HttpContent content) { }
        public BrotliCompressedContent(HttpContent content, BrotliCompressionOptions compressionOptions) { }
    }

    public sealed class GZipCompressedContent : System.Net.Http.HttpContent
    {
        public GZipCompressedContent(HttpContent content) { }
        public GZipCompressedContent(HttpContent content, ZLibCompressionOptions compressionOptions) { }
    }
}
```
## Mark Unsafe as caller-unsafe

**Approved** | [#runtime/126956](https://github.com/dotnet/runtime/issues/126956#issuecomment-4847122549) | [Video](https://www.youtube.com/watch?v=oguJUzjKP1c&t=1h37m58s)

* We discussed things like `Add<T>(void*, int)` and understand why it's not `unsafe`
* I (@bartonjs) adjusted the syntax to make it clear all we are doing is adding the unsafe keyword to the methoddecl.
* Looks good as proposed, modulo the syntax differentiation.

```diff
namespace System.Runtime.CompilerServices;

public static partial class Unsafe
{
+   unsafe
    public static ref T AddByteOffset<T>(ref T source, System.IntPtr byteOffset) where T : allows ref struct;

+   unsafe
    public static ref T AddByteOffset<T>(ref T source, nuint byteOffset) where T : allows ref struct;

+   unsafe
    public static ref T Add<T>(ref T source, int elementOffset) where T : allows ref struct;

+   unsafe
    public static ref T Add<T>(ref T source, System.IntPtr elementOffset) where T : allows ref struct;

+   unsafe
    public static ref T Add<T>(ref T source, nuint elementOffset) where T : allows ref struct;

+   unsafe
    public static ref T AsRef<T>(scoped ref readonly T source) where T : allows ref struct;

+   unsafe
    public static T? As<T>(object? o) where T : class?;

+   unsafe
    public static ref TTo As<TFrom, TTo>(ref TFrom source) where TFrom : allows ref struct where TTo : allows ref struct;

+   unsafe
    public static TTo BitCast<TFrom, TTo>(TFrom source) where TFrom : allows ref struct where TTo : allows ref struct;

+   unsafe
    public static void CopyBlock(ref byte destination, ref readonly byte source, uint byteCount);

+   unsafe
    public static void CopyBlockUnaligned(ref byte destination, ref readonly byte source, uint byteCount);

+   unsafe
    public static void InitBlock(ref byte startAddress, byte value, uint byteCount);

+   unsafe
    public static void InitBlockUnaligned(ref byte startAddress, byte value, uint byteCount);

+   unsafe
    public static T ReadUnaligned<T>(scoped ref readonly byte source) where T : allows ref struct;

+   unsafe
    public static void SkipInit<T>(out T value) where T : allows ref struct;

+   unsafe
    public static ref T SubtractByteOffset<T>(ref T source, System.IntPtr byteOffset) where T : allows ref struct;

+   unsafe
    public static ref T SubtractByteOffset<T>(ref T source, nuint byteOffset) where T : allows ref struct;

+   unsafe
    public static ref T Subtract<T>(ref T source, int elementOffset) where T : allows ref struct;

+   unsafe
    public static ref T Subtract<T>(ref T source, System.IntPtr elementOffset) where T : allows ref struct;

+   unsafe
    public static ref T Subtract<T>(ref T source, nuint elementOffset) where T : allows ref struct;

+   unsafe
    public static ref T Unbox<T>(object box) where T : struct;

+   unsafe
    public static void WriteUnaligned<T>(ref byte destination, T value) where T : allows ref struct;
}
```

