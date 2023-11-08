# API Review 11/07/2023

## `WasmImportLinkageAttribute` to control wasm module names

**Approved** | [#runtime/93824](https://github.com/dotnet/runtime/issues/93824#issuecomment-1777684948) | [Video](https://www.youtube.com/watch?v=5FVp8Mjuoog&t=0h0m0s)

* According to [this comment](https://github.com/dotnet/runtimelab/issues/2414#issuecomment-1772551025) it seems we decided that P/Invokes without this attribute would fail. Why don't we just make (1) behave like (3)? In other words, what does this attribute add?
* If there is a good reason, we're OK with the proposed name and shape, but it would be nice if we didn't need it.

```cs
namespace System.Runtime.InteropServices;

[AttributeUsage(AttributeTargets.Method, Inherited = false)]
public sealed class WasmImportLinkageAttribute : Attribute
{
    public WasmImportLinkageAttribute();
}
```
## ElapsedEventArgs doesn't have a public constructor

**Approved** | [#runtime/31204](https://github.com/dotnet/runtime/issues/31204#issuecomment-1799532061) | [Video](https://www.youtube.com/watch?v=5FVp8Mjuoog&t=0h32m29s)

* Currently people can't derive from this type (because there are no public constructors), we should seal it
* Do we need to add any argument validation?

```cs
namespace System.Timers;

public sealed class ElapsedEventArgs : EventArgs
{
    public ElapsedEventArgs(DateTime signalTime);
}
```
## Provide support for alternative rounding modes for division and remainder of division.

**Approved** | [#runtime/93568](https://github.com/dotnet/runtime/issues/93568#issuecomment-1799599570) | [Video](https://www.youtube.com/watch?v=5FVp8Mjuoog&t=0h40m14s)

* Looks good as proposed
* This implies those three methods become public statics on the primitives, such as `int`

```cs
namespace System.Numerics;

public enum DivisionRounding
{
    Truncate = 0,
    Floor = 1,
    Ceiling = 2,
    AwayFromZero = 3,
    Euclidean = 4,
}

public partial interface IBinaryInteger<T>
{
    // Existing:
    // static virtual (TSelf Quotient, TSelf Remainder) DivRem(TSelf left, TSelf right);

    static virtual TSelf Divide(TSelf left, TSelf right, DivisionRounding mode);
    static virtual (TSelf Quotient, TSelf Remainder) DivRem(TSelf left, TSelf right, DivisionRounding mode);
    static virtual TSelf Remainder(TSelf left, TSelf right, DivisionRounding mode);
}
```
## KMAC

**Approved** | [#runtime/93494](https://github.com/dotnet/runtime/issues/93494#issuecomment-1799778242) | [Video](https://www.youtube.com/watch?v=5FVp8Mjuoog&t=0h57m50s)

* Looks good as proposed
* There is a spelling error in `desintation`, should be `destination`

```cs
namespace System.Security.Cryptography;

public sealed class Kmac128 : IDisposable
{
    public Kmac128(byte[] key, byte[]? customizationString = null);
    public Kmac128(ReadOnlySpan<byte> key, ReadOnlySpan<byte> customizationString = default);

    public static bool IsSupported { get; }

    public void AppendData(byte[] data);
    public void AppendData(ReadOnlySpan<byte> data);

    public byte[] GetHashAndReset(int outputLength);
    public void GetHashAndReset(Span<byte> destination);

    public byte[] GetCurrentHash(int outputLength);
    public void GetCurrentHash(Span<byte> destination);

    public void Dispose();

    public static byte[] HashData(byte[] key, byte[] source, int outputLength, byte[]? customizationString = null);
    public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, int outputLength, ReadOnlySpan<byte> customizationString = default);
    public static void HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination, ReadOnlySpan<byte> customizationString = default);
    public static byte[] HashData(byte[] key, Stream source, int outputLength, byte[]? customizationString = null);
    public static byte[] HashData(ReadOnlySpan<byte> key, Stream source, int outputLength, ReadOnlySpan<byte> customizationString = default);
    public static void HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination, ReadOnlySpan<byte> customizationString = default);

    public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, int outputLength, byte[]? customizationString = null, CancellationToken cancellationToken = default);
    public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, int outputLength, ReadOnlyMemory<byte> customizationString = default, CancellationToken cancellationToken = default);
    public static ValueTask HashDataAsync(ReadOnlyMemory<byte> key, Stream source, Memory<byte> destination, ReadOnlyMemory<byte> customizationString = default, CancellationToken cancellationToken = default);
}

public sealed class Kmac256 : IDisposable
{
    public Kmac256(byte[] key, byte[]? customizationString = null);
    public Kmac256(ReadOnlySpan<byte> key, ReadOnlySpan<byte> customizationString = default);

    public static bool IsSupported { get; }

    public void AppendData(byte[] data);
    public void AppendData(ReadOnlySpan<byte> data);

    public byte[] GetHashAndReset(int outputLength);
    public void GetHashAndReset(Span<byte> destination);

    public byte[] GetCurrentHash(int outputLength);
    public void GetCurrentHash(Span<byte> destination);

    public void Dispose();

    public static byte[] HashData(byte[] key, byte[] source, int outputLength, byte[]? customizationString = null);
    public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, int outputLength, ReadOnlySpan<byte> customizationString = default);
    public static void HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination, ReadOnlySpan<byte> customizationString = default);
    public static byte[] HashData(byte[] key, Stream source, int outputLength, byte[]? customizationString = null);
    public static byte[] HashData(ReadOnlySpan<byte> key, Stream source, int outputLength, ReadOnlySpan<byte> customizationString = default);
    public static void HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination, ReadOnlySpan<byte> customizationString = default);

    public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, int outputLength, byte[]? customizationString = null, CancellationToken cancellationToken = default);
    public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, int outputLength, ReadOnlyMemory<byte> customizationString = default, CancellationToken cancellationToken = default);
    public static ValueTask HashDataAsync(ReadOnlyMemory<byte> key, Stream source, Memory<byte> destination, ReadOnlyMemory<byte> customizationString = default, CancellationToken cancellationToken = default);
}

public sealed class KmacXof128 : IDisposable
{
    public KmacXof128(byte[] key, byte[]? customizationString = null);
    public KmacXof128(ReadOnlySpan<byte> key, ReadOnlySpan<byte> customizationString = default);

    public static bool IsSupported { get; }

    public void AppendData(byte[] data);
    public void AppendData(ReadOnlySpan<byte> data);

    public byte[] GetHashAndReset(int outputLength);
    public void GetHashAndReset(Span<byte> destination);

    public byte[] GetCurrentHash(int outputLength);
    public void GetCurrentHash(Span<byte> destination);

    public void Dispose();

    public static byte[] HashData(byte[] key, byte[] source, int outputLength, byte[]? customizationString = null);
    public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, int outputLength, ReadOnlySpan<byte> customizationString = default);
    public static void HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination, ReadOnlySpan<byte> customizationString = default);
    public static byte[] HashData(byte[] key, Stream source, int outputLength, byte[]? customizationString = null);
    public static byte[] HashData(ReadOnlySpan<byte> key, Stream source, int outputLength, ReadOnlySpan<byte> customizationString = default);
    public static void HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination, ReadOnlySpan<byte> customizationString = default);

    public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, int outputLength, byte[]? customizationString = null, CancellationToken cancellationToken = default);
    public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, int outputLength, ReadOnlyMemory<byte> customizationString = default, CancellationToken cancellationToken = default);
    public static ValueTask HashDataAsync(ReadOnlyMemory<byte> key, Stream source, Memory<byte> destination, ReadOnlyMemory<byte> customizationString = default, CancellationToken cancellationToken = default);
}

public sealed class KmacXof256 : IDisposable
{
    public KmacXof256(byte[] key, byte[]? customizationString = null);
    public KmacXof256(ReadOnlySpan<byte> key, ReadOnlySpan<byte> customizationString = default);

    public static bool IsSupported { get; }

    public void AppendData(byte[] data);
    public void AppendData(ReadOnlySpan<byte> data);

    public byte[] GetHashAndReset(int outputLength);
    public void GetHashAndReset(Span<byte> destination);

    public byte[] GetCurrentHash(int outputLength);
    public void GetCurrentHash(Span<byte> destination);

    public void Dispose();

    public static byte[] HashData(byte[] key, byte[] source, int outputLength, byte[]? customizationString = null);
    public static byte[] HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, int outputLength, ReadOnlySpan<byte> customizationString = default);
    public static void HashData(ReadOnlySpan<byte> key, ReadOnlySpan<byte> source, Span<byte> destination, ReadOnlySpan<byte> customizationString = default);
    public static byte[] HashData(byte[] key, Stream source, int outputLength, byte[]? customizationString = null);
    public static byte[] HashData(ReadOnlySpan<byte> key, Stream source, int outputLength, ReadOnlySpan<byte> customizationString = default);
    public static void HashData(ReadOnlySpan<byte> key, Stream source, Span<byte> destination, ReadOnlySpan<byte> customizationString = default);

    public static ValueTask<byte[]> HashDataAsync(byte[] key, Stream source, int outputLength, byte[]? customizationString = null, CancellationToken cancellationToken = default);
    public static ValueTask<byte[]> HashDataAsync(ReadOnlyMemory<byte> key, Stream source, int outputLength, ReadOnlyMemory<byte> customizationString = default, CancellationToken cancellationToken = default);
    public static ValueTask HashDataAsync(ReadOnlyMemory<byte> key, Stream source, Memory<byte> destination, ReadOnlyMemory<byte> customizationString = default, CancellationToken cancellationToken = default);
}
```
## Billboard Lefthanded APIs for System.Numerics.Matrix4x4

**Approved** | [#runtime/93046](https://github.com/dotnet/runtime/issues/93046#issuecomment-1799787827) | [Video](https://www.youtube.com/watch?v=5FVp8Mjuoog&t=1h33m53s)

* Looks good as proposed

```cs
namespace System.Numerics;

public partial struct Matrix4x4
{
   public static Matrix4x4 CreateBillboardLeftHanded(Vector3 objectPosition, Vector3 cameraPosition, Vector3 cameraUpVector, Vector3 cameraForwardVector);
   public static Matrix4x4 CreateConstrainedBillboardLeftHanded(Vector3 objectPosition, Vector3 cameraPosition, Vector3 rotateAxis, Vector3 cameraForwardVector, Vector3 objectForwardVector);
}
```
## IsInitOnly boolean property for PropertyDefinition in SDK

**NeedsWork** | [#runtime/80205](https://github.com/dotnet/runtime/issues/80205#issuecomment-1799818209) | [Video](https://www.youtube.com/watch?v=5FVp8Mjuoog&t=1h42m53s)

* How much overhead does this add?
* How about other concepts we recently added, such as `required`?
* We recently added nullability information but we put them on a secondary type
* We should probably think about how we want to expose those concepts. It seems worthwhile to batch multiple C# features together, rather than doing this piecemeal.

```cs
namespace System.Reflection;

public partial class PropertyInfo
{
    public bool IsInitOnly { get; }
}
```
