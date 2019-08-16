# System.Collections.Concurrent

```diff
--- D:\Temp\nullable\before\System.Collections.Concurrent.dll.cs	2019-06-26 13:40:10.000000000 -0700
+++ D:\Temp\nullable\after\System.Collections.Concurrent.dll.cs	2019-06-26 13:40:29.000000000 -0700
@@ -24,31 +24,31 @@
         public System.Collections.Generic.IEnumerable<T> GetConsumingEnumerable(System.Threading.CancellationToken cancellationToken) { throw null; }
         System.Collections.Generic.IEnumerator<T> System.Collections.Generic.IEnumerable<T>.GetEnumerator() { throw null; }
         void System.Collections.ICollection.CopyTo(System.Array array, int index) { }
         System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() { throw null; }
         public T Take() { throw null; }
         public T Take(System.Threading.CancellationToken cancellationToken) { throw null; }
-        public static int TakeFromAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, out T item) { throw null; }
-        public static int TakeFromAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, out T item, System.Threading.CancellationToken cancellationToken) { throw null; }
+        public static int TakeFromAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]out T item) { throw null; }
+        public static int TakeFromAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]out T item, System.Threading.CancellationToken cancellationToken) { throw null; }
         public T[] ToArray() { throw null; }
         public bool TryAdd(T item) { throw null; }
         public bool TryAdd(T item, int millisecondsTimeout) { throw null; }
         public bool TryAdd(T item, int millisecondsTimeout, System.Threading.CancellationToken cancellationToken) { throw null; }
         public bool TryAdd(T item, System.TimeSpan timeout) { throw null; }
         public static int TryAddToAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, T item) { throw null; }
         public static int TryAddToAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, T item, int millisecondsTimeout) { throw null; }
         public static int TryAddToAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, T item, int millisecondsTimeout, System.Threading.CancellationToken cancellationToken) { throw null; }
         public static int TryAddToAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, T item, System.TimeSpan timeout) { throw null; }
-        public bool TryTake(out T item) { throw null; }
-        public bool TryTake(out T item, int millisecondsTimeout) { throw null; }
-        public bool TryTake(out T item, int millisecondsTimeout, System.Threading.CancellationToken cancellationToken) { throw null; }
-        public bool TryTake(out T item, System.TimeSpan timeout) { throw null; }
-        public static int TryTakeFromAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, out T item) { throw null; }
-        public static int TryTakeFromAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, out T item, int millisecondsTimeout) { throw null; }
-        public static int TryTakeFromAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, out T item, int millisecondsTimeout, System.Threading.CancellationToken cancellationToken) { throw null; }
-        public static int TryTakeFromAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, out T item, System.TimeSpan timeout) { throw null; }
+        public bool TryTake([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T item) { throw null; }
+        public bool TryTake([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T item, int millisecondsTimeout) { throw null; }
+        public bool TryTake([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T item, int millisecondsTimeout, System.Threading.CancellationToken cancellationToken) { throw null; }
+        public bool TryTake([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T item, System.TimeSpan timeout) { throw null; }
+        public static int TryTakeFromAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]out T item) { throw null; }
+        public static int TryTakeFromAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]out T item, int millisecondsTimeout) { throw null; }
+        public static int TryTakeFromAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]out T item, int millisecondsTimeout, System.Threading.CancellationToken cancellationToken) { throw null; }
+        public static int TryTakeFromAny(System.Collections.Concurrent.BlockingCollection<T>[] collections, [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]out T item, System.TimeSpan timeout) { throw null; }
     }
     public partial class ConcurrentBag<T> : System.Collections.Concurrent.IProducerConsumerCollection<T>, System.Collections.Generic.IEnumerable<T>, System.Collections.Generic.IReadOnlyCollection<T>, System.Collections.ICollection, System.Collections.IEnumerable
     {
         public ConcurrentBag() { }
         public ConcurrentBag(System.Collections.Generic.IEnumerable<T> collection) { }
         public int Count { get { throw null; } }
@@ -60,14 +60,14 @@
         public void CopyTo(T[] array, int index) { }
         public System.Collections.Generic.IEnumerator<T> GetEnumerator() { throw null; }
         bool System.Collections.Concurrent.IProducerConsumerCollection<T>.TryAdd(T item) { throw null; }
         void System.Collections.ICollection.CopyTo(System.Array array, int index) { }
         System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() { throw null; }
         public T[] ToArray() { throw null; }
-        public bool TryPeek(out T result) { throw null; }
-        public bool TryTake(out T result) { throw null; }
+        public bool TryPeek([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T result) { throw null; }
+        public bool TryTake([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T result) { throw null; }
     }
     public partial class ConcurrentDictionary<TKey, TValue> : System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey, TValue>>, System.Collections.Generic.IDictionary<TKey, TValue>, System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<TKey, TValue>>, System.Collections.Generic.IReadOnlyCollection<System.Collections.Generic.KeyValuePair<TKey, TValue>>, System.Collections.Generic.IReadOnlyDictionary<TKey, TValue>, System.Collections.ICollection, System.Collections.IDictionary, System.Collections.IEnumerable
     {
         public ConcurrentDictionary() { }
         public ConcurrentDictionary(System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<TKey, TValue>> collection) { }
         public ConcurrentDictionary(System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<TKey, TValue>> collection, System.Collections.Generic.IEqualityComparer<TKey> comparer) { }
@@ -110,14 +110,14 @@
         bool System.Collections.IDictionary.Contains(object key) { throw null; }
         System.Collections.IDictionaryEnumerator System.Collections.IDictionary.GetEnumerator() { throw null; }
         void System.Collections.IDictionary.Remove(object key) { }
         System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() { throw null; }
         public System.Collections.Generic.KeyValuePair<TKey, TValue>[] ToArray() { throw null; }
         public bool TryAdd(TKey key, TValue value) { throw null; }
-        public bool TryGetValue(TKey key, out TValue value) { throw null; }
-        public bool TryRemove(TKey key, out TValue value) { throw null; }
+        public bool TryGetValue(TKey key, [System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out TValue value) { throw null; }
+        public bool TryRemove(TKey key, [System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out TValue value) { throw null; }
         public bool TryUpdate(TKey key, TValue newValue, TValue comparisonValue) { throw null; }
     }
     public partial class ConcurrentQueue<T> : System.Collections.Concurrent.IProducerConsumerCollection<T>, System.Collections.Generic.IEnumerable<T>, System.Collections.Generic.IReadOnlyCollection<T>, System.Collections.ICollection, System.Collections.IEnumerable
     {
         public ConcurrentQueue() { }
         public ConcurrentQueue(System.Collections.Generic.IEnumerable<T> collection) { }
@@ -131,14 +131,14 @@
         public System.Collections.Generic.IEnumerator<T> GetEnumerator() { throw null; }
         bool System.Collections.Concurrent.IProducerConsumerCollection<T>.TryAdd(T item) { throw null; }
         bool System.Collections.Concurrent.IProducerConsumerCollection<T>.TryTake(out T item) { throw null; }
         void System.Collections.ICollection.CopyTo(System.Array array, int index) { }
         System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() { throw null; }
         public T[] ToArray() { throw null; }
-        public bool TryDequeue(out T result) { throw null; }
-        public bool TryPeek(out T result) { throw null; }
+        public bool TryDequeue([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T result) { throw null; }
+        public bool TryPeek([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T result) { throw null; }
     }
     public partial class ConcurrentStack<T> : System.Collections.Concurrent.IProducerConsumerCollection<T>, System.Collections.Generic.IEnumerable<T>, System.Collections.Generic.IReadOnlyCollection<T>, System.Collections.ICollection, System.Collections.IEnumerable
     {
         public ConcurrentStack() { }
         public ConcurrentStack(System.Collections.Generic.IEnumerable<T> collection) { }
         public int Count { get { throw null; } }
@@ -153,14 +153,14 @@
         public void PushRange(T[] items, int startIndex, int count) { }
         bool System.Collections.Concurrent.IProducerConsumerCollection<T>.TryAdd(T item) { throw null; }
         bool System.Collections.Concurrent.IProducerConsumerCollection<T>.TryTake(out T item) { throw null; }
         void System.Collections.ICollection.CopyTo(System.Array array, int index) { }
         System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() { throw null; }
         public T[] ToArray() { throw null; }
-        public bool TryPeek(out T result) { throw null; }
-        public bool TryPop(out T result) { throw null; }
+        public bool TryPeek([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T result) { throw null; }
+        public bool TryPop([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T result) { throw null; }
         public int TryPopRange(T[] items) { throw null; }
         public int TryPopRange(T[] items, int startIndex, int count) { throw null; }
     }
     [System.FlagsAttribute]
     public enum EnumerablePartitionerOptions
     {
@@ -169,13 +169,13 @@
     }
     public partial interface IProducerConsumerCollection<T> : System.Collections.Generic.IEnumerable<T>, System.Collections.ICollection, System.Collections.IEnumerable
     {
         void CopyTo(T[] array, int index);
         T[] ToArray();
         bool TryAdd(T item);
-        bool TryTake(out T item);
+        bool TryTake([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out T item);
     }
     public abstract partial class OrderablePartitioner<TSource> : System.Collections.Concurrent.Partitioner<TSource>
     {
         protected OrderablePartitioner(bool keysOrderedInEachPartition, bool keysOrderedAcrossPartitions, bool keysNormalized) { }
         public bool KeysNormalized { get { throw null; } }
         public bool KeysOrderedAcrossPartitions { get { throw null; } }
```