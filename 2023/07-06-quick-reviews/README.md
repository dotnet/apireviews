# API Review 07/06/2023

## treat Memory/ReadOnlyMemory<T> as T[] in System.Text.Json

**Approved** | [#runtime/86442](https://github.com/dotnet/runtime/issues/86442#issuecomment-1624024501) | [Video](https://www.youtube.com/watch?v=anXotqKsCRI&t=0h0m0s)

Looks good as proposed

```C#
namespace System.Text.Json.Serialization.Metadata;

public partial class JsonMetadataServices
{
    // Similar to byte[] serializes as Base64 strings
    public static JsonConverter<Memory<byte>> MemoryByteConverter { get; }
    public static JsonConverter<ReadOnlyMemory<byte>> ReadOnlyMemoryByteConverter { get; }

    // Other element types serialize as JSON arrays
    public static JsonTypeInfo<Memory<TElement>> CreateMemoryInfo<TElement>(JsonSerializerOptions options, JsonCollectionInfoValues<Memory<TElement>> collectionInfo);
    public static JsonTypeInfo<ReadOnlyMemory<TElement>> CreateReadOnlyMemoryInfo<TElement>(JsonSerializerOptions options, JsonCollectionInfoValues<ReadOnlyMemory<TElement>> collectionInfo);
}
```
## Add System.Text.Json built-in support for more numeric types

**Approved** | [#runtime/87994](https://github.com/dotnet/runtime/issues/87994#issuecomment-1624037897) | [Video](https://www.youtube.com/watch?v=anXotqKsCRI&t=0h6m54s)

Looks good as proposed.  There was discussion to inspire a future change of accepting things based on the new numeric interfaces, but that's not for this issue.  There was also a question of whether nint/nuint should be included, and the answer was 'no'.

```C#
namespace System.Text.Json.Serialization.Metadata;

public partial class JsonMetadataServices
{
    public static JsonConverter<Int128> Int128Converter { get; }
    public static JsonConverter<UInt128> UInt128Converter { get; }
    public static JsonConverter<Half> HalfConverter { get; }
}
```
## Decouple ValidateOnStart from Hosting

**Approved** | [#runtime/84347](https://github.com/dotnet/runtime/issues/84347#issuecomment-1624080126) | [Video](https://www.youtube.com/watch?v=anXotqKsCRI&t=0h14m58s)

We changed the interface name from `IValidateOnStart` (describing when it's called) to `IStartupValidator` (describing what it is).

```C#
namespace Microsoft.Extensions.Options
{
    public interface IStartupValidator
   {
        void Validate();
   }
}
```

and we acknowledge the movement of OptionsBuilderExtensions

```diff
+[assembly: System.Runtime.CompilerServices.TypeForwardedTo(
+    typeof(Microsoft.Extensions.DependencyInjection.OptionsBuilderExtensions))]
```

## S.R.I.Wasm.PackedSimd API changes

**Approved** | [#runtime/88092](https://github.com/dotnet/runtime/issues/88092#issuecomment-1624091323) | [Video](https://www.youtube.com/watch?v=anXotqKsCRI&t=0h37m4s)

Looks good as proposed

```diff
# Missing "U" in the type name embedded in the method
-    public static Vector128<uint> ConvertToInt32Saturate(Vector128<float> value);
-    public static Vector128<uint> ConvertToInt32Saturate(Vector128<double> value);
+    public static Vector128<uint> ConvertToUInt32Saturate(Vector128<float> value);
+    public static Vector128<uint> ConvertToUInt32Saturate(Vector128<double> value);

# These operations are not present in the final spec
-    public static Vector128<int>    ConvertNarrowingSaturate(Vector128<long>   lower, Vector128<long>   upper);
-    public static Vector128<uint>   ConvertNarrowingSaturate(Vector128<ulong>  lower, Vector128<ulong>  upper);

# Adding Signed/Unsigned suffixes
-    public static Vector128<sbyte>  ConvertNarrowingSaturate(Vector128<short>  lower, Vector128<short>  upper);
-    public static Vector128<byte>   ConvertNarrowingSaturate(Vector128<ushort> lower, Vector128<ushort> upper);
-    public static Vector128<short>  ConvertNarrowingSaturate(Vector128<int>    lower, Vector128<int>    upper);
-    public static Vector128<ushort> ConvertNarrowingSaturate(Vector128<uint>   lower, Vector128<uint>   upper);
+    public static Vector128<sbyte>  ConvertNarrowingSaturateSigned(Vector128<short> lower, Vector128<short> upper);
+    public static Vector128<short>  ConvertNarrowingSaturateSigned(Vector128<int>   lower, Vector128<int>   upper);
+    public static Vector128<byte>   ConvertNarrowingSaturateUnsigned(Vector128<short> lower, Vector128<short> upper);
+    public static Vector128<ushort> ConvertNarrowingSaturateUnsigned(Vector128<int>   lower, Vector128<int>   upper);

# Renaming "Lane" to "Scalar" for consistency
- public static int    ExtractLane(Vector128<sbyte>  value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(15))] byte index);    // takes ImmLaneIdx16
- public static uint   ExtractLane(Vector128<byte>   value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(15))] byte index);    // takes ImmLaneIdx16
- public static int    ExtractLane(Vector128<short>  value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(7))] byte index);    // takes ImmLaneIdx8
- public static uint   ExtractLane(Vector128<ushort> value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(7))] byte index);    // takes ImmLaneIdx8
- public static int    ExtractLane(Vector128<int>    value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte index);    // takes ImmLaneIdx4
- public static uint   ExtractLane(Vector128<uint>   value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte index);    // takes ImmLaneIdx4
- public static long   ExtractLane(Vector128<long>   value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(1))] byte index);    // takes ImmLaneIdx2
- public static ulong  ExtractLane(Vector128<ulong>  value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(1))] byte index);    // takes ImmLaneIdx2
- public static float  ExtractLane(Vector128<float>  value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte index);    // takes ImmLaneIdx4
- public static double ExtractLane(Vector128<double> value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(1))] byte index);    // takes ImmLaneIdx2
- public static nint   ExtractLane(Vector128<nint>   value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte index);
- public static nuint  ExtractLane(Vector128<nuint>  value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte index);
- public static Vector128<sbyte>  ReplaceLane(Vector128<sbyte>  vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(15))] byte imm, int    value);   // takes ImmLaneIdx16
- public static Vector128<byte>   ReplaceLane(Vector128<byte>   vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(15))] byte imm, uint   value);   // takes ImmLaneIdx16
- public static Vector128<short>  ReplaceLane(Vector128<short>  vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(7))] byte imm, int    value);   // takes ImmLaneIdx8
- public static Vector128<ushort> ReplaceLane(Vector128<ushort> vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(7))] byte imm, uint   value);   // takes ImmLaneIdx8
- public static Vector128<int>    ReplaceLane(Vector128<int>    vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte imm, int    value);   // takes ImmLaneIdx4
- public static Vector128<int>    ReplaceLane(Vector128<uint>   vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte imm, uint   value);   // takes ImmLaneIdx4
- public static Vector128<long>   ReplaceLane(Vector128<long>   vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(1))] byte imm, long   value);   // takes ImmLaneIdx2
- public static Vector128<ulong>  ReplaceLane(Vector128<ulong>  vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(1))] byte imm, ulong  value);   // takes ImmLaneIdx2
- public static Vector128<float>  ReplaceLane(Vector128<float>  vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte imm, float  value);   // takes ImmLaneIdx4
- public static Vector128<double> ReplaceLane(Vector128<double> vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(1))] byte imm, double value);   // takes ImmLaneIdx2
- public static Vector128<nint>   ReplaceLane(Vector128<nint>   vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte imm, nint   value);
- public static Vector128<nuint>  ReplaceLane(Vector128<nuint>  vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte imm, nuint  value);
+ public static int    ExtractScalar(Vector128<sbyte>  value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(15))] byte index);    // takes ImmLaneIdx16
+ public static uint   ExtractScalar(Vector128<byte>   value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(15))] byte index);    // takes ImmLaneIdx16
+ public static int    ExtractScalar(Vector128<short>  value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(7))] byte index);    // takes ImmLaneIdx8
+ public static uint   ExtractScalar(Vector128<ushort> value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(7))] byte index);    // takes ImmLaneIdx8
+ public static int    ExtractScalar(Vector128<int>    value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte index);    // takes ImmLaneIdx4
+ public static uint   ExtractScalar(Vector128<uint>   value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte index);    // takes ImmLaneIdx4
+ public static long   ExtractScalar(Vector128<long>   value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(1))] byte index);    // takes ImmLaneIdx2
+ public static ulong  ExtractScalar(Vector128<ulong>  value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(1))] byte index);    // takes ImmLaneIdx2
+ public static float  ExtractScalar(Vector128<float>  value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte index);    // takes ImmLaneIdx4
+ public static double ExtractScalar(Vector128<double> value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(1))] byte index);    // takes ImmLaneIdx2
+ public static nint   ExtractScalar(Vector128<nint>   value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte index);
+ public static nuint  ExtractScalar(Vector128<nuint>  value, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte index);
+ public static Vector128<sbyte>  ReplaceScalar(Vector128<sbyte>  vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(15))] byte imm, int    value);   // takes ImmLaneIdx16
+ public static Vector128<byte>   ReplaceScalar(Vector128<byte>   vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(15))] byte imm, uint   value);   // takes ImmLaneIdx16
+ public static Vector128<short>  ReplaceScalar(Vector128<short>  vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(7))] byte imm, int    value);   // takes ImmLaneIdx8
+ public static Vector128<ushort> ReplaceScalar(Vector128<ushort> vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(7))] byte imm, uint   value);   // takes ImmLaneIdx8
+ public static Vector128<int>    ReplaceScalar(Vector128<int>    vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte imm, int    value);   // takes ImmLaneIdx4
+ public static Vector128<int>    ReplaceScalar(Vector128<uint>   vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte imm, uint   value);   // takes ImmLaneIdx4
+ public static Vector128<long>   ReplaceScalar(Vector128<long>   vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(1))] byte imm, long   value);   // takes ImmLaneIdx2
+ public static Vector128<ulong>  ReplaceScalar(Vector128<ulong>  vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(1))] byte imm, ulong  value);   // takes ImmLaneIdx2
+ public static Vector128<float>  ReplaceScalar(Vector128<float>  vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte imm, float  value);   // takes ImmLaneIdx4
+ public static Vector128<double> ReplaceScalar(Vector128<double> vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(1))] byte imm, double value);   // takes ImmLaneIdx2
+ public static Vector128<nint>   ReplaceScalar(Vector128<nint>   vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte imm, nint   value);
+ public static Vector128<nuint>  ReplaceScalar(Vector128<nuint>  vector, [System.Diagnostics.CodeAnalysis.ConstantExpectedAttribute(Max = (byte)(3))] byte imm, nuint  value);
```


## Support error codes for HttpRequestException

**Approved** | [#runtime/76644](https://github.com/dotnet/runtime/issues/76644#issuecomment-1624144878) | [Video](https://www.youtube.com/watch?v=anXotqKsCRI&t=0h47m13s)

* Changed the HttpRequestException constructor to move the HttpRequestError to the end, and default it.
* Changed the HttpRequestError property on HttpRequestException to be nullable, to distinguish "the provider didn't give one" from "the provider had an error it couldn't map".
* We decided it's correct that HttpIOException.HttpRequestError is non-nullable.
* `UnsupportedExtendedConnect` => `ExtendedConnectNotSupported`
* We removed `InvalidResponseHeader`, recommending it just be bucketed to `InvalidResponse` for long-term compatibility and provider variance reasons.
* `ContentBufferSizeExceeded` and `ResponseHeaderExceededLengthLimit` were merged into `ConfigurationLimitExceeded`
* We renamed `Undefined` to `Unknown`

```C#
public class HttpRequestException : Exception
{
    public HttpRequestError? HttpRequestError { get; }

    public HttpRequestException(string? message, Exception? inner = null, HttpStatusCode? statusCode = null, HttpRequestError? httpRequestError = null) {}
}

// IOException subtype to throw from response read streams
public class HttpIOException : IOException  // ALTERNATIVE name: HttpResponseReadException
{
    public HttpRequestError HttpRequestError { get; }
    public HttpIOException(HttpRequestError httpRequestError, string? message = null, Exception? innerException = null) {}
} 

// The EXISTING HttpProtocolException should be the subtype of this new exception type
public class HttpProtocolException : HttpIOException
{
}

// Defines disjoint error categories with high granularity.
public enum HttpRequestError
{
    Unknown = 0,                          // Uncategorized/generic error

    NameResolutionError,                    // DNS request failed
    ConnectionError,                        // Transport-level error during connection
    TransportError,                         // Transport-level error after connection
    SecureConnectionError,                  // SSL/TLS error
    HttpProtocolError,                      // HTTP 2.0/3.0 protocol error occurred
    ExtendedConnectNotSupported,             // Extended CONNECT for WebSockets over HTTP/2 is not supported.
                                            // (SETTINGS_ENABLE_CONNECT_PROTOCOL has not been sent).
    VersionNegotiationError,                // Cannot negotiate the HTTP Version requested
    UserAuthenticationError,                // Authentication failed with the provided credentials
    ProxyTunnelError,

    InvalidResponse,                        // General error in response/malformed response
    
    // OPTIONAL TO NOT INCLUDE: The one's below are specific cases of errors with the response.
    // We may consider merging them into "InvalidResponse" and "InvalidResponseHeader"
    ResponseEnded,                          // Received EOF
    // These depend on SocketsHttpHandler configuration
    ConfigurationLimitExceeded,             // Response Content size exceeded MaxResponseContentBufferSize or
                                            // Response Header length exceeded MaxResponseHeadersLength or
                                            // any future limits are exceeded
}
```


## File.AppendAllBytes

**Approved** | [#runtime/84532](https://github.com/dotnet/runtime/issues/84532#issuecomment-1624153125) | [Video](https://www.youtube.com/watch?v=anXotqKsCRI&t=1h31m33s)

Because nothing else in this type uses span/memory, we are only comfortable adding it at this time as `byte[]`.

We don't disagree with adding span/memory overloads to this type, but it should be done holistically and not only for append bytes.

We also renamed `data` to `bytes` for consistency with the naming in WriteAllBytes.

```C#
namespace System.IO;

public partial class File
{
    public static void AppendAllBytes(string path, byte[] bytes);
    public static Task AppendAllBytesAsync(string path, byte[] bytes, CancellationToken cancellationToken = default);
}
```
## update API signatures to leverage ref readonly parameters

**Approved** | [#runtime/85911](https://github.com/dotnet/runtime/issues/85911#issuecomment-1624189321) | [Video](https://www.youtube.com/watch?v=anXotqKsCRI&t=1h38m28s)

* All of the `ref` to `ref readonly` seem fine with no further thought required.  (Assuming readonly is appropriate to those members, which was not evaluated)
* Changing `ref` to `in` or `in` to `ref readonly` could introduce problems for callers on an older compiler version.  Since none of the API changes proposed are in NuGet packages, and we don't generally support old compilers against new SDK/Runtime versions, those concerns aren't manifest in these specific proposals.

```diff
namespace System
{
    public static class Nullable
    {
-       public static ref readonly T GetValueRefOrDefaultRef<T>(in T? nullable) where T : struct;
+       public static ref readonly T GetValueRefOrDefaultRef<T>(ref readonly T? nullable) where T : struct;
    }

    public readonly ref struct ReadOnlySpan<T>
    {
-       public ReadOnlySpan(in T reference);
+       public ReadOnlySpan(ref readonly T reference);
    }
}

namespace System.Runtime.CompilerServices
{
    public static class Unsafe
    {
-       public static bool AreSame<T>(ref T left, ref T right);
+       public static bool AreSame<T>(ref readonly T left, ref readonly T right);
-       public static ref T AsRef<T>(in T source);
+       public static ref T AsRef<T>(ref readonly T source);
-       public static nint ByteOffset<T>(ref T origin, ref T target);
+       public static nint ByteOffset<T>(ref readonly T origin, ref readonly T target);
-       public static void Copy<T>(void* destination, ref T source);
+       public static void Copy<T>(void* destination, ref readonly T source);
-       public static void CopyBlock(ref byte destination, ref byte source, uint byteCount);
+       public static void CopyBlock(ref byte destination, ref readonly byte source, uint byteCount);
-       public static void CopyBlockUnaligned(ref byte destination, ref byte source, uint byteCount);
+       public static void CopyBlockUnaligned(ref byte destination, ref readonly byte source, uint byteCount);
-       public static bool IsAddressGreaterThan<T>(ref T left, ref T right);
+       public static bool IsAddressGreaterThan<T>(ref readonly T left, ref readonly T right);
-       public static bool IsAddressLessThan<T>(ref T left, ref T right);
+       public static bool IsAddressLessThan<T>(ref readonly T left, ref readonly T right);
-       public static bool IsNullRef<T>(ref T source);
+       public static bool IsNullRef<T>(ref readonly T source);
-       public static T ReadUnaligned<T>(ref byte source);
+       public static T ReadUnaligned<T>(ref readonly byte source);
    }
}

namespace namespace System.Runtime.InteropServices
{
    public static class Marshal
    {
-       public static int QueryInterface(IntPtr pUnk, ref Guid iid, out IntPtr ppv);
+       public static int QueryInterface(IntPtr pUnk, in Guid iid, out IntPtr ppv);
    }

    public static class MemoryMarshal
    {
-       public static ReadOnlySpan<T> CreateReadOnlySpan<T>(scoped ref T reference, int length);
+       public static ReadOnlySpan<T> CreateReadOnlySpan<T>(scoped ref readonly T reference, int length);
-       public static unsafe void Write<T>(Span<byte> destination, ref T value) where T : struct;
+       public static unsafe void Write<T>(Span<byte> destination, in T value) where T : struct;
-       public static unsafe bool TryWrite<T>(Span<byte> destination, ref T value) where T : struct;
+       public static unsafe bool TryWrite<T>(Span<byte> destination, in T value) where T : struct;
    }
}

namespace System.Threading
{
    public static class Volatile
    {
-       public static bool Read(ref bool location);
+       public static bool Read(ref readonly bool location);
-       public static byte Read(ref byte location);
+       public static byte Read(ref readonly byte location);
-       public static double Read(ref double location);
+       public static double Read(ref readonly double location);
-       public static short Read(ref short location);
+       public static short Read(ref readonly short location);
-       public static int Read(ref int location);
+       public static int Read(ref readonly int location);
-       public static long Read(ref long location);
+       public static long Read(ref readonly long location);
-       public static IntPtr Read(ref IntPtr location);
+       public static IntPtr Read(ref readonly IntPtr location);
-       public static sbyte Read(ref sbyte location);
+       public static sbyte Read(ref readonly sbyte location);
-       public static float Read(ref float location);
+       public static float Read(ref readonly float location);
-       public static ushort Read(ref ushort location);
+       public static ushort Read(ref readonly ushort location);
-       public static uint Read(ref uint location);
+       public static uint Read(ref readonly uint location);
-       public static ulong Read(ref ulong location);
+       public static ulong Read(ref readonly ulong location);
-       public static UIntPtr Read(ref UIntPtr location);
+       public static UIntPtr Read(ref readonly UIntPtr location);
-       public static T Read<T>(ref T location) where T : class?;
+       public static T Read<T>(ref readonly T location) where T : class?;
    }

    public static class Interlocked
    {
-       public static ulong Read(ref ulong location);
+       public static ulong Read(ref readonly ulong location);
-       public static long Read(ref long location);
+       public static long Read(ref readonly long location);
    }
}

namespace System.Numerics
{
    public static class Vector
    {
-       public static Vector<T> LoadUnsafe<T>(ref T source) where T : struct;
+       public static Vector<T> LoadUnsafe<T>(ref readonly T source) where T : struct;
-       public static Vector<T> LoadUnsafe<T>(ref T source, nuint elementOffset) where T : struct;
+       public static Vector<T> LoadUnsafe<T>(ref readonly T source, nuint elementOffset) where T : struct;
    }
}

namespace System.Runtime.Intrinsics
{
    public static class Vector64
    {
-       public static Vector64<T> LoadUnsafe<T>(ref T source) where T : struct;
+       public static Vector64<T> LoadUnsafe<T>(ref readonly T source) where T : struct;
-       public static Vector64<T> LoadUnsafe<T>(ref T source, nuint elementOffset) where T : struct;
+       public static Vector64<T> LoadUnsafe<T>(ref readonly T source, nuint elementOffset) where T : struct;
    }

    public static class Vector128
    {
-       public static Vector128<T> LoadUnsafe<T>(ref T source) where T : struct;
+       public static Vector128<T> LoadUnsafe<T>(ref readonly T source) where T : struct;
-       public static Vector128<T> LoadUnsafe<T>(ref T source, nuint elementOffset) where T : struct;
+       public static Vector128<T> LoadUnsafe<T>(ref readonly T source, nuint elementOffset) where T : struct;
    }

    public static class Vector256
    {
-       public static Vector256<T> LoadUnsafe<T>(ref T source) where T : struct;
+       public static Vector256<T> LoadUnsafe<T>(ref readonly T source) where T : struct;
-       public static Vector256<T> LoadUnsafe<T>(ref T source, nuint elementOffset) where T : struct;
+       public static Vector256<T> LoadUnsafe<T>(ref readonly T source, nuint elementOffset) where T : struct;
    }

    public static class Vector512
    {
-       public static Vector512<T> LoadUnsafe<T>(ref T source) where T : struct;
+       public static Vector512<T> LoadUnsafe<T>(ref readonly T source) where T : struct;
-       public static Vector512<T> LoadUnsafe<T>(ref T source, nuint elementOffset) where T : struct;
+       public static Vector512<T> LoadUnsafe<T>(ref readonly T source, nuint elementOffset) where T : struct;
    }
}
```
