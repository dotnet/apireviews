# System.Memory

```diff
--- D:\Temp\nullable\before\System.Memory.dll.cs	2019-06-26 13:40:15.000000000 -0700
+++ D:\Temp\nullable\after\System.Memory.dll.cs	2019-06-26 13:40:34.000000000 -0700
@@ -460,24 +460,24 @@
         public static System.Span<T> CreateSpan<T>(ref T reference, int length) { throw null; }
         public static ref T GetReference<T>(System.ReadOnlySpan<T> span) { throw null; }
         public static ref T GetReference<T>(System.Span<T> span) { throw null; }
         public static T Read<T>(System.ReadOnlySpan<byte> source) where T : struct { throw null; }
         public static System.Collections.Generic.IEnumerable<T> ToEnumerable<T>(System.ReadOnlyMemory<T> memory) { throw null; }
         public static bool TryGetArray<T>(System.ReadOnlyMemory<T> memory, out System.ArraySegment<T> segment) { throw null; }
-        public static bool TryGetMemoryManager<T, TManager>(System.ReadOnlyMemory<T> memory, out TManager manager) where TManager : System.Buffers.MemoryManager<T> { throw null; }
-        public static bool TryGetMemoryManager<T, TManager>(System.ReadOnlyMemory<T> memory, out TManager manager, out int start, out int length) where TManager : System.Buffers.MemoryManager<T> { throw null; }
-        public static bool TryGetString(System.ReadOnlyMemory<char> memory, out string text, out int start, out int length) { throw null; }
+        public static bool TryGetMemoryManager<T, TManager>(System.ReadOnlyMemory<T> memory, [System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(true)]out TManager manager) where TManager : System.Buffers.MemoryManager<T> { throw null; }
+        public static bool TryGetMemoryManager<T, TManager>(System.ReadOnlyMemory<T> memory, [System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(true)]out TManager manager, out int start, out int length) where TManager : System.Buffers.MemoryManager<T> { throw null; }
+        public static bool TryGetString(System.ReadOnlyMemory<char> memory, [System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(true)]out string text, out int start, out int length) { throw null; }
         public static bool TryRead<T>(System.ReadOnlySpan<byte> source, out T value) where T : struct { throw null; }
         public static bool TryWrite<T>(System.Span<byte> destination, ref T value) where T : struct { throw null; }
         public static void Write<T>(System.Span<byte> destination, ref T value) where T : struct { }
     }
     public static partial class SequenceMarshal
     {
         public static bool TryGetArray<T>(System.Buffers.ReadOnlySequence<T> sequence, out System.ArraySegment<T> segment) { throw null; }
         public static bool TryGetReadOnlyMemory<T>(System.Buffers.ReadOnlySequence<T> sequence, out System.ReadOnlyMemory<T> memory) { throw null; }
-        public static bool TryGetReadOnlySequenceSegment<T>(System.Buffers.ReadOnlySequence<T> sequence, out System.Buffers.ReadOnlySequenceSegment<T> startSegment, out int startIndex, out System.Buffers.ReadOnlySequenceSegment<T> endSegment, out int endIndex) { throw null; }
+        public static bool TryGetReadOnlySequenceSegment<T>(System.Buffers.ReadOnlySequence<T> sequence, [System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(true)]out System.Buffers.ReadOnlySequenceSegment<T> startSegment, out int startIndex, [System.Diagnostics.CodeAnalysis.NotNullWhenAttribute(true)]out System.Buffers.ReadOnlySequenceSegment<T> endSegment, out int endIndex) { throw null; }
         public static bool TryRead<T>(ref System.Buffers.SequenceReader<byte> reader, out T value) where T : struct, System.ValueType reqmod System.Runtime.InteropServices.UnmanagedType { throw null; }
     }
 }
 namespace System.Text
 {
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
```
