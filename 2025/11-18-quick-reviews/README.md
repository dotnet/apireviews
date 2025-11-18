# API Review 11/18/2025

## Add ZipArchiveEntry.Open overload

**Approved** | [#runtime/101243](https://github.com/dotnet/runtime/issues/101243#issuecomment-3548995443) | [Video](https://www.youtube.com/watch?v=NzqIFvMFxWg&t=0h0m0s)

Looks good as proposed.

```csharp
namespace System.IO.Compression;

public partial class ZipArchiveEntry
{
    public Stream Open(FileAccess access);
}
```
## Make ZipArchiveEntry.CompressionMethod public

**Approved** | [#runtime/95909](https://github.com/dotnet/runtime/issues/95909#issuecomment-3549079330) | [Video](https://www.youtube.com/watch?v=NzqIFvMFxWg&t=0h13m3s)

* Pre-approved Zstandard
* We should remove any ZipCompressionMethod enum members that the platform doesn't support. 

```csharp
namespace System.IO.Compression;

public class ZipArchiveEntry
{
    public ZipCompressionMethod CompressionMethod { get; }
}

// Corresponds to the Compression Method described by APPNOTE.TXT section 4.4.5
public enum ZipCompressionMethod : ushort
{
    Stored = 0x0,
    Deflate = 0x8,
    Deflate64 = 0x9,
//    BZip2 = 0xC,
//    Lzma = 0xE,
//    Zstandard = 0x5D,
}
```
## Be able to specify window size in BrotliCompressionOptions and let use large window size as native brotli encoder

**Approved** | [#runtime/116227](https://github.com/dotnet/runtime/issues/116227#issuecomment-3549085412) | [Video](https://www.youtube.com/watch?v=NzqIFvMFxWg&t=0h33m42s)

Looks good as proposed

```csharp
namespace System.IO.Compression;

public sealed partial class BrotliCompressionOptions
{
    public int WindowSize { get; set; }
}
```
## Custom dictionary support for Brotli

**NeedsWork** | [#runtime/118784](https://github.com/dotnet/runtime/issues/118784#issuecomment-3549181545) | [Video](https://www.youtube.com/watch?v=NzqIFvMFxWg&t=0h36m31s)


* We think the ctor-based approach is better, since the dictionaries can only be set before work is performed.
  * A future API like BrotliDictionary.Combine can solve the "multiple" problem without new overloads.
* BrotliEncoder should still take the window parameter
* Need more scenarios for using the dictionary so we can understand the API

```csharp
namespace System.IO.Compression;

public sealed class BrotliDictionary : System.IDisposable
{
    internal BrotliDictionary();

    public static BrotliDictionary Create(ReadOnlySpan<byte> buffer);
    public static BrotliDictionary Create(ReadOnlySpan<byte> buffer, int quality);

    public void Dispose();
}

public sealed class BrotliCompressionOptions
{
    // If set, supersedes Quality
    public BrotliDictionary? Dictionary { get; set; }
}

public struct BrotliDecoder : System.IDisposable
{
    public BrotliDecoder(BrotliDictionary dictionary);
}

public struct BrotliEncoder : System.IDisposable
{
    public BrotliEncoder(BrotliDictionary dictionary, int window);
}

public class BrotliStream
{
    public BrotliStream(Stream stream, CompressionMode mode, BrotliDictionary dictionary, bool leaveOpen = false);
}
```
## `DecompressionMethods.Zstd` for web requests

**Approved** | [#runtime/112075](https://github.com/dotnet/runtime/issues/112075#issuecomment-3549191021) | [Video](https://www.youtube.com/watch?v=NzqIFvMFxWg&t=1h4m36s)

* We think Zstandard is a better name that Zstd

```diff
namespace System.Net;

[Flags]
public enum DecompressionMethods
{
    None = 0,
    All = ~None
    GZip = 0x1,
    Deflate = 0x2,
    Brotli = 0x4,
+   Zstandard = 0x8,
}
```
## Linq Join return tuple similar to Zip

**Approved** | [#runtime/120596](https://github.com/dotnet/runtime/issues/120596#issuecomment-3549247430) | [Video](https://www.youtube.com/watch?v=NzqIFvMFxWg&t=1h6m48s)

* Added Queryable
* Named the tuple elements
* Decided to skip ParallelEnumerable for now, since it didn't get the Zip enhancement.

```csharp
namespace System.Linq;

public static class Enumerable
{
    public static IEnumerable<(TOuter Outer, TInner Inner)> Join<TOuter, TInner, TKey>(
        this IEnumerable<TOuter> outer,
        IEnumerable<TInner> inner,
        Func<TOuter, TKey> outerKeySelector,
        Func<TInner, TKey> innerKeySelector);

    public static IEnumerable<(TOuter Outer, TInner? Inner)> LeftJoin<TOuter, TInner, TKey>(
        this IEnumerable<TOuter> outer,
        IEnumerable<TInner> inner,
        Func<TOuter, TKey> outerKeySelector,
        Func<TInner, TKey> innerKeySelector);

    public static IEnumerable<(TOuter? Outer, TInner Inner)> RightJoin<TOuter, TInner, TKey>(
        this IEnumerable<TOuter> outer,
        IEnumerable<TInner> inner,
        Func<TOuter, TKey> outerKeySelector,
        Func<TInner, TKey> innerKeySelector);
}

public static class Queryable
{
    public static IQueryable<(TOuter Outer, TInner Inner)> Join<TOuter, TInner, TKey>(
        this IQueryable<TOuter> outer,
        IEnumerable<TInner> inner,
        Expression<Func<TOuter, TKey>> outerKeySelector,
        Expression<Func<TInner, TKey>> innerKeySelector);

    public static IQueryable<(TOuter Outer, TInner? Inner)> LeftJoin<TOuter, TInner, TKey>(
        this IQueryable<TOuter> outer,
        IEnumerable<TInner> inner,
        Expression<Func<TOuter, TKey>> outerKeySelector,
        Expression<Func<TInner, TKey>> innerKeySelector);

    public static IQueryable<(TOuter? Outer, TInner Inner)> RightJoin<TOuter, TInner, TKey>(
        this IQueryable<TOuter> outer,
        IEnumerable<TInner> inner,
        Expression<Func<TOuter, TKey>> outerKeySelector,
        Expression<Func<TInner, TKey>> innerKeySelector);
}
```
## Add a GroupJoin overload returning a tuple of joined entries.

**Approved** | [#runtime/120587](https://github.com/dotnet/runtime/issues/120587#issuecomment-3549296682) | [Video](https://www.youtube.com/watch?v=NzqIFvMFxWg&t=1h19m4s)

* We think the IGrouping variant is better than the tuple variant.

```csharp
namespace System.Linq;

public static partial class Enumerable
{
    public static IEnumerable<IGrouping<TOuter, TInner>> GroupJoin<TOuter, TInner, TKey>(
        this IEnumerable<TOuter> outer,
        IEnumerable<TInner> inner,
        Func<TOuter, TKey> outerKeySelector,
        Func<TInner, TKey> innerKeySelector);
}

public static partial class Queryable
{
    public static IQueryable<IGrouping<TOuter, TInner>> GroupJoin<TOuter, TInner, TKey>(
        this IQueryable<TOuter> outer,
        IEnumerable<TInner> inner,
        Expression<Func<TOuter, TKey>> outerKeySelector,
        Expression<Func<TInner, TKey>> innerKeySelector);
}
```
## System.Net.Mime.MediaTypeMap

**Approved** | [#runtime/121017](https://github.com/dotnet/runtime/issues/121017#issuecomment-3549447735) | [Video](https://www.youtube.com/watch?v=NzqIFvMFxWg&t=1h32m36s)

* To maximize usability, we went with the non-Try approach, and overloaded with string inputs
* Changed the parameter name for MIME -> Extension from extension to pathOrExtension, the implementation will route through Path.GetExtension (or equivalent)

```csharp
namespace System.Net.Mime;

public static class MediaTypeMap
{
    // foo.pdf, .pdf, or pdf => application/pdf
    public static string? GetMediaType(string pathOrExtension);
    public static string? GetMediaType(ReadOnlySpan<char> pathOrExtension);

    // application/pdf => .pdf
    public static string? GetExtension(string mediaType);
    public static string? GetExtension(ReadOnlySpan<char> mediaType);
}
```
