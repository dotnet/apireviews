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

**NeedsWork** | [#runtime/80204](https://github.com/dotnet/runtime/issues/80204#issuecomment-1478424502) | [Video](https://www.youtube.com/watch?v=hyyNUh4s3Qo&t=0h59m22s)

* The default is `ExceptionMarshalling.Custom`
    - If the developer doesn't specify a behavior, should we give a warning?
    - If the developer doesn't specify a behavior, what's the default? Swallow seems problematic. It seems a clean crash would be preferable.
* Should we have a `ExceptionFailFastMarshaller`?
    - Or should this be an enum member? If that's the default (`0` member) then this would nicely answer "what's the default behavior".
* Should we add an SEH marshaller for Windows?
* We should rename the marshallers by adding `As`
* @AaronRobinsonMSFT would prefer to wait a bit to see whether we actually need it

```C#
namespace System.Runtime.InteropServices.Marshalling;

public interface IUnmanagedObjectUnwrapper
{
    public static abstract object GetObjectForUnmanagedWrapper(void* ptr);
}

public interface IUnmanagedInterfaceType<TSelf>
    where TSelf: IUnmanagedInterfaceType<TSelf>
{
    public abstract static unsafe void** ManagedVirtualMethodTable { get; }
}

[AttributeUsage(AttributeTargets.Interface)]
public class UnmanagedObjectUnwrapperAttribute<TMapper> : Attribute
    where TMapper : IUnmanagedObjectUnwrapper
{
}

public sealed unsafe class ComWrappersUnwrapper : IUnmanagedObjectUnwrapper
{
    public static object GetObjectForUnmanagedWrapper(void* ptr);
}

[CustomMarshaller(typeof(Exception), MarshalMode.UnmanagedToManagedOut, typeof(ExceptionHResultMarshaller<>))]
public static class ExceptionAsHResultMarshaller<T>
    where T : unmanaged, INumber<T>
{
    public static T ConvertToUnmanaged(Exception e);
}

[CustomMarshaller(typeof(Exception), MarshalMode.UnmanagedToManagedOut, typeof(ExceptionNaNMarshaller<>))]
public static class ExceptionAsNaNMarshaller<T>
    where T : unmanaged, IFloatingPointIeee754<T>
{
    public static T ConvertToUnmanaged(Exception e);
}

[CustomMarshaller(typeof(Exception), MarshalMode.UnmanagedToManagedOut, typeof(ExceptionDefaultMarshaller<>))]
public static class ExceptionAsDefaultMarshaller<T>
    where T : unmanaged
{
    public static T ConvertToUnmanaged(Exception e);
}

[CustomMarshaller(typeof(Exception), MarshalMode.UnmanagedToManagedOut, typeof(SwallowExceptionMarshaller))]
public static class ExceptionAsVoidMarshaller
{
    public static void ConvertToUnmanaged(Exception e);
}

public enum ExceptionMarshalling
{
    Custom = 0,
    Com = 1
}

public enum MarshalDirection
{
    ManagedToUnmanaged = 0,
    UnmanagedToManaged = 1,
    Bidirectional = 2
}

[AttributeUsage(AttributeTargets.Method, AllowMultiple = false)]
public sealed class VirtualMethodIndexAttribute : Attribute
{
    public VirtualMethodIndexAttribute(int index);

    public int Index { get; }

    public bool ImplicitThisParameter { get; set; } = true;
    public StringMarshalling StringMarshalling { get; set; }
    public Type? StringMarshallingCustomType { get; set; }
    public bool SetLastError { get; set; }
    
    public MarshalDirection Direction { get; set; } = MarshalDirection.Bidirectional;

    public ExceptionMarshalling ExceptionMarshalling { get; set; }
    public Type? ExceptionMarshallingCustomType { get; set; }
}
```
