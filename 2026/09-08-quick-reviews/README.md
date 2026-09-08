# API Review 09/08/2026

## Hpke improvements

**Approved** | [#runtime/133320](https://github.com/dotnet/runtime/issues/133320#issuecomment-5589028058) | [Video](https://www.youtube.com/watch?v=tfDPiit0X0E&t=0h0m0s)

Looks good as proposed.

`ikm` as a non-expanded name has prior art in HKDF.Extract.

```diff
namespace System.Security.Cryptography;

public partial enum HpkeKem
{
    DHKEM_P384_HKDF_SHA384 = 17,
+   DHKEM_P521_HKDF_SHA512 = 18,
}

public partial class Hpke
{
    public static Hpke GenerateKey(HpkeSuite suite);
+   public static Hpke DeriveKey(HpkeSuite suite, byte[] ikm);
+   public static Hpke DeriveKey(HpkeSuite suite, ReadOnlySpan<byte> ikm);
}
```
## `Task<T>.CastUp`

**Approved** | [#runtime/89778](https://github.com/dotnet/runtime/issues/89778#issuecomment-5589114860) | [Video](https://www.youtube.com/watch?v=tfDPiit0X0E&t=0h9m45s)

Looks good as proposed.

```csharp
public partial class Task<T>
{
    public static Task<T> CastUp<TDerived>(Task<TDerived> task) where TDerived : class?, T;
}

public partial struct ValueTask<T>
{
    public static ValueTask<T> CastUp<TDerived>(ValueTask<TDerived> task) where TDerived : class?, T;
}
```
## Make IsApplePlatform public

**Approved** | [#runtime/113262](https://github.com/dotnet/runtime/issues/113262#issuecomment-5589201644) | [Video](https://www.youtube.com/watch?v=tfDPiit0X0E&t=0h17m1s)

Looks good as proposed

```csharp
namespace System;

public partial class OperatingSystem
{
    public static bool IsApplePlatform();
}
```
## Add OSPlatform.OpenBSD and OperatingSystem.IsOpenBSD APIs

**Approved** | [#runtime/129482](https://github.com/dotnet/runtime/issues/129482#issuecomment-5589307212) | [Video](https://www.youtube.com/watch?v=tfDPiit0X0E&t=0h23m50s)


* Given that OSPlatform does not have tvOS/watchOS/et al, it seems that we've moved past that type, so let's only add the OperatingSystem members.

```csharp
namespace System
{
    public static partial class OperatingSystem
    {
        public static bool IsOpenBSD();
        public static bool IsOpenBSDVersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0);
    }
}
```
## Add missing IsPlatform methods to `OperatingSystem` class

**Approved** | [#runtime/108632](https://github.com/dotnet/runtime/issues/108632#issuecomment-5589383617) | [Video](https://www.youtube.com/watch?v=tfDPiit0X0E&t=0h32m32s)

Looks good as proposed.

Let's leave Haiku out until someone asks for it.

```csharp
namespace System;

public partial class OperatingSystem
{
    public static bool IsNetBSD()
    public static bool IsNetBSDVersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0)

    public static bool IsIllumos()
    public static bool IsIllumosVersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0)

    public static bool IsSolaris()
    public static bool IsSolarisVersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0)
}
```
## Expose flattened index get/set on Array

**Rejected** | [#runtime/101244](https://github.com/dotnet/runtime/issues/101244#issuecomment-5589561933) | [Video](https://www.youtube.com/watch?v=tfDPiit0X0E&t=0h38m57s)

We believe the `Tensor` types obviate the need for this, and can do so without introducing forced boxing.

If there's some reason why it doesn't, we can re-discuss, but until then there's no real need.
## Suggestion `ComWrappers.TryGetComInstance()` to improve UX

**Approved** | [#runtime/106979](https://github.com/dotnet/runtime/issues/106979#issuecomment-5589613987) | [Video](https://www.youtube.com/watch?v=tfDPiit0X0E&t=0h54m4s)

Looks good as proposed.

```csharp
namespace System.Runtime.InteropServices;

public partial class ComWrappers
{
    public IntPtr GetOrCreateComInterfaceForObject(object instance, CreateComInterfaceFlags flags, in Guid interfaceId);
}

public partial class StrategyBasedComWrappers
{
    public IntPtr GetOrCreateComInterfaceForObject<TInterface>(object instance, CreateComInterfaceFlags flags);
}
```
## `ComWrappersObject`

**Approved** | [#runtime/132490](https://github.com/dotnet/runtime/issues/132490#issuecomment-5589782511) | [Video](https://www.youtube.com/watch?v=tfDPiit0X0E&t=0h58m31s)


* ComWrappersObject as a name feels weird for the base class, particularly with all the peer types.  ComObjectBase seems to satisfy the "low usage carved-out base class gains a Base suffix" guideline.

```csharp
namespace System.Runtime.InteropServices;

[UnsupportedOSPlatform("android")]
[UnsupportedOSPlatform("browser")]
[UnsupportedOSPlatform("ios")]
[UnsupportedOSPlatform("tvos")]
public abstract class ComObjectBase
{
    protected ComObjectBase();
}

public partial class ComObject : ComObjectBase
{
}
```
## ValidationAttribute.DescriptionMessage

**NeedsWork** | [#runtime/133433](https://github.com/dotnet/runtime/issues/133433#issuecomment-5589962784) | [Video](https://www.youtube.com/watch?v=tfDPiit0X0E&t=1h11m54s)

The proposal as-is doesn't seem to have a coherent use case attached to it, making it hard to evaluate the proposal.  More clear scenarios, and understanding how the various higher layer components that consume these attributes would use the description is important to understanding if we're adding the right thing.

```csharp
namespace System.ComponentModel.DataAnnotations;

public partial class ValidationAttribute
{
    // Describes the rule; available before validation runs (lifecycle moment ①).
    public string? DescriptionMessage { get; set; }

    // Formats the description for display; routes through the existing FormatMessage(format, name) hook.
    // Returns null when no description is configured.
    public virtual string? FormatDescriptionMessage(string name);
}
```
## MemoryExtensions.CommonSuffixLength<T>

**Approved** | [#runtime/127630](https://github.com/dotnet/runtime/issues/127630#issuecomment-5590096005) | [Video](https://www.youtube.com/watch?v=tfDPiit0X0E&t=1h27m6s)


A good parallel to CommonPrefixLength, but the longer overload shouldn't have a defaulted comparer.

```csharp
namespace System
{
    public static class MemoryExtensions
    {
        public static int CommonSuffixLength<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other);
        public static int CommonSuffixLength<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other, IEqualityComparer<T>? comparer);
    }
}
```
## Add constructor BitArray(IEnumerable<bool> values)

**Approved** | [#runtime/121737](https://github.com/dotnet/runtime/issues/121737#issuecomment-5590364301) | [Video](https://www.youtube.com/watch?v=tfDPiit0X0E&t=1h37m59s)

* Since BitArray already also takes byte[] and int[], and we paralleled that in the ReadOnlySpan ctors, we should do all three here.

```csharp
namespace System.Collections;

public sealed class BitArray
{
    public BitArray(System.Collections.Generic.IEnumerable<bool> values);
    public BitArray(System.Collections.Generic.IEnumerable<byte> values);
    public BitArray(System.Collections.Generic.IEnumerable<int> values);
}
```
