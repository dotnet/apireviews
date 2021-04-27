# Quick Reviews 04/27/2021

## AVX-VNNI intrinsics

**Approved** | [#runtime/43780](https://github.com/dotnet/runtime/issues/43780#issuecomment-827771780) | [Video](https://www.youtube.com/watch?v=r6OhQXOPmX4&t=0h0m0s)

Looks good as proposed.  One thing that was observed is one set of methods is short/short, but the other is byte/sbyte.  Double check the sign bits.

```cs
namespace System.Runtime.Intrinsics.X86
{
    public abstract class AvxVnni : Avx2
    {
        internal AvxVnni() { }

        public static new bool IsSupported { [Intrinsic] get { return false; } }

        public new abstract class X64 : Avx2.X64
        {
            internal X64() { }

            public static new bool IsSupported { [Intrinsic] get { return false; } }
        }

        /// <summary>
        /// __m128i _mm_dpbusd_epi32 (__m128i src, __m128i a, __m128i b)
        /// VPDPBUSD xmm, xmm, xmm
        /// </summary>
        public static Vector128<int> MultiplyWideningAndAdd(Vector128<int> addend, Vector128<byte> left, Vector128<sbyte> right) { throw new PlatformNotSupportedException(); }

        /// <summary>
        /// __m128i _mm_dpwssd_epi32 (__m128i src, __m128i a, __m128i b)
        /// VPDPWSSD xmm, xmm, xmm
        /// </summary>
        public static Vector128<int> MultiplyWideningAndAdd(Vector128<int> addend, Vector128<short> left, Vector128<short> right) { throw new PlatformNotSupportedException(); }

        /// <summary>
        /// __m256i _mm256_dpbusd_epi32 (__m256i src, __m256i a, __m256i b)
        /// VPDPBUSD ymm, ymm, ymm
        /// </summary>
        public static Vector256<int> MultiplyWideningAndAdd(Vector256<int> addend, Vector256<byte> left, Vector256<sbyte> right) { throw new PlatformNotSupportedException(); }

        /// <summary>
        /// __m256i _mm256_dpwssd_epi32 (__m256i src, __m256i a, __m256i b)
        /// VPDPWSSD ymm, ymm, ymm
        /// </summary>
        public static Vector256<int> MultiplyWideningAndAdd(Vector256<int> addend, Vector256<short> left, Vector256<short> right) { throw new PlatformNotSupportedException(); }

        /// <summary>
        /// __m128i _mm_dpbusds_epi32 (__m128i src, __m128i a, __m128i b)
        /// VPDPBUSDS xmm, xmm, xmm
        /// </summary>
        public static Vector128<int> MultiplyWideningAndAddSaturate(Vector128<int> addend, Vector128<byte> left, Vector128<sbyte> right) { throw new PlatformNotSupportedException(); }

        /// <summary>
        /// __m128i _mm_dpwssds_epi32 (__m128i src, __m128i a, __m128i b)
        /// VPDPWSSDS xmm, xmm, xmm
        /// </summary>
        public static Vector128<int> MultiplyWideningAndAddSaturate(Vector128<int> addend, Vector128<short> left, Vector128<short> right) { throw new PlatformNotSupportedException(); }

        /// <summary>
        /// __m256i _mm256_dpbusds_epi32 (__m256i src, __m256i a, __m256i b)
        /// VPDPBUSDS ymm, ymm, ymm
        /// </summary>
        public static Vector256<int> MultiplyWideningAndAddSaturate(Vector256<int> addend, Vector256<byte> left, Vector256<sbyte> right) { throw new PlatformNotSupportedException(); }

        /// <summary>
        /// __m256i _mm256_dpwssds_epi32 (__m256i src, __m256i a, __m256i b)
        /// VPDPWSSDS ymm, ymm, ymm
        /// </summary>
        public static Vector256<int> MultiplyWideningAndAddSaturate(Vector256<int> addend, Vector256<short> left, Vector256<short> right) { throw new PlatformNotSupportedException(); }
    }
}
```
## Add SupportedOSPlatformGuard and UnsupportedOSPlatformGuard Platform-Guard attributes

**Approved** | [#runtime/51541](https://github.com/dotnet/runtime/issues/51541#issuecomment-827825898) | [Video](https://www.youtube.com/watch?v=r6OhQXOPmX4&t=0h4m46s)

We did a long discussion as to whether we should accept a boolean in the manner of `[MaybeNullWhen]`, but felt like it probably doesn't come up much, and we could add it later.

We did remove AttributeTargets.Enum from the attribute decl, though.

```C#
namespace System.Runtime.Versioning
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Property | AttributeTargets.Field, AllowMultiple = true, Inherited = false)]
    public sealed class SupportedOSPlatformGuardAttribute : OSPlatformAttribute
    {
        public SupportedOSPlatformGuardAttribute(string platformName) ;
    }

    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Property | AttributeTargets.Field, AllowMultiple = true, Inherited = false)]
    public sealed class UnsupportedOSPlatformGuardAttribute : OSPlatformAttribute
    {
        public UnsupportedOSPlatformGuardAttribute(string platformName) ;
    }
}
```
## Add PipeReader.ReadAsync overloads to allow waiting for a specific amount of data

**Approved** | [#runtime/25063](https://github.com/dotnet/runtime/issues/25063#issuecomment-827837589) | [Video](https://www.youtube.com/watch?v=r6OhQXOPmX4&t=1h24m42s)

* Since there's a parameter that should be validated (if negative, throw), it should use the Template Method Pattern (ReadAsync => virtual ReadAsyncCore)
* To enhance discoverability we feel like not being an overload is better: ReadAsync => ReadAtLeastAsync

```C#
namespace System.IO.Pipelines
{
    partial class PipeReader
    {
        public ValueTask<ReadResult> ReadAtLeastAsync(int minimumBytes, CancellationToken cancellationToken = default);
        protected virtual ValueTask<ReadResult> ReadAtLeastAsyncCore(int minimumBytes, CancellationToken cancellationToken);
    }
}
```

@GrabYourPitchforks seems to have had some existential concerns with this.  The shape is approved as long as we don't outright axe the feature.
## Implement IEnumerable<> on various X509 collection types

**Approved** | [#runtime/49878](https://github.com/dotnet/runtime/issues/49878#issuecomment-827840659) | [Video](https://www.youtube.com/watch?v=r6OhQXOPmX4&t=1h42m59s)

Looks good as proposed.

```diff
namespace System.Security.Cryptography.X509Certificates
{
    public class X509Certificate2Collection : X509CertificateCollection,
+         IEnumerable<X509Certificate2>
    {
        ...
    }
    
    public sealed class X509Certificate2Enumerator : IEnumerator,
+         IEnumerator<X509Certificate2>
    {
        ...
    }
        
    
    public sealed class X509ChainElementCollection : ICollection,
+        IEnumerable<X509ChainElement>
    {
        ...
    }
    
    public sealed class X509ChainElementEnumerator : IEnumerator,
+        IEnumerator<X509ChainElement>
    {
        ...
    }
    
    public sealed class X509ExtensionCollection : ICollection,
+        IEnumerable<X509Extension>
    {
        ...
    }
    
    public sealed class X509ExtensionEnumerator : IEnumerator,
+        IEnumerator<X509Extension>
    {
        ...
    }
}
```
