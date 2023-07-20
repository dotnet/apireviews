# API Review 07/20/2023

## HttpMethod.From(string)

**Approved** | [#runtime/89161](https://github.com/dotnet/runtime/issues/89161#issuecomment-1644312507) | [Video](https://www.youtube.com/watch?v=XKrNmVjiEpY&t=0h0m0s)

* We think that the method should be `Parse` instead of `From`.
* It's probably reasonable for the method to do case normalization and maybe trimming ("get " -> "GET")
* It's mildly unfortunate for the 0.01% case when it hits the unknown case that it could be string -> ROS-char -> new allocated string, but since it's expected to be 0.01%, it's fine.

```C#
namespace System.Net.Http;

public class HttpMethod
{
    public static HttpMethod Parse(ReadOnlySpan<char> method);
}
```

## Warning for ArgumentNullException.ThrowIfNull when passing a struct

**Approved** | [#runtime/85154](https://github.com/dotnet/runtime/issues/85154#issuecomment-1644329386) | [Video](https://www.youtube.com/watch?v=XKrNmVjiEpY&t=0h14m58s)

Looks good as proposed.

For non-nullable struct types it should be

Category: Usage
Level: Warning

For nullable struct types it should be a different diagnostic ID

Category: Performance
Level: Info
## Add support for AVX-512 VNNI hardware instructions

**Approved** | [#runtime/86849](https://github.com/dotnet/runtime/issues/86849#issuecomment-1644345688) | [Video](https://www.youtube.com/watch?v=XKrNmVjiEpY&t=0h27m20s)

Looks good as proposed

```C#
namespace System.Runtime.Intrinsics.X86
{
    [Intrinsic]
    public abstract class Avx512Vnni : Avx512F
    {
        public static new bool IsSupported { get => IsSupported; }

        [Intrinsic]
        public new abstract class X64 : Avx512F.X64
        {
            public static new bool IsSupported { get => IsSupported; }
        }

        [Intrinsic]
        public new abstract class VL : Avx512F.VL
        {
            public static new bool IsSupported { get => IsSupported; }

            /// <summary>
            /// __m128i _mm_dpbusd_epi32 (__m128i src, __m128i a, __m128i b)
            /// VPDPBUSD xmm, xmm, xmm/m128
            /// </summary>
            public static Vector128<int> MultiplyWideningAndAdd(Vector128<int> addend, Vector128<byte> left, Vector128<sbyte> right) => MultiplyWideningAndAdd(addend, left, right);

            /// <summary>
            /// __m128i _mm_dpwssd_epi32 (__m128i src, __m128i a, __m128i b)
            /// VPDPWSSD xmm, xmm, xmm/m128
            /// </summary>
            public static Vector128<int> MultiplyWideningAndAdd(Vector128<int> addend, Vector128<short> left, Vector128<short> right) => MultiplyWideningAndAdd(addend, left, right);

            /// <summary>
            /// __m256i _mm256_dpbusd_epi32 (__m256i src, __m256i a, __m256i b)
            /// VPDPBUSD ymm, ymm, ymm/m256
            /// </summary>
            public static Vector256<int> MultiplyWideningAndAdd(Vector256<int> addend, Vector256<byte> left, Vector256<sbyte> right) => MultiplyWideningAndAdd(addend, left, right);

            /// <summary>
            /// __m256i _mm256_dpwssd_epi32 (__m256i src, __m256i a, __m256i b)
            /// VPDPWSSD ymm, ymm, ymm/m256
            /// </summary>
            public static Vector256<int> MultiplyWideningAndAdd(Vector256<int> addend, Vector256<short> left, Vector256<short> right) => MultiplyWideningAndAdd(addend, left, right);

            /// <summary>
            /// __m128i _mm_dpbusds_epi32 (__m128i src, __m128i a, __m128i b)
            /// VPDPBUSDS xmm, xmm, xmm/m128
            /// </summary>
            public static Vector128<int> MultiplyWideningAndAddSaturate(Vector128<int> addend, Vector128<byte> left, Vector128<sbyte> right) => MultiplyWideningAndAddSaturate(addend, left, right);

            /// <summary>
            /// __m128i _mm_dpwssds_epi32 (__m128i src, __m128i a, __m128i b)
            /// VPDPWSSDS xmm, xmm, xmm/m128
            /// </summary>
            public static Vector128<int> MultiplyWideningAndAddSaturate(Vector128<int> addend, Vector128<short> left, Vector128<short> right) => MultiplyWideningAndAddSaturate(addend, left, right);

            /// <summary>
            /// __m256i _mm256_dpbusds_epi32 (__m256i src, __m256i a, __m256i b)
            /// VPDPBUSDS ymm, ymm, ymm/m256
            /// </summary>
            public static Vector256<int> MultiplyWideningAndAddSaturate(Vector256<int> addend, Vector256<byte> left, Vector256<sbyte> right) => MultiplyWideningAndAddSaturate(addend, left, right);

            /// <summary>
            /// __m256i _mm256_dpwssds_epi32 (__m256i src, __m256i a, __m256i b)
            /// VPDPWSSDS ymm, ymm, ymm/m256
            /// </summary>
            public static Vector256<int> MultiplyWideningAndAddSaturate(Vector256<int> addend, Vector256<short> left, Vector256<short> right) => MultiplyWideningAndAddSaturate(addend, left, right);

        }

        /// <summary>
        /// __m512i _mm512_dpbusd_epi32 (__m512i src, __m512i a, __m512i b)
        /// VPDPBUSD zmm, zmm, zmm/m512
        /// </summary>
        public static Vector512<int> MultiplyWideningAndAdd(Vector512<int> addend, Vector512<byte> left, Vector512<sbyte> right) => MultiplyWideningAndAdd(addend, left, right);

        /// <summary>
        /// __m512i _mm512_dpwssd_epi32 (__m512i src, __m512i a, __m512i b)
        /// VPDPWSSD zmm, zmm, zmm/m512
        /// </summary>
        public static Vector512<int> MultiplyWideningAndAdd(Vector512<int> addend, Vector512<short> left, Vector512<short> right) => MultiplyWideningAndAdd(addend, left, right);

        /// <summary>
        /// __m512i _mm512_dpbusds_epi32 (__m512i src, __m512i a, __m512i b)
        /// VPDPBUSDS zmm, zmm, zmm/m512
        /// </summary>
        public static Vector512<int> MultiplyWideningAndAddSaturate(Vector512<int> addend, Vector512<byte> left, Vector512<sbyte> right) => MultiplyWideningAndAddSaturate(addend, left, right);

        /// <summary>
        /// __m512i _mm512_dpwssds_epi32 (__m512i src, __m512i a, __m512i b)
        /// VPDPWSSDS zmm, zmm, zmm/m512
        /// </summary>
        public static Vector512<int> MultiplyWideningAndAddSaturate(Vector512<int> addend, Vector512<short> left, Vector512<short> right) => MultiplyWideningAndAddSaturate(addend, left, right);
    }
}
```
## DI : add support to eager load a singleton service

**Rejected** | [#runtime/43149](https://github.com/dotnet/runtime/issues/43149#issuecomment-1644351819) | [Video](https://www.youtube.com/watch?v=XKrNmVjiEpY&t=0h37m20s)

Closing this issue as a duplicate of #86075 
## Proposal: obsolete or hide TypeInfo/GetTypeInfo

**Approved** | [#runtime/53217](https://github.com/dotnet/runtime/issues/53217#issuecomment-1644362447) | [Video](https://www.youtube.com/watch?v=XKrNmVjiEpY&t=0h42m1s)

* Marking the type as [Obsolete] feels like overkill, since it will force anything already taking one to add a suppression.
* The things that produce a TypeInfo are the more likely candidate.
* But at this point we think we're probably fine with just hiding the methods, instead of Obsoleting them.
* And since it's the only method we see on IntrospectionExtensions, let's mark that type as EB-Never, too.
* Someone should update the docs for `TypeInfo` to call out it's now an obsolete/unnecessary indirection.

```diff
namespace System.Reflection
{
+   [EditorBrowsable(Never)]
    public static class IntrospectionExtensions
    {
+       [EditorBrowsable(Never)]
        public static TypeInfo GetTypeInfo(this Type type);
    }
}
```

## Add an analyzer warning of erroneous usage of MaxResponseHeadersLength

**Approved** | [#runtime/75137](https://github.com/dotnet/runtime/issues/75137#issuecomment-1644377058) | [Video](https://www.youtube.com/watch?v=XKrNmVjiEpY&t=0h49m44s)

Looks good as proposed

Category: Usage
Level: Info
## Add the support of DateOnly and TimeOnly types in TryParse method  Utf8Parser class

**Rejected** | [#runtime/53768](https://github.com/dotnet/runtime/issues/53768#issuecomment-1644394250) | [Video](https://www.youtube.com/watch?v=XKrNmVjiEpY&t=1h1m21s)

Rather than adding things on Utf8Parser, the plan forward is to add the UTF-8 parse methods to the relevant types themselves (generally along with implementing `IUtf8SpanParsable`.

For DateOnly and TimeOnly, these API additions are already covered under #81500.

It came up in discussion that DateOnly (and maybe TimeOnly) might want more options than the simple parse, like `DateTime.TryParseExact` has, but those would just be overloads to the TryParse methods on their types, and still wouldn't affect Utf8Parser.  As those also weren't part of this original proposal, they're left for someone else to do the research on and create a dedicated issue for.
## Is AuthenticationManager obsolete

**Approved** | [#runtime/77459](https://github.com/dotnet/runtime/issues/77459) | [Video](https://www.youtube.com/watch?v=XKrNmVjiEpY&t=1h14m10s)

## Provide IEnumerable<T> support for Memory<T>

**NeedsWork** | [#runtime/23950](https://github.com/dotnet/runtime/issues/23950#issuecomment-1644432244) | [Video](https://www.youtube.com/watch?v=XKrNmVjiEpY&t=1h20m52s)

This came up in API Review, and the main question that we had was "why is `MemoryMarshal.ToEnumerable(ReadOnlyMemory<T>)` not good enough?".  We didn't seem to see any discussion related to that existing member.
