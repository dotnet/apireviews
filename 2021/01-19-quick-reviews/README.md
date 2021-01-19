# Quick Reviews 01/19/2021

## LINQ APIs to enable C# 8.0 index and range for IEnumerable<T>

**NeedsWork** | [#runtime/28776](https://github.com/dotnet/runtime/issues/28776#issuecomment-763102398) | [Video](https://www.youtube.com/watch?v=JVKNdfFC4bI&t=0h0m0s)

* We're OK with the `ElementAt` methods
* We're unsure about `Slice`, for two reasons:
    - We generally want `Slice` to return the same type it's invoked on (hard for interfaces)
    - It's unclear whether the API should have `Take` semantics (where bounds are min/max) or `Slice` where the enumerable must have that exact range
* We're also unsure about `Queryable`, some of us believe we evolved `Queryable` already, but we should make sure that's the case an inline with our breaking change policy on Linq providers
* Should we evolve PLinq as well?

```C#
namespace System.Linq
{
    public static partial class Enumerable
    {
        public static TSource ElementAt<TSource>(this IEnumerable<TSource> source, Index index);
        public static TSource? ElementAtOrDefault<TSource>(this IEnumerable<TSource> source, Index index);
        public static IEnumerable<TSource> Slice<TSource>(this IEnumerable<TSource> source, Range range);
    }

    public static partial class Queryable
    {
        public static TSource ElementAt<TSource>(this IQueryable<TSource> source, Index index);
        public static TSource? ElementAtOrDefault<TSource>(this IQueryable<TSource> source, Index index);
        public static IQueryable<TSource> Slice<TSource>(this IQueryable<TSource> source, Range range);
    }
}
```
