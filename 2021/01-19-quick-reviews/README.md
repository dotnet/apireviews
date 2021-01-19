# Quick Reviews 01/19/2021

## Writable DOM and dynamic support for 6.0

**NeedsWork** | [#designs/163](https://github.com/dotnet/designs/pull/163#issuecomment-763165357) | [Video 1](https://www.youtube.com/watch?v=-YO1L8CtNTM) | [Video 2](https://www.youtube.com/watch?v=JVKNdfFC4bI)

* It seems the serializer and the DOM are quite intertwined in this design. It seems the DOM should have parsing/formatting methods as well, rather than requiring to serialize it.
* In the design sketch it's unclear which members `JsonValue` and `JsonArray` expose from the interface implementations.
* It seems a bit unclear if/when methods will implicitly serialize/deserialize, for example the `GetValue<T>` would implicitly deserialize the JSON to `T` using a converter.

**Next steps:**

* Detailed API proposal

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
