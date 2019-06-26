# System.Runtime

```diff
--- D:\Temp\nullable\before\System.Runtime.dll.cs	2019-06-26 13:40:20.000000000 -0700
+++ D:\Temp\nullable\after\System.Runtime.dll.cs	2019-06-26 13:40:39.000000000 -0700
@@ -208,14 +208,14 @@
         public static int FindIndex<T>(T[] array, int startIndex, int count, System.Predicate<T> match) { throw null; }
         public static int FindIndex<T>(T[] array, int startIndex, System.Predicate<T> match) { throw null; }
         public static int FindIndex<T>(T[] array, System.Predicate<T> match) { throw null; }
         public static int FindLastIndex<T>(T[] array, int startIndex, int count, System.Predicate<T> match) { throw null; }
         public static int FindLastIndex<T>(T[] array, int startIndex, System.Predicate<T> match) { throw null; }
         public static int FindLastIndex<T>(T[] array, System.Predicate<T> match) { throw null; }
-        public static T FindLast<T>(T[] array, System.Predicate<T> match) { throw null; }
-        public static T Find<T>(T[] array, System.Predicate<T> match) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]T FindLast<T>(T[] array, System.Predicate<T> match) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]T Find<T>(T[] array, System.Predicate<T> match) { throw null; }
         public static void ForEach<T>(T[] array, System.Action<T> action) { }
         public System.Collections.IEnumerator GetEnumerator() { throw null; }
         public int GetLength(int dimension) { throw null; }
         public long GetLongLength(int dimension) { throw null; }
         public int GetLowerBound(int dimension) { throw null; }
         public int GetUpperBound(int dimension) { throw null; }
@@ -237,13 +237,13 @@
         public static int LastIndexOf(System.Array array, object value) { throw null; }
         public static int LastIndexOf(System.Array array, object value, int startIndex) { throw null; }
         public static int LastIndexOf(System.Array array, object value, int startIndex, int count) { throw null; }
         public static int LastIndexOf<T>(T[] array, T value) { throw null; }
         public static int LastIndexOf<T>(T[] array, T value, int startIndex) { throw null; }
         public static int LastIndexOf<T>(T[] array, T value, int startIndex, int count) { throw null; }
-        public static void Resize<T>(ref T[] array, int newSize) { }
+        public static void Resize<T>([System.Diagnostics.CodeAnalysis.NotNullAttribute]ref T[] array, int newSize) { }
         public static void Reverse(System.Array array) { }
         public static void Reverse(System.Array array, int index, int length) { }
         public static void Reverse<T>(T[] array) { }
         public static void Reverse<T>(T[] array, int index, int length) { }
         public void SetValue(object value, int index) { }
         public void SetValue(object value, int index1, int index2) { }
@@ -1007,13 +1007,13 @@
     {
         protected Delegate(object target, string method) { }
         protected Delegate(System.Type target, string method) { }
         public System.Reflection.MethodInfo Method { get { throw null; } }
         public object Target { get { throw null; } }
         public virtual object Clone() { throw null; }
-        public static System.Delegate Combine(System.Delegate a, System.Delegate b) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("a"), System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("b")]System.Delegate Combine(System.Delegate a, System.Delegate b) { throw null; }
         public static System.Delegate Combine(params System.Delegate[] delegates) { throw null; }
         protected virtual System.Delegate CombineImpl(System.Delegate d) { throw null; }
         public static System.Delegate CreateDelegate(System.Type type, object firstArgument, System.Reflection.MethodInfo method) { throw null; }
         public static System.Delegate CreateDelegate(System.Type type, object firstArgument, System.Reflection.MethodInfo method, bool throwOnBindFailure) { throw null; }
         public static System.Delegate CreateDelegate(System.Type type, object target, string method) { throw null; }
         public static System.Delegate CreateDelegate(System.Type type, object target, string method, bool ignoreCase) { throw null; }
@@ -1426,13 +1426,13 @@
     public partial interface IComparable
     {
         int CompareTo(object obj);
     }
     public partial interface IComparable<in T>
     {
-        int CompareTo(T other);
+        int CompareTo([System.Diagnostics.CodeAnalysis.AllowNullAttribute]T other);
     }
     [System.CLSCompliantAttribute(false)]
     public partial interface IConvertible
     {
         System.TypeCode GetTypeCode();
         bool ToBoolean(System.IFormatProvider provider);
@@ -1459,13 +1459,13 @@
     public partial interface IDisposable
     {
         void Dispose();
     }
     public partial interface IEquatable<T>
     {
-        bool Equals(T other);
+        bool Equals([System.Diagnostics.CodeAnalysis.AllowNullAttribute]T other);
     }
     public partial interface IFormatProvider
     {
         object GetFormat(System.Type formatType);
     }
     public partial interface IFormattable
@@ -2413,14 +2413,14 @@
         public int IndexOfAny(char[] anyOf, int startIndex, int count) { throw null; }
         public System.String Insert(int startIndex, System.String value) { throw null; }
         public static System.String Intern(System.String str) { throw null; }
         public static System.String IsInterned(System.String str) { throw null; }
         public bool IsNormalized() { throw null; }
         public bool IsNormalized(System.Text.NormalizationForm normalizationForm) { throw null; }
-        public static bool IsNullOrEmpty(System.String value) { throw null; }
-        public static bool IsNullOrWhiteSpace(System.String value) { throw null; }
+        public static bool IsNullOrEmpty([System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(false)]System.String value) { throw null; }
+        public static bool IsNullOrWhiteSpace([System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(false)]System.String value) { throw null; }
         public static System.String Join(char separator, params object[] values) { throw null; }
         public static System.String Join(char separator, params string[] value) { throw null; }
         public static System.String Join(char separator, string[] value, int startIndex, int count) { throw null; }
         public static System.String Join(System.String separator, System.Collections.Generic.IEnumerable<string> values) { throw null; }
         public static System.String Join(System.String separator, params object[] values) { throw null; }
         public static System.String Join(System.String separator, params string[] value) { throw null; }
@@ -3510,15 +3510,15 @@
         public static bool operator ==(System.Uri uri1, System.Uri uri2) { throw null; }
         public static bool operator !=(System.Uri uri1, System.Uri uri2) { throw null; }
         [System.ObsoleteAttribute("The method has been deprecated. It is not used by the system. https://go.microsoft.com/fwlink/?linkid=14202")]
         protected virtual void Parse() { }
         void System.Runtime.Serialization.ISerializable.GetObjectData(System.Runtime.Serialization.SerializationInfo serializationInfo, System.Runtime.Serialization.StreamingContext streamingContext) { }
         public override string ToString() { throw null; }
-        public static bool TryCreate(string uriString, System.UriKind uriKind, out System.Uri result) { throw null; }
-        public static bool TryCreate(System.Uri baseUri, string relativeUri, out System.Uri result) { throw null; }
-        public static bool TryCreate(System.Uri baseUri, System.Uri relativeUri, out System.Uri result) { throw null; }
+        public static bool TryCreate(string uriString, System.UriKind uriKind, [System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(true)]out System.Uri result) { throw null; }
+        public static bool TryCreate(System.Uri baseUri, string relativeUri, [System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(true)]out System.Uri result) { throw null; }
+        public static bool TryCreate(System.Uri baseUri, System.Uri relativeUri, [System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(true)]out System.Uri result) { throw null; }
         [System.ObsoleteAttribute("The method has been deprecated. Please use GetComponents() or static UnescapeDataString() to unescape a Uri component or a string. https://go.microsoft.com/fwlink/?linkid=14202")]
         protected virtual string Unescape(string path) { throw null; }
         public static string UnescapeDataString(string stringToUnescape) { throw null; }
     }
     [System.FlagsAttribute]
     public enum UriComponents
@@ -3812,14 +3812,14 @@
         public static System.Version Parse(System.ReadOnlySpan<char> input) { throw null; }
         public static System.Version Parse(string input) { throw null; }
         public override string ToString() { throw null; }
         public string ToString(int fieldCount) { throw null; }
         public bool TryFormat(System.Span<char> destination, int fieldCount, out int charsWritten) { throw null; }
         public bool TryFormat(System.Span<char> destination, out int charsWritten) { throw null; }
-        public static bool TryParse(System.ReadOnlySpan<char> input, out System.Version result) { throw null; }
-        public static bool TryParse(string input, out System.Version result) { throw null; }
+        public static bool TryParse(System.ReadOnlySpan<char> input, [System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(true)]out System.Version result) { throw null; }
+        public static bool TryParse(string input, [System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(true)]out System.Version result) { throw null; }
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
     public partial struct Void
     {
     }
     public partial class WeakReference : System.Runtime.Serialization.ISerializable
@@ -3837,13 +3837,13 @@
     {
         public WeakReference(T target) { }
         public WeakReference(T target, bool trackResurrection) { }
         ~WeakReference() { }
         public void GetObjectData(System.Runtime.Serialization.SerializationInfo info, System.Runtime.Serialization.StreamingContext context) { }
         public void SetTarget(T target) { }
-        public bool TryGetTarget(out T target) { throw null; }
+        public bool TryGetTarget([System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false), System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(true)]out T target) { throw null; }
     }
 }
 namespace System.Buffers
 {
     public partial interface IMemoryOwner<T> : System.IDisposable
     {
@@ -3912,12 +3912,13 @@
         int Compare(object x, object y);
     }
     public partial interface IDictionary : System.Collections.ICollection, System.Collections.IEnumerable
     {
         bool IsFixedSize { get; }
         bool IsReadOnly { get; }
+        [System.Diagnostics.CodeAnalysis.DisallowNullAttribute]
         object this[object key] { get; set; }
         System.Collections.ICollection Keys { get; }
         System.Collections.ICollection Values { get; }
         void Add(object key, object value);
         void Clear();
         bool Contains(object key);
@@ -3988,36 +3989,36 @@
         bool Contains(T item);
         void CopyTo(T[] array, int arrayIndex);
         bool Remove(T item);
     }
     public partial interface IComparer<in T>
     {
-        int Compare(T x, T y);
+        int Compare([System.Diagnostics.CodeAnalysis.AllowNullAttribute]T x, [System.Diagnostics.CodeAnalysis.AllowNullAttribute]T y);
     }
     public partial interface IDictionary<TKey, TValue> : System.Collections.Generic.ICollection<System.Collections.Generic.KeyValuePair<TKey, TValue>>, System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<TKey, TValue>>, System.Collections.IEnumerable
     {
         TValue this[TKey key] { get; set; }
         System.Collections.Generic.ICollection<TKey> Keys { get; }
         System.Collections.Generic.ICollection<TValue> Values { get; }
         void Add(TKey key, TValue value);
         bool ContainsKey(TKey key);
         bool Remove(TKey key);
-        bool TryGetValue(TKey key, out TValue value);
+        bool TryGetValue(TKey key, [System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out TValue value);
     }
     public partial interface IEnumerable<out T> : System.Collections.IEnumerable
     {
         new System.Collections.Generic.IEnumerator<T> GetEnumerator();
     }
     public partial interface IEnumerator<out T> : System.Collections.IEnumerator, System.IDisposable
     {
         new T Current { get; }
     }
     public partial interface IEqualityComparer<in T>
     {
-        bool Equals(T x, T y);
-        int GetHashCode(T obj);
+        bool Equals([System.Diagnostics.CodeAnalysis.AllowNullAttribute]T x, [System.Diagnostics.CodeAnalysis.AllowNullAttribute]T y);
+        int GetHashCode([System.Diagnostics.CodeAnalysis.DisallowNullAttribute]T obj);
     }
     public partial interface IList<T> : System.Collections.Generic.ICollection<T>, System.Collections.Generic.IEnumerable<T>, System.Collections.IEnumerable
     {
         T this[int index] { get; set; }
         int IndexOf(T item);
         void Insert(int index, T item);
@@ -4030,13 +4031,13 @@
     public partial interface IReadOnlyDictionary<TKey, TValue> : System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<TKey, TValue>>, System.Collections.Generic.IReadOnlyCollection<System.Collections.Generic.KeyValuePair<TKey, TValue>>, System.Collections.IEnumerable
     {
         TValue this[TKey key] { get; }
         System.Collections.Generic.IEnumerable<TKey> Keys { get; }
         System.Collections.Generic.IEnumerable<TValue> Values { get; }
         bool ContainsKey(TKey key);
-        bool TryGetValue(TKey key, out TValue value);
+        bool TryGetValue(TKey key, [System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out TValue value);
     }
     public partial interface IReadOnlyList<out T> : System.Collections.Generic.IEnumerable<T>, System.Collections.Generic.IReadOnlyCollection<T>, System.Collections.IEnumerable
     {
         T this[int index] { get; }
     }
     public partial interface ISet<T> : System.Collections.Generic.ICollection<T>, System.Collections.Generic.IEnumerable<T>, System.Collections.IEnumerable
@@ -6672,13 +6673,13 @@
         public void Clear() { }
         public TValue GetOrCreateValue(TKey key) { throw null; }
         public TValue GetValue(TKey key, System.Runtime.CompilerServices.ConditionalWeakTable<TKey, TValue>.CreateValueCallback createValueCallback) { throw null; }
         public bool Remove(TKey key) { throw null; }
         System.Collections.Generic.IEnumerator<System.Collections.Generic.KeyValuePair<TKey, TValue>> System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<TKey,TValue>>.GetEnumerator() { throw null; }
         System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() { throw null; }
-        public bool TryGetValue(TKey key, out TValue value) { throw null; }
+        public bool TryGetValue(TKey key, [System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute(false)]out TValue value) { throw null; }
         public delegate TValue CreateValueCallback(TKey key);
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public readonly partial struct ConfiguredTaskAwaitable
     {
         private readonly object _dummy;
@@ -6925,13 +6926,13 @@
     {
         public static int OffsetToStringData { get { throw null; } }
         public static void EnsureSufficientExecutionStack() { }
         public static new bool Equals(object o1, object o2) { throw null; }
         public static void ExecuteCodeWithGuaranteedCleanup(System.Runtime.CompilerServices.RuntimeHelpers.TryCode code, System.Runtime.CompilerServices.RuntimeHelpers.CleanupCode backoutCode, object userData) { }
         public static int GetHashCode(object o) { throw null; }
-        public static object GetObjectValue(object obj) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("obj")]object GetObjectValue(object obj) { throw null; }
         public static T[] GetSubArray<T>(T[] array, System.Range range) { throw null; }
         public static object GetUninitializedObject(System.Type type) { throw null; }
         public static void InitializeArray(System.Array array, System.RuntimeFieldHandle fldHandle) { }
         public static bool IsReferenceOrContainsReferences<T>() { throw null; }
         public static void PrepareConstrainedRegions() { }
         public static void PrepareConstrainedRegionsNoOP() { }
@@ -6967,12 +6968,13 @@
     public sealed partial class StringFreezingAttribute : System.Attribute
     {
         public StringFreezingAttribute() { }
     }
     public partial class StrongBox<T> : System.Runtime.CompilerServices.IStrongBox
     {
+        [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]
         public T Value;
         public StrongBox() { }
         public StrongBox(T value) { }
         object System.Runtime.CompilerServices.IStrongBox.Value { get { throw null; } set { } }
     }
     [System.AttributeUsageAttribute(System.AttributeTargets.Assembly | System.AttributeTargets.Module)]
@@ -7091,13 +7093,15 @@
 {
     public sealed partial class ExceptionDispatchInfo
     {
         internal ExceptionDispatchInfo() { }
         public System.Exception SourceException { get { throw null; } }
         public static System.Runtime.ExceptionServices.ExceptionDispatchInfo Capture(System.Exception source) { throw null; }
+        [System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute]
         public void Throw() { }
+        [System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute]
         public static void Throw(System.Exception source) { }
     }
     public partial class FirstChanceExceptionEventArgs : System.EventArgs
     {
         public FirstChanceExceptionEventArgs(System.Exception exception) { }
         public System.Exception Exception { get { throw null; } }
@@ -8047,12 +8051,13 @@
     {
         protected static readonly System.IntPtr InvalidHandle;
         public const int WaitTimeout = 258;
         protected WaitHandle() { }
         [System.ObsoleteAttribute("Use the SafeWaitHandle property instead.")]
         public virtual System.IntPtr Handle { get { throw null; } set { } }
+        [System.Diagnostics.CodeAnalysis.AllowNullAttribute]
         public Microsoft.Win32.SafeHandles.SafeWaitHandle SafeWaitHandle { get { throw null; } set { } }
         public virtual void Close() { }
         public void Dispose() { }
         protected virtual void Dispose(bool explicitDisposing) { }
         public static bool SignalAndWait(System.Threading.WaitHandle toSignal, System.Threading.WaitHandle toWaitOn) { throw null; }
         public static bool SignalAndWait(System.Threading.WaitHandle toSignal, System.Threading.WaitHandle toWaitOn, int millisecondsTimeout, bool exitContext) { throw null; }
```
