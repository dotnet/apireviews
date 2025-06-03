# API Review 06/03/2025

## ClaimsIdentity to perform case-sensitive comparison

**Approved** | [#runtime/113562](https://github.com/dotnet/runtime/issues/113562#issuecomment-2936382729) | [Video](https://www.youtube.com/watch?v=sH_NeYqiTQQ&t=0h0m0s)

Updated proposal looks good as proposed.

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

        public ClaimsIdentity(BinaryReader reader, StringComparison stringComparison);
        public ClaimsIdentity(ClaimsIdentity other, StringComparison stringComparison);
    }
}
```
## Ssl*AuthenticationOptions.AllowRsaPssPad, AllowRsaPkcs1Pad

**Approved** | [#runtime/114740](https://github.com/dotnet/runtime/issues/114740#issuecomment-2936442997) | [Video](https://www.youtube.com/watch?v=sH_NeYqiTQQ&t=0h9m38s)

* Changed "Pad" to "Padding"

```C#
namespace System.Net.Security;

public partial class SslClientAuthenticationOptions
{
        public bool AllowRsaPssPadding { get; set; } = true;
        public bool AllowRsaPkcsPadding { get; set; } = true;
}

public partial class SslServerAuthenticationOptions
{
        public bool AllowRsaPssPadding { get; set; } = true;
        public bool AllowRsaPkcs1Padding { get; set; } = true;
}
```
## Amend MLKem, MLDsa, and SlhDsa ImportEncryptedPkcs8PrivateKey

**Approved** | [#runtime/115024](https://github.com/dotnet/runtime/issues/115024#issuecomment-2936456675) | [Video](https://www.youtube.com/watch?v=sH_NeYqiTQQ&t=0h29m27s)

* Looks good as amended

```diff
namespace System.Security.Cryptography
{
    public class MLDsa, MLKem, SlhDsa, CompositeMLDsa
    {
-       public static MLKem ImportEncryptedPkcs8PrivateKey(string password, ReadOnlySpan<byte> source);
+       public static MLKem ImportEncryptedPkcs8PrivateKey(string password, byte[] source);
    }
}
```
## `BigMul` for `nuint` and `nint`

**Approved** | [#runtime/114731](https://github.com/dotnet/runtime/issues/114731#issuecomment-2936486864) | [Video](https://www.youtube.com/watch?v=sH_NeYqiTQQ&t=0h33m43s)

* Changed `(a, b, out low)` to `(left, right, out lower)` to match the newer versions of these methods.
* Exposed the UInt128 and Int128 versions, too (because we thought they already were public)

```C#
namespace System
{
    public readonly partial struct UIntPtr
    {
        public static nuint BigMul(nuint left, nuint right, out nuint lower);
    }

    public readonly partial struct IntPtr
    {
        public static nint BigMul(nint left, nint right, out nint lower);
    }

    public readonly partial struct UInt128
    {
        public static UInt128 BigMul(UInt128 left, UInt128 right, out UInt128 lower);
    }

    public readonly partial struct Int128
    {
        public static Int128 BigMul(Int128 left, Int128 right, out Int128 lower);
    }
}
```
## RSA PSS salt length

**Approved** | [#runtime/104080](https://github.com/dotnet/runtime/issues/104080#issuecomment-2936559595) | [Video](https://www.youtube.com/watch?v=sH_NeYqiTQQ&t=0h47m44s)

Fixed the spelling of "length", otherwise look good as proposed.

```C#
using System.Security.Cryptography;

public partial class RSASignaturePadding
{
    // 0 is valid, so "default" has to be some other number.
    public const int PssSaltLengthIsHashLength = -1;
    public const int PssSaltLengthMax = -2;

    public int PssSaltLength { get; }
 
    public static RSASignaturePadding CreatePss(int saltLength);
}
```
## BitArray.AsBytes()

**Approved** | [#runtime/115400](https://github.com/dotnet/runtime/issues/115400#issuecomment-2936661233) | [Video](https://www.youtube.com/watch?v=sH_NeYqiTQQ&t=0h58m57s)

* Rather than an instance method, we feel this belongs on CollectionsMarshal.
* Now on CollectionsMarshal, it can be a writable span that is returned.
* The type will have to be forwarded down to corelib to be in CollectionsMarshal.

```C#
public partial class CollectionsMarshal
{
    public static Span<byte> AsBytes(BitArray? bitArray);
}
```
## Java Marshalling API

**Approved** | [#runtime/115506](https://github.com/dotnet/runtime/issues/115506#issuecomment-2936824529) | [Video](https://www.youtube.com/watch?v=sH_NeYqiTQQ&t=1h23m59s)

* Since this is heavy interop and already using pointers, IntPtr => `void*`
* Non-generic GCHandle is better here, because of FinishCrossReferenceProcessing
* FinishCrossReferenceProcessing should take ReadOnlySpan, not writable Span.
* MarkCrossReferences => MarkCrossReferencesArgs
* All of the nint counts/lenghts changed to nuint.
  * If the indexes are unsigned, change them to nuint, too.
* `sLen` suffixes became `Count`

```C#
namespace System.Runtime.InteropServices.Java;

[SupportedOSPlatform("android")]
[StructLayout(LayoutKind.Sequential)]
public struct ComponentCrossReference
{
    public nint SourceGroupIndex;
    public nint DestinationGroupIndex;
}

[SupportedOSPlatform("android")]
[StructLayout(LayoutKind.Sequential)]
public unsafe struct StronglyConnectedComponent
{
    public nuint Count;
    public void** Contexts;
}

[SupportedOSPlatform("android")]
[StructLayout(LayoutKind.Sequential)]
public unsafe struct MarkCrossReferencesArgs
{
    public nuint ComponentCount;
    public StronglyConnectedComponent* Components;
    public nuint CrossReferenceCount;
    public ComponentCrossReference* CrossReferences;
}

[SupportedOSPlatform("android")]
partial static class JavaMarshal
{
    public static unsafe void Initialize(delegate* unmanaged<MarkCrossReferencesArgs*, void> markCrossReferences);

    public static unsafe GCHandle CreateCrossReferenceHandle(object obj, void* context);

    public static unsafe void* GetContext(GCHandle obj);

    public static unsafe void FinishCrossReferenceProcessing(
        MarkCrossReferencesArgs* crossReferences,
        ReadOnlySpan<GCHandle> unreachableObjectHandles);
}
```
