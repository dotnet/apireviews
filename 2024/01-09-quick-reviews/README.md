# API Review 01/09/2024

## Introduce `CallConvSwift` to represent the Swift ABI calling convention

**Approved** | [#runtime/64215](https://github.com/dotnet/runtime/issues/64215#issuecomment-1883616516) | [Video](https://www.youtube.com/watch?v=O66ZW4m6pOs&t=0h0m0s)


* SwiftSelf and SwiftError were proposed as `IntPtr Value`, but we accepted changing them to `void* Value`.

```C#
namespace System.Runtime.CompilerServices
{
    public class CallConvSwift
    {
        public CallConvSwift() { }
    }
}

namespace System.Runtime.InteropServices.Swift
{
    public readonly unsafe struct SwiftSelf
    {
        public SwiftSelf(void* value) {
            Value = value;
        }

        public void* Value { get; }
    }

    public readonly unsafe struct SwiftError
    {
        public SwiftError(void* value) {
            Value = value;
        }

        public void* Value { get; }
    }
}
```
## Will there be GetEnumerator() for MemoryCache ?

**Approved** | [#runtime/36554](https://github.com/dotnet/runtime/issues/36554#issuecomment-1883634438) | [Video](https://www.youtube.com/watch?v=O66ZW4m6pOs&t=0h52m43s)

* During review there was a question of whether it should be `IEnumerable<object>` or should use a custom struct-based enumerator-pattern type; but the answer to that depends on usage data we don't have, so it's a question for the area owners.

```C#
namespace Microsoft.Extensions.Caching.Memory
{
    public partial class MemoryCache
    {
       public IEnumerable<object> Keys { get; }
    }
}
```
## Obsolete invalid overloads of `AdvSimd.ShiftRightLogicalRoundedNarrowingSaturate` family

**Approved** | [#runtime/95525](https://github.com/dotnet/runtime/issues/95525#issuecomment-1883648184) | [Video](https://www.youtube.com/watch?v=O66ZW4m6pOs&t=1h6m2s)

* The error message should be improved, but the obsoletions themselves are approved.
* All of them should use the same new diagnostic code.

```diff
 namespace System.Runtime.Intrinsics.Arm;

 public abstract class AdvSimd : ArmBase
 {
     public new abstract class Arm64 : ArmBase.Arm64
     {
         /// <summary>
         /// uint16_t vqrshrns_n_u32 (uint32_t a, const int n)
         ///   A64: UQRSHRN Hd, Sn, #n
         /// </summary>
+        [Obsolete("Use unsigned overload or signed instruction.")]
         public static Vector64<short> ShiftRightLogicalRoundedNarrowingSaturateScalar(Vector64<int> value, [ConstantExpected(Min = 1, Max = (byte)(16))] byte count) { throw new PlatformNotSupportedException(); }
  
         /// <summary>
         /// uint32_t vqrshrnd_n_u64 (uint64_t a, const int n)
         ///   A64: UQRSHRN Sd, Dn, #n
         /// </summary>
+        [Obsolete("Use unsigned overload or signed instruction.")]
         public static Vector64<int> ShiftRightLogicalRoundedNarrowingSaturateScalar(Vector64<long> value, [ConstantExpected(Min = 1, Max = (byte)(8))] byte count) { throw new PlatformNotSupportedException(); }
  
         /// <summary>
         /// uint8_t vqrshrnh_n_u16 (uint16_t a, const int n)
         ///   A64: UQRSHRN Bd, Hn, #n
         /// </summary>
+        [Obsolete("Use unsigned overload or signed instruction.")]
         public static Vector64<sbyte> ShiftRightLogicalRoundedNarrowingSaturateScalar(Vector64<short> value, [ConstantExpected(Min = 1, Max = (byte)(32))] byte count) { throw new PlatformNotSupportedException(); }
     }
 
     /// <summary>
     /// uint16x4_t vqrshrn_n_u32 (uint32x4_t a, const int n)
     ///   A32: VQRSHRN.U32 Dd, Qm, #n
     ///   A64: UQRSHRN Vd.4H, Vn.4S, #n
     /// </summary>
+    [Obsolete("Use unsigned overload or signed instruction.")]
     public static Vector64<short> ShiftRightLogicalRoundedNarrowingSaturateLower(Vector128<int> value, [ConstantExpected(Min = 1, Max = (byte)(32))] byte count) { throw new PlatformNotSupportedException(); }
  
     /// <summary>
     /// uint32x2_t vqrshrn_n_u64 (uint64x2_t a, const int n)
     ///   A32: VQRSHRN.U64 Dd, Qm, #n
     ///   A64: UQRSHRN Vd.2S, Vn.2D, #n
     /// </summary>
+    [Obsolete("Use unsigned overload or signed instruction.")]
     public static Vector64<int> ShiftRightLogicalRoundedNarrowingSaturateLower(Vector128<long> value, [ConstantExpected(Min = 1, Max = (byte)(16))] byte count) { throw new PlatformNotSupportedException(); }
  
     /// <summary>
     /// uint8x8_t vqrshrn_n_u16 (uint16x8_t a, const int n)
     ///   A32: VQRSHRN.U16 Dd, Qm, #n
     ///   A64: UQRSHRN Vd.8B, Vn.8H, #n
     /// </summary>
+    [Obsolete("Use unsigned overload or signed instruction.")]
     public static Vector64<sbyte> ShiftRightLogicalRoundedNarrowingSaturateLower(Vector128<short> value, [ConstantExpected(Min = 1, Max = (byte)(64))] byte count) { throw new PlatformNotSupportedException(); }
 
     /// <summary>
     /// uint16x8_t vqrshrn_high_n_u32 (uint16x4_t r, uint32x4_t a, const int n)
     ///   A32: VQRSHRN.U32 Dd+1, Dn, #n
     ///   A64: UQRSHRN2 Vd.8H, Vn.4S, #n
     /// </summary>
+    [Obsolete("Use unsigned overload or signed instruction.")]
     public static Vector128<short> ShiftRightLogicalRoundedNarrowingSaturateUpper(Vector64<short> lower, Vector128<int> value, [ConstantExpected(Min = 1, Max = (byte)(32))] byte count) { throw new PlatformNotSupportedException(); }
  
     /// <summary>
     /// uint32x4_t vqrshrn_high_n_u64 (uint32x2_t r, uint64x2_t a, const int n)
     ///   A32: VQRSHRN.U64 Dd+1, Dn, #n
     ///   A64: UQRSHRN2 Vd.4S, Vn.2D, #n
     /// </summary>
+    [Obsolete("Use unsigned overload or signed instruction.")]
     public static Vector128<int> ShiftRightLogicalRoundedNarrowingSaturateUpper(Vector64<int> lower, Vector128<long> value, [ConstantExpected(Min = 1, Max = (byte)(16))] byte count) { throw new PlatformNotSupportedException(); }
  
     /// <summary>
     /// uint8x16_t vqrshrn_high_n_u16 (uint8x8_t r, uint16x8_t a, const int n)
     ///   A32: VQRSHRN.U16 Dd+1, Dn, #n
     ///   A64: UQRSHRN2 Vd.16B, Vn.8H, #n
     /// </summary>
+    [Obsolete("Use unsigned overload or signed instruction.")]
     public static Vector128<sbyte> ShiftRightLogicalRoundedNarrowingSaturateUpper(Vector64<sbyte> lower, Vector128<short> value, [ConstantExpected(Min = 1, Max = (byte)(64))] byte count) { throw new PlatformNotSupportedException(); }
 }
```
## Add public [Compute/Verify]IntegrityCheck methods to NegotiateAuthentication

**Approved** | [#runtime/86950](https://github.com/dotnet/runtime/issues/86950#issuecomment-1883657772) | [Video](https://www.youtube.com/watch?v=O66ZW4m6pOs&t=1h16m34s)

* `ComputeIntegrityCheck`'s `signature` parameter was renamed to `signatureWriter` to be consistent with how the `IBufferWriter` parameters are named in `Wrap` and `Unwrap`.

```C#
namespace System.Net.Security;

public sealed class NegotiateAuthentication : IDisposable
{
    /// <summary>
    /// Computes the message integrity check of a given message.
    /// </summary>
    /// <param name="message">Input message for MIC calculation.</param>
    /// <param name="signatureWriter">Buffer writer where the MIC is written.</param>
    /// <remarks>
    /// Implements the GSSAPI GetMIC operation.
    ///
    /// The method modifies the internal state and may update sequence numbers depending on the
    /// selected algorithm. Two successive invocations thus don't produce the same result and
    /// it's important to carefully pair GetMIC and VerifyMIC calls on the both sides of the
    /// authenticated session.
    /// </remarks>
    public void ComputeIntegrityCheck(ReadOnlySpan<byte> message, IBufferWriter<byte> signatureWriter);

    /// <summary>
    /// Verifies the message integrity check of a given message.
    /// </summary>
    /// <param name="message">Input message for MIC calculation.</param>
    /// <param name="signature">MIC to be verified.</param>
    /// <returns>For successfully verified MIC, the method returns true.</returns>
    /// <remarks>
    /// Implements the GSSAPI VerifyMIC operation.
    ///
    /// The method modifies the internal state and may update sequence numbers depending on the
    /// selected algorithm. Two successive invocations thus don't produce the same result and
    /// it's important to carefully pair GetMIC and VerifyMIC calls on the both sides of the
    /// authenticated session.
    /// </remarks>
    public bool VerifyIntegrityCheck(ReadOnlySpan<byte> message, ReadOnlySpan<byte> signature);
}
```
## TextWriter.Aggregate

**Approved** | [#runtime/93623](https://github.com/dotnet/runtime/issues/93623#issuecomment-1883682498) | [Video](https://www.youtube.com/watch?v=O66ZW4m6pOs&t=1h23m31s)

* We stuck with the static method alternative, but renamed Aggregate to CreateBroadcaster
* And converted it to a params-array method because of concern from non-C# callers.

```C#
public abstract partial class TextWriter
{
    public static TextWriter CreateBroadcaster(params TextWriter[] writers);
}
```
## VPCLMULQDQ Intrinsics

**Approved** | [#runtime/95772](https://github.com/dotnet/runtime/issues/95772#issuecomment-1883688755) | [Video](https://www.youtube.com/watch?v=O66ZW4m6pOs&t=1h42m21s)

Looks good as proposed.

```C#
namespace System.Runtime.Intrinsics.X86;

/// <summary>
/// This class provides access to Intel VPCLMULQDQ hardware instructions via intrinsics
/// </summary>
[Intrinsic]
[CLSCompliant(false)]
public abstract class Pclmulqdq256 : Pclmulqdq
{
    internal Pclmulqdq256() { }

    // This would depend on the VPCLMULQDQ CPUID bit for VEX encoding and VPCLMULQDQ + AVX512VL for EVEX
    public static new bool IsSupported { get => IsSupported; }

    [Intrinsic]
    public new abstract class X64 : Pclmulqdq.X64
    {
        internal X64() { }

        public static new bool IsSupported { get => IsSupported; }
    }

    /// <summary>
    /// __m256i _mm256_clmulepi64_si128 (__m256i a, __m256i b, const int imm8)
    ///   VPCLMULQDQ ymm1, ymm2, ymm3/m256, imm8
    /// </summary>
    public static Vector256<long> CarrylessMultiply(Vector256<long> left, Vector256<long> right, [ConstantExpected] byte control) => CarrylessMultiply(left, right, control);
    /// __m256i _mm256_clmulepi64_si128 (__m256i a, __m256i b, const int imm8)
    ///   VPCLMULQDQ ymm1, ymm2, ymm3/m256, imm8
    /// </summary>
    public static Vector256<ulong> CarrylessMultiply(Vector256<ulong> left, Vector256<ulong> right, [ConstantExpected] byte control) => CarrylessMultiply(left, right, control);
}


/// <summary>
/// This class provides access to Intel AVX-512 VPCLMULQDQ hardware instructions via intrinsics
/// </summary>
[Intrinsic]
[CLSCompliant(false)]
public abstract class Pclmulqdq512 : Pclmulqdq
{
    internal Pclmulqdq512() { }

    // This would depend on the VPCLMULQDQ + AVX512F CPUID bits
    public static new bool IsSupported { get => IsSupported; }

    [Intrinsic]
    public new abstract class X64 : Pclmulqdq.X64
    {
        internal X64() { }

        public static new bool IsSupported { get => IsSupported; }
    }

    /// <summary>
    /// __m512i _mm512_clmulepi64_si128 (__m512i a, __m512i b, const int imm8)
    ///   VPCLMULQDQ zmm1, zmm2, zmm3/m512, imm8
    /// </summary>
    public static Vector512<long> CarrylessMultiply(Vector512<long> left, Vector512<long> right, [ConstantExpected] byte control) => CarrylessMultiply(left, right, control);
    /// __m512i _mm512_clmulepi64_si128 (__m512i a, __m512i b, const int imm8)
    ///   VPCLMULQDQ zmm1, zmm2, zmm3/m512, imm8
    /// </summary>
    public static Vector512<ulong> CarrylessMultiply(Vector512<ulong> left, Vector512<ulong> right, [ConstantExpected] byte control) => CarrylessMultiply(left, right, control);
}
```
## Using UMONITOR, UMWAIT, TPAUSE in CLR and exposing in Intel specific hardware intrinsics

**Approved** | [#runtime/66873](https://github.com/dotnet/runtime/issues/66873#issuecomment-1883699424) | [Video](https://www.youtube.com/watch?v=O66ZW4m6pOs&t=1h46m38s)

Looks good as proposed.

```C#
namespace System.Runtime.Intrinsics.X86;

[Intrinsic]
[CLSCompliant(false)]
public abstract class WaitPkg : X86Base
{
    public static new bool IsSupported { get; }

    // UMONITOR: void _umonitor(void *address);
    public static unsafe void SetUpUserLevelMonitor(void* address);
    
    // UMWAIT: uint8_t _umwait(uint32_t control, uint64_t counter);
    public static bool WaitForUserLevelMonitor(uint control, ulong counter);
    
    // TPAUSE: uint8_t _tpause(uint32_t control, uint64_t counter);
    public static bool TimedPause(uint control, ulong counter);

    [Intrinsic]
    public new abstract class X64 : X86Base.X64
    {
        internal X64() { }

        public static new bool IsSupported { get; }
    }
}
```
