# System.Collections

```diff
--- D:\Temp\nullable\before\System.Collections.dll.cs	2019-06-26 13:40:11.000000000 -0700
+++ D:\Temp\nullable\after\System.Collections.dll.cs	2019-06-26 13:40:30.000000000 -0700
@@ -34,22 +34,22 @@
     }
 }
 namespace System.Collections.Generic
 {
     public static partial class CollectionExtensions
     {
-        public static TValue GetValueOrDefault<TKey, TValue>(this System.Collections.Generic.IReadOnlyDictionary<TKey, TValue> dictionary, TKey key) { throw null; }
-        public static TValue GetValueOrDefault<TKey, TValue>(this System.Collections.Generic.IReadOnlyDictionary<TKey, TValue> dictionary, TKey key, TValue defaultValue) { throw null; }
-        public static bool Remove<TKey, TValue>(this System.Collections.Generic.IDictionary<TKey, TValue> dictionary, TKey key, out TValue value) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]TValue GetValueOrDefault<TKey, TValue>(this System.Collections.Generic.IReadOnlyDictionary<TKey, TValue> dictionary, TKey key) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]TValue GetValueOrDefault<TKey, TValue>(this System.Collections.Generic.IReadOnlyDictionary<TKey, TValue> dictionary, TKey key, [System.Diagnostics.CodeAnalysis.AllowNullAttribute]TValue defaultValue) { throw null; }
+        public static bool Remove<TKey, TValue>(this System.Collections.Generic.IDictionary<TKey, TValue> dictionary, TKey key, [System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out TValue value) { throw null; }
         public static bool TryAdd<TKey, TValue>(this System.Collections.Generic.IDictionary<TKey, TValue> dictionary, TKey key, TValue value) { throw null; }
     }
     public abstract partial class Comparer<T> : System.Collections.Generic.IComparer<T>, System.Collections.IComparer
     {
         protected Comparer() { }
         public static System.Collections.Generic.Comparer<T> Default { get { throw null; } }
-        public abstract int Compare(T x, T y);
+        public abstract int Compare([System.Diagnostics.CodeAnalysis.AllowNullAttribute]T x, [System.Diagnostics.CodeAnalysis.AllowNullAttribute]T y);
         public static System.Collections.Generic.Comparer<T> Create(System.Comparison<T> comparison) { throw null; }
         int System.Collections.IComparer.Compare(object x, object y) { throw null; }
     }
     public partial class Dictionary<TKey, TValue> : System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey, TValue>>, System.Collections.Generic.IDictionary<TKey, TValue>, System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<TKey, TValue>>, System.Collections.Generic.IReadOnlyCollection<System.Collections.Generic.KeyValuePair<TKey, TValue>>, System.Collections.Generic.IReadOnlyDictionary<TKey, TValue>, System.Collections.ICollection, System.Collections.IDictionary, System.Collections.IEnumerable, System.Runtime.Serialization.IDeserializationCallback, System.Runtime.Serialization.ISerializable
     {
         public Dictionary() { }
@@ -71,12 +71,13 @@
         System.Collections.Generic.IEnumerable<TKey> System.Collections.Generic.IReadOnlyDictionary<TKey,TValue>.Keys { get { throw null; } }
         System.Collections.Generic.IEnumerable<TValue> System.Collections.Generic.IReadOnlyDictionary<TKey,TValue>.Values { get { throw null; } }
         bool System.Collections.ICollection.IsSynchronized { get { throw null; } }
         object System.Collections.ICollection.SyncRoot { get { throw null; } }
         bool System.Collections.IDictionary.IsFixedSize { get { throw null; } }
         bool System.Collections.IDictionary.IsReadOnly { get { throw null; } }
+        [System.Diagnostics.CodeAnalysis.DisallowNullAttribute]
         object System.Collections.IDictionary.this[object key] { get { throw null; } set { } }
         System.Collections.ICollection System.Collections.IDictionary.Keys { get { throw null; } }
         System.Collections.ICollection System.Collections.IDictionary.Values { get { throw null; } }
         public System.Collections.Generic.Dictionary<TKey, TValue>.ValueCollection Values { get { throw null; } }
         public void Add(TKey key, TValue value) { }
         public void Clear() { }
@@ -84,13 +85,13 @@
         public bool ContainsValue(TValue value) { throw null; }
         public int EnsureCapacity(int capacity) { throw null; }
         public System.Collections.Generic.Dictionary<TKey, TValue>.Enumerator GetEnumerator() { throw null; }
         public virtual void GetObjectData(System.Runtime.Serialization.SerializationInfo info, System.Runtime.Serialization.StreamingContext context) { }
         public virtual void OnDeserialization(object sender) { }
         public bool Remove(TKey key) { throw null; }
-        public bool Remove(TKey key, out TValue value) { throw null; }
+        public bool Remove(TKey key, [System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out TValue value) { throw null; }
         void System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.Add(System.Collections.Generic.KeyValuePair<TKey, TValue> keyValuePair) { }
         bool System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.Contains(System.Collections.Generic.KeyValuePair<TKey, TValue> keyValuePair) { throw null; }
         void System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.CopyTo(System.Collections.Generic.KeyValuePair<TKey, TValue>[] array, int index) { }
         bool System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey,TValue>>.Remove(System.Collections.Generic.KeyValuePair<TKey, TValue> keyValuePair) { throw null; }
         System.Collections.Generic.IEnumerator<System.Collections.Generic.KeyValuePair<TKey, TValue>> System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<TKey,TValue>>.GetEnumerator() { throw null; }
         void System.Collections.ICollection.CopyTo(System.Array array, int index) { }
@@ -99,13 +100,13 @@
         System.Collections.IDictionaryEnumerator System.Collections.IDictionary.GetEnumerator() { throw null; }
         void System.Collections.IDictionary.Remove(object key) { }
         System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() { throw null; }
         public void TrimExcess() { }
         public void TrimExcess(int capacity) { }
         public bool TryAdd(TKey key, TValue value) { throw null; }
-        public bool TryGetValue(TKey key, out TValue value) { throw null; }
+        public bool TryGetValue(TKey key, [System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out TValue value) { throw null; }
         [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
         public partial struct Enumerator : System.Collections.Generic.IEnumerator<System.Collections.Generic.KeyValuePair<TKey, TValue>>, System.Collections.IDictionaryEnumerator, System.Collections.IEnumerator, System.IDisposable
         {
             private object _dummy;
             public System.Collections.Generic.KeyValuePair<TKey, TValue> Current { get { throw null; } }
             System.Collections.DictionaryEntry System.Collections.IDictionaryEnumerator.Entry { get { throw null; } }
@@ -176,14 +177,14 @@
         }
     }
     public abstract partial class EqualityComparer<T> : System.Collections.Generic.IEqualityComparer<T>, System.Collections.IEqualityComparer
     {
         protected EqualityComparer() { }
         public static System.Collections.Generic.EqualityComparer<T> Default { get { throw null; } }
-        public abstract bool Equals(T x, T y);
-        public abstract int GetHashCode(T obj);
+        public abstract bool Equals([System.Diagnostics.CodeAnalysis.AllowNullAttribute]T x, [System.Diagnostics.CodeAnalysis.AllowNullAttribute]T y);
+        public abstract int GetHashCode([System.Diagnostics.CodeAnalysis.DisallowNullAttribute]T obj);
         bool System.Collections.IEqualityComparer.Equals(object x, object y) { throw null; }
         int System.Collections.IEqualityComparer.GetHashCode(object obj) { throw null; }
     }
     public partial class HashSet<T> : System.Collections.Generic.ICollection<T>, System.Collections.Generic.IEnumerable<T>, System.Collections.Generic.IReadOnlyCollection<T>, System.Collections.Generic.ISet<T>, System.Collections.IEnumerable, System.Runtime.Serialization.IDeserializationCallback, System.Runtime.Serialization.ISerializable
     {
         public HashSet() { }
@@ -219,13 +220,13 @@
         public bool SetEquals(System.Collections.Generic.IEnumerable<T> other) { throw null; }
         public void SymmetricExceptWith(System.Collections.Generic.IEnumerable<T> other) { }
         void System.Collections.Generic.ICollection<T>.Add(T item) { }
         System.Collections.Generic.IEnumerator<T> System.Collections.Generic.IEnumerable<T>.GetEnumerator() { throw null; }
         System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() { throw null; }
         public void TrimExcess() { }
-        public bool TryGetValue(T equalValue, out T actualValue) { throw null; }
+        public bool TryGetValue(T equalValue, [System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T actualValue) { throw null; }
         public void UnionWith(System.Collections.Generic.IEnumerable<T> other) { }
         [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
         public partial struct Enumerator : System.Collections.Generic.IEnumerator<T>, System.Collections.IEnumerator, System.IDisposable
         {
             private T _current;
             private object _dummy;
@@ -319,18 +320,18 @@
         public bool Contains(T item) { throw null; }
         public System.Collections.Generic.List<TOutput> ConvertAll<TOutput>(System.Converter<T, TOutput> converter) { throw null; }
         public void CopyTo(int index, T[] array, int arrayIndex, int count) { }
         public void CopyTo(T[] array) { }
         public void CopyTo(T[] array, int arrayIndex) { }
         public bool Exists(System.Predicate<T> match) { throw null; }
-        public T Find(System.Predicate<T> match) { throw null; }
+        public [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]T Find(System.Predicate<T> match) { throw null; }
         public System.Collections.Generic.List<T> FindAll(System.Predicate<T> match) { throw null; }
         public int FindIndex(int startIndex, int count, System.Predicate<T> match) { throw null; }
         public int FindIndex(int startIndex, System.Predicate<T> match) { throw null; }
         public int FindIndex(System.Predicate<T> match) { throw null; }
-        public T FindLast(System.Predicate<T> match) { throw null; }
+        public [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]T FindLast(System.Predicate<T> match) { throw null; }
         public int FindLastIndex(int startIndex, int count, System.Predicate<T> match) { throw null; }
         public int FindLastIndex(int startIndex, System.Predicate<T> match) { throw null; }
         public int FindLastIndex(System.Predicate<T> match) { throw null; }
         public void ForEach(System.Action<T> action) { }
         public System.Collections.Generic.List<T>.Enumerator GetEnumerator() { throw null; }
         public System.Collections.Generic.List<T> GetRange(int index, int count) { throw null; }
@@ -393,14 +394,14 @@
         public T Peek() { throw null; }
         System.Collections.Generic.IEnumerator<T> System.Collections.Generic.IEnumerable<T>.GetEnumerator() { throw null; }
         void System.Collections.ICollection.CopyTo(System.Array array, int index) { }
         System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() { throw null; }
         public T[] ToArray() { throw null; }
         public void TrimExcess() { }
-        public bool TryDequeue(out T result) { throw null; }
-        public bool TryPeek(out T result) { throw null; }
+        public bool TryDequeue([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T result) { throw null; }
+        public bool TryPeek([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T result) { throw null; }
         [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
         public partial struct Enumerator : System.Collections.Generic.IEnumerator<T>, System.Collections.IEnumerator, System.IDisposable
         {
             private T _currentElement;
             private object _dummy;
             private int _dummyPrimitive;
@@ -448,13 +449,13 @@
         void System.Collections.ICollection.CopyTo(System.Array array, int index) { }
         void System.Collections.IDictionary.Add(object key, object value) { }
         bool System.Collections.IDictionary.Contains(object key) { throw null; }
         System.Collections.IDictionaryEnumerator System.Collections.IDictionary.GetEnumerator() { throw null; }
         void System.Collections.IDictionary.Remove(object key) { }
         System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() { throw null; }
-        public bool TryGetValue(TKey key, out TValue value) { throw null; }
+        public bool TryGetValue(TKey key, [System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out TValue value) { throw null; }
         [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
         public partial struct Enumerator : System.Collections.Generic.IEnumerator<System.Collections.Generic.KeyValuePair<TKey, TValue>>, System.Collections.IDictionaryEnumerator, System.Collections.IEnumerator, System.IDisposable
         {
             private object _dummy;
             public System.Collections.Generic.KeyValuePair<TKey, TValue> Current { get { throw null; } }
             System.Collections.DictionaryEntry System.Collections.IDictionaryEnumerator.Entry { get { throw null; } }
@@ -564,24 +565,26 @@
         void System.Collections.IDictionary.Add(object key, object value) { }
         bool System.Collections.IDictionary.Contains(object key) { throw null; }
         System.Collections.IDictionaryEnumerator System.Collections.IDictionary.GetEnumerator() { throw null; }
         void System.Collections.IDictionary.Remove(object key) { }
         System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() { throw null; }
         public void TrimExcess() { }
-        public bool TryGetValue(TKey key, out TValue value) { throw null; }
+        public bool TryGetValue(TKey key, [System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out TValue value) { throw null; }
     }
     public partial class SortedSet<T> : System.Collections.Generic.ICollection<T>, System.Collections.Generic.IEnumerable<T>, System.Collections.Generic.IReadOnlyCollection<T>, System.Collections.Generic.ISet<T>, System.Collections.ICollection, System.Collections.IEnumerable, System.Runtime.Serialization.IDeserializationCallback, System.Runtime.Serialization.ISerializable
     {
         public SortedSet() { }
         public SortedSet(System.Collections.Generic.IComparer<T> comparer) { }
         public SortedSet(System.Collections.Generic.IEnumerable<T> collection) { }
         public SortedSet(System.Collections.Generic.IEnumerable<T> collection, System.Collections.Generic.IComparer<T> comparer) { }
         protected SortedSet(System.Runtime.Serialization.SerializationInfo info, System.Runtime.Serialization.StreamingContext context) { }
         public System.Collections.Generic.IComparer<T> Comparer { get { throw null; } }
         public int Count { get { throw null; } }
+        [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]
         public T Max { get { throw null; } }
+        [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]
         public T Min { get { throw null; } }
         bool System.Collections.Generic.ICollection<T>.IsReadOnly { get { throw null; } }
         bool System.Collections.ICollection.IsSynchronized { get { throw null; } }
         object System.Collections.ICollection.SyncRoot { get { throw null; } }
         public bool Add(T item) { throw null; }
         public virtual void Clear() { }
@@ -610,13 +613,13 @@
         void System.Collections.Generic.ICollection<T>.Add(T item) { }
         System.Collections.Generic.IEnumerator<T> System.Collections.Generic.IEnumerable<T>.GetEnumerator() { throw null; }
         void System.Collections.ICollection.CopyTo(System.Array array, int index) { }
         System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() { throw null; }
         void System.Runtime.Serialization.IDeserializationCallback.OnDeserialization(object sender) { }
         void System.Runtime.Serialization.ISerializable.GetObjectData(System.Runtime.Serialization.SerializationInfo info, System.Runtime.Serialization.StreamingContext context) { }
-        public bool TryGetValue(T equalValue, out T actualValue) { throw null; }
+        public bool TryGetValue(T equalValue, [System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T actualValue) { throw null; }
         public void UnionWith(System.Collections.Generic.IEnumerable<T> other) { }
         [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
         public partial struct Enumerator : System.Collections.Generic.IEnumerator<T>, System.Collections.IEnumerator, System.IDisposable, System.Runtime.Serialization.IDeserializationCallback, System.Runtime.Serialization.ISerializable
         {
             private object _dummy;
             public T Current { get { throw null; } }
@@ -645,14 +648,14 @@
         public void Push(T item) { }
         System.Collections.Generic.IEnumerator<T> System.Collections.Generic.IEnumerable<T>.GetEnumerator() { throw null; }
         void System.Collections.ICollection.CopyTo(System.Array array, int arrayIndex) { }
         System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() { throw null; }
         public T[] ToArray() { throw null; }
         public void TrimExcess() { }
-        public bool TryPeek(out T result) { throw null; }
-        public bool TryPop(out T result) { throw null; }
+        public bool TryPeek([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T result) { throw null; }
+        public bool TryPop([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T result) { throw null; }
         [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
         public partial struct Enumerator : System.Collections.Generic.IEnumerator<T>, System.Collections.IEnumerator, System.IDisposable
         {
             private T _currentElement;
             private object _dummy;
             private int _dummyPrimitive;
```
