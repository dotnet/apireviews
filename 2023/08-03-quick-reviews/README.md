# API Review 08/03/2023

## [release/7.0-staging] Zlib: Add some protections to the allocator used by zlib

**Approved** | [#runtime/89532](https://github.com/dotnet/runtime/pull/89532) | [Video](https://www.youtube.com/watch?v=S57A79jybvs&t=0h0m0s)

## Add variation of TimeZoneInfo.GetSystemTimeZones() not sorted on DisplayName

**Approved** | [#runtime/88691](https://github.com/dotnet/runtime/issues/88691#issuecomment-1664370733) | [Video](https://www.youtube.com/watch?v=S57A79jybvs&t=0h0m52s)

* We discussed boolean vs an enumeration, and feel confident that the bool will last us a while.
* We then spent a long time on naming, and renamed `unsorted` to `skipSorting`

```C#
namespace System
{
    public sealed partial class TimeZoneInfo : IEquatable<TimeZoneInfo?>, ISerializable, IDeserializationCallback
    {
        // Existing
        //public static ReadOnlyCollection<TimeZoneInfo> GetSystemTimeZones();
        
        // New
        public static ReadOnlyCollection<TimeZoneInfo> GetSystemTimeZones(bool skipSorting);
    }
}
```
## Provide an API for configuring `JsonSerializerOptions` instances for reflection-based serialization.

**Approved** | [#runtime/89934](https://github.com/dotnet/runtime/issues/89934#issuecomment-1664399971) | [Video](https://www.youtube.com/watch?v=S57A79jybvs&t=0h27m54s)

Rather than a distinct name (largely because we couldn't come up with one), we added a bool parameter, where false matches the existing (AOT-friendly) behavior, and true means "just make it work".

If it feels "natural", consider [ConstantExpected(Min=true,Max=true)].

```C#
namespace System.Text.Json
{
    public partial class JsonSerializerOptions
    {
        // Existing
        // Throws IOE if TypeInfoResolver is null
        public void MakeReadOnly();

        [RequiresUnreferencedCode, RequiresDynamicCode]
        public void MakeReadOnly(bool populateMissingResolver);
    }  
}
```
## add ToolStripItem.SelectedChanged event

**Approved** | [#winforms/8548](https://github.com/dotnet/winforms/issues/8548#issuecomment-1664409644) | [Video](https://www.youtube.com/watch?v=S57A79jybvs&t=0h51m3s)

Looks good as proposed

```C#
namespace System.Windows.Forms
{
    public partial class ToolStripItem
    {
        public event EventHandler? SelectedChanged;
        protected virtual void OnSelectedChanged(EventArgs e);
    }
}
```
## Linq CountBy method

**Approved** | [#runtime/77716](https://github.com/dotnet/runtime/issues/77716#issuecomment-1664448577) | [Video](https://www.youtube.com/watch?v=S57A79jybvs&t=0h57m32s)

* We don't think LongCountBy would be called often enough to warrant adding it at this time (based on low call counts of LongCount)
* We think we'd be more likely to add AggregateBy than LongCountBy to solve the long-count-by needs, but also don't think that it's warranted at this time.
* We added the IQueryable variant, consistent with how we maintain Enumerable and Queryable in general.

```C#
namespace System.Linq;

public static partial class Enumerable
{
    public static IEnumerable<KeyValuePair<TKey, int>> CountBy<TSource, TKey>(
        this IEnumerable<TSource> source,
        Func<TSource, TKey> keySelector,
        IEqualityComparer<TKey>? keyComparer = null) where TKey : notnull;
}

public static partial class Queryable
{
    public static IQueryable<KeyValuePair<TKey, int>> CountBy<TSource, TKey>(
        this IQueryable<TSource> source,
        Expression<Func<TSource, TKey>> keySelector,
        IEqualityComparer<TKey>? keyComparer = null) where TKey : notnull;
}
```
## Extend `JsonIgnoreCondition`

**Approved** | [#runtime/66490](https://github.com/dotnet/runtime/issues/66490#issuecomment-1664470166) | [Video](https://www.youtube.com/watch?v=S57A79jybvs&t=1h27m44s)

None of us seem to be convinced that there's a real scenario here, but the shape as proposed is correct for what it's proposing.

```diff
namespace System.Text.Json.Serialization;

public enum JsonIgnoreCondition
{
  Never = 0,
  Always = 1,
  WhenWritingDefault = 2,
  WhenWritingNull = 3,
+ WhenWriting = 4,
+ WhenReading = 5,
}
```
## Add async parse overloads for `JsonNode` and `JsonElement`

**Approved** | [#runtime/88248](https://github.com/dotnet/runtime/issues/88248#issuecomment-1664473873) | [Video](https://www.youtube.com/watch?v=S57A79jybvs&t=1h44m45s)

Looks good as proposed

```C#
namespace System.Text.Json;

public class JsonNode
{
    public static Task<JsonNode?> ParseAsync(
        Stream utf8Json,
        JsonNodeOptions? nodeOptions = null,
        JsonDocumentOptions documentOptions = default,
        CancellationToken cancellationToken = default);
}
```
## Implement IDirectoryContents on PhysicalDirectoryInfo at MS.Extensions.FileProviders.Physical

**Approved** | [#runtime/86354](https://github.com/dotnet/runtime/issues/86354#issuecomment-1664476920) | [Video](https://www.youtube.com/watch?v=S57A79jybvs&t=1h47m53s)

Looks good as proposed

```diff
namespace Microsoft.Extensions.FileProviders.Physical
{
    public partial class PhysicalDirectoryInfo : IFileInfo
+       , IDirectoryContents  
    {
        // IDirectoryContents implementation
    }
```
