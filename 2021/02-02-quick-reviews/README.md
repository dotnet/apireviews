# Quick Reviews 02/02/2021

## Add ConcurrentDictionary.Comparer property

**Approved** | [#runtime/31350](https://github.com/dotnet/runtime/issues/31350#issuecomment-771899948) | [Video](https://www.youtube.com/watch?v=mecKToY4JaE&t=0h0m0s)

* Looks good as proposed

```C#
namespace System.Collections.Concurrent
{
    public partial class ConcurrentDictionary<TKey, TValue>
    {
        public IEqualityComparer<TKey> Comparer { get; }
    }
}
```
## Align API surface of immutable collections and their corresponding builder types

**Approved** | [#runtime/822](https://github.com/dotnet/runtime/issues/822#issuecomment-771917845) | [Video](https://www.youtube.com/watch?v=mecKToY4JaE&t=0h5m54s)

* Looks good as proposed
    - Note: this is just syncing builder and immutable container, no API innovation happened here
* We should consider adding a unit test to make sure we keep API surface in sync between the builder and the immutable container
* We removed `Reverse` due to a source breaking change concern. We should follow up. We believe the concern is very low.

```C#
namespace System.Collections.Immutable
{
    public struct ImmutableArray<T>
    {
        public ImmutableArray<T> AddRange(params T[] items);
        public ImmutableArray<T> AddRange(T[] items, int length);
        public ImmutableArray<T> AddRange<TDerived>(TDerived[] items) where TDerived : T;
        public ImmutableArray<T> AddRange(ImmutableArray<T> items, int length);
        public ImmutableArray<T> AddRange<TDerived>(ImmutableArray<TDerived> items) where TDerived : T;
        public partial class Builder
        { 
            public void CopyTo(T[] destination);
            public void CopyTo(int sourceIndex, T[] destination, int destinationIndex, int length);
        
            public int IndexOf(T item, int startIndex, IEqualityComparer<T>? equalityComparer);
            public void InsertRange(int index, IEnumerable<T> items);
            public void InsertRange(int index, ImmutableArray<T> items);
            public void Replace(T oldValue, T newValue);
            public void Replace(T oldValue, T newValue, IEqualityComparer<T>? equalityComparer);
            public void Remove(T item, IEqualityComparer<T>? equalityComparer);
            public void RemoveRange(int index, int length);
            public void RemoveRange(IEnumerable<T> items);
            public void RemoveRange(IEnumerable<T> items, IEqualityComparer<T>? equalityComparer);
            public void RemoveAll(Predicate<T> match);
        }
    }
    public partial class ImmutableList<T>
    {
        public partial class Builder
        { 
            public void Remove(T value, IEqualityComparer<T>? equalityComparer);
            public void RemoveRange(int index, int count);
            public void RemoveRange(IEnumerable<T> items);
            public void RemoveRange(IEnumerable<T> items, IEqualityComparer<T>? equalityComparer);
            public void Replace(T oldValue, T newValue);
            public void Replace(T oldValue, T newValue, IEqualityComparer<T>? equalityComparer);
        }        
    }
    public partial class ImmutableSortedSet<T>
    {
        public partial class Builder
        { 
            public int IndexOf(T item);
        }
    }
}
```

## Pass number of bytes to Buffer.BlockCopy

**Approved** | [#runtime/33765](https://github.com/dotnet/runtime/issues/33765#issuecomment-771928647) | [Video](https://www.youtube.com/watch?v=mecKToY4JaE&t=0h18m48s)

* On by default as warning seems reasonable, given the scoped nature
* Adding a fixer that offers to multiply the length or call `Array.Copy` instead
## Prefer string.AsSpan over string.Substring

**Approved** | [#runtime/33778](https://github.com/dotnet/runtime/issues/33778#issuecomment-771935014) | [Video](https://www.youtube.com/watch?v=mecKToY4JaE&t=0h45m11s)

* On by default as info seems fine

## Use Span.SequenceEquals instead of open-coded comparison loops

**NeedsWork** | [#runtime/33779](https://github.com/dotnet/runtime/issues/33779#issuecomment-771948766) | [Video](https://www.youtube.com/watch?v=mecKToY4JaE&t=0h54m8s)

* We believe detecting open-ended for loops is too expensive and generally not worth it
* It seems detecting usages of `Equals` with arrays might be worth flagging
* Should we have an analzyer that detects `SequenceEquals` with two arrays and suggests `AsSpan()`
* Should we add an SZ member for `SequenceEquals` that by-passes `Enumerable.SequenceEquals` implementation? Or an extension method?
* And/or should we add a type check for `Enumerable.SequenceEquals` that does a type check and uses span's implementation if both arguments are arrays?

