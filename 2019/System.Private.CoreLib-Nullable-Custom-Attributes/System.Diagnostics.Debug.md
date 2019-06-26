# System.Diagnostics.Debug

```diff
--- D:\Temp\nullable\before\System.Diagnostics.Debug.dll.cs	2019-06-26 13:40:12.000000000 -0700
+++ D:\Temp\nullable\after\System.Diagnostics.Debug.dll.cs	2019-06-26 13:40:31.000000000 -0700
@@ -3,23 +3,25 @@
     public static partial class Debug
     {
         public static bool AutoFlush { get { throw null; } set { } }
         public static int IndentLevel { get { throw null; } set { } }
         public static int IndentSize { get { throw null; } set { } }
         [System.Diagnostics.ConditionalAttribute("DEBUG")]
-        public static void Assert(bool condition) { }
+        public static void Assert([System.Diagnostics.CodeAnalysis.DoesNotReturnIfAttribute(false)]bool condition) { }
         [System.Diagnostics.ConditionalAttribute("DEBUG")]
-        public static void Assert(bool condition, string message) { }
+        public static void Assert([System.Diagnostics.CodeAnalysis.DoesNotReturnIfAttribute(false)]bool condition, string message) { }
         [System.Diagnostics.ConditionalAttribute("DEBUG")]
-        public static void Assert(bool condition, string message, string detailMessage) { }
+        public static void Assert([System.Diagnostics.CodeAnalysis.DoesNotReturnIfAttribute(false)]bool condition, string message, string detailMessage) { }
         [System.Diagnostics.ConditionalAttribute("DEBUG")]
-        public static void Assert(bool condition, string message, string detailMessageFormat, params object[] args) { }
+        public static void Assert([System.Diagnostics.CodeAnalysis.DoesNotReturnIfAttribute(false)]bool condition, string message, string detailMessageFormat, params object[] args) { }
         [System.Diagnostics.ConditionalAttribute("DEBUG")]
         public static void Close() { }
+        [System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute]
         [System.Diagnostics.ConditionalAttribute("DEBUG")]
         public static void Fail(string message) { }
+        [System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute]
         [System.Diagnostics.ConditionalAttribute("DEBUG")]
         public static void Fail(string message, string detailMessage) { }
         [System.Diagnostics.ConditionalAttribute("DEBUG")]
         public static void Flush() { }
         [System.Diagnostics.ConditionalAttribute("DEBUG")]
         public static void Indent() { }
```
