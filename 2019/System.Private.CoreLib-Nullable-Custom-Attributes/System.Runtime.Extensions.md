# System.Runtime.Extensions

```diff
--- D:\Temp\nullable\before\System.Runtime.Extensions.dll.cs	2019-06-26 13:40:21.000000000 -0700
+++ D:\Temp\nullable\after\System.Runtime.Extensions.dll.cs	2019-06-26 13:40:40.000000000 -0700
@@ -205,13 +205,13 @@
         public ContextStaticAttribute() { }
     }
     public static partial class Convert
     {
         public static readonly object DBNull;
         public static object ChangeType(object value, System.Type conversionType) { throw null; }
-        public static object ChangeType(object value, System.Type conversionType, System.IFormatProvider provider) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("value")]object ChangeType(object value, System.Type conversionType, System.IFormatProvider provider) { throw null; }
         public static object ChangeType(object value, System.TypeCode typeCode) { throw null; }
         public static object ChangeType(object value, System.TypeCode typeCode, System.IFormatProvider provider) { throw null; }
         public static byte[] FromBase64CharArray(char[] inArray, int offset, int length) { throw null; }
         public static byte[] FromBase64String(string s) { throw null; }
         public static System.TypeCode GetTypeCode(object value) { throw null; }
         public static bool IsDBNull(object value) { throw null; }
@@ -511,14 +511,14 @@
         [System.CLSCompliantAttribute(false)]
         public static string ToString(sbyte value) { throw null; }
         [System.CLSCompliantAttribute(false)]
         public static string ToString(sbyte value, System.IFormatProvider provider) { throw null; }
         public static string ToString(float value) { throw null; }
         public static string ToString(float value, System.IFormatProvider provider) { throw null; }
-        public static string ToString(string value) { throw null; }
-        public static string ToString(string value, System.IFormatProvider provider) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("value")]string ToString(string value) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("value")]string ToString(string value, System.IFormatProvider provider) { throw null; }
         [System.CLSCompliantAttribute(false)]
         public static string ToString(ushort value) { throw null; }
         [System.CLSCompliantAttribute(false)]
         public static string ToString(ushort value, System.IFormatProvider provider) { throw null; }
         [System.CLSCompliantAttribute(false)]
         public static string ToString(uint value) { throw null; }
@@ -666,15 +666,18 @@
         public static long TickCount64 { get { throw null; } }
         public static string UserDomainName { get { throw null; } }
         public static bool UserInteractive { get { throw null; } }
         public static string UserName { get { throw null; } }
         public static System.Version Version { get { throw null; } }
         public static long WorkingSet { get { throw null; } }
+        [System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute]
         public static void Exit(int exitCode) { }
         public static string ExpandEnvironmentVariables(string name) { throw null; }
+        [System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute]
         public static void FailFast(string message) { }
+        [System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute]
         public static void FailFast(string message, System.Exception exception) { }
         public static string[] GetCommandLineArgs() { throw null; }
         public static string GetEnvironmentVariable(string variable) { throw null; }
         public static string GetEnvironmentVariable(string variable, System.EnvironmentVariableTarget target) { throw null; }
         public static System.Collections.IDictionary GetEnvironmentVariables() { throw null; }
         public static System.Collections.IDictionary GetEnvironmentVariables(System.EnvironmentVariableTarget target) { throw null; }
@@ -1002,20 +1005,27 @@
         public UriBuilder(string uri) { }
         public UriBuilder(string schemeName, string hostName) { }
         public UriBuilder(string scheme, string host, int portNumber) { }
         public UriBuilder(string scheme, string host, int port, string pathValue) { }
         public UriBuilder(string scheme, string host, int port, string path, string extraValue) { }
         public UriBuilder(System.Uri uri) { }
+        [System.Diagnostics.CodeAnalysis.AllowNullAttribute]
         public string Fragment { get { throw null; } set { } }
+        [System.Diagnostics.CodeAnalysis.AllowNullAttribute]
         public string Host { get { throw null; } set { } }
+        [System.Diagnostics.CodeAnalysis.AllowNullAttribute]
         public string Password { get { throw null; } set { } }
+        [System.Diagnostics.CodeAnalysis.AllowNullAttribute]
         public string Path { get { throw null; } set { } }
         public int Port { get { throw null; } set { } }
+        [System.Diagnostics.CodeAnalysis.AllowNullAttribute]
         public string Query { get { throw null; } set { } }
+        [System.Diagnostics.CodeAnalysis.AllowNullAttribute]
         public string Scheme { get { throw null; } set { } }
         public System.Uri Uri { get { throw null; } }
+        [System.Diagnostics.CodeAnalysis.AllowNullAttribute]
         public string UserName { get { throw null; } set { } }
         public override bool Equals(object rparam) { throw null; }
         public override int GetHashCode() { throw null; }
         public override string ToString() { throw null; }
     }
 }
@@ -1026,12 +1036,13 @@
         public const string DefaultTabString = "    ";
         public IndentedTextWriter(System.IO.TextWriter writer) { }
         public IndentedTextWriter(System.IO.TextWriter writer, string tabString) { }
         public override System.Text.Encoding Encoding { get { throw null; } }
         public int Indent { get { throw null; } set { } }
         public System.IO.TextWriter InnerWriter { get { throw null; } }
+        [System.Diagnostics.CodeAnalysis.AllowNullAttribute]
         public override string NewLine { get { throw null; } set { } }
         public override void Close() { }
         public override void Flush() { }
         protected virtual void OutputTabs() { }
         public override void Write(bool value) { }
         public override void Write(char value) { }
@@ -1388,27 +1399,27 @@
         public static readonly char AltDirectorySeparatorChar;
         public static readonly char DirectorySeparatorChar;
         [System.ObsoleteAttribute("Please use GetInvalidPathChars or GetInvalidFileNameChars instead.")]
         public static readonly char[] InvalidPathChars;
         public static readonly char PathSeparator;
         public static readonly char VolumeSeparatorChar;
-        public static string ChangeExtension(string path, string extension) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("path")]string ChangeExtension(string path, string extension) { throw null; }
         public static string Combine(string path1, string path2) { throw null; }
         public static string Combine(string path1, string path2, string path3) { throw null; }
         public static string Combine(string path1, string path2, string path3, string path4) { throw null; }
         public static string Combine(params string[] paths) { throw null; }
         public static bool EndsInDirectorySeparator(System.ReadOnlySpan<char> path) { throw null; }
         public static bool EndsInDirectorySeparator(string path) { throw null; }
         public static System.ReadOnlySpan<char> GetDirectoryName(System.ReadOnlySpan<char> path) { throw null; }
         public static string GetDirectoryName(string path) { throw null; }
         public static System.ReadOnlySpan<char> GetExtension(System.ReadOnlySpan<char> path) { throw null; }
-        public static string GetExtension(string path) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("path")]string GetExtension(string path) { throw null; }
         public static System.ReadOnlySpan<char> GetFileName(System.ReadOnlySpan<char> path) { throw null; }
-        public static string GetFileName(string path) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("path")]string GetFileName(string path) { throw null; }
         public static System.ReadOnlySpan<char> GetFileNameWithoutExtension(System.ReadOnlySpan<char> path) { throw null; }
-        public static string GetFileNameWithoutExtension(string path) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("path")]string GetFileNameWithoutExtension(string path) { throw null; }
         public static string GetFullPath(string path) { throw null; }
         public static string GetFullPath(string path, string basePath) { throw null; }
         public static char[] GetInvalidFileNameChars() { throw null; }
         public static char[] GetInvalidPathChars() { throw null; }
         public static System.ReadOnlySpan<char> GetPathRoot(System.ReadOnlySpan<char> path) { throw null; }
         public static string GetPathRoot(string path) { throw null; }
@@ -1590,12 +1601,13 @@
         protected char[] CoreNewLine;
         public static readonly System.IO.TextWriter Null;
         protected TextWriter() { }
         protected TextWriter(System.IFormatProvider formatProvider) { }
         public abstract System.Text.Encoding Encoding { get; }
         public virtual System.IFormatProvider FormatProvider { get { throw null; } }
+        [System.Diagnostics.CodeAnalysis.AllowNullAttribute]
         public virtual string NewLine { get { throw null; } set { } }
         public virtual void Close() { }
         public void Dispose() { }
         protected virtual void Dispose(bool disposing) { }
         public virtual System.Threading.Tasks.ValueTask DisposeAsync() { throw null; }
         public virtual void Flush() { }
@@ -1660,20 +1672,20 @@
     }
 }
 namespace System.Net
 {
     public static partial class WebUtility
     {
-        public static string HtmlDecode(string value) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("value")]string HtmlDecode(string value) { throw null; }
         public static void HtmlDecode(string value, System.IO.TextWriter output) { }
-        public static string HtmlEncode(string value) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("value")]string HtmlEncode(string value) { throw null; }
         public static void HtmlEncode(string value, System.IO.TextWriter output) { }
-        public static string UrlDecode(string encodedValue) { throw null; }
-        public static byte[] UrlDecodeToBytes(byte[] encodedValue, int offset, int count) { throw null; }
-        public static string UrlEncode(string value) { throw null; }
-        public static byte[] UrlEncodeToBytes(byte[] value, int offset, int count) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("encodedValue")]string UrlDecode(string encodedValue) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("encodedValue")]byte[] UrlDecodeToBytes(byte[] encodedValue, int offset, int count) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("value")]string UrlEncode(string value) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("value")]byte[] UrlEncodeToBytes(byte[] value, int offset, int count) { throw null; }
     }
 }
 namespace System.Numerics
 {
     public static partial class BitOperations
     {
```
