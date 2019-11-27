# Quick Reviews 11/26/2019

## Add asynchronous overload of WindowsIdentity.RunImpersonated 

**Approved** | [#corefx/24977](https://github.com/dotnet/corefx/issues/24977#issuecomment-558756052) | [Video](https://www.youtube.com/watch?v=b509D73mmGc&t=0h0m0s)

* These APIs can be simply implemented over the existing one, thanks for generics.
* However, it's not obvious that is in fact supported. Adding explicit overload that are named with the `Async` suffix would make sense
* We considered adding a cancellation token, but given callers likely have to allocate a closure anyway, this doesn't save anything and impersonation is already expensive.

```C#
#nullable enable
namespace System.Security.Principal
{
    public partial class WindowsIdentity
    {
        public static Task<T> RunImpersonatedAsync<T>(SafeAccessTokenHandle safeAccessTokenHandle, Func<Task<T>> func)
            => RunImpersonated(safeAccessTokenHandle, func);

        public static Task RunImpersonatedAsync(SafeAccessTokenHandle safeAccessTokenHandle, Func<Task> func)
            => RunImpersonated(safeAccessTokenHandle, func);    
    }
}
```
## Consider Socket.OSSupportsUnixDomainSockets property

**Approved** | [#corefx/27643](https://github.com/dotnet/corefx/issues/27643#issuecomment-558760298) | [Video](https://www.youtube.com/watch?v=b509D73mmGc&t=0h12m30s)

* Some version of Windows 10 supports Unix domain sockets; having a capability API to check this without having to know the exact OS version
* Only concern is that there are more capability APIs we might have to expose in the future. It's debatable whether a Boolean properties on this type are the correct pattern, but it seems consistent with prior art and domain sockets are reasonably common.

```C#
namespace System.Net.Sockets
{
    public partial class Socket
    {
        public static bool OSSupportsUnixDomainSockets { get; }
        // Existing APIs
        // public static bool OSSupportsIPv4 { get; }
        // public static bool OSSupportsIPv6 { get; }
    }
}
```
## System.Threading.Channels : Expose Channel.EstimatedCount

**Approved** | [#corefx/30852](https://github.com/dotnet/corefx/issues/30852#issuecomment-558762980) | [Video](https://www.youtube.com/watch?v=b509D73mmGc&t=0h23m36s)

* We considered naming the property with `EstimatedCount` but that seems overkill because you never get an exact count unless you freeze the world, but that's the nature of concurrent APIs.
* We can't make the API abstract, but most of us believe that the API guideline is to use the Tester-Doer pattern (capability API + API).

```C#
namespace System.Threading.Channels
{
    public abstract partial class ChannelReader<T>
    {
        public virtual bool CanCount => false;
        public virtual int Count { get; }
    }
    public abstract partial class ChannelWriter<T>
    {
        public virtual bool CanCount => false;
        public virtual int Count { get; }
    }
}
```
## Support Rfc3279 signature format for DSA and EcDSA

**Approved** | [#corefx/42684](https://github.com/dotnet/corefx/issues/42684#issuecomment-558782564) | [Video](https://www.youtube.com/watch?v=b509D73mmGc&t=1h1m14s)

* The APIs cannot be resumed, thus no `OperationStatus` or `bytesConsumed` outputs
* The naming is slightly different between `DSA` and `ECDsa` but this mirrors existing conventions
* `out` should go last

```C#
#nullable enable
namespace System.Security.Cryptography
{
    public enum DSASignatureFormat
    {
        IeeeP1363FixedFieldConcatenation,
        Rfc3279DerSequence
    }

    public abstract partial class DSA : AsymmetricAlgorithm
    {
        public byte[] SignData(byte[] data, int offset, int count, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        public byte[] SignData(byte[] data, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        public byte[] SignData(Stream data, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        public bool TrySignData(ReadOnlySpan<byte> data, Span<byte> destination, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat, out int bytesWritten);
        protected virtual byte[] SignDataCore(ReadOnlySpan<byte> data, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        protected virtual byte[] SignDataCore(Stream data, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        protected virtual bool TrySignDataCore(ReadOnlySpan<byte> data, Span<byte> destination, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat, out int bytesWritten);

        public bool VerifyData(byte[] data, byte[] signature, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        public bool VerifyData(byte[] data, int offset, int count, byte[] signature, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        public bool VerifyData(Stream data, byte[] signature, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        public bool VerifyData(ReadOnlySpan<byte> data, ReadOnlySpan<byte> signature, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        protected virtual bool VerifyDataCore(Stream data, ReadOnlySpan<byte> signature, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        protected virtual bool VerifyDataCore(ReadOnlySpan<byte> data, ReadOnlySpan<byte> signature, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);

        public byte[] CreateSignature(byte[] rgbHash, DSASignatureFormat signatureFormat);
        public bool TryCreateSignature(ReadOnlySpan<byte> hash, Span<byte> destination, DSASignatureFormat signatureFormat, out int bytesWritten);
        protected virtual byte[] CreateSignatureCore(ReadOnlySpan<byte> hash, DSASignatureFormat signatureFormat);
        protected virtual bool TryCreateSignatureCore(ReadOnlySpan<byte> hash, Span<byte> destination, DSASignatureFormat signatureFormat, out int bytesWritten);

        public bool VerifySignature(byte[] rgbHash, byte[] rgbSignature, DSASignatureFormat signatureFormat);
        public bool VerifySignature(ReadOnlySpan<byte> hash, ReadOnlySpan<byte> signature, DSASignatureFormat signatureFormat);
        protected virtual bool VerifySignatureCore(ReadOnlySpan<byte> hash, ReadOnlySpan<byte> signature, DSASignatureFormat signatureFormat);

        public int GetMaxSignatureSize(DSASignatureFormat signatureFormat);
    }

    public abstract partial class ECDsa : AsymmetricAlgorithm
    {
        public byte[] SignData(byte[] data, int offset, int count, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        public byte[] SignData(byte[] data, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        public byte[] SignData(Stream data, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        public bool TrySignData(ReadOnlySpan<byte> data, Span<byte> destination, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat, out int bytesWritten);
        protected virtual byte[] SignDataCore(ReadOnlySpan<byte> data, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        protected virtual byte[] SignDataCore(Stream data, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        protected virtual bool TrySignDataCore(ReadOnlySpan<byte> data, Span<byte> destination, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat, out int bytesWritten);

        public bool VerifyData(byte[] data, byte[] signature, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        public bool VerifyData(byte[] data, int offset, int count, byte[] signature, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        public bool VerifyData(Stream data, byte[] signature, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        public bool VerifyData(ReadOnlySpan<byte> data, ReadOnlySpan<byte> signature, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        protected virtual bool VerifyDataCore(ReadOnlySpan<byte> data, ReadOnlySpan<byte> signature, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);
        protected virtual bool VerifyDataCore(Stream data, ReadOnlySpan<byte> signature, HashAlgorithmName hashAlgorithm, DSASignatureFormat signatureFormat);

        public byte[] SignHash(byte[] hash, DSASignatureFormat signatureFormat);
        public bool TrySignHash(ReadOnlySpan<byte> hash, Span<byte> destination, DSASignatureFormat signatureFormat, out int bytesWritten);
        protected virtual byte[] SignHashCore(ReadOnlySpan<byte> hash, DSASignatureFormat signatureFormat);
        protected virtual bool TrySignHashCore(ReadOnlySpan<byte> hash, Span<byte> destination, DSASignatureFormat signatureFormat, out int bytesWritten);

        public bool VerifyHash(byte[] hash, byte[] signature, DSASignatureFormat signatureFormat);
        public bool VerifyHash(ReadOnlySpan<byte> hash, ReadOnlySpan<byte> signature, DSASignatureFormat signatureFormat);
        protected virtual bool VerifyHashCore(ReadOnlySpan<byte> hash, ReadOnlySpan<byte> signature, DSASignatureFormat signatureFormat);

        public int GetMaxSignatureSize(DSASignatureFormat signatureFormat);
    }
}
```
## Consider exposing a HWIntrinsic that allows efficient loading, regardless of encoding.

**Approved** | [#corefx/33566](https://github.com/dotnet/corefx/issues/33566#issuecomment-558793376) | [Video](https://www.youtube.com/watch?v=b509D73mmGc&t=1h21m6s)

* `LoadAlignedVector128Unsafe` will throw an `AccessViolationException` when optimizations are disabled when the data isn't aligned. When optimizations are on, it depends on the hardware. On newer hardware, the access will work but be slower. Older hardware will AV.
* We should include the word `Aligned` in the API
* In general, `Unsafe` should be a prefix, rather than a suffix (to make sure it pops) but to be consistent with other APIs we'll go with a suffix here.

```C#
namespace System.Runtime.Intrinsics.X86
{
    public partial abstract class Sse
    {
        // New APIs:
        public unsafe Vector128<float> LoadAlignedVector128Unsafe(float* address);

        // Existing APIs:
        // public unsafe Vector128<float> LoadVector128(float* address);
        // public unsafe Vector128<float> LoadAlignedVector128(float* address);
    }

    public partial abstract class Sse2
    {
        // New APIs:
        public unsafe Vector128<double> LoadAlignedVector128Unsafe(double* address);
        public unsafe Vector128<byte> LoadAlignedVector128Unsafe(byte* address);
        public unsafe Vector128<sbyte> LoadAlignedVector128Unsafe(sbyte* address);
        public unsafe Vector128<short> LoadAlignedVector128Unsafe(short* address);
        public unsafe Vector128<ushort> LoadAlignedVector128Unsafe(ushort* address);
        public unsafe Vector128<int> LoadAlignedVector128Unsafe(int* address);
        public unsafe Vector128<uint> LoadAlignedVector128Unsafe(uint* address);
        public unsafe Vector128<long> LoadAlignedVector128Unsafe(long* address);
        public unsafe Vector128<ulong> LoadAlignedVector128Unsafe(ulong* address);

        // Existing APIs:
        // public unsafe Vector128<double> LoadVector128(double* address);
        // public unsafe Vector128<byte> LoadVector128(byte* address);
        // public unsafe Vector128<sbyte> LoadVector128(sbyte* address);
        // public unsafe Vector128<short> LoadVector128(short* address);
        // public unsafe Vector128<ushort> LoadVector128(ushort* address);
        // public unsafe Vector128<int> LoadVector128(int* address);
        // public unsafe Vector128<uint> LoadVector128(uint* address);
        // public unsafe Vector128<long> LoadVector128(long* address);
        // public unsafe Vector128<ulong> LoadVector128(ulong* address);
        //
        // public unsafe Vector128<double> LoadAlignedVector128(double* address);
        // public unsafe Vector128<byte> LoadAlignedVector128(byte* address);
        // public unsafe Vector128<sbyte> LoadAlignedVector128(sbyte* address);
        // public unsafe Vector128<short> LoadAlignedVector128(short* address);
        // public unsafe Vector128<ushort> LoadAlignedVector128(ushort* address);
        // public unsafe Vector128<int> LoadAlignedVector128(int* address);
        // public unsafe Vector128<uint> LoadAlignedVector128(uint* address);
        // public unsafe Vector128<long> LoadAlignedVector128(long* address);
        // public unsafe Vector128<ulong> LoadAlignedVector128(ulong* address);
    }
}
```
