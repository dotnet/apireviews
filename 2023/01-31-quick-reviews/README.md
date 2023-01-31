# API Review 01/31/2023

## API proposal for SP800-108-CTR-HMAC cryptographic algorithm

**Approved** | [#runtime/65577](https://github.com/dotnet/runtime/issues/65577#issuecomment-1410900869) | [Video](https://www.youtube.com/watch?v=dEynVhV0wJU&t=0h0m0s)

We discussed the packaging for this, and have come up with this plan:

.NET8: Inbox, part of System.Security.Cryptography.dll

Add a new compatibility package: Microsoft.Bcl.Cryptography.  It will build for
* .NETSTANDARD20
* .NETFRAMEWORK (whatever version we target)
* .NET8: typeforward

```C#
namespace System.Security.Cryptography;

public sealed class SP800108HmacCounterKdf : IDisposable
{
    public SP800108HmacCounterKdf(ReadOnlySpan<byte> key, HashAlgorithmName hashAlgorithm);
    public SP800108HmacCounterKdf(byte[] key, HashAlgorithmName hashAlgorithm);

    public byte[] DeriveKey(byte[] label, byte[] context, int derivedKeyLengthInBytes);
    public byte[] DeriveKey(ReadOnlySpan<byte> label, ReadOnlySpan<byte> context, int derivedKeyLengthInBytes);
    public byte[] DeriveKey(string label, string context, int derivedKeyLengthInBytes);
    public byte[] DeriveKey(ReadOnlySpan<char> label, ReadOnlySpan<char> context, int derivedKeyLengthInBytes);
    public void DeriveKey(ReadOnlySpan<byte> label, ReadOnlySpan<byte> context, Span<byte> destination);
    public void DeriveKey(ReadOnlySpan<char> label, ReadOnlySpan<char> context, Span<byte> destination);

    public static byte[] DeriveBytes(byte[] key, HashAlgorithmName hashAlgorithm, byte[] label, byte[] context, int derivedKeyLengthInBytes);
    public static byte[] DeriveBytes(ReadOnlySpan<byte> key, HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> label, ReadOnlySpan<byte> context, int derivedKeyLengthInBytes);
    public static byte[] DeriveBytes(byte[] key, HashAlgorithmName hashAlgorithm, string label, string context, int derivedKeyLengthInBytes);
    public static byte[] DeriveBytes(ReadOnlySpan<byte> key, HashAlgorithmName hashAlgorithm, ReadOnlySpan<char> label, ReadOnlySpan<char> context, int derivedKeyLengthInBytes);
    public static void DeriveBytes(ReadOnlySpan<byte> key, HashAlgorithmName hashAlgorithm, ReadOnlySpan<byte> label, ReadOnlySpan<byte> context, Span<byte> destination);
    public static void DeriveBytes(ReadOnlySpan<byte> key, HashAlgorithmName hashAlgorithm, ReadOnlySpan<char> label, ReadOnlySpan<char> context, Span<byte> destination);

    public void Dispose();
}
```
## Host.CreateEmptyApplicationBuilder

**Approved** | [#runtime/81280](https://github.com/dotnet/runtime/issues/81280#issuecomment-1410969741) | [Video](https://www.youtube.com/watch?v=dEynVhV0wJU&t=0h48m16s)

Through a lot of adding and removing things during the discussion, we added an overload of CreateApplicationBuilder that takes the settings object for symmetry and a clear path from the existing pattern to the new one.  This new overload is a source-breaking change for the `null` literal, but we believe the number of callers will be small enough that it's not a concern.

```diff
namespace Microsoft.Extensions.Hosting;

public static class Host
{
        public static HostApplicationBuilder CreateApplicationBuilder();
+       public static HostApplicationBuilder CreateApplicationBuilder(HostApplicationBuilderSettings? settings);
        public static HostApplicationBuilder CreateApplicationBuilder(string[]? args);
+       public static HostApplicationBuilder CreateEmptyApplicationBuilder(HostApplicationBuilderSettings? settings);
        public static IHostBuilder CreateDefaultBuilder();
        public static IHostBuilder CreateDefaultBuilder(string[]? args);
}
public sealed partial class HostApplicationBuilder
{
    public HostApplicationBuilder() { }
    public HostApplicationBuilder(HostApplicationBuilderSettings? settings) { }
    public HostApplicationBuilder(string[]? args) { }
}
```

## Proposal: Asynchronously cancel a CancellationTokenSource

**Approved** | [#runtime/23405](https://github.com/dotnet/runtime/issues/23405#issuecomment-1410993808) | [Video](https://www.youtube.com/watch?v=dEynVhV0wJU&t=1h44m3s)

Looks good as proposed

```C#
namespace System.Threading
{
    public partial class CancellationTokenSource
    {
        public Task CancelAsync();
    }
}
```
