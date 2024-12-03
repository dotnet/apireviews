# API Review 12/03/2024

## Introduce LeftJoin LINQ operator

**Approved** | [#runtime/110292](https://github.com/dotnet/runtime/issues/110292#issuecomment-2515329399) | [Video](https://www.youtube.com/watch?v=itbo6fSB9nA&t=10m43s)

* We should do both `LeftJoin` and `RightJoin` as reordering the Linq query expression is much more involved than it would be in SQL
* We could do an overload that doesn't take a result selector and returns a tuple. But we decided to keep this separate as we should do the same for the existing `Join` method
* We decided against including `Outer` because the existing `Join` method omitted `Inner`.

```C#
namespace System.Linq;

public static class Enumerable
{
    public static IEnumerable<TResult> LeftJoin<TOuter, TInner, TKey, TResult>(
        this IEnumerable<TOuter> outer,  
        IEnumerable<TInner> inner,
        Func<TOuter, TKey> outerKeySelector,
        Func<TInner, TKey> innerKeySelector,
        Func<TOuter, TInner?, TResult> resultSelector);

    public static IEnumerable<TResult> LeftJoin<TOuter, TInner, TKey, TResult>(
        this IEnumerable<TOuter> outer,  
        IEnumerable<TInner> inner,
        Func<TOuter, TKey> outerKeySelector,
        Func<TInner, TKey> innerKeySelector,
        Func<TOuter, TInner?, TResult> resultSelector,
        IEqualityComparer<TKey>? comparer);

    public static IEnumerable<TResult> RightJoin<TOuter, TInner, TKey, TResult>(
        this IEnumerable<TOuter> outer,  
        IEnumerable<TInner> inner,
        Func<TOuter, TKey> outerKeySelector,
        Func<TInner, TKey> innerKeySelector,
        Func<TOuter?, TInner, TResult> resultSelector);

    public static IEnumerable<TResult> RightJoin<TOuter, TInner, TKey, TResult>(
        this IEnumerable<TOuter> outer,  
        IEnumerable<TInner> inner,
        Func<TOuter, TKey> outerKeySelector,
        Func<TInner, TKey> innerKeySelector,
        Func<TOuter?, TInner, TResult> resultSelector,
        IEqualityComparer<TKey>? comparer);
}

public static class Queryable
{
    public static IQueryable<TResult> LeftJoin<TOuter, TInner, TKey, TResult>(
        this IQueryable<TOuter> outer,
        IEnumerable<TInner> inner,
        Expression<Func<TOuter, TKey>> outerKeySelector,
        Expression<Func<TInner, TKey>> innerKeySelector,
        Expression<Func<TOuter, TInner?, TResult>> resultSelector);
	
    public static IQueryable<TResult> LeftJoin<TOuter, TInner, TKey, TResult>(
        this IQueryable<TOuter> outer,
        IEnumerable<TInner> inner,
        Expression<Func<TOuter, TKey>> outerKeySelector,
        Expression<Func<TInner, TKey>> innerKeySelector,
        Expression<Func<TOuter, TInner?, TResult>> resultSelector,
        IEqualityComparer<TKey>? comparer);
    
    public static IQueryable<TResult> RightJoin<TOuter, TInner, TKey, TResult>(
        this IQueryable<TOuter> outer,
        IEnumerable<TInner> inner,
        Expression<Func<TOuter, TKey>> outerKeySelector,
        Expression<Func<TInner, TKey>> innerKeySelector,
        Expression<Func<TOuter?, TInner, TResult>> resultSelector);
	
    public static IQueryable<TResult> RightJoin<TOuter, TInner, TKey, TResult>(
        this IQueryable<TOuter> outer,
        IEnumerable<TInner> inner,
        Expression<Func<TOuter, TKey>> outerKeySelector,
        Expression<Func<TInner, TKey>> innerKeySelector,
        Expression<Func<TOuter?, TInner, TResult>> resultSelector,
        IEqualityComparer<TKey>? comparer);
}
```

## allow collection expressions for 'System.Collections.ObjectModel' collection types

**Approved** | [#runtime/110161](https://github.com/dotnet/runtime/issues/110161#issuecomment-2515391504) | [Video](https://www.youtube.com/watch?v=itbo6fSB9nA&t=34m54s)

* We can't put the methods on `CollectionExtensions` because the object model ones live in a higher assembly
* We could add another type (e.g. `System.Collections.ObjectModel.Collection` or one non-generic type per collection (like we do in immutable).
* Also, we only really need it for read-only collections because anything that supports collection initializers also supports collection expressions.
    - `Collection<T>` already works
    - `ObservableCollection<T>` already works
* We considered asking for constructor support, but object model collections typically wrap the passed-in collection which create weirdness if we add a span-based one that copies.

```C#
namespace System.Collections.ObjectModel
{
    [CollectionBuilder(typeof(ReadOnlyCollection), "CreateCollection")]
    public partial class ReadOnlyCollection<T>;

    [CollectionBuilder(typeof(ReadOnlyCollection), "CreateSet")]
    public partial class ReadOnlySet<T>;

    public static class ReadOnlyCollection
    {
        public static ReadOnlyCollection<T> CreateCollection<T>(params ReadOnlySpan<T> values);
        public static ReadOnlySet<T> CreateSet<T>(params ReadOnlySpan<T> values);
    }
}
```
## Add API to enable BackgroundService startup to avoid blocking host startup

**Rejected** | [#runtime/36063](https://github.com/dotnet/runtime/issues/36063#issuecomment-2515480657) | [Video](https://www.youtube.com/watch?v=itbo6fSB9nA&1h9m21s)

* We don't believe many people will be broken by the new behavior
    - People who are, can either implement `IHostedService` or do their own dependency handling
* IOW, we like the behavior breaking change but not the API.
    - We will close this issue (as it tracks the API). There will be another issue.

```C#
namespace Microsoft.Extensions.Hosting;

public class BackgroundService : IHostedService
{
    public virtual bool StartAsynchronously { get; } = true;
}
```
## Add a `JsonObject.TryAdd` method.

**Approved** | [#runtime/110244](https://github.com/dotnet/runtime/issues/110244#issuecomment-2515481213) | [Video](https://www.youtube.com/watch?v=itbo6fSB9nA&t=1h30m36s)

* Looks good as proposed but we should add an overload of `TryAdd` without the index for consistency and ergonomics
* The behavior of what we return for the index should match `OrderedDictionary<TKey, TValue>`

```C#
namespace System.Text.Json.Nodes;

public partial class JsonObject
{
    public bool TryAdd(string propertyName, JsonNode? value);
    public bool TryAdd(string propertyName, JsonNode? value, out int index);
    public bool TryGetPropertyValue(string propertyName, out JsonNode? jsonNode, out int index);
}
``` 
