# System.Collections

```diff
---D:\Temp\nullable\before\System.Collections.dll
+++D:\Temp\nullable\after\System.Collections.dll
 namespace System.Collections {
  public sealed class BitArray : ICloneable, ICollection, IEnumerable {
    public BitArray(bool[] values);
    public BitArray(byte[] bytes);
    public BitArray(BitArray bits);
    public BitArray(int length);
    public BitArray(int length, bool defaultValue);
    public BitArray(int[] values);
    public int Count { get; }
    public bool IsReadOnly { get; }
    public bool IsSynchronized { get; }
    public int Length { get; set; }
    public object SyncRoot { get; }
    public bool this[int index] { get; set; }
    public BitArray And(BitArray value);
    public object Clone();
    public void CopyTo(Array array, int index);
    public bool Get(int index);
    public IEnumerator GetEnumerator();
    public BitArray LeftShift(int count);
    public BitArray Not();
    public BitArray Or(BitArray value);
    public BitArray RightShift(int count);
    public void Set(int index, bool value);
    public void SetAll(bool value);
    public BitArray Xor(BitArray value);
  }
  public static class StructuralComparisons {
    public static IComparer StructuralComparer { get; }
    public static IEqualityComparer StructuralEqualityComparer { get; }
  }
 }
 namespace System.Collections.Generic {
  public static class CollectionExtensions {
    public static TValue GetValueOrDefault<TKey, TValue>(this IReadOnlyDictionary<TKey, TValue> dictionary, TKey key) where TKey : object;
    public static TValue GetValueOrDefault<TKey, TValue>(this IReadOnlyDictionary<TKey, TValue> dictionary, TKey key, TValue defaultValue) where TKey : object;
    public static bool Remove<TKey, TValue>(this IDictionary<TKey, TValue> dictionary, TKey key, out TValue value) where TKey : object;
    public static bool TryAdd<TKey, TValue>(this IDictionary<TKey, TValue> dictionary, TKey key, TValue value) where TKey : object;
  }
  public abstract class Comparer<T> : IComparer, IComparer<T> {
    protected Comparer();
    public static Comparer<T> Default { get; }
    public abstract int Compare(T x, T y);
    public static Comparer<T> Create(Comparison<T> comparison);
    int System.Collections.IComparer.Compare(object? x, object? y);
  }
  public class Dictionary<TKey, TValue> : ICollection, ICollection<KeyValuePair<TKey, TValue>>, IDeserializationCallback, IDictionary, IDictionary<TKey, TValue>, IEnumerable, IEnumerable<KeyValuePair<TKey, TValue>>, IReadOnlyCollection<KeyValuePair<TKey, TValue>>, IReadOnlyDictionary<TKey, TValue>, ISerializable where TKey : object {
    public Dictionary();
    public Dictionary(IDictionary<TKey, TValue> dictionary);
    public Dictionary(IDictionary<TKey, TValue> dictionary, IEqualityComparer<TKey>? comparer);
    public Dictionary(IEnumerable<KeyValuePair<TKey, TValue>> collection);
    public Dictionary(IEnumerable<KeyValuePair<TKey, TValue>> collection, IEqualityComparer<TKey>? comparer);
    public Dictionary(IEqualityComparer<TKey>? comparer);
    public Dictionary(int capacity);
    public Dictionary(int capacity, IEqualityComparer<TKey>? comparer);
    protected Dictionary(SerializationInfo info, StreamingContext context);
    public IEqualityComparer<TKey> Comparer { get; }
    public int Count { get; }
    public Dictionary<TKey, TValue>.KeyCollection Keys { get; }
    bool System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.IsReadOnlyICollection<KeyValuePair<TKey, TValue>>.IsReadOnly { get; }
    ICollection<TKey> System.Collections.Generic.IDictionary<TKey,TValue>.KeysIDictionary<TKey, TValue>.Keys { get; }
    ICollection<TValue> System.Collections.Generic.IDictionary<TKey,TValue>.ValuesIDictionary<TKey, TValue>.Values { get; }
    IEnumerable<TKey> System.Collections.Generic.IReadOnlyDictionary<TKey,TValue>.KeysIReadOnlyDictionary<TKey, TValue>.Keys { get; }
    IEnumerable<TValue> System.Collections.Generic.IReadOnlyDictionary<TKey,TValue>.ValuesIReadOnlyDictionary<TKey, TValue>.Values { get; }
    bool System.Collections.ICollection.IsSynchronized { get; }
    object System.Collections.ICollection.SyncRoot { get; }
    bool System.Collections.IDictionary.IsFixedSize { get; }
    bool System.Collections.IDictionary.IsReadOnly { get; }
    object? System.Collections.IDictionary.this[object key] { get; set; }
    ICollection System.Collections.IDictionary.Keys { get; }
    ICollection System.Collections.IDictionary.Values { get; }
    public TValue this[TKey key] { get; set; }
    public Dictionary<TKey, TValue>.ValueCollection Values { get; }
    public void Add(TKey key, TValue value);
    public void Clear();
    public bool ContainsKey(TKey key);
    public bool ContainsValue(TValue value);
    public int EnsureCapacity(int capacity);
    public Dictionary<TKey, TValue>.Enumerator GetEnumerator();
    public virtual void GetObjectData(SerializationInfo info, StreamingContext context);
    public virtual void OnDeserialization(object sender);
    public bool Remove(TKey key);
    public bool Remove(TKey key, out TValue value);
    void System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.AddICollection<KeyValuePair<TKey, TValue>>.Add(KeyValuePair<TKey, TValue> keyValuePair);
    bool System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.ContainsICollection<KeyValuePair<TKey, TValue>>.Contains(KeyValuePair<TKey, TValue> keyValuePair);
    void System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.CopyToICollection<KeyValuePair<TKey, TValue>>.CopyTo(KeyValuePair<TKey, TValue>[] array, int index);
    bool System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.RemoveICollection<KeyValuePair<TKey, TValue>>.Remove(KeyValuePair<TKey, TValue> keyValuePair);
    IEnumerator<KeyValuePair<TKey, TValue>> System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<TKey,TValue>>.GetEnumeratorIEnumerable<KeyValuePair<TKey, TValue>>.GetEnumerator();
    void System.Collections.ICollection.CopyTo(Array array, int index);
    void System.Collections.IDictionary.Add(object key, object? value);
    bool System.Collections.IDictionary.Contains(object key);
    IDictionaryEnumerator System.Collections.IDictionary.GetEnumerator();
    void System.Collections.IDictionary.Remove(object key);
    IEnumerator System.Collections.IEnumerable.GetEnumerator();
    public void TrimExcess();
    public void TrimExcess(int capacity);
    public bool TryAdd(TKey key, TValue value);
    public bool TryGetValue(TKey key, out TValue value);
    public struct Enumerator : IDictionaryEnumerator, IDisposable, IEnumerator, IEnumerator<KeyValuePair<TKey, TValue>> {
      public KeyValuePair<TKey, TValue> Current { get; }
      DictionaryEntry System.Collections.IDictionaryEnumerator.Entry { get; }
      object System.Collections.IDictionaryEnumerator.Key { get; }
      object? System.Collections.IDictionaryEnumerator.Value { get; }
      object? System.Collections.IEnumerator.Current { get; }
      public void Dispose();
      public bool MoveNext();
      void System.Collections.IEnumerator.Reset();
    }
    public sealed class KeyCollection : ICollection, ICollection<TKey>, IEnumerable, IEnumerable<TKey>, IReadOnlyCollection<TKey> {
      public KeyCollection(Dictionary<TKey, TValue> dictionary);
      public int Count { get; }
      bool System.Collections.Generic.ICollection<TKey>.IsReadOnlyICollection<TKey>.IsReadOnly { get; }
      bool System.Collections.ICollection.IsSynchronized { get; }
      object System.Collections.ICollection.SyncRoot { get; }
      public void CopyTo(TKey[] array, int index);
      public Dictionary<TKey, TValue>.KeyCollection.Enumerator GetEnumerator();
      void System.Collections.Generic.ICollection<TKey>.AddICollection<TKey>.Add(TKey item);
      void System.Collections.Generic.ICollection<TKey>.ClearICollection<TKey>.Clear();
      bool System.Collections.Generic.ICollection<TKey>.ContainsICollection<TKey>.Contains(TKey item);
      bool System.Collections.Generic.ICollection<TKey>.RemoveICollection<TKey>.Remove(TKey item);
      IEnumerator<TKey> System.Collections.Generic.IEnumerable<TKey>.GetEnumeratorIEnumerable<TKey>.GetEnumerator();
      void System.Collections.ICollection.CopyTo(Array array, int index);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      public struct Enumerator : IDisposable, IEnumerator, IEnumerator<TKey> {
        public TKey Current { get; }
        object? System.Collections.IEnumerator.Current { get; }
        public void Dispose();
        public bool MoveNext();
        void System.Collections.IEnumerator.Reset();
      }
    }
    public sealed class ValueCollection : ICollection, ICollection<TValue>, IEnumerable, IEnumerable<TValue>, IReadOnlyCollection<TValue> {
      public ValueCollection(Dictionary<TKey, TValue> dictionary);
      public int Count { get; }
      bool System.Collections.Generic.ICollection<TValue>.IsReadOnlyICollection<TValue>.IsReadOnly { get; }
      bool System.Collections.ICollection.IsSynchronized { get; }
      object System.Collections.ICollection.SyncRoot { get; }
      public void CopyTo(TValue[] array, int index);
      public Dictionary<TKey, TValue>.ValueCollection.Enumerator GetEnumerator();
      void System.Collections.Generic.ICollection<TValue>.AddICollection<TValue>.Add(TValue item);
      void System.Collections.Generic.ICollection<TValue>.ClearICollection<TValue>.Clear();
      bool System.Collections.Generic.ICollection<TValue>.ContainsICollection<TValue>.Contains(TValue item);
      bool System.Collections.Generic.ICollection<TValue>.RemoveICollection<TValue>.Remove(TValue item);
      IEnumerator<TValue> System.Collections.Generic.IEnumerable<TValue>.GetEnumeratorIEnumerable<TValue>.GetEnumerator();
      void System.Collections.ICollection.CopyTo(Array array, int index);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      public struct Enumerator : IDisposable, IEnumerator, IEnumerator<TValue> {
        public TValue Current { get; }
        object? System.Collections.IEnumerator.Current { get; }
        public void Dispose();
        public bool MoveNext();
        void System.Collections.IEnumerator.Reset();
      }
    }
  }
  public abstract class EqualityComparer<T> : IEqualityComparer, IEqualityComparer<T> {
    protected EqualityComparer();
    public static EqualityComparer<T> Default { get; }
    public abstract bool Equals(T x, T y);
    public abstract int GetHashCode(T obj);
    bool System.Collections.IEqualityComparer.Equals(object? x, object? y);
    int System.Collections.IEqualityComparer.GetHashCode(object? obj);
  }
  public class HashSet<T> : ICollection<T>, IDeserializationCallback, IEnumerable, IEnumerable<T>, IReadOnlyCollection<T>, ISerializable, ISet<T> {
    public HashSet();
    public HashSet(IEnumerable<T> collection);
    public HashSet(IEnumerable<T> collection, IEqualityComparer<T>? comparer);
    public HashSet(IEqualityComparer<T>? comparer);
    public HashSet(int capacity);
    public HashSet(int capacity, IEqualityComparer<T> comparer);
    protected HashSet(SerializationInfo info, StreamingContext context);
    public IEqualityComparer<T> Comparer { get; }
    public int Count { get; }
    bool System.Collections.Generic.ICollection<T>.IsReadOnlyICollection<T>.IsReadOnly { get; }
    public bool Add(T item);
    public void Clear();
    public bool Contains(T item);
    public void CopyTo(T[] array);
    public void CopyTo(T[] array, int arrayIndex);
    public void CopyTo(T[] array, int arrayIndex, int count);
    public static IEqualityComparer<HashSet<T>> CreateSetComparer();
    public int EnsureCapacity(int capacity);
    public void ExceptWith(IEnumerable<T> other);
    public HashSet<T>.Enumerator GetEnumerator();
    public virtual void GetObjectData(SerializationInfo info, StreamingContext context);
    public void IntersectWith(IEnumerable<T> other);
    public bool IsProperSubsetOf(IEnumerable<T> other);
    public bool IsProperSupersetOf(IEnumerable<T> other);
    public bool IsSubsetOf(IEnumerable<T> other);
    public bool IsSupersetOf(IEnumerable<T> other);
    public virtual void OnDeserialization(object sender);
    public bool Overlaps(IEnumerable<T> other);
    public bool Remove(T item);
    public int RemoveWhere(Predicate<T> match);
    public bool SetEquals(IEnumerable<T> other);
    public void SymmetricExceptWith(IEnumerable<T> other);
    void System.Collections.Generic.ICollection<T>.AddICollection<T>.Add(T item);
    IEnumerator<T> System.Collections.Generic.IEnumerable<T>.GetEnumeratorIEnumerable<T>.GetEnumerator();
    IEnumerator System.Collections.IEnumerable.GetEnumerator();
    public void TrimExcess();
    public bool TryGetValue(T equalValue, out T actualValue);
    public void UnionWith(IEnumerable<T> other);
    public struct Enumerator : IDisposable, IEnumerator, IEnumerator<T> {
      public T Current { get; }
      object? System.Collections.IEnumerator.Current { get; }
      public void Dispose();
      public bool MoveNext();
      void System.Collections.IEnumerator.Reset();
    }
  }
  public class LinkedList<T> : ICollection, ICollection<T>, IDeserializationCallback, IEnumerable, IEnumerable<T>, IReadOnlyCollection<T>, ISerializable {
    public LinkedList();
    public LinkedList(IEnumerable<T> collection);
    protected LinkedList(SerializationInfo info, StreamingContext context);
    public int Count { get; }
    public LinkedListNode<T>? First { get; }
    public LinkedListNode<T>? Last { get; }
    bool System.Collections.Generic.ICollection<T>.IsReadOnlyICollection<T>.IsReadOnly { get; }
    bool System.Collections.ICollection.IsSynchronized { get; }
    object System.Collections.ICollection.SyncRoot { get; }
    public void AddAfter(LinkedListNode<T> node, LinkedListNode<T> newNode);
    public LinkedListNode<T> AddAfter(LinkedListNode<T> node, T value);
    public void AddBefore(LinkedListNode<T> node, LinkedListNode<T> newNode);
    public LinkedListNode<T> AddBefore(LinkedListNode<T> node, T value);
    public void AddFirst(LinkedListNode<T> node);
    public LinkedListNode<T> AddFirst(T value);
    public void AddLast(LinkedListNode<T> node);
    public LinkedListNode<T> AddLast(T value);
    public void Clear();
    public bool Contains(T value);
    public void CopyTo(T[] array, int index);
    public LinkedListNode<T>? Find(T value);
    public LinkedListNode<T>? FindLast(T value);
    public LinkedList<T>.Enumerator GetEnumerator();
    public virtual void GetObjectData(SerializationInfo info, StreamingContext context);
    public virtual void OnDeserialization(object sender);
    public void Remove(LinkedListNode<T> node);
    public bool Remove(T value);
    public void RemoveFirst();
    public void RemoveLast();
    void System.Collections.Generic.ICollection<T>.AddICollection<T>.Add(T value);
    IEnumerator<T> System.Collections.Generic.IEnumerable<T>.GetEnumeratorIEnumerable<T>.GetEnumerator();
    void System.Collections.ICollection.CopyTo(Array array, int index);
    IEnumerator System.Collections.IEnumerable.GetEnumerator();
    public struct Enumerator : IDeserializationCallback, IDisposable, IEnumerator, IEnumerator<T>, ISerializable {
      public T Current { get; }
      object? System.Collections.IEnumerator.Current { get; }
      public void Dispose();
      public bool MoveNext();
      void System.Collections.IEnumerator.Reset();
      void System.Runtime.Serialization.IDeserializationCallback.OnDeserialization(object sender);
      void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
    }
  }
  public sealed class LinkedListNode<T> {
    public LinkedListNode(T value);
    public LinkedList<T>? List { get; }
    public LinkedListNode<T>? Next { get; }
    public LinkedListNode<T>? Previous { get; }
    public T Value { get; set; }
  }
  public class List<T> : ICollection, ICollection<T>, IEnumerable, IEnumerable<T>, IList, IList<T>, IReadOnlyCollection<T>, IReadOnlyList<T> {
    public List();
    public List(IEnumerable<T> collection);
    public List(int capacity);
    public int Capacity { get; set; }
    public int Count { get; }
    bool System.Collections.Generic.ICollection<T>.IsReadOnlyICollection<T>.IsReadOnly { get; }
    bool System.Collections.ICollection.IsSynchronized { get; }
    object System.Collections.ICollection.SyncRoot { get; }
    bool System.Collections.IList.IsFixedSize { get; }
    bool System.Collections.IList.IsReadOnly { get; }
    object? System.Collections.IList.this[int index] { get; set; }
    public T this[int index] { get; set; }
    public void Add(T item);
    public void AddRange(IEnumerable<T> collection);
    public ReadOnlyCollection<T> AsReadOnly();
    public int BinarySearch(int index, int count, T item, IComparer<T>? comparer);
    public int BinarySearch(T item);
    public int BinarySearch(T item, IComparer<T>? comparer);
    public void Clear();
    public bool Contains(T item);
    public List<TOutput> ConvertAll<TOutput>(Converter<T, TOutput> converter);
    public void CopyTo(int index, T[] array, int arrayIndex, int count);
    public void CopyTo(T[] array);
    public void CopyTo(T[] array, int arrayIndex);
    public bool Exists(Predicate<T> match);
    public T Find(Predicate<T> match);
    public List<T> FindAll(Predicate<T> match);
    public int FindIndex(int startIndex, int count, Predicate<T> match);
    public int FindIndex(int startIndex, Predicate<T> match);
    public int FindIndex(Predicate<T> match);
    public T FindLast(Predicate<T> match);
    public int FindLastIndex(int startIndex, int count, Predicate<T> match);
    public int FindLastIndex(int startIndex, Predicate<T> match);
    public int FindLastIndex(Predicate<T> match);
    public void ForEach(Action<T> action);
    public List<T>.Enumerator GetEnumerator();
    public List<T> GetRange(int index, int count);
    public int IndexOf(T item);
    public int IndexOf(T item, int index);
    public int IndexOf(T item, int index, int count);
    public void Insert(int index, T item);
    public void InsertRange(int index, IEnumerable<T> collection);
    public int LastIndexOf(T item);
    public int LastIndexOf(T item, int index);
    public int LastIndexOf(T item, int index, int count);
    public bool Remove(T item);
    public int RemoveAll(Predicate<T> match);
    public void RemoveAt(int index);
    public void RemoveRange(int index, int count);
    public void Reverse();
    public void Reverse(int index, int count);
    public void Sort();
    public void Sort(IComparer<T>? comparer);
    public void Sort(Comparison<T> comparison);
    public void Sort(int index, int count, IComparer<T>? comparer);
    IEnumerator<T> System.Collections.Generic.IEnumerable<T>.GetEnumeratorIEnumerable<T>.GetEnumerator();
    void System.Collections.ICollection.CopyTo(Array array, int arrayIndex);
    IEnumerator System.Collections.IEnumerable.GetEnumerator();
    int System.Collections.IList.Add(object? item);
    bool System.Collections.IList.Contains(object? item);
    int System.Collections.IList.IndexOf(object? item);
    void System.Collections.IList.Insert(int index, object? item);
    void System.Collections.IList.Remove(object? item);
    public T[] ToArray();
    public void TrimExcess();
    public bool TrueForAll(Predicate<T> match);
    public struct Enumerator : IDisposable, IEnumerator, IEnumerator<T> {
      public T Current { get; }
      object? System.Collections.IEnumerator.Current { get; }
      public void Dispose();
      public bool MoveNext();
      void System.Collections.IEnumerator.Reset();
    }
  }
  public class Queue<T> : ICollection, IEnumerable, IEnumerable<T>, IReadOnlyCollection<T> {
    public Queue();
    public Queue(IEnumerable<T> collection);
    public Queue(int capacity);
    public int Count { get; }
    bool System.Collections.ICollection.IsSynchronized { get; }
    object System.Collections.ICollection.SyncRoot { get; }
    public void Clear();
    public bool Contains(T item);
    public void CopyTo(T[] array, int arrayIndex);
    public T Dequeue();
    public void Enqueue(T item);
    public Queue<T>.Enumerator GetEnumerator();
    public T Peek();
    IEnumerator<T> System.Collections.Generic.IEnumerable<T>.GetEnumeratorIEnumerable<T>.GetEnumerator();
    void System.Collections.ICollection.CopyTo(Array array, int index);
    IEnumerator System.Collections.IEnumerable.GetEnumerator();
    public T[] ToArray();
    public void TrimExcess();
    public bool TryDequeue(out T result);
    public bool TryPeek(out T result);
    public struct Enumerator : IDisposable, IEnumerator, IEnumerator<T> {
      public T Current { get; }
      object? System.Collections.IEnumerator.Current { get; }
      public void Dispose();
      public bool MoveNext();
      void System.Collections.IEnumerator.Reset();
    }
  }
  public class SortedDictionary<TKey, TValue> : ICollection, ICollection<KeyValuePair<TKey, TValue>>, IDictionary, IDictionary<TKey, TValue>, IEnumerable, IEnumerable<KeyValuePair<TKey, TValue>>, IReadOnlyCollection<KeyValuePair<TKey, TValue>>, IReadOnlyDictionary<TKey, TValue> where TKey : object {
    public SortedDictionary();
    public SortedDictionary(IComparer<TKey>? comparer);
    public SortedDictionary(IDictionary<TKey, TValue> dictionary);
    public SortedDictionary(IDictionary<TKey, TValue> dictionary, IComparer<TKey>? comparer);
    public IComparer<TKey> Comparer { get; }
    public int Count { get; }
    public SortedDictionary<TKey, TValue>.KeyCollection Keys { get; }
    bool System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.IsReadOnlyICollection<KeyValuePair<TKey, TValue>>.IsReadOnly { get; }
    ICollection<TKey> System.Collections.Generic.IDictionary<TKey,TValue>.KeysIDictionary<TKey, TValue>.Keys { get; }
    ICollection<TValue> System.Collections.Generic.IDictionary<TKey,TValue>.ValuesIDictionary<TKey, TValue>.Values { get; }
    IEnumerable<TKey> System.Collections.Generic.IReadOnlyDictionary<TKey,TValue>.KeysIReadOnlyDictionary<TKey, TValue>.Keys { get; }
    IEnumerable<TValue> System.Collections.Generic.IReadOnlyDictionary<TKey,TValue>.ValuesIReadOnlyDictionary<TKey, TValue>.Values { get; }
    bool System.Collections.ICollection.IsSynchronized { get; }
    object System.Collections.ICollection.SyncRoot { get; }
    bool System.Collections.IDictionary.IsFixedSize { get; }
    bool System.Collections.IDictionary.IsReadOnly { get; }
    object? System.Collections.IDictionary.this[object key] { get; set; }
    ICollection System.Collections.IDictionary.Keys { get; }
    ICollection System.Collections.IDictionary.Values { get; }
    public TValue this[TKey key] { get; set; }
    public SortedDictionary<TKey, TValue>.ValueCollection Values { get; }
    public void Add(TKey key, TValue value);
    public void Clear();
    public bool ContainsKey(TKey key);
    public bool ContainsValue(TValue value);
    public void CopyTo(KeyValuePair<TKey, TValue>[] array, int index);
    public SortedDictionary<TKey, TValue>.Enumerator GetEnumerator();
    public bool Remove(TKey key);
    void System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.AddICollection<KeyValuePair<TKey, TValue>>.Add(KeyValuePair<TKey, TValue> keyValuePair);
    bool System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.ContainsICollection<KeyValuePair<TKey, TValue>>.Contains(KeyValuePair<TKey, TValue> keyValuePair);
    bool System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.RemoveICollection<KeyValuePair<TKey, TValue>>.Remove(KeyValuePair<TKey, TValue> keyValuePair);
    IEnumerator<KeyValuePair<TKey, TValue>> System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<TKey,TValue>>.GetEnumeratorIEnumerable<KeyValuePair<TKey, TValue>>.GetEnumerator();
    void System.Collections.ICollection.CopyTo(Array array, int index);
    void System.Collections.IDictionary.Add(object key, object? value);
    bool System.Collections.IDictionary.Contains(object key);
    IDictionaryEnumerator System.Collections.IDictionary.GetEnumerator();
    void System.Collections.IDictionary.Remove(object key);
    IEnumerator System.Collections.IEnumerable.GetEnumerator();
    public bool TryGetValue(TKey key, out TValue value);
    public struct Enumerator : IDictionaryEnumerator, IDisposable, IEnumerator, IEnumerator<KeyValuePair<TKey, TValue>> {
      public KeyValuePair<TKey, TValue> Current { get; }
      DictionaryEntry System.Collections.IDictionaryEnumerator.Entry { get; }
      object System.Collections.IDictionaryEnumerator.Key { get; }
      object? System.Collections.IDictionaryEnumerator.Value { get; }
      object? System.Collections.IEnumerator.Current { get; }
      public void Dispose();
      public bool MoveNext();
      void System.Collections.IEnumerator.Reset();
    }
    public sealed class KeyCollection : ICollection, ICollection<TKey>, IEnumerable, IEnumerable<TKey>, IReadOnlyCollection<TKey> {
      public KeyCollection(SortedDictionary<TKey, TValue> dictionary);
      public int Count { get; }
      bool System.Collections.Generic.ICollection<TKey>.IsReadOnlyICollection<TKey>.IsReadOnly { get; }
      bool System.Collections.ICollection.IsSynchronized { get; }
      object System.Collections.ICollection.SyncRoot { get; }
      public void CopyTo(TKey[] array, int index);
      public SortedDictionary<TKey, TValue>.KeyCollection.Enumerator GetEnumerator();
      void System.Collections.Generic.ICollection<TKey>.AddICollection<TKey>.Add(TKey item);
      void System.Collections.Generic.ICollection<TKey>.ClearICollection<TKey>.Clear();
      bool System.Collections.Generic.ICollection<TKey>.ContainsICollection<TKey>.Contains(TKey item);
      bool System.Collections.Generic.ICollection<TKey>.RemoveICollection<TKey>.Remove(TKey item);
      IEnumerator<TKey> System.Collections.Generic.IEnumerable<TKey>.GetEnumeratorIEnumerable<TKey>.GetEnumerator();
      void System.Collections.ICollection.CopyTo(Array array, int index);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      public struct Enumerator : IDisposable, IEnumerator, IEnumerator<TKey> {
        public TKey Current { get; }
        object? System.Collections.IEnumerator.Current { get; }
        public void Dispose();
        public bool MoveNext();
        void System.Collections.IEnumerator.Reset();
      }
    }
    public sealed class ValueCollection : ICollection, ICollection<TValue>, IEnumerable, IEnumerable<TValue>, IReadOnlyCollection<TValue> {
      public ValueCollection(SortedDictionary<TKey, TValue> dictionary);
      public int Count { get; }
      bool System.Collections.Generic.ICollection<TValue>.IsReadOnlyICollection<TValue>.IsReadOnly { get; }
      bool System.Collections.ICollection.IsSynchronized { get; }
      object System.Collections.ICollection.SyncRoot { get; }
      public void CopyTo(TValue[] array, int index);
      public SortedDictionary<TKey, TValue>.ValueCollection.Enumerator GetEnumerator();
      void System.Collections.Generic.ICollection<TValue>.AddICollection<TValue>.Add(TValue item);
      void System.Collections.Generic.ICollection<TValue>.ClearICollection<TValue>.Clear();
      bool System.Collections.Generic.ICollection<TValue>.ContainsICollection<TValue>.Contains(TValue item);
      bool System.Collections.Generic.ICollection<TValue>.RemoveICollection<TValue>.Remove(TValue item);
      IEnumerator<TValue> System.Collections.Generic.IEnumerable<TValue>.GetEnumeratorIEnumerable<TValue>.GetEnumerator();
      void System.Collections.ICollection.CopyTo(Array array, int index);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      public struct Enumerator : IDisposable, IEnumerator, IEnumerator<TValue> {
        public TValue Current { get; }
        object? System.Collections.IEnumerator.Current { get; }
        public void Dispose();
        public bool MoveNext();
        void System.Collections.IEnumerator.Reset();
      }
    }
  }
  public class SortedList<TKey, TValue> : ICollection, ICollection<KeyValuePair<TKey, TValue>>, IDictionary, IDictionary<TKey, TValue>, IEnumerable, IEnumerable<KeyValuePair<TKey, TValue>>, IReadOnlyCollection<KeyValuePair<TKey, TValue>>, IReadOnlyDictionary<TKey, TValue> where TKey : object {
    public SortedList();
    public SortedList(IComparer<TKey>? comparer);
    public SortedList(IDictionary<TKey, TValue> dictionary);
    public SortedList(IDictionary<TKey, TValue> dictionary, IComparer<TKey>? comparer);
    public SortedList(int capacity);
    public SortedList(int capacity, IComparer<TKey>? comparer);
    public int Capacity { get; set; }
    public IComparer<TKey> Comparer { get; }
    public int Count { get; }
    public IList<TKey> Keys { get; }
    bool System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.IsReadOnlyICollection<KeyValuePair<TKey, TValue>>.IsReadOnly { get; }
    ICollection<TKey> System.Collections.Generic.IDictionary<TKey,TValue>.KeysIDictionary<TKey, TValue>.Keys { get; }
    ICollection<TValue> System.Collections.Generic.IDictionary<TKey,TValue>.ValuesIDictionary<TKey, TValue>.Values { get; }
    IEnumerable<TKey> System.Collections.Generic.IReadOnlyDictionary<TKey,TValue>.KeysIReadOnlyDictionary<TKey, TValue>.Keys { get; }
    IEnumerable<TValue> System.Collections.Generic.IReadOnlyDictionary<TKey,TValue>.ValuesIReadOnlyDictionary<TKey, TValue>.Values { get; }
    bool System.Collections.ICollection.IsSynchronized { get; }
    object System.Collections.ICollection.SyncRoot { get; }
    bool System.Collections.IDictionary.IsFixedSize { get; }
    bool System.Collections.IDictionary.IsReadOnly { get; }
    object? System.Collections.IDictionary.this[object key] { get; set; }
    ICollection System.Collections.IDictionary.Keys { get; }
    ICollection System.Collections.IDictionary.Values { get; }
    public TValue this[TKey key] { get; set; }
    public IList<TValue> Values { get; }
    public void Add(TKey key, TValue value);
    public void Clear();
    public bool ContainsKey(TKey key);
    public bool ContainsValue(TValue value);
    public IEnumerator<KeyValuePair<TKey, TValue>> GetEnumerator();
    public int IndexOfKey(TKey key);
    public int IndexOfValue(TValue value);
    public bool Remove(TKey key);
    public void RemoveAt(int index);
    void System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.AddICollection<KeyValuePair<TKey, TValue>>.Add(KeyValuePair<TKey, TValue> keyValuePair);
    bool System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.ContainsICollection<KeyValuePair<TKey, TValue>>.Contains(KeyValuePair<TKey, TValue> keyValuePair);
    void System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.CopyToICollection<KeyValuePair<TKey, TValue>>.CopyTo(KeyValuePair<TKey, TValue>[] array, int arrayIndex);
    bool System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.RemoveICollection<KeyValuePair<TKey, TValue>>.Remove(KeyValuePair<TKey, TValue> keyValuePair);
    IEnumerator<KeyValuePair<TKey, TValue>> System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<TKey,TValue>>.GetEnumeratorIEnumerable<KeyValuePair<TKey, TValue>>.GetEnumerator();
    void System.Collections.ICollection.CopyTo(Array array, int arrayIndexindex);
    void System.Collections.IDictionary.Add(object key, object? value);
    bool System.Collections.IDictionary.Contains(object key);
    IDictionaryEnumerator System.Collections.IDictionary.GetEnumerator();
    void System.Collections.IDictionary.Remove(object key);
    IEnumerator System.Collections.IEnumerable.GetEnumerator();
    public void TrimExcess();
    public bool TryGetValue(TKey key, out TValue value);
  }
  public class SortedSet<T> : ICollection, ICollection<T>, IDeserializationCallback, IEnumerable, IEnumerable<T>, IReadOnlyCollection<T>, ISerializable, ISet<T> {
    public SortedSet();
    public SortedSet(IComparer<T>? comparer);
    public SortedSet(IEnumerable<T> collection);
    public SortedSet(IEnumerable<T> collection, IComparer<T>? comparer);
    protected SortedSet(SerializationInfo info, StreamingContext context);
    public IComparer<T> Comparer { get; }
    public int Count { get; }
    public T Max { get; }
    public T Min { get; }
    bool System.Collections.Generic.ICollection<T>.IsReadOnlyICollection<T>.IsReadOnly { get; }
    bool System.Collections.ICollection.IsSynchronized { get; }
    object System.Collections.ICollection.SyncRoot { get; }
    public bool Add(T item);
    public virtual void Clear();
    public virtual bool Contains(T item);
    public void CopyTo(T[] array);
    public void CopyTo(T[] array, int index);
    public void CopyTo(T[] array, int index, int count);
    public static IEqualityComparer<SortedSet<T>> CreateSetComparer();
    public static IEqualityComparer<SortedSet<T>> CreateSetComparer(IEqualityComparer<T>? memberEqualityComparer);
    public void ExceptWith(IEnumerable<T> other);
    public SortedSet<T>.Enumerator GetEnumerator();
    protected virtual void GetObjectData(SerializationInfo info, StreamingContext context);
    public virtual SortedSet<T> GetViewBetween(T lowerValue, T upperValue);
    public virtual void IntersectWith(IEnumerable<T> other);
    public bool IsProperSubsetOf(IEnumerable<T> other);
    public bool IsProperSupersetOf(IEnumerable<T> other);
    public bool IsSubsetOf(IEnumerable<T> other);
    public bool IsSupersetOf(IEnumerable<T> other);
    protected virtual void OnDeserialization(object sender);
    public bool Overlaps(IEnumerable<T> other);
    public bool Remove(T item);
    public int RemoveWhere(Predicate<T> match);
    public IEnumerable<T> Reverse();
    public bool SetEquals(IEnumerable<T> other);
    public void SymmetricExceptWith(IEnumerable<T> other);
    void System.Collections.Generic.ICollection<T>.AddICollection<T>.Add(T item);
    IEnumerator<T> System.Collections.Generic.IEnumerable<T>.GetEnumeratorIEnumerable<T>.GetEnumerator();
    void System.Collections.ICollection.CopyTo(Array array, int index);
    IEnumerator System.Collections.IEnumerable.GetEnumerator();
    void System.Runtime.Serialization.IDeserializationCallback.OnDeserialization(object sender);
    void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
    public bool TryGetValue(T equalValue, out T actualValue);
    public void UnionWith(IEnumerable<T> other);
    public struct Enumerator : IDeserializationCallback, IDisposable, IEnumerator, IEnumerator<T>, ISerializable {
      public T Current { get; }
      object? System.Collections.IEnumerator.Current { get; }
      public void Dispose();
      public bool MoveNext();
      void System.Collections.IEnumerator.Reset();
      void System.Runtime.Serialization.IDeserializationCallback.OnDeserialization(object sender);
      void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
    }
  }
  public class Stack<T> : ICollection, IEnumerable, IEnumerable<T>, IReadOnlyCollection<T> {
    public Stack();
    public Stack(IEnumerable<T> collection);
    public Stack(int capacity);
    public int Count { get; }
    bool System.Collections.ICollection.IsSynchronized { get; }
    object System.Collections.ICollection.SyncRoot { get; }
    public void Clear();
    public bool Contains(T item);
    public void CopyTo(T[] array, int arrayIndex);
    public Stack<T>.Enumerator GetEnumerator();
    public T Peek();
    public T Pop();
    public void Push(T item);
    IEnumerator<T> System.Collections.Generic.IEnumerable<T>.GetEnumeratorIEnumerable<T>.GetEnumerator();
    void System.Collections.ICollection.CopyTo(Array array, int arrayIndex);
    IEnumerator System.Collections.IEnumerable.GetEnumerator();
    public T[] ToArray();
    public void TrimExcess();
    public bool TryPeek(out T result);
    public bool TryPop(out T result);
    public struct Enumerator : IDisposable, IEnumerator, IEnumerator<T> {
      public T Current { get; }
      object? System.Collections.IEnumerator.Current { get; }
      public void Dispose();
      public bool MoveNext();
      void System.Collections.IEnumerator.Reset();
    }
  }
 }
```