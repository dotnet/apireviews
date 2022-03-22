# API Review 03/22/2022

## Let consumers of MemoryCache access metrics

**Approved** | [#runtime/50406](https://github.com/dotnet/runtime/issues/50406#issuecomment-1075417371) | [Video](https://www.youtube.com/watch?v=M5Lhf1G3SgQ&t=0h0m0s)

* Looks good as proposed

```C#
namespace Microsoft.Extensions.Caching.Memory;

public partial class MemoryCacheOptions : IOptions<MemoryCacheOptions>
{
    public bool TrackStatistics { get; set; }
}

// Already approved API:
//
// public static partial class MemoryCache
// {
//     public MemoryCacheStatistics? GetCurrentStatistics();
// }
// 
// public partial interface IMemoryCache
// {
// #if NET_7_OR_GREATER
//      public MemoryCacheStatistics? GetCurrentStatistics() => null;
// #endif
// }
// 
// public partial class MemoryCacheStatistics
// {
//     public MemoryCacheStatistics() {}
//     
//     public long TotalMisses { get; init; } 
//     public long TotalHits { get; init; }
//     public long CurrentEntryCount { get; init; }
//     public long? CurrentEstimatedSize { get; init; }
// }
```
## Generic Rate Limiter

**Approved** | [#runtime/65400](https://github.com/dotnet/runtime/issues/65400#issuecomment-1075473959) | [Video](https://www.youtube.com/watch?v=M5Lhf1G3SgQ&t=0h16m11s)

* The code sample is show casing a problematic pattern because it allows an attacker to flood the internal dictionary with data they control (i.e. they can flood the server with random URLs causing the app to OOM). It might be worthwhile to invest in an analyzer that detects known bad patterns and warns about them (e.g. passing in the request URI directly as a key).
    ```C#
    else
    {
        return RateLimitPartition.CreateConcurrencyLimiter(resource.RequestUri.ToString(), s => new ConcurrencyLimiterOptions(...));
    }
    ```
* `PartitionedRateLimiter.Create` should allow to pass in an `IEqualityComparer<TPartitionKey>`
* `RateLimitPartition<TKey>` should probably be a struct and at least expose the key as a get-only property, mostly for debugging purposes
* `PartitionedRateLimiterExtensions` should be removed -- we already have the methods on `PartitionedRateLimiter`
    - `AdaptPartitionedRateLimiter` should just be an instance method
    - `CreateChainedRateLimiter` should just be `CreateChained`. Params seem fine.
* `ReplenishingRateLimiter` and making `TokenBucketRateLimiter` seems fine.
* Consider rename `TKey` to just `T`

```C#
namespace System.Threading.RateLimiting;

public sealed class PartitionedRateLimiter
{
    public static PartitionedRateLimiter<TResource> Create<TResource, TPartitionKey>(
        Func<TResource, RateLimitPartition<TPartitionKey>> partitioner,
        IEqualityComparer<TPartitionKey> equalityComparer = null) where TPartitionKey : notnull;

    public PartitionedRateLimiter<TOuter> TranslateKey<TOuter, TInner>(
        Func<TOuter, TInner> keyAdapter);

    public static PartitionedRateLimiter<TResource> CreateChained<TResource>(
        params PartitionedRateLimiter<TResource>[] limiters);
}

public static class RateLimitPartition
{
    public static RateLimitPartition<TKey> Create<TKey>(
        TKey partitionKey, Func<TKey, RateLimiter> factory);
    public static RateLimitPartition<TKey> CreateConcurrencyLimiter<TKey>(
        TKey partitionKey, Func<TKey, ConcurrencyLimiterOptions> factory);
    public static RateLimitPartition<TKey> CreateNoLimiter<TKey>(TKey partitionKey);
    public static RateLimitPartition<TKey> CreateTokenBucketLimiter<TKey>(
        TKey partitionKey,
        Func<TKey, TokenBucketRateLimiterOptions> factory);
}

public struct RateLimitPartition<TKey>
{
    public RateLimitPartition(TKey partitionKey, Func<TKey, RateLimiter> factory);
    public TKey PartitionKey { get; }
}

public partial class RateLimiter
{
    public abstract TimeSpan? IdleDuration { get; }
}

public abstract class ReplenishingRateLimiter : RateLimiter
{
    public abstract TimeSpan ReplenishmentPeriod { get; }
    public abstract bool IsAutoReplenishing { get; }
    public abstract bool TryReplenish();
}

public partial class TokenBucketRateLimiter : ReplenishingRateLimiter
{   
}

```
## APIs to support tar archives

**NeedsWork** | [#runtime/65951](https://github.com/dotnet/runtime/issues/65951#issuecomment-1075547345) | [Video](https://www.youtube.com/watch?v=M5Lhf1G3SgQ&t=1h13m46s)

* We need to figure how `System.Formats.Tar` interacts with compression
    - We could add overload to `TarFile` that takes stream so that the caller can construct the appropriate compression stream
* `TarReader`
    - Shouldn't read the attributes in the ctor and instead should defer this when the first entry is read
    - Having the property return `null` before first read make sense. Formats that don't support them can return an empty collection after construction.
* `TarWriter` 
    - Should take the attributes as an `IEnumerable`
    - `bool leaveOpen` should go after the attributes such that it's always last
    - We should make `AddFile` an overload of `WriteEntry` and change the parameter to `path` as it also works for directories
* It was suggested in chat to consider the notion of a tar entry builder

```C#
namespace System.Formats.Tar;

public static class TarFile
{
    public static void CreateFromDirectory(string sourceDirectoryName, string destinationArchiveFileName, bool includeBaseDirectory);
    public static Task CreateFromDirectoryAsync(string sourceDirectoryName, string destinationArchiveFileName, bool includeBaseDirectory, CancellationToken cancellationToken = default);

    public static void ExtractToDirectory(string sourceArchiveFileName, string destinationDirectoryName, bool overwriteFiles);
    public static Task ExtractToDirectoryAsync(string sourceArchiveFileName, string destinationDirectoryName, bool overwriteFiles, CancellationToken cancellationToken = default);
}
public enum TarEntryType : byte
{
    OldRegularFile = '\0',
    RegularFile = '0',
    HardLink = '1',
    SymbolicLink = '2',
    CharacterDevice = '3',
    BlockDevice = '4',
    Directory = '5',
    Fifo = '6',
    GlobalExtendedAttributes = 'g',
    ExtendedAttributes = 'x',
    ContiguousFile = '7',
    DirectoryList = 'D',
    LongLink = 'K',
    LongPath = 'L',
    MultiVolume = 'M',
    RenamedOrSymlinked = 'N',
    SparseFile = 'S',
    TapeVolume = 'T',
}
public enum TarFormat
{
    Unknown = 0, 
    V7 = 1,
    Ustar = 2,
    Pax = 3,
    Gnu = 4,
}
public sealed class TarReader : IDisposable, IAsyncDisposable
{
    public TarReader(Stream archiveStream, bool leaveOpen = false);
    public TarFormat Format { get; }
    public IReadOnlyDictionary<string, string>? GlobalExtendedAttributes { get; }
    public void Dispose();
    public ValueTask DisposeAsync();
    public TarEntry? GetNextEntry(bool copyData = false);
    public ValueTask<TarEntry?> GetNextEntryAsync(bool copyData = false, CancellationToken cancellationToken = default);
}
public sealed class TarWriter : IDisposable, IAsyncDisposable
{
    public TarWriter(Stream archiveStream, IEnumerable<KeyValuePair<string, string>>? globalExtendedAttributes = null, bool leaveOpen = false);
    public TarWriter(Stream archiveStream, TarFormat archiveFormat, bool leaveOpen = false);
    public TarFormat Format { get; }
    public void Dispose();
    public ValueTask DisposeAsync();
    public void WriteEntry(TarEntry entry);
    public Task WriteEntryAsync(TarEntry entry, CancellationToken cancellationToken = default);
    public void WriteEntry(string path, string? entryName = null);
    public Task WriteEntryAsync(string path, string? entryName = null, CancellationToken cancellationToken = default);
}
public abstract class TarEntry
{
    internal TarEntry();
    public int Checksum { get; }
    public Stream? DataStream { get; set; }
    public TarEntryType EntryType { get; }
    public int Gid { get; set; }
    public long Length { get; }
    public string LinkName { get; set; }
    public UnixFileMode Mode { get; set; }
    public DateTimeOffset MTime { get; set; }
    public string Name { get; set; }
    public int Uid { get; set; }
    public void ExtractToFile(string destinationFileName, bool overwrite);
    public Task ExtractToFileAsync(string destinationFileName, bool overwrite, CancellationToken cancellationToken = default);
    public override string ToString();
}
public sealed class TarEntryV7 : TarEntry
{
    public TarEntryV7(TarEntryType entryType, string entryName);
}
public abstract class TarEntryPosix : TarEntry
{
    internal TarEntryPosix();
    public int DeviceMajor { get; set; }
    public int DeviceMinor { get; set; }
    public string GName { get; set; }
    public string UName { get; set; }
    public override string ToString();
}
public sealed class TarEntryUstar : TarEntryPosix
{
    public TarEntryUstar(TarEntryType entryType, string entryName);
}
public sealed class TarEntryPax : TarEntryPosix
{
    public TarEntryPax(TarEntryType entryType, string entryName, IEnumerable<KeyValuePair<string, string>>? extendedAttributes);
    public IReadOnlyDictionary<string, string> ExtendedAttributes { get; }
}
public sealed class TarEntryGnu : TarEntryPosix
{
    public TarEntryGnu(TarEntryType entryType, string entryName);
    public DateTimeOffset ATime { get; set; }
    public DateTimeOffset CTime { get; set; }
}

namespace System.IO;

[Flags]
public enum UnixFileMode
{
    None = 0,
    OtherExecute = 1,
    OtherWrite = 2,
    OtherRead = 4,
    GroupExecute = 8,
    GroupWrite = 16,
    GroupRead = 32,
    UserExecute = 64,
    UserWrite = 128,
    UserRead = 256,
    StickyBit = 512,
    GroupSpecial = 1024,
    UserSpecial = 2048,
}
```
