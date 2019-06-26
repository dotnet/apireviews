# System.Threading

```diff
--- D:\Temp\nullable\before\System.Threading.dll.cs	2019-06-26 13:40:25.000000000 -0700
+++ D:\Temp\nullable\after\System.Threading.dll.cs	2019-06-26 13:40:45.000000000 -0700
@@ -28,20 +28,23 @@
     public readonly partial struct AsyncLocalValueChangedArgs<T>
     {
         private readonly T _PreviousValue_k__BackingField;
         private readonly T _CurrentValue_k__BackingField;
         private readonly object _dummy;
         private readonly int _dummyPrimitive;
+        [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]
         public T CurrentValue { get { throw null; } }
+        [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]
         public T PreviousValue { get { throw null; } }
         public bool ThreadContextChanged { get { throw null; } }
     }
     public sealed partial class AsyncLocal<T>
     {
         public AsyncLocal() { }
         public AsyncLocal(System.Action<System.Threading.AsyncLocalValueChangedArgs<T>> valueChangedHandler) { }
+        [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]
         public T Value { get { throw null; } set { } }
     }
     public sealed partial class AutoResetEvent : System.Threading.EventWaitHandle
     {
         public AutoResetEvent(bool initialState) : base (default(bool), default(System.Threading.EventResetMode)) { }
     }
@@ -108,13 +111,13 @@
         public EventWaitHandle(bool initialState, System.Threading.EventResetMode mode) { }
         public EventWaitHandle(bool initialState, System.Threading.EventResetMode mode, string name) { }
         public EventWaitHandle(bool initialState, System.Threading.EventResetMode mode, string name, out bool createdNew) { throw null; }
         public static System.Threading.EventWaitHandle OpenExisting(string name) { throw null; }
         public bool Reset() { throw null; }
         public bool Set() { throw null; }
-        public static bool TryOpenExisting(string name, out System.Threading.EventWaitHandle result) { throw null; }
+        public static bool TryOpenExisting(string name, [System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(true)]out System.Threading.EventWaitHandle result) { throw null; }
     }
     public sealed partial class ExecutionContext : System.IDisposable, System.Runtime.Serialization.ISerializable
     {
         internal ExecutionContext() { }
         public static System.Threading.ExecutionContext Capture() { throw null; }
         public System.Threading.ExecutionContext CreateCopy() { throw null; }
@@ -155,28 +158,28 @@
         public static int Decrement(ref int location) { throw null; }
         public static long Decrement(ref long location) { throw null; }
         public static double Exchange(ref double location1, double value) { throw null; }
         public static int Exchange(ref int location1, int value) { throw null; }
         public static long Exchange(ref long location1, long value) { throw null; }
         public static System.IntPtr Exchange(ref System.IntPtr location1, System.IntPtr value) { throw null; }
-        public static object Exchange(ref object location1, object value) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("value")]object Exchange([System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("value")]ref object location1, object value) { throw null; }
         public static float Exchange(ref float location1, float value) { throw null; }
-        public static T Exchange<T>(ref T location1, T value) where T : class { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("value")]T Exchange<T>([System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("value")]ref T location1, T value) where T : class { throw null; }
         public static int Increment(ref int location) { throw null; }
         public static long Increment(ref long location) { throw null; }
         public static void MemoryBarrier() { }
         public static void MemoryBarrierProcessWide() { }
         public static long Read(ref long location) { throw null; }
     }
     public static partial class LazyInitializer
     {
-        public static T EnsureInitialized<T>(ref T target) where T : class { throw null; }
-        public static T EnsureInitialized<T>(ref T target, ref bool initialized, ref object syncLock) { throw null; }
-        public static T EnsureInitialized<T>(ref T target, ref bool initialized, ref object syncLock, System.Func<T> valueFactory) { throw null; }
-        public static T EnsureInitialized<T>(ref T target, System.Func<T> valueFactory) where T : class { throw null; }
-        public static T EnsureInitialized<T>(ref T target, ref object syncLock, System.Func<T> valueFactory) where T : class { throw null; }
+        public static T EnsureInitialized<T>([System.Diagnostics.CodeAnalysis.NotNullAttribute]ref T target) where T : class { throw null; }
+        public static T EnsureInitialized<T>([System.Diagnostics.CodeAnalysis.AllowNullAttribute]ref T target, ref bool initialized, [System.Diagnostics.CodeAnalysis.NotNullAttribute]ref object syncLock) { throw null; }
+        public static T EnsureInitialized<T>([System.Diagnostics.CodeAnalysis.AllowNullAttribute]ref T target, ref bool initialized, [System.Diagnostics.CodeAnalysis.NotNullAttribute]ref object syncLock, System.Func<T> valueFactory) { throw null; }
+        public static T EnsureInitialized<T>([System.Diagnostics.CodeAnalysis.NotNullAttribute]ref T target, System.Func<T> valueFactory) where T : class { throw null; }
+        public static T EnsureInitialized<T>([System.Diagnostics.CodeAnalysis.NotNullAttribute]ref T target, [System.Diagnostics.CodeAnalysis.NotNullAttribute]ref object syncLock, System.Func<T> valueFactory) where T : class { throw null; }
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public partial struct LockCookie
     {
         private object _dummy;
         public override bool Equals(object obj) { throw null; }
@@ -246,13 +249,13 @@
         public Mutex() { }
         public Mutex(bool initiallyOwned) { }
         public Mutex(bool initiallyOwned, string name) { }
         public Mutex(bool initiallyOwned, string name, out bool createdNew) { throw null; }
         public static System.Threading.Mutex OpenExisting(string name) { throw null; }
         public void ReleaseMutex() { }
-        public static bool TryOpenExisting(string name, out System.Threading.Mutex result) { throw null; }
+        public static bool TryOpenExisting(string name, [System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(true)]out System.Threading.Mutex result) { throw null; }
     }
     public sealed partial class ReaderWriterLock : System.Runtime.ConstrainedExecution.CriticalFinalizerObject
     {
         public ReaderWriterLock() { }
         public bool IsReaderLockHeld { get { throw null; } }
         public bool IsWriterLockHeld { get { throw null; } }
@@ -304,13 +307,13 @@
         public Semaphore(int initialCount, int maximumCount) { }
         public Semaphore(int initialCount, int maximumCount, string name) { }
         public Semaphore(int initialCount, int maximumCount, string name, out bool createdNew) { throw null; }
         public static System.Threading.Semaphore OpenExisting(string name) { throw null; }
         public int Release() { throw null; }
         public int Release(int releaseCount) { throw null; }
-        public static bool TryOpenExisting(string name, out System.Threading.Semaphore result) { throw null; }
+        public static bool TryOpenExisting(string name, [System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(true)]out System.Threading.Semaphore result) { throw null; }
     }
     public partial class SemaphoreFullException : System.SystemException
     {
         public SemaphoreFullException() { }
         protected SemaphoreFullException(System.Runtime.Serialization.SerializationInfo info, System.Runtime.Serialization.StreamingContext context) { }
         public SemaphoreFullException(string message) { }
@@ -398,12 +401,13 @@
     {
         public ThreadLocal() { }
         public ThreadLocal(bool trackAllValues) { }
         public ThreadLocal(System.Func<T> valueFactory) { }
         public ThreadLocal(System.Func<T> valueFactory, bool trackAllValues) { }
         public bool IsValueCreated { get { throw null; } }
+        [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]
         public T Value { get { throw null; } set { } }
         public System.Collections.Generic.IList<T> Values { get { throw null; } }
         public void Dispose() { }
         protected virtual void Dispose(bool disposing) { }
         ~ThreadLocal() { }
         public override string ToString() { throw null; }
@@ -425,13 +429,13 @@
         [System.CLSCompliantAttribute(false)]
         public static uint Read(ref uint location) { throw null; }
         [System.CLSCompliantAttribute(false)]
         public static ulong Read(ref ulong location) { throw null; }
         [System.CLSCompliantAttribute(false)]
         public static System.UIntPtr Read(ref System.UIntPtr location) { throw null; }
-        public static T Read<T>(ref T location) where T : class { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("location")]T Read<T>(ref T location) where T : class { throw null; }
         public static void Write(ref bool location, bool value) { }
         public static void Write(ref byte location, byte value) { }
         public static void Write(ref double location, double value) { }
         public static void Write(ref short location, short value) { }
         public static void Write(ref int location, int value) { }
         public static void Write(ref long location, long value) { }
@@ -444,16 +448,16 @@
         [System.CLSCompliantAttribute(false)]
         public static void Write(ref uint location, uint value) { }
         [System.CLSCompliantAttribute(false)]
         public static void Write(ref ulong location, ulong value) { }
         [System.CLSCompliantAttribute(false)]
         public static void Write(ref System.UIntPtr location, System.UIntPtr value) { }
-        public static void Write<T>(ref T location, T value) where T : class { }
+        public static void Write<T>([System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("value")]ref T location, T value) where T : class { }
     }
     public partial class WaitHandleCannotBeOpenedException : System.ApplicationException
     {
         public WaitHandleCannotBeOpenedException() { }
         protected WaitHandleCannotBeOpenedException(System.Runtime.Serialization.SerializationInfo info, System.Runtime.Serialization.StreamingContext context) { }
         public WaitHandleCannotBeOpenedException(string message) { }
         public WaitHandleCannotBeOpenedException(string message, System.Exception innerException) { }
     }
 }
```
