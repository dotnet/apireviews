```diff
---C:\Users\jaredpar\temp\review\before\System.Collections.Immutable.dll
+++C:\Users\jaredpar\temp\review\after-builder\System.Collections.Immutable.dll
 namespace System.Collections.Immutable {
  public static class ImmutableArray {
    public static int BinarySearch<T>(this ImmutableArray<T> array, int index, int length, T value);
    public static int BinarySearch<T>(this ImmutableArray<T> array, int index, int length, T value, IComparer<T> comparer);
    public static int BinarySearch<T>(this ImmutableArray<T> array, T value);
    public static int BinarySearch<T>(this ImmutableArray<T> array, T value, IComparer<T> comparer);
    public static ImmutableArray<T> Create<T, TDerived>(ImmutableArray<TDerived> items) where TDerived : class, T;
    public static ImmutableArray<T> Create<T>();
    public static ImmutableArray<T> Create<T>(ImmutableArray<T> items, int start, int length);
    public static ImmutableArray<T> Create<T>(T item);
    public static ImmutableArray<T> Create<T>(T item1, T item2);
    public static ImmutableArray<T> Create<T>(T item1, T item2, T item3);
    public static ImmutableArray<T> Create<T>(T item1, T item2, T item3, T item4);
    public static ImmutableArray<T> Create<T>(params T[] items);
    public static ImmutableArray<T> Create<T>(T[] items, int start, int length);
    public static ImmutableArray<T>.Builder CreateBuilder<T>();
    public static ImmutableArray<T>.Builder CreateBuilder<T>(int initialCapacity);
+   public static ImmutableArray<T>.FixedLengthBuilder CreateFixedLengthBuilder<T>(int length);
    public static ImmutableArray<T> CreateRange<T>(IEnumerable<T> items);
    public static ImmutableArray<TResult> CreateRange<TSource, TArg, TResult>(ImmutableArray<TSource> items, Func<TSource, TArg, TResult> selector, TArg arg);
    public static ImmutableArray<TResult> CreateRange<TSource, TArg, TResult>(ImmutableArray<TSource> items, int start, int length, Func<TSource, TArg, TResult> selector, TArg arg);
    public static ImmutableArray<TResult> CreateRange<TSource, TResult>(ImmutableArray<TSource> items, Func<TSource, TResult> selector);
    public static ImmutableArray<TResult> CreateRange<TSource, TResult>(ImmutableArray<TSource> items, int start, int length, Func<TSource, TResult> selector);
    public static ImmutableArray<TSource> ToImmutableArray<TSource>(this IEnumerable<TSource> items);
  }
  public struct ImmutableArray<T> : ICollection, ICollection<T>, IEnumerable, IEnumerable<T>, IEquatable<ImmutableArray<T>>, IImmutableList<T>, IList, IList<T>, IReadOnlyCollection<T>, IReadOnlyList<T>, IStructuralComparable, IStructuralEquatable {
+   public sealed class FixedLengthBuilder : IEnumerable, IEnumerable<T>, IReadOnlyCollection<T>, IReadOnlyList<T> {
+     public bool IsInitialized { get; }
+     public int Length { get; }
+     int System.Collections.Generic.IReadOnlyCollection<T>.Count { get; }
+     public T this[int index] { get; set; }
+     public ImmutableArray<T>.Enumerator GetEnumerator();
+     public void Reset(int length);
+     IEnumerator<T> System.Collections.Generic.IEnumerable<T>.GetEnumerator();
+     IEnumerator System.Collections.IEnumerable.GetEnumerator();
+     public T[] ToArray();
+     public T[] ToArrayAndClear();
+     public ImmutableArray<T> ToImmutableAndClear();
    }
  }
 }
```
