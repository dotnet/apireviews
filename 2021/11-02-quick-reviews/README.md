# API Review 11/02/2021

## Add extension method AsReadOnly() for IDictionary<TKey, TValue>

**Approved** | [#runtime/33033](https://github.com/dotnet/runtime/issues/33033#issuecomment-958004623) | [Video](https://www.youtube.com/watch?v=cH20Po63ymw&t=0h0m0s)

* This makes sense, but it seems we should be consistent and offer one for `ReadOnlyCollection<T>`, which today only exists as instance methods on `List<T>` and `Array`.

```C#
namespace System.Collections.Generic
{
    public static class CollectionExtensions
    {
        public static ReadOnlyCollection<T> AsReadOnly<T>(this IList<T> list);
        public static ReadOnlyDictionary<TKey, TValue> AsReadOnly<TKey, TValue>(this IDictionary<TKey, TValue> dictionary);
    }
}
```
## Add MaxDepth to JsonWriterOptions

**Approved** | [#runtime/44947](https://github.com/dotnet/runtime/issues/44947#issuecomment-958009583) | [Video](https://www.youtube.com/watch?v=cH20Po63ymw&t=0h11m57s)

* Looks good as proposed

```C#
namespace System.Text.Json
{
    public partial struct JsonWriterOptions
    {
        public int MaxDepth { get; set; }
    }
}
```
## Add unbounded SortedSet<T>.TreeSubSet API

**NeedsWork** | [#runtime/55510](https://github.com/dotnet/runtime/issues/55510#issuecomment-958020733) | [Video](https://www.youtube.com/watch?v=cH20Po63ymw&t=0h17m24s)

* It seems unfortunate to force the caller to inspect the values to decide which of the three methods to call. It would be nicer if we could pass in "null" to indicate that the value is missing and have the method do the right thing.
    - I put "null" in quotes because the `T` is unconstrained so we can't actually make it nullable if `T` is a value type.
* Can we make this better with extension methods? Or should we accept a boolean for lower/upper bound?
* How do these virtuals behave when someone overrode `IsViewBetween()`?

```C#
namespace System.Collections.Generic
{
    public partial class SortedSet<T>
    {
        public virtual SortedSet<T> GetViewUntil(T? lowerValue);
        public virtual SortedSet<T> GetViewFrom(T? upperValue);
    }
}
```
## Expose a JsonSerializerOptions.Default property

**Approved** | [#runtime/61093](https://github.com/dotnet/runtime/issues/61093#issuecomment-958028498) | [Video](https://www.youtube.com/watch?v=cH20Po63ymw&t=0h26m37s)

* We need to make sure the instance is marked as "you can't touch this anymore"

```C#
namespace System.Text.Json
{
    public partial class JsonSerializerOptions
    {
        public static JsonSerializerOptions Default { get; }
    }
}
```
