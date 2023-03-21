# API Review 03/21/2023

## MemoryExtensions.TryWrite for UTF8

**Approved** | [#runtime/79376](https://github.com/dotnet/runtime/issues/79376#issuecomment-1478318474) | [Video](https://www.youtube.com/watch?v=hyyNUh4s3Qo&t=0h0m0s)

* We should rename the method to make it clear that the buffer receives the UTF8 encoding, not the UTF16 encoding
* We're OK with supporting both UTF8 and UTF16, with UTF16 potentially generating a warning, telling the user that should use a u8 literal to avoid the transcoding
* Will C# support interpolating strings with the u8 suffix? Seems like it should? @dotnet/ldm
* We should consider making it work that one removes the `$` that the code still compiles, but this would apply for the existing UTF16 version as well

```C#
namespace System;

public static partial class MemoryExtensions
{
    public static bool TryWriteUtf8(this Span<byte> destination, [InterpolatedStringHandlerArgument("destination")] ref TryWriteUtf8InterpolatedStringHandler handler, out int bytesWritten);
    public static bool TryWriteUtf8(this Span<byte> destination, IFormatProvider? provider, [InterpolatedStringHandlerArgument("destination", "provider")] ref TryWriteUtf8InterpolatedStringHandler handler, out int bytesWritten);

    [EditorBrowsable(EditorBrowsableState.Never)]
    [InterpolatedStringHandlerAttribute]
    public ref struct TryWriteUtf8InterpolatedStringHandler
    {
        public TryWriteInterpolatedStringHandler(int literalLength, int formattedCount, Span<byte> utf8Destination, out bool shouldAppend);
        public TryWriteInterpolatedStringHandler(int literalLength, int formattedCount, Span<byte> utf8Destination, IFormatProvider? provider, out bool shouldAppend);

        public bool AppendLiteral(string value);
        // public bool AppendLiteral(ReadOnlySpan<byte> value); // if the C# compiler supports interpolation with u8 literals

        public bool AppendFormatted(scoped ReadOnlySpan<char> value);
        public bool AppendFormatted(scoped ReadOnlySpan<char> value, int alignment = 0, string? format = null);

        public bool AppendFormatted(scoped ReadOnlySpan<byte> utf8Value);
        public bool AppendFormatted(scoped ReadOnlySpan<byte> utf8Value, int alignment = 0, string? format = null);

        public bool AppendFormatted<T>(T value);
        public bool AppendFormatted<T>(T value, string? format);
        public bool AppendFormatted<T>(T value, int alignment);
        public bool AppendFormatted<T>(T value, int alignment, string? format);

        public bool AppendFormatted(object? value, int alignment = 0, string? format = null);

        public bool AppendFormatted(string? value);
        public bool AppendFormatted(string? value, int alignment = 0, string? format = null);
    }
}
```
## Support filtering property names on dictionary deserialization.

**Approved** | [#runtime/79059](https://github.com/dotnet/runtime/issues/79059#issuecomment-1478365634) | [Video](https://www.youtube.com/watch?v=hyyNUh4s3Qo&t=0h28m55s)

* It seems odd that we'd use UTF8 in this API, given that `JsonNamingPolicy` doesn't
    - Since this is an opt-in policy we're OK with allocating for cases where the keys aren't strings
* We should name the method `ShouldIgnoreKey`
* We will apply this policy before `DictionaryKeyPolicy` and we will run it for both string and non-string key types.

```C#
namespace System.Text.Json.Serialization;

public abstract class JsonDictionaryKeyFilter
{
    public static JsonDictionaryKeyFilter IgnoreMetadataNames { get; }
    public abstract bool ShouldIgnoreKey(string key);
}
```

```C#
namespace System.Text.Json;

public partial class JsonSerializerOptions
{
    public JsonDictionaryKeyFilter? DictionaryKeyFilter { get; set; } = null;
}
```
## APIs for source generating interop stubs for unmanaged virtual function tables

**NeedsWork** | [#runtime/80204](https://github.com/dotnet/runtime/issues/80204#issuecomment-1478403421) | [Video](https://www.youtube.com/watch?v=hyyNUh4s3Qo&t=0h59m22s)

From #79121,

```csharp
/// <summary>
/// This interface allows another interface to define that it represents a managed projection of an unmanaged interface from some unmanaged type system and supports passing managed implementations of unmanaged interfaces to unmanaged code.
/// </summary>
public interface IUnmanagedInterfaceType<TSelf> where TSelf: IUnmanagedInterfaceType<TSelf>
{
    /// <summary>
    /// Get a pointer to the virtual method table of managed implementations of the unmanaged interface type.
    /// </summary>
    /// <returns>A pointer to the virtual method table of managed implementations of the unmanaged interface type</returns>
    /// <remarks>
    /// Implementation will be provided by a source generator if not explicitly implemented.
    /// This property can return <c>null</c>. If it does, then the interface is not supported for passing managed implementations to unmanaged code.
    /// </remarks>
    public abstract static unsafe void** ManagedVirtualMethodTable { get; }
}
````
