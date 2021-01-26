# Quick Reviews 01/26/2021

## Add Status field to Activity API

**Approved** | [#runtime/46689](https://github.com/dotnet/runtime/issues/46689#issuecomment-767748332) | [Video](https://www.youtube.com/watch?v=efvl_wtYSw8&t=0h0m0s)

* It seems problematic to have well-known fields/properties on `ActivityStatus` because it encourages code like `activity.Status == ActivityStatus.Ok` which would break if the description changes, which seems odd. At the same time, it would be odd for `ActivityStatus`'s implementation of equality to ignore the description.
    - One option is to remove the well-known values from `ActivityStatus`
    - Another option is to remove `ActivityStatus` and expose code and description as separate properties on `Activity`. If we are concerned about extending the code, we can make `ActivityStatusCode` a struct with well known fields. The downside is that this allows for impossible states, such as an unset code but a non-null description.
* We concluded that two properties plus a method to throw on invalid combinations.

```C#
namespace System.Diagnostics
{
    public enum ActivityStatusCode
    {
        Unset = 0,
        Ok = 1,
        Error = 2
    }
    public partial class Activity
    {
        public ActivityStatus Status { get; }
        public string? StatusDescription { get; }
        public void SetStatus(ActivityStatusCode code, string? description = null);
    }
}
```
## LINQ APIs to enable C# 8.0 index and range for IEnumerable<T>

**Approved** | [#runtime/28776](https://github.com/dotnet/runtime/issues/28776#issuecomment-767752600) | [Video](https://www.youtube.com/watch?v=efvl_wtYSw8&t=0h36m27s)

* Looks good as proposed.
* We're not providing an implementation for PLinq
* Query providers can choose implement the method, if no there will be some default behavior.

```C#
namespace System.Linq
{
    public static partial class Enumerable
    {
        public static TSource ElementAt<TSource>(this IEnumerable<TSource> source, Index index);
        public static TSource ElementAtOrDefault<TSource>(this IEnumerable<TSource> source, Index index);
        public static IEnumerable<TSource> Take<TSource>(this IEnumerable<TSource> source, Range range);
    }
    public static partial class Queryable
    {
        public static TSource ElementAt<TSource>(this IQueryable<TSource> source, Index index);
        public static TSource ElementAtOrDefault<TSource>(this IQueryable<TSource> source, Index index);
        public static IQueryable<TSource> Take<TSource>(this IQueryable<TSource> source, Range range);
    }
}
```
## Add analyzer: Call async methods when in an async method

**NeedsWork** | [#runtime/45661](https://github.com/dotnet/runtime/issues/45661#issuecomment-767756782) | [Video](https://www.youtube.com/watch?v=efvl_wtYSw8&t=0h44m12s)

* This might be noisy, at least in the runtime repo
* We should run this first so we can see what the noise is to make a determination of on/off by default and severity
## Expose setting/getting of system error code

**Approved** | [#runtime/46843](https://github.com/dotnet/runtime/issues/46843#issuecomment-767762870) | [Video](https://www.youtube.com/watch?v=efvl_wtYSw8&t=0h51m16s)

* Looks good as proposed
* The naming of `SetLastWin32Error()` is unfortunate, because it really sets the last P/Invoke error, regardless of OS but it matches the existing method name. Let's just obsolete the current concept and add a new pair. This also makes people aware of the conceptual difference between `GetLastSystemError()` and `GetLastPInvokeError()`.

```C#
namespace System.Runtime.InteropServices
{
    public static class Marshal
    {
        public static int GetLastSystemError();
        public static void SetLastSystemError(int error);

        public static int GetLastPInvokeError();
        public static void SetLastPInvokeError(int error);

        [Obsolete("Use GetLastSystemError() or GetLastPInvokeError()", DiagnosticId="<NextID>", UrlFormat="<the proper URL>")]
        public static int GetLastWin32Error();
    }
}
```
## Non-enumerating Count Linq Method

**Approved** | [#runtime/27183](https://github.com/dotnet/runtime/issues/27183#issuecomment-767765647) | [Video](https://www.youtube.com/watch?v=efvl_wtYSw8&t=1h1m18s)

* The concept makes sense but it seems nicer as an extension method

```C#
namespace System.Linq
{
    public static class Enumerable
    {
        public static bool TryGetNonEnumeratedCount(this IEnumerable<T> source, out int count);
    }
}
```
## IEnumerable should have a extension for creating fixed size chunks

**NeedsWork** | [#runtime/27449](https://github.com/dotnet/runtime/issues/27449#issuecomment-767791986) | [Video](https://www.youtube.com/watch?v=efvl_wtYSw8&t=1h6m10s)

* Looks good as proposed, but: does this need to be on `IQuerable<T>`?

```C#
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<T[]> Chunk(this IEnumerable<T> source, int size);
    }
}
```
