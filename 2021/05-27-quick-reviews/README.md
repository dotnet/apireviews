# Quick Reviews 05/27/2021

## Proposed API for symbolic links

**Approved** | [#runtime/24271](https://github.com/dotnet/runtime/issues/24271#issuecomment-849816518) | [Video](https://www.youtube.com/watch?v=wY0t4bkXzS4&t=0h0m0s)

* "GetLinkTargetPath" had some naming concerns for being too close to "GetLinkTarget"
  * The resolution to this was to rename "GetLinkTarget" to "ResolveLinkTarget" since it resolves the target into a FileSystemInfo, and to change the GetLinkTargetPath method a LinkTarget property

```diff
namespace System.IO
{
    public abstract partial class FileSystemInfo
    {
        public void CreateAsSymbolicLink(string pathToTarget); // Already approved

-       public FileSystemInfo? GetSymbolicLinkTarget(bool returnFinalTarget = false); // Already approved
+       public FileSystemInfo? ResolveLinkTarget(bool returnFinalTarget = false); // Rename

+       public string? LinkTarget { get; }
    }
    public static class File
    {
        public static FileSystemInfo CreateSymbolicLink(string path, string pathToTarget); // Already approved

-       public static FileSystemInfo? GetSymbolicLinkTarget(string linkPath, bool returnFinalTarget = false); // Already approved
+       public static FileSystemInfo? ResolveLinkTarget(string linkPath, bool returnFinalTarget = false); // Rename
    }
    public static class Directory
    {
        public static FileSystemInfo CreateSymbolicLink(string path, string pathToTarget); // Already approved

-       public static FileSystemInfo? GetSymbolicLinkTarget(string linkPath, bool returnFinalTarget = false); // Already approved
+       public static FileSystemInfo? ResolveLinkTarget(string linkPath, bool returnFinalTarget = false); // Rename
    }
}
```
## Add StringContent ctor providing tighter control over charset

**Approved** | [#runtime/17036](https://github.com/dotnet/runtime/issues/17036#issuecomment-849820882) | [Video](https://www.youtube.com/watch?v=wY0t4bkXzS4&t=0h33m50s)

Looks good as proposed.  We even discussed the casing on "charSet", and decided it's correct since it matches the existing property.

```diff
namespace System.Net.Http.Headers
{
    public class StringContent
    {
       public StringContent(string content);
+      public StringContent(string content, MediaTypeHeaderValue mediaType); // Already approved in iteration 1
       public StringContent(string content, Encoding encoding);
       public StringContent(string content, Encoding encoding);
       public StringContent(string content, Encoding encoding, string mediaType);
+      public StringContent(string content, Encoding encoding, MediaTypeHeaderValue mediaType); // NEW API proposal
    }
    
    public class MediaTypeHeaderValue
    {
        public MediaTypeHeaderValue(string mediaType)
+       public MediaTypeHeaderValue(string mediaType, string charSet);  // NEW API proposal
    }
}
## Add isAsync overload for Create/Open/etc.

**Approved** | [#runtime/24698](https://github.com/dotnet/runtime/issues/24698#issuecomment-849822584) | [Video](https://www.youtube.com/watch?v=wY0t4bkXzS4&t=0h41m10s)

Trusting that all of the removals are not yet implemented, or were to be new for .NET 6, the update looks good as proposed.

```diff
namespace System.IO
{
    public static partial class File
    {
-        public StreamWriter AppendText(string path, FileOptions options)
+        public StreamWriter AppendText(string path, FileStreamOptions options)
-        public static FileStream Create(string path, FileOptions options)
+        public static FileStream Create(string path, FileStreamOptions options)
-        public static StreamWriter CreateText(string path, FileOptions options)   
+        public static StreamWriter CreateText(string path, FileStreamOptions options)  
-        public static FileStream Open(string path, FileMode mode, FileOptions options)
-        public static FileStream Open(string path, FileMode mode, FileAccess access, FileOptions options)
-        public static FileStream Open(string path, FileMode mode, FileAccess access, FileShare share, FileOptions options)
+        public static FileStream Open(string path, FileMode mode, FileStreamOptions options)
-        public static FileStream OpenRead(string path, FileOptions options)
+        public static FileStream OpenRead(string path, FileStreamOptions options)
-        public static StreamReader OpenText(string path, FileOptions options)
+        public static StreamReader OpenText(string path, FileStreamOptions options)
-        public static FileStream OpenWrite(string path, FileOptions options)
+        public static FileStream OpenWrite(string path, FileStreamOptions options)
    }
    public sealed partial class FileInfo : FileSystemInfo
    {
-        public StreamWriter AppendText(FileOptions options)
+        public StreamWriter AppendText(FileStreamOptions options)
-        public FileStream Create(FileOptions options)
+        public FileStream Create(FileStreamOptions options)
-        public StreamWriter CreateText(FileOptions options)
+        public StreamWriter CreateText(FileStreamOptions options)
-        public FileStream Open(FileMode mode, FileOptions options)
-        public FileStream Open(FileMode mode, FileAccess access, FileOptions options)
-        public FileStream Open(FileMode mode, FileAccess access, FileShare share, FileOptions options)
+        public FileStream Open(FileMode mode, FileStreamOptions options)
-        public FileStream OpenRead(FileOptions options)
+        public FileStream OpenRead(FileStreamOptions options)
-        public StreamReader OpenText(FileOptions options)    
+        public StreamReader OpenText(FileStreamOptions options)    
-        public FileStream OpenWrite(FileOptions options)
+        public FileStream OpenWrite(FileStreamOptions options)
    }
    public partial class StreamReader : TextReader
    {
-        public StreamReader(string path, FileOptions options)
+        public StreamReader(string path, FileStreamOptions options)
-        public StreamReader(string path, bool detectEncodingFromByteOrderMarks, FileOptions options)
-        public StreamReader(string path, System.Text.Encoding encoding, FileOptions options)
-        public StreamReader(string path, System.Text.Encoding encoding, bool detectEncodingFromByteOrderMarks, FileOptions options)
-        public StreamReader(string path, System.Text.Encoding encoding, bool detectEncodingFromByteOrderMarks, int bufferSize, FileOptions options)
+        public StreamReader(string path, System.Text.Encoding encoding, bool detectEncodingFromByteOrderMarks, FileStreamOptions options)
    }
    public partial class StreamWriter : TextWriter
    {
-        public StreamWriter(string path, FileOptions options)
+        public StreamWriter(string path, FileStreamOptions options)
-        public StreamWriter(string path, bool append, FileOptions options)
-        public StreamWriter(string path, bool append, System.Text.Encoding encoding, FileOptions options)
-        public StreamWriter(string path, bool append, System.Text.Encoding encoding, int bufferSize, FileOptions options)
+        public StreamWriter(string path, System.Text.Encoding encoding, FileStreamOptions options)
    }
}
```
## Added cookie accessor/iterator to CookieContainer

**Approved** | [#runtime/44094](https://github.com/dotnet/runtime/issues/44094#issuecomment-849825241) | [Video](https://www.youtube.com/watch?v=wY0t4bkXzS4&t=0h44m7s)

Looks good as proposed

```C#
namespace System.Net
{
    partial class CookieContainer
    {
        public CookieCollection GetAllCookies();
    }
}
## Add more AsSpan overloads to StringSegment

**Approved** | [#runtime/50428](https://github.com/dotnet/runtime/issues/50428#issuecomment-849827151) | [Video](https://www.youtube.com/watch?v=wY0t4bkXzS4&t=0h48m22s)

Looks good as proposed.

```C#
namespace Microsoft.Extensions.Primitives
{
    partial struct StringSegment : IEquatable<StringSegment>, IEquatable<string>
    {
       public ReadOnlySpan<char> AsSpan(int start);
       public ReadOnlySpan<char> AsSpan(int start, int length);
    }
}
## Allow replacing IExternalScopeProvider via DI

**Approved** | [#runtime/50590](https://github.com/dotnet/runtime/issues/50590#issuecomment-849832463) | [Video](https://www.youtube.com/watch?v=wY0t4bkXzS4&t=0h51m36s)

The existing longest constructor needs to have its default removed to follow the guidance for how to add new defaulted parameters safely.

```diff
namespace Microsoft.Extensions.Logging
{
    public partial class LoggerFactory : Microsoft.Extensions.Logging.ILoggerFactory
    {
         public LoggerFactory(
             IEnumerable<ILoggerProvider> providers,
             IOptionsMonitor<LoggerFilterOptions> filterOption,
-            IOptions<LoggerFactoryOptions> options = null)
+            IOptions<LoggerFactoryOptions> options) { }

+        public LoggerFactory(
+            IEnumerable<ILoggerProvider> providers,
+            IOptionsMonitor<LoggerFilterOptions> filterOption,
+            LoggerFactoryOptions> options = null,
+            IExternalScopeProvider scopeProvider = null) { }

        }
    }
}
## Array.Clear(Array)

**Approved** | [#runtime/51581](https://github.com/dotnet/runtime/issues/51581#issuecomment-849834961) | [Video](https://www.youtube.com/watch?v=wY0t4bkXzS4&t=1h0m14s)

Looks good as proposed

```C#
namespace System
{
    partial class Array
    {
        public static void Clear(Array array);
    }
}
```
## Extend Vector64<T>, Vector128<T>, and Vector256<T> to support nint and nuint

**Approved** | [#runtime/52017](https://github.com/dotnet/runtime/issues/52017#issuecomment-849839836) | [Video](https://www.youtube.com/watch?v=wY0t4bkXzS4&t=1h3m30s)

Looks good as proposed.  A *very* forward looking question of "what about when nint is bigger than 64 bits?" was asked, and deferred to such a hypothetical date.

```C#
namespace System.Runtime.Intrinsics
{
    public static partial class Vector64
    {
        public static Vector64<nint> AsNInt<T>(Vector64<T> value);
        public static Vector64<nuint> AsNUInt<T>(Vector64<T> value);

        public static Vector64<nint> Create(nint value);
        public static Vector64<nuint> Create(nuint value);

        public static Vector64<nint> CreateScalar(nint value);
        public static Vector64<nuint> CreateScalar(nuint value);

        public static Vector64<nint> CreateScalarUnsafe(nint value);
        public static Vector64<nuint> CreateScalarUnsafe(nuint value);
    }

    public static partial class Vector128
    {
        public static Vector128<nint> AsNInt<T>(Vector128<T> value);
        public static Vector128<nuint> AsNUInt<T>(Vector128<T> value);

        public static Vector128<nint> Create(nint value);
        public static Vector128<nuint> Create(nuint value);

        public static Vector128<nint> Create(Vector64<nint> lower, Vector64<nint> upper);
        public static Vector128<nuint> Create(Vector64<nuint> lower, Vector64<nuint> upper);

        public static Vector128<nint> CreateScalar(nint value);
        public static Vector128<nuint> CreateScalar(nuint value);

        public static Vector128<nint> CreateScalarUnsafe(nint value);
        public static Vector128<nuint> CreateScalarUnsafe(nuint value);
    }

    public static partial class Vector256
    {
        public static Vector256<nint> AsNInt<T>(Vector256<T> value);
        public static Vector256<nuint> AsNUInt<T>(Vector256<T> value);

        public static Vector256<nint> Create(nint value);
        public static Vector256<nuint> Create(nuint value);

        public static Vector256<nint> Create(Vector128<nint> lower, Vector128<nint> upper);
        public static Vector256<nuint> Create(Vector128<nuint> lower, Vector128<nuint> upper);

        public static Vector256<nint> CreateScalar(nint value);
        public static Vector256<nuint> CreateScalar(nuint value);

        public static Vector256<nint> CreateScalarUnsafe(nint value);
        public static Vector256<nuint> CreateScalarUnsafe(nuint value);
    }
}
```
## Extend System.Runtime.Intrinsics.X86 to support nint and nuint

**Approved** | [#runtime/52021](https://github.com/dotnet/runtime/issues/52021#issuecomment-849843507) | [Video](https://www.youtube.com/watch?v=wY0t4bkXzS4&t=1h9m42s)

Looks good as proposed.


```C#
namespace System.Runtime.Intrinsics.X86
{
    public abstract partial class Sse
    {
        public static Vector128<float> ConvertScalarToVector128Single(Vector128<float> upper, nint value);

        public static nint ConvertToNInt(Vector128<float> value);
        public static nint ConvertToNIntWithTruncation(Vector128<float> value);
    }

    public abstract partial class Sse2
    {
        public static Vector128<double> ConvertScalarToVector128Double(Vector128<double> upper, nint value);

        public static Vector128<nint> ConvertScalarToVector128NInt(nint value);
        public static Vector128<nuint> ConvertScalarToVector128NUInt(nuint value);

        public static nint ConvertToNInt(Vector128<double> value);
        public static nint ConvertToNIntWithTruncation(Vector128<double> value);

        public static nint ConvertToNInt(Vector128<nint> value);
        public static nuint ConvertToNUInt(Vector128<nuint> value);

        public static unsafe void StoreNonTemporal(nint* address, nint value);
        public static unsafe void StoreNonTemporal(nuint* address, nuint value);

        public static Vector128<nint> Add(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> Add(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector128<nint> And(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> And(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector128<nint> AndNot(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> AndNot(Vector128<nuint> left, Vector128<nuint> right);

        public static unsafe Vector128<nint> LoadVector128(nint* address);
        public static unsafe Vector128<nuint> LoadVector128(nuint* address);

        public static unsafe Vector128<nint> LoadAlignedVector128(nint* address);
        public static unsafe Vector128<nuint> LoadAlignedVector128(nuint* address);

        public static unsafe Vector128<nint> LoadScalarVector128(nint* address);
        public static unsafe Vector128<nuint> LoadScalarVector128(nuint* address);

        public static Vector128<nint> MoveScalar(Vector128<nint> value);
        public static Vector128<nuint> MoveScalar(Vector128<nuint> value);

        public static Vector128<nint> Or(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> Or(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector128<nint> ShiftLeftLogical(Vector128<nint> value, Vector128<nint> count);
        public static Vector128<nuint> ShiftLeftLogical(Vector128<nuint> value, Vector128<nuint> count);

        public static Vector128<nint> ShiftLeftLogical(Vector128<nint> value, byte count);
        public static Vector128<nuint> ShiftLeftLogical(Vector128<nuint> value, byte count);

        public static Vector128<nint> ShiftLeftLogical128BitLane(Vector128<nint> value, byte numBytes);
        public static Vector128<nuint> ShiftLeftLogical128BitLane(Vector128<nuint> value, byte numBytes);

        public static Vector128<nint> ShiftRightLogical(Vector128<nint> value, Vector128<nint> count);
        public static Vector128<nuint> ShiftRightLogical(Vector128<nuint> value, Vector128<nuint> count);

        public static Vector128<nint> ShiftRightLogical(Vector128<nint> value, byte count);
        public static Vector128<nuint> ShiftRightLogical(Vector128<nuint> value, byte count);

        public static Vector128<nint> ShiftRightLogical128BitLane(Vector128<nint> value, byte numBytes);
        public static Vector128<nuint> ShiftRightLogical128BitLane(Vector128<nuint> value, byte numBytes);

        public static unsafe void StoreScalar(nint* address, Vector128<nint> source);
        public static unsafe void StoreScalar(nuint* address, Vector128<nuint> source);

        public static unsafe void StoreAligned(nint* address, Vector128<nint> source);
        public static unsafe void StoreAligned(nuint* address, Vector128<nuint> source);

        public static unsafe void StoreAlignedNonTemporal(nint* address, Vector128<nint> source);
        public static unsafe void StoreAlignedNonTemporal(nuint* address, Vector128<nuint> source);

        public static unsafe void Store(nint* address, Vector128<nint> source);
        public static unsafe void Store(nuint* address, Vector128<nuint> source);

        public static Vector128<nint> Subtract(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> Subtract(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector128<nint> UnpackHigh(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> UnpackHigh(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector128<nint> UnpackLow(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> UnpackLow(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector128<nint> Xor(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> Xor(Vector128<nuint> left, Vector128<nuint> right);
    }

    public abstract partial class Sse3
    {
        public static unsafe Vector128<nint> LoadDquVector128(nint* address);
        public static unsafe Vector128<nuint> LoadDquVector128(nuint* address);
    }

    public abstract partial class Ssse3
    {
        public static Vector128<nint> AlignRight(Vector128<nint> left, Vector128<nint> right, byte mask);
        public static Vector128<nuint> AlignRight(Vector128<nuint> left, Vector128<nuint> right, byte mask);
    }

    public abstract partial class Sse41
    {
        public static nint Extract(Vector128<nint> value, byte index);
        public static nuint Extract(Vector128<nuint> value, byte index);

        public static Vector128<nint> Insert(Vector128<nint> value, nint data, byte index);
        public static Vector128<nuint> Insert(Vector128<nuint> value, nuint data, byte index);

        public static Vector128<nint> BlendVariable(Vector128<nint> left, Vector128<nint> right, Vector128<nint> mask);
        public static Vector128<nuint> BlendVariable(Vector128<nuint> left, Vector128<nuint> right, Vector128<nuint> mask);

        public static Vector128<nint> CompareEqual(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> CompareEqual(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector128<nint> ConvertToVector128NInt(Vector128<sbyte> value);
        public static Vector128<nint> ConvertToVector128NInt(Vector128<byte> value);
        public static Vector128<nint> ConvertToVector128NInt(Vector128<short> value);
        public static Vector128<nint> ConvertToVector128NInt(Vector128<ushort> value);
        public static Vector128<nint> ConvertToVector128NInt(Vector128<int> value);
        public static Vector128<nint> ConvertToVector128NInt(Vector128<uint> value);

        public static unsafe Vector128<nint> ConvertToVector128NInt(sbyte* address);
        public static unsafe Vector128<nint> ConvertToVector128NInt(byte* address);
        public static unsafe Vector128<nint> ConvertToVector128NInt(short* address);
        public static unsafe Vector128<nint> ConvertToVector128NInt(ushort* address);
        public static unsafe Vector128<nint> ConvertToVector128NInt(int* address);
        public static unsafe Vector128<nint> ConvertToVector128NInt(uint* address);

        public static Vector128<nint> Multiply(Vector128<int> left, Vector128<int> right);

        public static unsafe Vector128<nint> LoadAlignedVector128NonTemporal(nint* address);
        public static unsafe Vector128<nuint> LoadAlignedVector128NonTemporal(nuint* address);

        public static bool TestC(Vector128<nint> left, Vector128<nint> right);
        public static bool TestC(Vector128<nuint> left, Vector128<nuint> right);

        public static bool TestNotZAndNotC(Vector128<nint> left, Vector128<nint> right);
        public static bool TestNotZAndNotC(Vector128<nuint> left, Vector128<nuint> right);

        public static bool TestZ(Vector128<nint> left, Vector128<nint> right);
        public static bool TestZ(Vector128<nuint> left, Vector128<nuint> right);
    }

    public abstract partial class Sse42
    {
        public static Vector128<nint> CompareGreaterThan(Vector128<nint> left, Vector128<nint> right);

        public static nuint Crc32(nuint crc, nuint data);
    }

    public abstract partial class Avx
    {
        public static Vector128<nint> ExtractVector128(Vector256<nint> value, byte index);
        public static Vector128<nuint> ExtractVector128(Vector256<nuint> value, byte index);

        public static Vector256<nint> InsertVector128(Vector256<nint> value, Vector128<nint> data, byte index);
        public static Vector256<nuint> InsertVector128(Vector256<nuint> value, Vector128<nuint> data, byte index);

        public static unsafe Vector256<nint> LoadVector256(nint* address);
        public static unsafe Vector256<nuint> LoadVector256(nuint* address);

        public static unsafe Vector256<nint> LoadAlignedVector256(nint* address);
        public static unsafe Vector256<nuint> LoadAlignedVector256(nuint* address);

        public static unsafe Vector256<nint> LoadDquVector256(nint* address);
        public static unsafe Vector256<nuint> LoadDquVector256(nuint* address);

        public static Vector256<nint> Permute2x128(Vector256<nint> left, Vector256<nint> right, byte control);
        public static Vector256<nuint> Permute2x128(Vector256<nuint> left, Vector256<nuint> right, byte control);

        public static unsafe void StoreAligned(nint* address, Vector256<nint> source);
        public static unsafe void StoreAligned(nuint* address, Vector256<nuint> source);

        public static unsafe void StoreAlignedNonTemporal(nint* address, Vector256<nint> source);
        public static unsafe void StoreAlignedNonTemporal(nuint* address, Vector256<nuint> source);

        public static unsafe void Store(nint* address, Vector256<nint> source);
        public static unsafe void Store(nuint* address, Vector256<nuint> source);

        public static bool TestC(Vector256<nint> left, Vector256<nint> right);
        public static bool TestC(Vector256<nuint> left, Vector256<nuint> right);

        public static bool TestNotZAndNotC(Vector256<nint> left, Vector256<nint> right);
        public static bool TestNotZAndNotC(Vector256<nuint> left, Vector256<nuint> right);

        public static bool TestZ(Vector256<nint> left, Vector256<nint> right);
        public static bool TestZ(Vector256<nuint> left, Vector256<nuint> right);
    }

    public abstract partial class Avx2
    {
        public static Vector256<nint> Add(Vector256<nint> left, Vector256<nint> right);
        public static Vector256<nuint> Add(Vector256<nuint> left, Vector256<nuint> right);

        public static Vector256<nint> AlignRight(Vector256<nint> left, Vector256<nint> right, byte mask);
        public static Vector256<nuint> AlignRight(Vector256<nuint> left, Vector256<nuint> right, byte mask);

        public static Vector256<nint> And(Vector256<nint> left, Vector256<nint> right);
        public static Vector256<nuint> And(Vector256<nuint> left, Vector256<nuint> right);

        public static Vector256<nint> AndNot(Vector256<nint> left, Vector256<nint> right);
        public static Vector256<nuint> AndNot(Vector256<nuint> left, Vector256<nuint> right);

        public static Vector256<nint> BlendVariable(Vector256<nint> left, Vector256<nint> right, Vector256<nint> mask);
        public static Vector256<nuint> BlendVariable(Vector256<nuint> left, Vector256<nuint> right, Vector256<nuint> mask);

        public static Vector128<nint> BroadcastScalarToVector128(Vector128<nint> value);
        public static Vector128<nuint> BroadcastScalarToVector128(Vector128<nuint> value);

        public static unsafe Vector128<nint> BroadcastScalarToVector128(nint* source);
        public static unsafe Vector128<nuint> BroadcastScalarToVector128(nuint* source);

        public static Vector256<nint> BroadcastScalarToVector256(Vector128<nint> value);
        public static Vector256<nuint> BroadcastScalarToVector256(Vector128<nuint> value);

        public static unsafe Vector256<nint> BroadcastScalarToVector256(nint* source);
        public static unsafe Vector256<nuint> BroadcastScalarToVector256(nuint* source);

        public static unsafe Vector256<nint> BroadcastVector128ToVector256(nint* address);
        public static unsafe Vector256<nuint> BroadcastVector128ToVector256(nuint* address);

        public static Vector256<nint> CompareEqual(Vector256<nint> left, Vector256<nint> right);
        public static Vector256<nuint> CompareEqual(Vector256<nuint> left, Vector256<nuint> right);

        public static Vector256<nint> CompareGreaterThan(Vector256<nint> left, Vector256<nint> right);

        public static Vector256<nint> ConvertToVector256NInt(Vector128<sbyte> value);
        public static Vector256<nint> ConvertToVector256NInt(Vector128<byte> value);
        public static Vector256<nint> ConvertToVector256NInt(Vector128<short> value);
        public static Vector256<nint> ConvertToVector256NInt(Vector128<ushort> value);
        public static Vector256<nint> ConvertToVector256NInt(Vector128<int> value);
        public static Vector256<nint> ConvertToVector256NInt(Vector128<uint> value);

        public static unsafe Vector256<nint> ConvertToVector256NInt(sbyte* address);
        public static unsafe Vector256<nint> ConvertToVector256NInt(byte* address);
        public static unsafe Vector256<nint> ConvertToVector256NInt(short* address);
        public static unsafe Vector256<nint> ConvertToVector256NInt(ushort* address);
        public static unsafe Vector256<nint> ConvertToVector256NInt(int* address);
        public static unsafe Vector256<nint> ConvertToVector256NInt(uint* address);

        public static new Vector128<nint> ExtractVector128(Vector256<nint> value, byte index);
        public static new Vector128<nuint> ExtractVector128(Vector256<nuint> value, byte index);

        public static unsafe Vector128<nint> GatherVector128(nint* baseAddress, Vector128<int> index, byte scale);
        public static unsafe Vector128<nuint> GatherVector128(nuint* baseAddress, Vector128<int> index, byte scale);

        public static unsafe Vector128<int> GatherVector128(int* baseAddress, Vector128<nint> index, byte scale);
        public static unsafe Vector128<uint> GatherVector128(uint* baseAddress, Vector128<nint> index, byte scale);
        public static unsafe Vector128<nint> GatherVector128(long* baseAddress, Vector128<nint> index, byte scale);
        public static unsafe Vector128<nuint> GatherVector128(ulong* baseAddress, Vector128<nint> index, byte scale);
        public static unsafe Vector128<nint> GatherVector128(nint* baseAddress, Vector128<nint> index, byte scale);
        public static unsafe Vector128<nuint> GatherVector128(nuint* baseAddress, Vector128<nint> index, byte scale);
        public static unsafe Vector128<float> GatherVector128(float* baseAddress, Vector128<nint> index, byte scale);
        public static unsafe Vector128<double> GatherVector128(double* baseAddress, Vector128<nint> index, byte scale);

        public static unsafe Vector256<nint> GatherVector256(nint* baseAddress, Vector128<int> index, byte scale);
        public static unsafe Vector256<nuint> GatherVector256(nuint* baseAddress, Vector128<int> index, byte scale);

        public static unsafe Vector128<int> GatherVector128(int* baseAddress, Vector256<nint> index, byte scale);
        public static unsafe Vector128<uint> GatherVector128(uint* baseAddress, Vector256<nint> index, byte scale);
        public static unsafe Vector256<nint> GatherVector256(long* baseAddress, Vector256<nint> index, byte scale);
        public static unsafe Vector256<nuint> GatherVector256(ulong* baseAddress, Vector256<nint> index, byte scale);
        public static unsafe Vector256<nint> GatherVector256(nint* baseAddress, Vector256<nint> index, byte scale);
        public static unsafe Vector256<nuint> GatherVector256(nuint* baseAddress, Vector256<nint> index, byte scale);
        public static unsafe Vector128<float> GatherVector128(float* baseAddress, Vector256<nint> index, byte scale);
        public static unsafe Vector256<double> GatherVector256(double* baseAddress, Vector256<nint> index, byte scale);

        public static unsafe Vector128<nint> GatherMaskVector128(Vector128<nint> source, nint* baseAddress, Vector128<int> index, Vector128<nint> mask, byte scale);
        public static unsafe Vector128<nuint> GatherMaskVector128(Vector128<nuint> source, nuint* baseAddress, Vector128<int> index, Vector128<nuint> mask, byte scale);

        public static unsafe Vector128<int> GatherMaskVector128(Vector128<int> source, int* baseAddress, Vector128<nint> index, Vector128<int> mask, byte scale);
        public static unsafe Vector128<uint> GatherMaskVector128(Vector128<uint> source, uint* baseAddress, Vector128<nint> index, Vector128<uint> mask, byte scale);
        public static unsafe Vector128<long> GatherMaskVector128(Vector128<long> source, long* baseAddress, Vector128<nint> index, Vector128<long> mask, byte scale);
        public static unsafe Vector128<ulong> GatherMaskVector128(Vector128<ulong> source, ulong* baseAddress, Vector128<nint> index, Vector128<long> mask, byte scale);

        public static unsafe Vector128<nint> GatherMaskVector128(Vector128<nint> source, nint* baseAddress, Vector128<nint> index, Vector128<nint> mask, byte scale);
        public static unsafe Vector128<nuint> GatherMaskVector128(Vector128<nuint> source, nuint* baseAddress, Vector128<nint> index, Vector128<nuint> mask, byte scale);
        public static unsafe Vector128<float> GatherMaskVector128(Vector128<float> source, float* baseAddress, Vector128<nint> index, Vector128<float> mask, byte scale);
        public static unsafe Vector128<double> GatherMaskVector128(Vector128<double> source, double* baseAddress, Vector128<nint> index, Vector128<double> mask, byte scale);

        public static unsafe Vector256<nint> GatherMaskVector256(Vector256<nint> source, nint* baseAddress, Vector128<int> index, Vector256<nint> mask, byte scale);
        public static unsafe Vector256<nuint> GatherMaskVector256(Vector256<nuint> source, nuint* baseAddress, Vector128<int> index, Vector256<nuint> mask, byte scale);

        public static unsafe Vector128<int> GatherMaskVector128(Vector128<int> source, int* baseAddress, Vector256<nint> index, Vector128<int> mask, byte scale);
        public static unsafe Vector128<uint> GatherMaskVector128(Vector128<uint> source, uint* baseAddress, Vector256<nint> index, Vector128<uint> mask, byte scale);
        public static unsafe Vector256<long> GatherMaskVector256(Vector256<long> source, long* baseAddress, Vector256<nint> index, Vector256<long> mask, byte scale);
        public static unsafe Vector256<ulong> GatherMaskVector256(Vector256<ulong> source, ulong* baseAddress, Vector256<nint> index, Vector256<ulong> mask, byte scale);

        public static unsafe Vector256<nint> GatherMaskVector256(Vector256<nint> source, nint* baseAddress, Vector256<nint> index, Vector256<nint> mask, byte scale);
        public static unsafe Vector256<nuint> GatherMaskVector256(Vector256<nuint> source, nuint* baseAddress, Vector256<nint> index, Vector256<nuint> mask, byte scale);
        public static unsafe Vector128<float> GatherMaskVector128(Vector128<float> source, float* baseAddress, Vector256<nint> index, Vector128<float> mask, byte scale);
        public static unsafe Vector256<double> GatherMaskVector256(Vector256<double> source, double* baseAddress, Vector256<nint> index, Vector256<double> mask, byte scale);

        public static new Vector256<nint> InsertVector128(Vector256<nint> value, Vector128<nint> data, byte index);
        public static new Vector256<nuint> InsertVector128(Vector256<nuint> value, Vector128<nuint> data, byte index);

        public static unsafe Vector256<nint> LoadAlignedVector256NonTemporal(nint* address);
        public static unsafe Vector256<nuint> LoadAlignedVector256NonTemporal(nuint* address);

        public static unsafe Vector128<nint> MaskLoad(nint* address, Vector128<nint> mask);
        public static unsafe Vector128<nuint> MaskLoad(nuint* address, Vector128<nuint> mask);

        public static unsafe Vector256<nint> MaskLoad(nint* address, Vector256<nint> mask);
        public static unsafe Vector256<nuint> MaskLoad(nuint* address, Vector256<nuint> mask);

        public static unsafe void MaskStore(nint* address, Vector128<nint> mask, Vector128<nint> source);
        public static unsafe void MaskStore(nuint* address, Vector128<nuint> mask, Vector128<nuint> source);

        public static unsafe void MaskStore(nint* address, Vector256<nint> mask, Vector256<nint> source);
        public static unsafe void MaskStore(nuint* address, Vector256<nuint> mask, Vector256<nuint> source);

        public static Vector256<nint> Or(Vector256<nint> left, Vector256<nint> right);
        public static Vector256<nuint> Or(Vector256<nuint> left, Vector256<nuint> right);

        public static new Vector256<nint> Permute2x128(Vector256<nint> left, Vector256<nint> right, byte control);
        public static new Vector256<nuint> Permute2x128(Vector256<nuint> left, Vector256<nuint> right, byte control);

        public static Vector256<nint> Permute4x64(Vector256<nint> value, byte control);
        public static Vector256<nuint> Permute4x64(Vector256<nuint> value, byte control);

        public static Vector256<nint> ShiftLeftLogical(Vector256<nint> value, Vector128<nint> count);
        public static Vector256<nuint> ShiftLeftLogical(Vector256<nuint> value, Vector128<nuint> count);

        public static Vector256<nint> ShiftLeftLogical(Vector256<nint> value, byte count);
        public static Vector256<nuint> ShiftLeftLogical(Vector256<nuint> value, byte count);

        public static Vector256<nint> ShiftLeftLogical128BitLane(Vector256<nint> value, byte numBytes);
        public static Vector256<nuint> ShiftLeftLogical128BitLane(Vector256<nuint> value, byte numBytes);

        public static Vector256<nint> ShiftLeftLogicalVariable(Vector256<nint> value, Vector256<nuint> count);
        public static Vector256<nuint> ShiftLeftLogicalVariable(Vector256<nuint> value, Vector256<nuint> count);

        public static Vector128<nint> ShiftLeftLogicalVariable(Vector128<nint> value, Vector128<nuint> count);
        public static Vector128<nuint> ShiftLeftLogicalVariable(Vector128<nuint> value, Vector128<nuint> count);

        public static Vector256<nint> ShiftRightLogical(Vector256<nint> value, Vector128<nint> count);
        public static Vector256<nuint> ShiftRightLogical(Vector256<nuint> value, Vector128<nuint> count);

        public static Vector256<nint> ShiftRightLogical(Vector256<nint> value, byte count);
        public static Vector256<nuint> ShiftRightLogical(Vector256<nuint> value, byte count);

        public static Vector256<nint> ShiftRightLogical128BitLane(Vector256<nint> value, byte numBytes);
        public static Vector256<nuint> ShiftRightLogical128BitLane(Vector256<nuint> value, byte numBytes);

        public static Vector256<nint> ShiftRightLogicalVariable(Vector256<nint> value, Vector256<nuint> count);
        public static Vector256<nuint> ShiftRightLogicalVariable(Vector256<nuint> value, Vector256<nuint> count);

        public static Vector128<nint> ShiftRightLogicalVariable(Vector128<nint> value, Vector128<nuint> count);
        public static Vector128<nuint> ShiftRightLogicalVariable(Vector128<nuint> value, Vector128<nuint> count);

        public static Vector256<nint> Subtract(Vector256<nint> left, Vector256<nint> right);
        public static Vector256<nuint> Subtract(Vector256<nuint> left, Vector256<nuint> right);

        public static Vector256<nint> UnpackHigh(Vector256<nint> left, Vector256<nint> right);
        public static Vector256<nuint> UnpackHigh(Vector256<nuint> left, Vector256<nuint> right);

        public static Vector256<nint> UnpackLow(Vector256<nint> left, Vector256<nint> right);
        public static Vector256<nuint> UnpackLow(Vector256<nuint> left, Vector256<nuint> right);

        public static Vector256<nint> Xor(Vector256<nint> left, Vector256<nint> right);
        public static Vector256<nuint> Xor(Vector256<nuint> left, Vector256<nuint> right);
    }

    public abstract partial class Bmi1
    {
        public static nuint AndNot(nuint left, nuint right);

        public static nuint BitFieldExtract(nuint value, byte start, byte length);
        public static nuint BitFieldExtract(nuint value, ushort control);

        public static nuint ExtractLowestSetBit(nuint value);

        public static nuint GetMaskUpToLowestSetBit(nuint value);

        public static nuint ResetLowestSetBit(nuint value);

        public static nuint TrailingZeroCount(nuint value);
    }

    public abstract partial class Bmi2
    {
        public static nuint ZeroHighBits(nuint value, nuint index);

        public static nuint MultiplyNoFlags(nuint left, nuint right);
        public static unsafe nuint MultiplyNoFlags(nuint left, nuint right, nuint* low);

        public static nuint ParallelBitDeposit(nuint value, nuint mask);

        public static nuint ParallelBitExtract(nuint value, nuint mask);
    }

    public abstract partial class Lzcnt
    {
        public static nuint LeadingZeroCount(nuint value);
    }

    public abstract partial class Popcnt
    {
        public static nuint PopCount(nuint value);
    }
}
```
## Extend System.Runtime.Intrinsics.Arm to support nint and nuint

**Approved** | [#runtime/52027](https://github.com/dotnet/runtime/issues/52027#issuecomment-849844261) | [Video](https://www.youtube.com/watch?v=wY0t4bkXzS4&t=1h15m44s)

Looks good as proposed

```C#
namespace System.Runtime.Intrinsics.Arm
{
    public abstract partial class ArmBase
    {
        public abstract partial class Arm64
        {
            public static int LeadingSignCount(nint value);
        }

        public static int LeadingZeroCount(nint value);
        public static int LeadingZeroCount(nuint value);

        public static nint ReverseElementBits(nint value);
        public static nuint ReverseElementBits(nuint value)
    }

    public abstract partial class AdvSimd
    {
        public abstract partial class Arm64
        {
            public static Vector128<nint> AddPairwise(Vector128<nint> left, Vector128<nint> right);
            public static Vector128<nuint> AddPairwise(Vector128<nuint> left, Vector128<nuint> right);

            public static Vector64<nint> AddSaturate(Vector64<nint> left, Vector64<nuint> right);
            public static Vector64<nuint> AddSaturate(Vector64<nuint> left, Vector64<nint> right);
            public static Vector128<nint> AddSaturate(Vector128<nint> left, Vector128<nuint> right);
            public static Vector128<nuint> AddSaturate(Vector128<nuint> left, Vector128<nint> right);

            public static Vector64<nint> InsertSelectedScalar(Vector64<nint> result, byte resultIndex, Vector64<nint> value, byte valueIndex);
            public static Vector64<nuint> InsertSelectedScalar(Vector64<nuint> result, byte resultIndex, Vector64<nuint> value, byte valueIndex);
            public static Vector128<nint> InsertSelectedScalar(Vector128<nint> result, byte resultIndex, Vector128<nint> value, byte valueIndex);
            public static Vector128<nuint> InsertSelectedScalar(Vector128<nuint> result, byte resultIndex, Vector128<nuint> value, byte valueIndex);

            public static unsafe void StorePair(nint* address, Vector64<nint> value1, Vector64<nint> value2);
            public static unsafe void StorePair(nuint* address, Vector64<nuint> value1, Vector64<nuint> value2);
            public static unsafe void StorePair(nint* address, Vector128<nint> value1, Vector128<nint> value2);
            public static unsafe void StorePair(nuint* address, Vector128<nuint> value1, Vector128<nuint> value2);

            public static unsafe void StorePairNonTemporal(nint* address, Vector64<nint> value1, Vector64<nint> value2);
            public static unsafe void StorePairNonTemporal(nuint* address, Vector64<nuint> value1, Vector64<nuint> value2);
            public static unsafe void StorePairNonTemporal(nint* address, Vector128<nint> value1, Vector128<nint> value2);
            public static unsafe void StorePairNonTemporal(nuint* address, Vector128<nuint> value1, Vector128<nuint> value2);

            public static Vector64<nint> TransposeEven(Vector64<nint> left, Vector64<nint> right);
            public static Vector64<nuint> TransposeEven(Vector64<nuint> left, Vector64<nuint> right);
            public static Vector128<nint> TransposeEven(Vector128<nint> left, Vector128<nint> right);
            public static Vector128<nuint> TransposeEven(Vector128<nuint> left, Vector128<nuint> right);

            public static Vector64<nint> TransposeOdd(Vector64<nint> left, Vector64<nint> right);
            public static Vector64<nuint> TransposeOdd(Vector64<nuint> left, Vector64<nuint> right);
            public static Vector128<nint> TransposeOdd(Vector128<nint> left, Vector128<nint> right);
            public static Vector128<nuint> TransposeOdd(Vector128<nuint> left, Vector128<nuint> right);

            public static Vector64<nint> UnzipEven(Vector64<nint> left, Vector64<nint> right);
            public static Vector64<nuint> UnzipEven(Vector64<nuint> left, Vector64<nuint> right);
            public static Vector128<nint> UnzipEven(Vector128<nint> left, Vector128<nint> right);
            public static Vector128<nuint> UnzipEven(Vector128<nuint> left, Vector128<nuint> right);

            public static Vector64<nint> UnzipOdd(Vector64<nint> left, Vector64<nint> right);
            public static Vector64<nuint> UnzipOdd(Vector64<nuint> left, Vector64<nuint> right);
            public static Vector128<nint> UnzipOdd(Vector128<nint> left, Vector128<nint> right);
            public static Vector128<nuint> UnzipOdd(Vector128<nuint> left, Vector128<nuint> right);

            public static Vector64<nint> ZipHigh(Vector64<nint> left, Vector64<nint> right);
            public static Vector64<nuint> ZipHigh(Vector64<nuint> left, Vector64<nuint> right);
            public static Vector128<nint> ZipHigh(Vector128<nint> left, Vector128<nint> right);
            public static Vector128<nuint> ZipHigh(Vector128<nuint> left, Vector128<nuint> right);

            public static Vector64<nint> ZipLow(Vector64<nint> left, Vector64<nint> right);
            public static Vector64<nuint> ZipLow(Vector64<nuint> left, Vector64<nuint> right);
            public static Vector128<nint> ZipLow(Vector128<nint> left, Vector128<nint> right);
            public static Vector128<nuint> ZipLow(Vector128<nuint> left, Vector128<nuint> right);
        }

        public static Vector64<nuint> Abs(Vector64<nint> value);
        public static Vector128<nuint> Abs(Vector128<nint> value);

        public static Vector64<nint> AbsSaturate(Vector64<nint> value);
        public static Vector128<nint> AbsSaturate(Vector128<nint> value);

        public static Vector64<nint> Add(Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> Add(Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> Add(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> Add(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector64<nint> AddPairwiseScalar(Vector128<nint> value);
        public static Vector64<nuint> AddPairwiseScalar(Vector128<nuint> value);

        public static Vector64<nint> AddSaturate(Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> AddSaturate(Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> AddSaturate(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> AddSaturate(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector64<nint> And(Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> And(Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> And(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> And(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector64<nint> BitwiseClear(Vector64<nint> value, Vector64<nint> mask);
        public static Vector64<nuint> BitwiseClear(Vector64<nuint> value, Vector64<nuint> mask);
        public static Vector128<nint> BitwiseClear(Vector128<nint> value, Vector128<nint> mask);
        public static Vector128<nuint> BitwiseClear(Vector128<nuint> value, Vector128<nuint> mask);

        public static Vector64<nint> BitwiseSelect(Vector64<nint> select, Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> BitwiseSelect(Vector64<nuint> select, Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> BitwiseSelect(Vector128<nint> select, Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> BitwiseSelect(Vector128<nuint> select, Vector128<nuint> left, Vector128<nuint> right);

        public static Vector64<nint> CompareEqual(Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> CompareEqual(Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> CompareEqual(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> CompareEqual(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector64<nint> CompareGreaterThan(Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> CompareGreaterThan(Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> CompareGreaterThan(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> CompareGreaterThan(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector64<nint> CompareGreaterThanOrEqual(Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> CompareGreaterThanOrEqual(Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> CompareGreaterThanOrEqual(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> CompareGreaterThanOrEqual(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector64<nint> CompareLessThan(Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> CompareLessThan(Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> CompareLessThan(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> CompareLessThan(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector64<nint> CompareLessThanOrEqual(Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> CompareLessThanOrEqual(Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> CompareLessThanOrEqual(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> CompareLessThanOrEqual(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector64<nint> CompareTest(Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> CompareTest(Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> CompareTest(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> CompareTest(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector64<nint> DuplicateSelectedScalarToVector64(Vector128<nint> value, byte index);
        public static Vector64<nuint> DuplicateSelectedScalarToVector64(Vector128<nuint> value, byte index);
        public static Vector128<nint> DuplicateSelectedScalarToVector128(Vector128<nint> value, byte index);
        public static Vector128<nuint> DuplicateSelectedScalarToVector128(Vector128<nuint> value, byte index);

        public static Vector64<nint> DuplicateToVector64(nint value);
        public static Vector64<nuint> DuplicateToVector64(nuint value);
        public static Vector128<nint> DuplicateToVector128(nint value);
        public static Vector128<nuint> DuplicateToVector128(nuint value);

        public static nint Extract(Vector64<nint> vector, byte index);
        public static nuint Extract(Vector64<nuint> vector, byte index);
        public static nint Extract(Vector128<nint> vector, byte index);
        public static nuint Extract(Vector128<nuint> vector, byte index);

        public static Vector64<nint> ExtractVector64(Vector64<nint> upper, Vector64<nint> lower, byte index);
        public static Vector64<nuint> ExtractVector64(Vector64<nuint> upper, Vector64<nuint> lower, byte index);
        public static Vector128<nint> ExtractVector128(Vector128<nint> upper, Vector128<nint> lower, byte index);
        public static Vector128<nuint> ExtractVector128(Vector128<nuint> upper, Vector128<nuint> lower, byte index);

        public static Vector64<nint> Insert(Vector64<nint> vector, byte index, nint data);
        public static Vector64<nuint> Insert(Vector64<nuint> vector, byte index, nuint data);
        public static Vector128<nint> Insert(Vector128<nint> vector, byte index, nint data);
        public static Vector128<nuint> Insert(Vector128<nuint> vector, byte index, nuint data);

        public static unsafe Vector64<nint> LoadAndInsertScalar(Vector64<nint> value, byte index, nint* address);
        public static unsafe Vector64<nuint> LoadAndInsertScalar(Vector64<nuint> value, byte index, nuint* address);
        public static unsafe Vector128<nint> LoadAndInsertScalar(Vector128<nint> value, byte index, nint* address);
        public static unsafe Vector128<nuint> LoadAndInsertScalar(Vector128<nuint> value, byte index, nuint* address);

        public static unsafe Vector64<nint> LoadAndReplicateToVector64(nint* address);
        public static unsafe Vector64<nuint> LoadAndReplicateToVector64(nuint* address);
        public static unsafe Vector128<nint> LoadAndReplicateToVector128(nint* address);
        public static unsafe Vector128<nuint> LoadAndReplicateToVector128(nuint* address);

        public static unsafe Vector64<nint> LoadVector64(nint* address);
        public static unsafe Vector64<nuint> LoadVector64(nuint* address);
        public static unsafe Vector128<nint> LoadVector128(nint* address);
        public static unsafe Vector128<nuint> LoadVector128(nuint* address);

        public static Vector64<nint> Negate(Vector64<nint> value);
        public static Vector128<nint> Negate(Vector128<nint> value);

        public static Vector64<nint> NegateSaturate(Vector64<nint> value);
        public static Vector128<nint> NegateSaturate(Vector128<nint> value);

        public static Vector64<nint> Not(Vector64<nint> value);
        public static Vector64<nuint> Not(Vector64<nuint> value);
        public static Vector128<nint> Not(Vector128<nint> value);
        public static Vector128<nuint> Not(Vector128<nuint> value);

        public static Vector64<nint> Or(Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> Or(Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> Or(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> Or(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector64<nint> OrNot(Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> OrNot(Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> OrNot(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> OrNot(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector64<nint> ReverseElement8(Vector64<nint> value);
        public static Vector64<nuint> ReverseElement8(Vector64<nuint> value);
        public static Vector128<nint> ReverseElement8(Vector128<nint> value);
        public static Vector128<nuint> ReverseElement8(Vector128<nuint> value);

        public static Vector64<nint> ReverseElement16(Vector64<nint> value);
        public static Vector64<nuint> ReverseElement16(Vector64<nuint> value);
        public static Vector128<nint> ReverseElement16(Vector128<nint> value);
        public static Vector128<nuint> ReverseElement16(Vector128<nuint> value);

        public static Vector64<nint> ReverseElement32(Vector64<nint> value);
        public static Vector64<nuint> ReverseElement32(Vector64<nuint> value);
        public static Vector128<nint> ReverseElement32(Vector128<nint> value);
        public static Vector128<nuint> ReverseElement32(Vector128<nuint> value);

        public static Vector64<nint> ShiftArithmetic(Vector64<nint> value, Vector64<nint> count);
        public static Vector128<nint> ShiftArithmetic(Vector128<nint> value, Vector128<nint> count);

        public static Vector64<nint> ShiftArithmeticRounded(Vector64<nint> value, Vector64<nint> count);
        public static Vector128<nint> ShiftArithmeticRounded(Vector128<nint> value, Vector128<nint> count);

        public static Vector64<nint> ShiftArithmeticRoundedSaturate(Vector64<nint> value, Vector64<nint> count);
        public static Vector128<nint> ShiftArithmeticRoundedSaturate(Vector128<nint> value, Vector128<nint> count);

        public static Vector64<nint> ShiftArithmeticSaturate(Vector64<nint> value, Vector64<nint> count);
        public static Vector128<nint> ShiftArithmeticSaturate(Vector128<nint> value, Vector128<nint> count);

        public static Vector64<nint> ShiftLeftAndInsert(Vector64<nint> left, Vector64<nint> right, byte shift);
        public static Vector64<nuint> ShiftLeftAndInsert(Vector64<nuint> left, Vector64<nuint> right, byte shift);
        public static Vector128<nint> ShiftLeftAndInsert(Vector128<nint> left, Vector128<nint> right, byte shift);
        public static Vector128<nuint> ShiftLeftAndInsert(Vector128<nuint> left, Vector128<nuint> right, byte shift);

        public static Vector64<nint> ShiftLeftLogical(Vector64<nint> value, byte count);
        public static Vector64<nuint> ShiftLeftLogical(Vector64<nuint> value, byte count);
        public static Vector128<nint> ShiftLeftLogical(Vector128<nint> value, byte count);
        public static Vector128<nuint> ShiftLeftLogical(Vector128<nuint> value, byte count);

        public static Vector64<nint> ShiftLeftLogicalSaturate(Vector64<nint> value, byte count);
        public static Vector64<nuint> ShiftLeftLogicalSaturate(Vector64<nuint> value, byte count);
        public static Vector128<nint> ShiftLeftLogicalSaturate(Vector128<nint> value, byte count);
        public static Vector128<nuint> ShiftLeftLogicalSaturate(Vector128<nuint> value, byte count);

        public static Vector64<nuint> ShiftLeftLogicalSaturateUnsigned(Vector64<nint> value, byte count);
        public static Vector128<nuint> ShiftLeftLogicalSaturateUnsigned(Vector128<nint> value, byte count);

        public static Vector64<nint> ShiftLogical(Vector64<nint> value, Vector64<nint> count);
        public static Vector64<nuint> ShiftLogical(Vector64<nuint> value, Vector64<nint> count);
        public static Vector128<nint> ShiftLogical(Vector128<nint> value, Vector128<nint> count);
        public static Vector128<nuint> ShiftLogical(Vector128<nuint> value, Vector128<nint> count);

        public static Vector64<nint> ShiftLogicalRounded(Vector64<nint> value, Vector64<nint> count);
        public static Vector64<nuint> ShiftLogicalRounded(Vector64<nuint> value, Vector64<nint> count);
        public static Vector128<nint> ShiftLogicalRounded(Vector128<nint> value, Vector128<nint> count);
        public static Vector128<nuint> ShiftLogicalRounded(Vector128<nuint> value, Vector128<nint> count);

        public static Vector64<nint> ShiftLogicalRoundedSaturate(Vector64<nint> value, Vector64<nint> count);
        public static Vector64<nuint> ShiftLogicalRoundedSaturate(Vector64<nuint> value, Vector64<nint> count);
        public static Vector128<nint> ShiftLogicalRoundedSaturate(Vector128<nint> value, Vector128<nint> count);
        public static Vector128<nuint> ShiftLogicalRoundedSaturate(Vector128<nuint> value, Vector128<nint> count);

        public static Vector64<nint> ShiftLogicalSaturate(Vector64<nint> value, Vector64<nint> count);
        public static Vector64<nuint> ShiftLogicalSaturate(Vector64<nuint> value, Vector64<nint> count);
        public static Vector128<nint> ShiftLogicalSaturate(Vector128<nint> value, Vector128<nint> count);
        public static Vector128<nuint> ShiftLogicalSaturate(Vector128<nuint> value, Vector128<nint> count);

        public static Vector64<nint> ShiftRightAndInsert(Vector64<nint> left, Vector64<nint> right, byte shift);
        public static Vector64<nuint> ShiftRightAndInsert(Vector64<nuint> left, Vector64<nuint> right, byte shift);
        public static Vector128<nint> ShiftRightAndInsert(Vector128<nint> left, Vector128<nint> right, byte shift);
        public static Vector128<nuint> ShiftRightAndInsert(Vector128<nuint> left, Vector128<nuint> right, byte shift);

        public static Vector64<nint> ShiftRightArithmetic(Vector64<nint> value, byte count);
        public static Vector128<nint> ShiftRightArithmetic(Vector128<nint> value, byte count);

        public static Vector64<nint> ShiftRightArithmeticAdd(Vector64<nint> addend, Vector64<nint> value, byte count);
        public static Vector128<nint> ShiftRightArithmeticAdd(Vector128<nint> addend, Vector128<nint> value, byte count);

        public static Vector64<nint> ShiftRightArithmeticRounded(Vector64<nint> value, byte count);
        public static Vector128<nint> ShiftRightArithmeticRounded(Vector128<nint> value, byte count);

        public static Vector64<nint> ShiftRightArithmeticRoundedAdd(Vector64<nint> addend, Vector64<nint> value, byte count);
        public static Vector128<nint> ShiftRightArithmeticRoundedAdd(Vector128<nint> addend, Vector128<nint> value, byte count);

        public static Vector64<nint> ShiftRightLogical(Vector64<nint> value, byte count);
        public static Vector64<nuint> ShiftRightLogical(Vector64<nuint> value, byte count);
        public static Vector128<nint> ShiftRightLogical(Vector128<nint> value, byte count);
        public static Vector128<nuint> ShiftRightLogical(Vector128<nuint> value, byte count);

        public static Vector64<nint> ShiftRightLogicalAdd(Vector64<nint> addend, Vector64<nint> value, byte count);
        public static Vector64<nuint> ShiftRightLogicalAdd(Vector64<nuint> addend, Vector64<nuint> value, byte count);
        public static Vector128<nint> ShiftRightLogicalAdd(Vector128<nint> addend, Vector128<nint> value, byte count);
        public static Vector128<nuint> ShiftRightLogicalAdd(Vector128<nuint> addend, Vector128<nuint> value, byte count);

        public static Vector64<nint> ShiftRightLogicalRounded(Vector64<nint> value, byte count);
        public static Vector64<nuint> ShiftRightLogicalRounded(Vector64<nuint> value, byte count);
        public static Vector128<nint> ShiftRightLogicalRounded(Vector128<nint> value, byte count);
        public static Vector128<nuint> ShiftRightLogicalRounded(Vector128<nuint> value, byte count);

        public static Vector64<nint> ShiftRightLogicalRoundedAddScalar(Vector64<nint> addend, Vector64<nint> value, byte count);
        public static Vector64<nuint> ShiftRightLogicalRoundedAddScalar(Vector64<nuint> addend, Vector64<nuint> value, byte count);
        public static Vector128<nint> ShiftRightLogicalRoundedAdd(Vector128<nint> addend, Vector128<nint> value, byte count);
        public static Vector128<nuint> ShiftRightLogicalRoundedAdd(Vector128<nuint> addend, Vector128<nuint> value, byte count);

        public static unsafe void Store(nint* address, Vector64<nint> source);
        public static unsafe void Store(nuint* address, Vector64<nuint> source);
        public static unsafe void Store(nint* address, Vector128<nint> source);
        public static unsafe void Store(nuint* address, Vector128<nuint> source);

        public static unsafe void StoreSelectedScalar(nint* address, Vector128<nint> value, byte index);
        public static unsafe void StoreSelectedScalar(nuint* address, Vector128<nuint> value, byte index);
        public static unsafe void StoreSelectedScalar(nint* address, Vector128<nint> value, byte index);
        public static unsafe void StoreSelectedScalar(nuint* address, Vector128<nuint> value, byte index);

        public static Vector64<nint> Subtract(Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> Subtract(Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> Subtract(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> Subtract(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector64<nint> SubtractSaturate(Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> SubtractSaturate(Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> SubtractSaturate(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> SubtractSaturate(Vector128<nuint> left, Vector128<nuint> right);

        public static Vector64<nint> Xor(Vector64<nint> left, Vector64<nint> right);
        public static Vector64<nuint> Xor(Vector64<nuint> left, Vector64<nuint> right);
        public static Vector128<nint> Xor(Vector128<nint> left, Vector128<nint> right);
        public static Vector128<nuint> Xor(Vector128<nuint> left, Vector128<nuint> right);
    }

    public abstract partial class Crc32
    {
        public static uint ComputeCrc32(uint crc, nuint data);
        public static uint ComputeCrc32C(uint crc, nuint data);
    }
}
```
## UdpClient - add Span support

**Approved** | [#runtime/864](https://github.com/dotnet/runtime/issues/864#issuecomment-849850488) | [Video](https://www.youtube.com/watch?v=wY0t4bkXzS4&t=1h17m9s)

* The CancellationToken parameters should have defaults, particularly in the ones where `byte[]` becomes `ReadOnlyMemory<byte>`
* There was some debate as to the mismatch between current Task-returning members and new ValueTask-returning members, and "all the new ones here are ValueTask" was determined to be acceptable.


```C#
namespace System.Net.Sockets
{
    partial class UdpClient
    {
	// existing: public int Send(byte[] dgram, int bytes);
	public int Send(ReadOnlySpan<byte> datagram);
	
	// existing: public int Send(byte[] dgram, int bytes, IPEndPoint endPoint);
	public int Send(ReadOnlySpan<byte> datagram, IPEndPoint endPoint);
	
	// existing: public int Send(byte[] dgram, int bytes, string hostname, int port);
	public int Send(ReadOnlySpan<byte> datagram, string hostname, int port);
	
	// existing: public Task<int> SendAsync(byte[] datagram, int bytes);
	public ValueTask<int> SendAsync(ReadOnlyMemory<byte> datagram, CancellationToken cancellationToken = default);
	
	// existing: public Task<int> SendAsync(byte[] datagram, int bytes, IPEndPoint endPoint);
	public ValueTask<int> SendAsync(ReadOnlyMemory<byte> datagram, IPEndPoint endPoint, CancellationToken cancellationToken = default);

	// existing: public Task<UdpReceiveResult> ReceiveAsync();
	public ValueTask<UdpReceiveResult> ReceiveAsync(CancellationToken cancellationToken);
    }
}
```
## Add DivMod instrinct for intel x86/x64

**Approved** | [#runtime/27292](https://github.com/dotnet/runtime/issues/27292#issuecomment-849856474) | [Video](https://www.youtube.com/watch?v=wY0t4bkXzS4&t=1h27m9s)

* Since the original proposal, the return+out style for intrinsics has changed to named tuples `(Quotient, Remainder)` for these.
* "hi" should be "upper", "low" should be "lower"
* The canonical order in these APIs is (lower, upper), not (upper, lower).
* Added n(u)int as well
* The signedness of things is probably correct, but if not, it's an API review typo and should just be fixed as appropriate.

```C#
namespace System.Runtime.Intrinsics.X86
{
  partial class X86Base 
  { 
    public (uint Quotient, uint Remainder) DivRem(uint lower, uint upper, uint divisor);

    public (int Quotient, int Remainder) DivRem(uint lower, int upper, int divisor);

    public (nuint Quotient, nuint Remainder) DivRem(nuint lower, nuint upper, nuint divisor);

    public (nint Quotient, nint Remainder) DivRem(nuint lower, nint upper, nint divisor);
 
    [Intrinsic]
    public static class X64
    {
        public (ulong Quotient, ulong Remainder) DivRem(ulong lower, ulong upper, ulong divisor);

        public (long Quotient, long Remainder) DivRem(ulong lower, long upper, long divisor);
    }
}
```
## AssemblyMetadataAttribute should be applicable to everything

**Rejected** | [#runtime/29756](https://github.com/dotnet/runtime/issues/29756#issuecomment-849858838) | [Video](https://www.youtube.com/watch?v=wY0t4bkXzS4&t=1h37m40s)

The general feeling was that since the name says "Assembly" the target being Assembly-only is appropriate.  If we want a more general key-value pair bucket then we can make a more general name for a new thing.
## Proposal: MemoryMarshal.GetArrayDataReference<T>(T[,]) overload

**Approved** | [#runtime/35528](https://github.com/dotnet/runtime/issues/35528#issuecomment-849865575) | [Video](https://www.youtube.com/watch?v=wY0t4bkXzS4&t=1h41m32s)

Approved as a System.Array-based overload of GetArrayDataReference.

There's an open question of what happens if the array is an array of reference types: is byte the right answer for the return type?

```C#
namespace System.Runtime.InteropServices
{
    partial class MemoryMarshal
    {
        public static ref byte GetArrayDataReference(Array arr);
    }
}
```
