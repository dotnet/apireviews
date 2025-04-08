# API Review 04/08/2025

## Raise the `AppDomain.UnhandledException` event for exceptions outside the runtime.

**Approved** | [#runtime/102730](https://github.com/dotnet/runtime/issues/102730#issuecomment-2787199046) | [Video](https://www.youtube.com/watch?v=mwzPjJocAXc&t=0h0m0s)

* Changed the argument type to `object` to match UnhandledExceptionEventArgs.ExceptionObject
  * Changing it to UnhandledExceptionEventArgs was discussed, but dismissed.
* We inserted "AppDomain", yielding `RaiseAppDomainUnhandledExceptionEvent`, to clearly distinguish between AppDomain.UnhandledException and ExceptionHandling's UnhandledExceptionHandler.

```C#
namespace System.Runtime.ExceptionServices;

public static partial class ExceptionHandling
{
    public static void RaiseAppDomainUnhandledExceptionEvent(object exception);
}
```
## HybridCache: span-based fetch

**Approved** | [#runtime/112866](https://github.com/dotnet/runtime/issues/112866#issuecomment-2787257651) | [Video](https://www.youtube.com/watch?v=mwzPjJocAXc&t=0h31m40s)


* We made the protected span overload public
  * And added the sibling method to match string.
* Added a note that the new APIs are .NET 10+, even though the class is .NET Standard 2.0

```C#
namespace Microsoft.Extensions.Caching.Hybrid;

public abstract partial class HybridCache
{
#if NET_10_OR_GREATER
    public ValueTask<T> GetOrCreateAsync<T>(
        ReadOnlySpan<char> key,
        Func<CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string!>? tags = null,
        CancellationToken cancellationToken = default);

    public virtual ValueTask<T> GetOrCreateAsync<TState, T>(
        ReadOnlySpan<char> key,
        TState state,
        Func<TState, CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default)
    {
        return GetOrCreateAsync(key.ToString(), state, factory, options, tags, cancellationToken);
    }

    public ValueTask<T> GetOrCreateAsync<T>(
        ref DefaultInterpolatedStringHandler key,
        Func<CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default);
  
    public ValueTask<T> GetOrCreateAsync<TState, T>(
        ref DefaultInterpolatedStringHandler key,
        TState state,
        Func<TState, CancellationToken, ValueTask<T>> factory,
        HybridCacheEntryOptions? options = null,
        IEnumerable<string>? tags = null,
        CancellationToken cancellationToken = default);
#endif
}
```
## Add AVX-VNNI-INT8 and AVX-VNNI-INT16 API

**Approved** | [#runtime/112586](https://github.com/dotnet/runtime/issues/112586#issuecomment-2787271818) | [Video](https://www.youtube.com/watch?v=mwzPjJocAXc&t=0h54m5s)

* Looks good as proposed

```C#
namespace System.Runtime.Intrinsics.X86
{
    [Intrinsic]
    [CLSCompliant(false)]
    public abstract class AvxVnniInt8 : Avx2
    {
        internal AvxVnniInt8() { }

        public static new bool IsSupported { get => IsSupported; }

        [Intrinsic]
        public new abstract class X64 : Avx2.X64
        {
            internal X64() { }

            public static new bool IsSupported { get => IsSupported; }
        }

        // VPDPBSSD xmm1, xmm2, xmm3/m128
        public static Vector128<int> MultiplyWideningAndAdd(Vector128<int> addend, Vector128<sbyte> left, Vector128<sbyte> right) => MultiplyWideningAndAdd(addend, left, right);

        // VPDPBSUD xmm1, xmm2, xmm3/m128
        public static Vector128<int> MultiplyWideningAndAdd(Vector128<int> addend, Vector128<sbyte> left, Vector128<byte> right) => MultiplyWideningAndAdd(addend, left, right);

        // VPDPBUUD xmm1, xmm2, xmm3/m128
        public static Vector128<uint> MultiplyWideningAndAdd(Vector128<uint> addend, Vector128<byte> left, Vector128<byte> right) => MultiplyWideningAndAdd(addend, left, right);

        // VPDPBSSD ymm1, ymm2, ymm3/m256
        public static Vector256<int> MultiplyWideningAndAdd(Vector256<int> addend, Vector256<sbyte> left, Vector256<sbyte> right) => MultiplyWideningAndAdd(addend, left, right);

        // VPDPBSUD ymm1, ymm2, ymm3/m256
        public static Vector256<int> MultiplyWideningAndAdd(Vector256<int> addend, Vector256<sbyte> left, Vector256<byte> right) => MultiplyWideningAndAdd(addend, left, right);

        // VPDPBUUD ymm1, ymm2, ymm3/m256
        public static Vector256<uint> MultiplyWideningAndAdd(Vector256<uint> addend, Vector256<byte> left, Vector256<byte> right) => MultiplyWideningAndAdd(addend, left, right);

        // VPDPBSSDS xmm1, xmm2, xmm3/m128
        public static Vector128<int> MultiplyWideningAndAddSaturate(Vector128<int> addend, Vector128<sbyte> left, Vector128<sbyte> right) => MultiplyWideningAndAddSaturate(addend, left, right);

        // VPDPBSUDS xmm1, xmm2, xmm3/m128
        public static Vector128<int> MultiplyWideningAndAddSaturate(Vector128<int> addend, Vector128<sbyte> left, Vector128<byte> right) => MultiplyWideningAndAddSaturate(addend, left, right);

        // VPDPBUUDS xmm1, xmm2, xmm3/m128
        public static Vector128<uint> MultiplyWideningAndAddSaturate(Vector128<uint> addend, Vector128<byte> left, Vector128<byte> right) => MultiplyWideningAndAddSaturate(addend, left, right);

        // VPDPBSSDS ymm1, ymm2, ymm3/m256
        public static Vector256<int> MultiplyWideningAndAddSaturate(Vector256<int> addend, Vector256<sbyte> left, Vector256<sbyte> right) => MultiplyWideningAndAddSaturate(addend, left, right);

        // VPDPBSUDS ymm1, ymm2, ymm3/m256
        public static Vector256<int> MultiplyWideningAndAddSaturate(Vector256<int> addend, Vector256<sbyte> left, Vector256<byte> right) => MultiplyWideningAndAddSaturate(addend, left, right);

        // VPDPBUUDS ymm1, ymm2, ymm3/m256
        public static Vector256<uint> MultiplyWideningAndAddSaturate(Vector256<uint> addend, Vector256<byte> left, Vector256<byte> right) => MultiplyWideningAndAddSaturate(addend, left, right);

        [Intrinsic]
        public abstract class V512
        {
            internal V512() { }

            public static bool IsSupported { get => IsSupported; }

            // VPDPBSSD zmm1{k1}{z}, zmm2, zmm3/m512/m32bcst
            public static Vector512<int> MultiplyWideningAndAdd(Vector512<int> addend, Vector512<sbyte> left, Vector512<sbyte> right) => MultiplyWideningAndAdd(addend, left, right);

            // VPDPBSUD zmm1{k1}{z}, zmm2, zmm3/m512/m32bcst
            public static Vector512<int> MultiplyWideningAndAdd(Vector512<int> addend, Vector512<sbyte> left, Vector512<byte> right) => MultiplyWideningAndAdd(addend, left, right);

            // VPDPBUUD zmm1{k1}{z}, zmm2, zmm3/m512/m32bcst
            public static Vector512<uint> MultiplyWideningAndAdd(Vector512<uint> addend, Vector512<byte> left, Vector512<byte> right) => MultiplyWideningAndAdd(addend, left, right);

            // VPDPBSSDS zmm1{k1}{z}, zmm2, zmm3/m512/m32bcst
            public static Vector512<int> MultiplyWideningAndAddSaturate(Vector512<int> addend, Vector512<sbyte> left, Vector512<sbyte> right) => MultiplyWideningAndAddSaturate(addend, left, right);

            // VPDPBSUDS zmm1{k1}{z}, zmm2, zmm3/m512/m32bcst
            public static Vector512<int> MultiplyWideningAndAddSaturate(Vector512<int> addend, Vector512<sbyte> left, Vector512<byte> right) => MultiplyWideningAndAddSaturate(addend, left, right);

            // VPDPBUUDS zmm1{k1}{z}, zmm2, zmm3/m512/m32bcst
            public static Vector512<uint> MultiplyWideningAndAddSaturate(Vector512<uint> addend, Vector512<byte> left, Vector512<byte> right) => MultiplyWideningAndAddSaturate(addend, left, right);
        }
    }

    [Intrinsic]
    [CLSCompliant(false)]
    public abstract class AvxVnniInt16 : Avx2
    {
        internal AvxVnniInt16() { }

        public static new bool IsSupported { get => IsSupported; }

        /// <summary>Provides access to the x86 AVX-VNNI-INT8 hardware instructions, that are only available to 64-bit processes, via intrinsics.</summary>
        [Intrinsic]
        public new abstract class X64 : Avx2.X64
        {
            internal X64() { }

            public static new bool IsSupported { get => IsSupported; }
        }

        // VPDPWSUD xmm1, xmm2, xmm3/m128
        public static Vector128<int> MultiplyWideningAndAdd(Vector128<int> addend, Vector128<short> left, Vector128<ushort> right) => MultiplyWideningAndAdd(addend, left, right);

        // VPDPWUSD xmm1, xmm2, xmm3/m128
        public static Vector128<int> MultiplyWideningAndAdd(Vector128<int> addend, Vector128<ushort> left, Vector128<short> right) => MultiplyWideningAndAdd(addend, left, right);

        // VPDPWUUD xmm1, xmm2, xmm3/m128
        public static Vector128<uint> MultiplyWideningAndAdd(Vector128<uint> addend, Vector128<ushort> left, Vector128<ushort> right) => MultiplyWideningAndAdd(addend, left, right);

        // VPDPWSUD ymm1, ymm2, ymm3/m256
        public static Vector256<int> MultiplyWideningAndAdd(Vector256<int> addend, Vector256<short> left, Vector256<ushort> right) => MultiplyWideningAndAdd(addend, left, right);

        // VPDPWUSD ymm1, ymm2, ymm3/m256
        public static Vector256<int> MultiplyWideningAndAdd(Vector256<int> addend, Vector256<ushort> left, Vector256<short> right) => MultiplyWideningAndAdd(addend, left, right);

        // VPDPWUUD ymm1, ymm2, ymm3/m256
        public static Vector256<uint> MultiplyWideningAndAdd(Vector256<uint> addend, Vector256<ushort> left, Vector256<ushort> right) => MultiplyWideningAndAdd(addend, left, right);

        // VPDPWSUDS xmm1, xmm2, xmm3/m128
        public static Vector128<int> MultiplyWideningAndAddSaturate(Vector128<int> addend, Vector128<short> left, Vector128<ushort> right) => MultiplyWideningAndAddSaturate(addend, left, right);

        // VPDPWUSDS xmm1, xmm2, xmm3/m128
        public static Vector128<int> MultiplyWideningAndAddSaturate(Vector128<int> addend, Vector128<ushort> left, Vector128<short> right) => MultiplyWideningAndAddSaturate(addend, left, right);

        // VPDPWUUDS xmm1, xmm2, xmm3/m128
        public static Vector128<uint> MultiplyWideningAndAddSaturate(Vector128<uint> addend, Vector128<ushort> left, Vector128<ushort> right) => MultiplyWideningAndAddSaturate(addend, left, right);

        // VPDPWSUDS ymm1, ymm2, ymm3/m256
        public static Vector256<int> MultiplyWideningAndAddSaturate(Vector256<int> addend, Vector256<short> left, Vector256<ushort> right) => MultiplyWideningAndAddSaturate(addend, left, right);

        // VPDPWUSDS ymm1, ymm2, ymm3/m256
        public static Vector256<int> MultiplyWideningAndAddSaturate(Vector256<int> addend, Vector256<ushort> left, Vector256<short> right) => MultiplyWideningAndAddSaturate(addend, left, right);

        // VPDPWUUDS ymm1, ymm2, ymm3/m256
        public static Vector256<uint> MultiplyWideningAndAddSaturate(Vector256<uint> addend, Vector256<ushort> left, Vector256<ushort> right) => MultiplyWideningAndAddSaturate(addend, left, right);

        /// <summary>Provides access to the x86 AVX10.2/512 hardware instructions for AVX-VNNI-INT16 via intrinsics.</summary>
        [Intrinsic]
        public abstract class V512
        {
            internal V512() { }

            public static bool IsSupported { get => IsSupported; }

            // VPDPWSUD zmm1{k1}{z}, zmm2, zmm3/m512/m32bcst
            public static Vector512<int> MultiplyWideningAndAdd(Vector512<int> addend, Vector512<short> left, Vector512<ushort> right) => MultiplyWideningAndAdd(addend, left, right);

            // VPDPWUSD zmm1{k1}{z}, zmm2, zmm3/m512/m32bcst
            public static Vector512<int> MultiplyWideningAndAdd(Vector512<int> addend, Vector512<ushort> left, Vector512<short> right) => MultiplyWideningAndAdd(addend, left, right);

            // VPDPWUUD zmm1{k1}{z}, zmm2, zmm3/m512/m32bcst
            public static Vector512<uint> MultiplyWideningAndAdd(Vector512<uint> addend, Vector512<ushort> left, Vector512<ushort> right) => MultiplyWideningAndAdd(addend, left, right);

            // VPDPWSUDS zmm1{k1}{z}, zmm2, zmm3/m512/m32bcst
            public static Vector512<int> MultiplyWideningAndAddSaturate(Vector512<int> addend, Vector512<short> left, Vector512<ushort> right) => MultiplyWideningAndAddSaturate(addend, left, right);

            // VPDPWUSDS zmm1{k1}{z}, zmm2, zmm3/m512/m32bcst
            public static Vector512<int> MultiplyWideningAndAddSaturate(Vector512<int> addend, Vector512<ushort> left, Vector512<short> right) => MultiplyWideningAndAddSaturate(addend, left, right);

            // VPDPWUUDS zmm1{k1}{z}, zmm2, zmm3/m512/m32bcst
            public static Vector512<uint> MultiplyWideningAndAddSaturate(Vector512<uint> addend, Vector512<ushort> left, Vector512<ushort> right) => MultiplyWideningAndAddSaturate(addend, left, right);
        }
    }
}
```
## ClaimsIdentity to perform case-sensitive comparison

**Approved** | [#runtime/113562](https://github.com/dotnet/runtime/issues/113562#issuecomment-2787339679) | [Video](https://www.youtube.com/watch?v=mwzPjJocAXc&t=1h1m39s)

* Instead of constructor overload explosions, we added a default-argument constructor to enable the new parameter.
* The new ctor parameter should not be exposed as a property (until there's a compelling reason) to avoid confusion where a derived type exists solely to add ordinal semantics but the property defaults to OrdinalIgnoreCase.
* The ctors should throw for the culture-sensitive options, at least until they are well understood in context.

```C#
namespace System.Security.Claims
{
    public partial class ClaimsIdentity
    {
        public ClaimsIdentity(
            IIdentity? identity = null,
            IEnumerable<Claim>? claims = null,
            string? authenticationType = null,
            string? nameType = null,
            string? roleType = null,
            StringComparison stringComparison = StringComparison.OrdinalIgnoreCase);
    }
}
```
## change 'Unsafe.AsPointer' parameter to be 'ref readonly'

**Approved** | [#runtime/114189](https://github.com/dotnet/runtime/issues/114189#issuecomment-2787351311) | [Video](https://www.youtube.com/watch?v=mwzPjJocAXc&t=1h25m2s)

* Looks good as proposed

```diff
namespace System.Runtime.CompilerServices
{
    public static class Unsafe
    {
-       public static void* AsPointer<T>(ref T value);
+       public static void* AsPointer<T>(ref readonly T value);
    }
}
```
## Add APIs to WebSocket which allow it to be read as a Stream

**Approved** | [#runtime/111217](https://github.com/dotnet/runtime/issues/111217#issuecomment-2787433647) | [Video](https://www.youtube.com/watch?v=mwzPjJocAXc&t=1h30m31s)

* Changed the constructor to a triad of factory methods.
  * That both enables the multiple-frame-single-message scenario and should give the caller pause as to which mode they want.

```C#
namespace System.Net.WebSockets;

public class WebSocketStream : Stream
{
    internal WebSocketStream();

    public WebSocket WebSocket { get; }

    public static WebSocketStream Create(WebSocket webSocket, bool ownsWebSocket = false);
    public static WebSocketStream CreateWritableMessageStream(WebSocket webSocket);
    public static WebSocketStream CreateReadableMessageStream(WebSocket webSocket);

    ... // relevant Stream overrides
}
```
