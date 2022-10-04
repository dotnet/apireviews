# API Review 10/04/2022

## Add `TransactionManager.ImplicitDistributedTransactions`

**Approved** | [#runtime/76469](https://github.com/dotnet/runtime/issues/76469#issuecomment-1267337197) | [Video](https://www.youtube.com/watch?v=Ukx2-SYD-zk&t=0h0m0s)

* @roji suggested to put the attribute on the setter only, which makes sense.
* We'd prefer a design where the second time the property is set to something else, we throw `NotSupportedException`
    - Also, `System.Transactions` should freeze the value once it's read, also causing subsequent sets to throw

```C#
namespace System.Transactions;

public partial class TransactionManager
{   
    public static bool ImplicitDistributedTransactions { get; [SupportedOSPlatform("windows")] set; }
}
```
## ReadOnly empty collection singletons

**Approved** | [#runtime/76028](https://github.com/dotnet/runtime/issues/76028#issuecomment-1267363581) | [Video](https://www.youtube.com/watch?v=Ukx2-SYD-zk&t=0h33m17s)

* We're undecided on the statics on the interface
    - We haven't used non-virtual statics on interface
    - Unclear what this means for language interop
    - Unclear how it impacts IntelliSense on interfaces extending these
    - It's new pattern
* We are in favor of exposing them on the concrete types
* We currently don't have a `ReadOnlySet<T>`; if we had this, we should also a static `Empty` there

```C#
namespace System.Collections.ObjectModel
{
    public partial class ReadOnlyCollection<T>
    {
        public static ReadOnlyCollection<T> Empty { get; }
    }

    public partial class ReadOnlyDictionary<TKey, TValue>
    {
        public static ReadOnlyDictionary<TKey, TValue> Empty { get; }
    }

    public partial class ReadOnlyObservableCollection<T>
    {
        public static ReadOnlyObservableCollection<T> Empty { get; }
    }
}
```
## {Last}IndexOf{AnyExcept}Range

**Approved** | [#runtime/76106](https://github.com/dotnet/runtime/issues/76106#issuecomment-1267386055) | [Video](https://www.youtube.com/watch?v=Ukx2-SYD-zk&t=0h58m58s)

* We should rename the methods to say `InRange` and include `Any` for consistency.

```C#
namespace System;

public partial class MemoryExtensions
{
    public static int IndexOfAnyInRange<T>(this ReadOnlySpan<T> span, T lowInclusive, T highInclusive) where T : IComparable<T>;
    public static int IndexOfAnyInRange<T>(this Span<T> span, T lowInclusive, T highInclusive) where T : IComparable<T>;

    public static int IndexOfAnyExceptInRange<T>(this ReadOnlySpan<T> span, T lowInclusive, T highInclusive) where T : IComparable<T>;
    public static int IndexOfAnyExceptInRange<T>(this Span<T> span, T lowInclusive, T highInclusive) where T : IComparable<T>;

    public static int LastIndexOfAnyInRange<T>(this ReadOnlySpan<T> span, T lowInclusive, T highInclusive) where T : IComparable<T>;
    public static int LastIndexOfAnyInRange<T>(this Span<T> span, T lowInclusive, T highInclusive) where T : IComparable<T>;

    public static int LastIndexOfAnyExceptInRange<T>(this ReadOnlySpan<T> span, T lowInclusive, T highInclusive) where T : IComparable<T>;
    public static int LastIndexOfAnyExceptInRange<T>(this Span<T> span, T lowInclusive, T highInclusive) where T : IComparable<T>;
}
```
## Add IList<T> extension methods providing in-place update operations

**NeedsWork** | [#runtime/76375](https://github.com/dotnet/runtime/issues/76375#issuecomment-1267465127) | [Video](https://www.youtube.com/watch?v=Ukx2-SYD-zk&t=1h19m45s)

* It seems the proposal is mostly about convenience, not performance. As such we don't think the cons outweigh the pros
* Cons
    - The proposal is equivalent to a DIM in that they there is a default implementation that works for any case. The performance of these methods, whether they are DIM'ed or extension methods, would be hard to reason about it.
    - Furthermore, without DIMs the only fast implementation could be in the same assembly (or lower) as `CollectionExtension` (which lives in corlib).
* It seems consensus at this point is mixed. DIMs would be ideal, but some folks feel extension methods would be "good enough", give that these interfaces making perf a secondary concern.
* For .NET 8, we should investigate the usage of DIMs in core types to see whether it's viable.

```C#
namespace System.Collections.Generic;

public partial static class CollectionExtensions
{
    public static void InsertRange<T>(this IList<T> list, int index, IEnumerable<T> collection);
    public static void RemoveAll<T>(this IList<T> list, Predicate<T> predicate);
    public static void RemoveRange<T>(this IList<T> list, int index, int count);
    public static void Reverse<T>(this IList<T> list);

    public static void Sort<T>(this IList<T> list);
    public static void Sort<T>(this IList<T> list, Comparison<T> comparison);
    public static void Sort<T>(this IList<T> list, IComparer<T> comparer);
    public static void Sort<T>(this IList<T> list, int index, int count, IComparer<T> comparer);
}
```
