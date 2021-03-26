# Quick Reviews 03/26/2021

## Time Zone IANA Ids to/From Windows Ids conversion APIs

**Approved** | [#runtime/49407](https://github.com/dotnet/runtime/issues/49407#issuecomment-808455616)

* Looks good as proposed but please make sure to properly null-annotate them.

```C#
namespace System
{
    public partial class TimeZoneInfo
    {
        public bool HasIanaId { get; }
        public static bool TryConvertIanaIdToWindowsId(string ianaId, out string windowsId);
        public static bool TryConvertWindowsIdToIanaId(string windowsId, out string ianaId);
        public static bool TryConvertWindowsIdToIanaId(string windowsId, string region, out string ianaId);
    }
}
```

## Add BaseUtcOffset to TimeZoneInfo.AdjustmentRule.

**NeedsWork** | [#runtime/50256](https://github.com/dotnet/runtime/issues/50256#issuecomment-808463210)

* We probably want an overload for `CreateAdjustmentRule` that allows people to construct an instance with an offset
* We need to think about what this means for instances that were created without an offset. Should it be nullable?

```C#
namespace System
{
   public partial class TimeZoneInfo
   {
        public partial class AdjustmentRule
        {
            public TimeSpan BaseUtcOffset { get; }
            public static AdjustmentRule CreateAdjustmentRule(
                DateTime dateStart,
                DateTime dateEnd,
                TimeSpan daylightDelta,
                TransitionTime daylightTransitionStart,
                TransitionTime daylightTransitionEnd,
                TimeSpan baseUtcOffset
            );
         }
    }
}
```

## General purpose non-cryptographic hashing API for .NET

**Approved** | [#runtime/24328](https://github.com/dotnet/runtime/issues/24328#issuecomment-808489277)

* Namespace
    - We're not convinced it deserves a top level namespace like `System.Hashing`.
    - Let's use `System.IO.Hashing`, we could consider a peer to compression which we have in `System.IO.Compression`.
    - `System.Buffers` and `System.Numerics` both seem less discoverable because they seem to focus on the implementation, not the scenario.
* Assembly
    - Should probably be in a separate assembly because there seems to be a need to have it OOB and be able to extend the set of types
* `NonCryptographicHashAlgorithm`
    - `Length` is ambiguous. It could mean digest length or how much data was fed to it. How about `HashLengthInBytes`?
    - Should we keep track of the length of the data that was appended? This probably requires `AppendCore` and `ResetCore` so that we can factor common code into a non-abstract version. We don't believe this is common enough to justify the complexity.
    - We should obsolete and hide `GetHashCode()`, like we did for `HashCode`

```C#
namespace System.IO.Hashing
{
    public abstract class NonCryptographicHashAlgorithm
    {
        public int HashLengthInBytes { get; }

        protected NonCryptographicHashAlgorithm(int hashLengthInBytes);

        public abstract void Append(ReadOnlySpan<byte> source);
        public abstract void Reset();
        protected abstract int GetCurrentHashCore(Span<byte> destination);

        public void Append(byte[] source);
        public void Append(Stream stream);
        public Task AppendAsync(Stream stream, CancellationToken cancellationToken = default);
        public byte[] GetCurrentHash();
        public bool TryGetCurrentHash(Span<byte> destination, out int bytesWritten);
        public int GetCurrentHash(Span<byte> destination);
        public byte[] GetHashAndReset();
        public bool TryGetHashAndReset(Span<byte> destination, out int bytesWritten);
        public int GetHashAndReset(Span<byte> destination);

        protected virtual int GetHashAndResetCore(Span<byte> destination);

        [EditorBrowsable(EditorBrowsableState.Never)]
        [Obsolete("Use GetCurrentHash() to retrieve the computed hash code.", true)]
        public int GetHashCode();
    }
    public class XxHash32 : NonCryptographicHashAlgorithm
    {
        public XxHash32();
        public static byte[] Hash(byte[] source);
        public static byte[] Hash(ReadOnlySpan<byte> source);
        public static bool TryHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
        public static int Hash(ReadOnlySpan<byte> source, Span<byte> destination);
    }
    public class XxHash64 : NonCryptographicHashAlgorithm
    {
        public XxHash64();
        public static byte[] Hash(byte[] source);
        public static byte[] Hash(ReadOnlySpan<byte> source);
        public static bool TryHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
        public static int Hash(ReadOnlySpan<byte> source, Span<byte> destination);
    }
    public class Crc32 : NonCryptographicHashAlgorithm
    {
        public Crc32();
        public static byte[] Hash(byte[] source);
        public static byte[] Hash(ReadOnlySpan<byte> source);
        public static bool TryHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
        public static int Hash(ReadOnlySpan<byte> source, Span<byte> destination);
    }
    public class Crc64 : NonCryptographicHashAlgorithm
    {
        public Crc64();
        public static byte[] Hash(byte[] source);
        public static byte[] Hash(ReadOnlySpan<byte> source);
        public static bool TryHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
        public static int Hash(ReadOnlySpan<byte> source, Span<byte> destination);
    }
}
```

## StringComparer.IsWellKnownComparer

**Approved** | [#runtime/50059](https://github.com/dotnet/runtime/issues/50059#issuecomment-808496914)

* Looks good as proposed

```C#
namespace System
{
    public abstract class StringComparer
    {
        public static bool IsWellKnownOrdinalComparer(IEqualityComparer<string?>? comparer,
                                                      out bool ignoreCase);

        public static bool IsWellKnownCultureAwareComparer(IEqualityComparer<string?>? comparer,
                                                           [NotNullWhen(true)] out CompareInfo? compareInfo,
                                                           out CompareOptions compareOptions);
    }
}
```

## Add CancellationTokenSource.TryReset()

**Approved** | [#runtime/48492](https://github.com/dotnet/runtime/issues/48492#issuecomment-808499702)

* Looks good as proposed

```C#
namespace System.Threading
{
    public partial class CancellationTokenSource
    {
        public bool TryReset();    
    }
}
```
## Proposal: add a static, thread-static Random.Current instance

**Approved** | [#runtime/43887](https://github.com/dotnet/runtime/issues/43887#issuecomment-808500968)

* Looks good as proposed

```C#
namespace System
{
    public partial class Random
    {
        public static Random Shared { get; }
    }
}
```

## Inform client code when a BoundedChannel drops an item

**Approved** | [#runtime/36522](https://github.com/dotnet/runtime/issues/36522#issuecomment-808502110)

* Looks good as proposed

```C#
namespace System.Threading.Channels
{
    public static class Channel
    {
        // Existing:
        // public static Channel<T> CreateBounded<T>(int capacity);
        // public static Channel<T> CreateBounded<T>(BoundedChannelOptions options);
        public static Channel<T> CreateBounded<T>(BoundedChannelOptions options, Action<T> itemDropped);
    }
}
```
## Configuration > Binder - Support for binding by custom attribute names. 

**Approved** | [#runtime/36010](https://github.com/dotnet/runtime/issues/36010#issuecomment-808504315)

* Looks good as proposed
    - Does this need an analyzer?

```C#
namespace Microsoft.Extensions.Configuration
{
    [AttributeUsage(AttributeTargets.Field | AttributeTargets.Property)]
    public sealed class ConfigurationKeyNameAttribute : Attribute
    {
        public ConfigurationKeyNameAttribute(string name);
        public string Name { get; }
    }
}
```

