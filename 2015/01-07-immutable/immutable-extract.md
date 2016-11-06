```diff
---C:\Users\jaredpar\temp\review\before\System.Collections.Immutable.dll
+++C:\Users\jaredpar\temp\review\after-extract\System.Collections.Immutable.dll
 namespace System.Collections.Immutable {
  public struct ImmutableArray<T> : ICollection, ICollection<T>, IEnumerable, IEnumerable<T>, IEquatable<ImmutableArray<T>>, IImmutableList<T>, IList, IList<T>, IReadOnlyCollection<T>, IReadOnlyList<T>, IStructuralComparable, IStructuralEquatable {
    public sealed class Builder : ICollection<T>, IEnumerable, IEnumerable<T>, IList<T>, IReadOnlyCollection<T>, IReadOnlyList<T> {
+     public int Capacity { get; }
      ^ Immo Landwerth: Add a setter. Should have the same behavior as List<T>.
      public int Count { get; set; }
      bool System.Collections.Generic.ICollection<T>.IsReadOnly { get; }
      public T this[int index] { get; set; }
      public void Add(T item);
      public void AddRange(ImmutableArray<T>.Builder items);
      public void AddRange(IEnumerable<T> items);
      public void AddRange(ImmutableArray<T> items);
      public void AddRange(ImmutableArray<T> items, int length);
      public void AddRange(params T[] items);
      public void AddRange(T[] items, int length);
      public void AddRange<TDerived>(ImmutableArray<TDerived>.Builder items) where TDerived : T;
      public void AddRange<TDerived>(ImmutableArray<TDerived> items) where TDerived : T;
      public void AddRange<TDerived>(TDerived[] items) where TDerived : T;
      public void Clear();
      public bool Contains(T item);
      public void CopyTo(T[] array, int index);
      public void EnsureCapacity(int capacity);
      ^ Immo Landwerth: Remove or make private
+     public ImmutableArray<T> ExtractToImmutable();
      ^ Immo Landwerth: Change the name and open a thread to dicusss naming. We lean towards MoveToImmutable().
      | Immo Landwerth:
      | Immo Landwerth: We throw InvalidOperationException if Count != Capacity
      public IEnumerator<T> GetEnumerator();
      public int IndexOf(T item);
      public int IndexOf(T item, int startIndex);
      public int IndexOf(T item, int startIndex, int count);
      public int IndexOf(T item, int startIndex, int count, IEqualityComparer<T> equalityComparer);
      public void Insert(int index, T item);
      public int LastIndexOf(T item);
      public int LastIndexOf(T item, int startIndex);
      public int LastIndexOf(T item, int startIndex, int count);
      public int LastIndexOf(T item, int startIndex, int count, IEqualityComparer<T> equalityComparer);
      public bool Remove(T element);
      public void RemoveAt(int index);
      public void ReverseContents();
      ^ Immo Landwerth: Rename to Reverse(), because that's the name List<T> is using.
      public void Sort();
      public void Sort(IComparer<T> comparer);
      public void Sort(int index, int count, IComparer<T> comparer);
      IEnumerator<T> System.Collections.Generic.IEnumerable<T>.GetEnumerator();
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      public T[] ToArray();
      public ImmutableArray<T> ToImmutable();
    }
  }
 }
```
