# API Review 03/11/2025

## Predefined `InlineArrayX<T>` types up to some X

**Approved** | [#runtime/111973](https://github.com/dotnet/runtime/issues/111973#issuecomment-2715156288) | [Video](https://www.youtube.com/watch?v=ySD35yIvZa0&t=0h0m0s)

* Using Func as a guide, we will go ahead and just go up to 15.
* Neither 0 nor 1 should be needed, so they're cut.

```C#
namespace System.Runtime.CompilerServices;

[InlineArray(2)]
public struct InlineArray2<T>
{
    private T t;
}
[InlineArray(3)]
public struct InlineArray3<T>
{
    private T t;
}
[InlineArray(4)]
public struct InlineArray4<T>
{
    private T t;
}
[InlineArray(5)]
public struct InlineArray5<T>
{
    private T t;
}
[InlineArray(6)]
public struct InlineArray6<T>
{
    private T t;
}
[InlineArray(7)]
public struct InlineArray7<T>
{
    private T t;
}
[InlineArray(8)]
public struct InlineArray8<T>
{
    private T t;
}
[InlineArray(9)]
public struct InlineArray9<T>
{
    private T t;
}
[InlineArray(10)]
public struct InlineArray10<T>
{
    private T t;
}
[InlineArray(11)]
public struct InlineArray11<T>
{
    private T t;
}
[InlineArray(12)]
public struct InlineArray12<T>
{
    private T t;
}
[InlineArray(13)]
public struct InlineArray13<T>
{
    private T t;
}
[InlineArray(14)]
public struct InlineArray14<T>
{
    private T t;
}
[InlineArray(15)]
public struct InlineArray15<T>
{
    private T t;
}

```
## Add async ZipFile APIs

**Approved** | [#runtime/1541](https://github.com/dotnet/runtime/issues/1541#issuecomment-2715269236) | [Video](https://www.youtube.com/watch?v=ySD35yIvZa0&t=0h18m30s)


* ZipArchive.GetEntriesAsync and GetEntryAsync can be replaced with a single EnsureLoadedAsync as they only do work once
  * Since the new CreateAsync can just do the full reading before exiting, this method isn't needed, either.
* Since the ZipArchive constructor does I/O it needs a static CreateAsync method to avoid doing synchronous I/O
* ZipArchiveEntry.Delete doesn't actually do I/O, so DeleteAsync is not needed.

```diff
- public partial class ZipArchive : IDisposable
+ public partial class ZipArchive : IAsyncDisposable, IDisposable
```

```C#
public partial class ZipArchive
{
    public ValueTask DisposeAsync();
    protected virtual ValueTask DisposeAsyncCore();

    public static Task<ZipArchive> CreateAsync(Stream stream, ZipArchiveMode mode, bool leaveOpen = false, Encoding? entryNameEncoding = null, CancellationToken cancellationToken = default);
}

public partial class ZipArchiveEntry
{
    public Task<Stream> OpenAsync(CancellationToken cancellationToken = default);
}

public static partial class ZipFile
{
    public static Task CreateFromDirectoryAsync(string sourceDirectoryName, Stream destination, CancellationToken cancellationToken = default);

    public static Task CreateFromDirectoryAsync(string sourceDirectoryName, Stream destination, CompressionLevel compressionLevel, bool includeBaseDirectory, CancellationToken cancellationToken = default);

    public static Task CreateFromDirectoryAsync(string sourceDirectoryName, Stream destination, CompressionLevel compressionLevel, bool includeBaseDirectory, Encoding? entryNameEncoding, CancellationToken cancellationToken = default);

    public static Task CreateFromDirectoryAsync(string sourceDirectoryName, string destinationArchiveFileName, CancellationToken cancellationToken = default);

    public static Task CreateFromDirectoryAsync(string sourceDirectoryName, string destinationArchiveFileName, CompressionLevel compressionLevel, bool includeBaseDirectory, CancellationToken cancellationToken = default);

    public static Task CreateFromDirectoryAsync(string sourceDirectoryName, string destinationArchiveFileName, CompressionLevel compressionLevel, bool includeBaseDirectory, Encoding? entryNameEncoding, CancellationToken cancellationToken = default);

    public static Task ExtractToDirectoryAsync(Stream source, string destinationDirectoryName, CancellationToken cancellationToken = default);

    public static Task ExtractToDirectoryAsync(Stream source, string destinationDirectoryName, bool overwriteFiles, CancellationToken cancellationToken = default);

    public static Task ExtractToDirectoryAsync(Stream source, string destinationDirectoryName, Encoding? entryNameEncoding, CancellationToken cancellationToken = default);

    public static Task ExtractToDirectoryAsync(Stream source, string destinationDirectoryName, Encoding? entryNameEncoding, bool overwriteFiles, CancellationToken cancellationToken = default);

    public static Task ExtractToDirectoryAsync(string sourceArchiveFileName, string destinationDirectoryName, CancellationToken cancellationToken = default);

    public static Task ExtractToDirectoryAsync(string sourceArchiveFileName, string destinationDirectoryName, bool overwriteFiles, CancellationToken cancellationToken = default);

    public static Task ExtractToDirectoryAsync(string sourceArchiveFileName, string destinationDirectoryName, Encoding? entryNameEncoding, CancellationToken cancellationToken = default);

    public static Task ExtractToDirectoryAsync(string sourceArchiveFileName, string destinationDirectoryName, Encoding? entryNameEncoding, bool overwriteFiles, CancellationToken cancellationToken = default);

    public static Task<ZipArchive> OpenAsync(string archiveFileName, ZipArchiveMode mode, CancellationToken cancellationToken = default);

    public static Task<ZipArchive> OpenAsync(string archiveFileName, ZipArchiveMode mode, Encoding? entryNameEncoding, CancellationToken cancellationToken = default);

    public static Task<ZipArchive> OpenReadAsync(string archiveFileName, CancellationToken cancellationToken = default);
}
  
public static partial class ZipFileExtensions
{
    public static Task<ZipArchiveEntry> CreateEntryFromFileAsync(this ZipArchive destination, string sourceFileName, string entryName, CancellationToken cancellationToken = default);

    public static Task<ZipArchiveEntry> CreateEntryFromFileAsync(this ZipArchive destination, string sourceFileName, string entryName, CompressionLevel compressionLevel, CancellationToken cancellationToken = default);

    public static Task ExtractToDirectoryAsync(this ZipArchive source, string destinationDirectoryName, CancellationToken cancellationToken = default);

    public static Task ExtractToDirectoryAsync(this ZipArchive source, string destinationDirectoryName, bool overwriteFiles, CancellationToken cancellationToken = default);

    public static Task ExtractToFileAsync(this ZipArchiveEntry source, string destinationFileName, CancellationToken cancellationToken = default);

    public static Task ExtractToFileAsync(this ZipArchiveEntry source, string destinationFileName, bool overwrite, CancellationToken cancellationToken = default);
}
```
## Provide trim-safe overload for `ValidationContext` construction

**Approved** | [#runtime/113134](https://github.com/dotnet/runtime/issues/113134#issuecomment-2715310131) | [Video](https://www.youtube.com/watch?v=ySD35yIvZa0&t=0h58m11s)


* The type has three existing constructors, which did suggest using optional parameters on this fouth one was reasonable, but usage data says that the average caller is using the serviceProvider+items constructor, so no defaults are really needed.

```C#
namespace System.ComponentModel.DataAnnotations;

public sealed class ValidationContext
{
    public ValidationContext(object instance, string displayName, IServiceProvider? serviceProvider, IDictionary<object, object?>? items)
}
```
## Add constructor to StreamReader and StreamWriter accepting ArrayPools

**Rejected** | [#runtime/107490](https://github.com/dotnet/runtime/issues/107490#issuecomment-2715349860) | [Video](https://www.youtube.com/watch?v=ySD35yIvZa0&t=1h11m13s)

We have yet to add ArrayPool into construction patterns in the BCL, and there doesn't seem to be a strong enough compelling reason to do so at this time.

We can revisit this later if we change our minds, of course.
## Ordered evaluation in FileSystemGlobbing Matcher

**Approved** | [#runtime/109408](https://github.com/dotnet/runtime/issues/109408#issuecomment-2715409854) | [Video](https://www.youtube.com/watch?v=ySD35yIvZa0&t=1h21m26s)

* None of the API changes for MatcherContext need to be public.  They should be withheld until there's a compelling reason.
* "ordered" doesn't clearly refer to how the inclusions/exclusions are applied, as it could be about whether the output is sorted.  "preserveFilterOrder" seems an acceptable alternative
* We seem to have landed on just adding one new ctor, with defaulted parameters
* Based on comparisonType already not being exposed via a property, we will omit the preserveFilterOrder value being exposed as a property.

```C#
namespace Microsoft.Extensions.FileSystemGlobbing;

public partial class Matcher
{
    public Matcher(StringComparison comparisonType = StringComparison.OrdinalIgnoreCase, bool preserveFilterOrder = false) { }
}
```
