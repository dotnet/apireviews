# API Review 03/23/2023

## MemoryExtensions.TryWrite for UTF8

**Approved** | [#runtime/79376](https://github.com/dotnet/runtime/issues/79376#issuecomment-1481589975) | [Video](https://www.youtube.com/watch?v=NMYy4NSTSR0&t=0h0m0s)

* We revisited this and decided to remove the extension methods, and change to be direct static methods on System.Text.Unicode.Utf8

```C#
namespace System.Text.Unicode;

public static partial class Utf8
{
    public static bool TryWrite(Span<byte> destination, [InterpolatedStringHandlerArgument("destination")] ref TryWriteUtf8InterpolatedStringHandler handler, out int bytesWritten);
    public static bool TryWrite(Span<byte> destination, IFormatProvider? provider, [InterpolatedStringHandlerArgument("destination", "provider")] ref TryWriteUtf8InterpolatedStringHandler handler, out int bytesWritten);

    [EditorBrowsable(EditorBrowsableState.Never)]
    [InterpolatedStringHandlerAttribute]
    public ref struct TryWriteInterpolatedStringHandler
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
## Flag to disable TLS Resume/Session Caching in SslStream

**Approved** | [#runtime/78305](https://github.com/dotnet/runtime/issues/78305#issuecomment-1481619947) | [Video](https://www.youtube.com/watch?v=NMYy4NSTSR0&t=0h11m15s)

* We feel that the server side is warranted, too.
* We flipped the property from Disallow (default false) to Allow (default true)

```C#
namespace System.Net.Security;

public partial class SslClientAuthenticationOptions
{
    public bool AllowTlsResume { get; set; } = true;
}

public partial class SslServerAuthenticationOptions
{
    public bool AllowTlsResume { get; set; } = true;
}
```
## `SignatureTypeEncoder.TypedReference()`

**Approved** | [#runtime/80812](https://github.com/dotnet/runtime/issues/80812#issuecomment-1481629886) | [Video](https://www.youtube.com/watch?v=NMYy4NSTSR0&t=0h31m35s)

Looks good as proposed.

```C#
namespace System.Reflection.Metadata.Ecma335;
public readonly partial struct SignatureTypeEncoder
{
    public void TypedReference();
}
```
## Add `Stream` wrappers for memory and text-based types

**NeedsWork** | [#runtime/82801](https://github.com/dotnet/runtime/issues/82801#issuecomment-1481694752) | [Video](https://www.youtube.com/watch?v=NMYy4NSTSR0&t=0h37m49s)

There were two main approaches discussed in review.

For the first, having `FromText` and `FromData` methods on `Stream`.  For ReadOnlySequence this would either be to move the type down, or to defer it to the proposed static-extensions C# feature (to allow a higher layer to project a `static Stream.FromData(ReadOnlySequence<byte>)` down).

The second was to see if MemoryStream can be changed (without significant penalty) to work off of Memory/ReadOnlyMemory. If it can, then we'd move `FromText` to something off of System.Text.Encoding (and we'd do something like `AsStream()` for ReadOnlySequence).

The second seemed to have a bit more favor, but requires further investigation, so classifying this as needs-work.

```C#
// It will be in a new assembly with a dependency on System.Memory.dll and System.Runtime.dll
namespace System.IO
{
    public partial class StreamFactory
    {
        public static Stream FromText(string text, Encoding encoding);
        public static Stream FromText(Memory<char> text, Encoding encoding);
        public static Stream FromText(ReadOnlyMemory<char> text, Encoding encoding);

        public static Stream FromData(Memory<byte> data);
        public static Stream FromData(ReadOnlyMemory<byte> data);
        public static Stream FromData(ReadOnlySequence<byte> data);
    }
}
```
## Vector128/256 AddSaturate/RemoveSaturate

**Approved** | [#runtime/82559](https://github.com/dotnet/runtime/issues/82559#issuecomment-1481709875) | [Video](https://www.youtube.com/watch?v=NMYy4NSTSR0&t=1h12m7s)

* The `AddSaturate` name is consistent with the intrinsics
* We squared the circle to also include Vector, Vector64, and Vector512

```C#
namespace System.Runtime.Intrinsics;

public static partial class Vector
{
      public static Vector<T>   AddSaturate(Vector<T>   left, Vector<T>   right);
      public static Vector<T>   SubtractSaturate(Vector<T>   left, Vector<T>   right);
}

public static partial class Vector64
{
      public static Vector64<T>   AddSaturate(Vector64<T>   left, Vector64<T>   right);
      public static Vector64<T>   SubtractSaturate(Vector64<T>   left, Vector64<T>   right);
}

public static partial class Vector128
{
      public static Vector128<T>   AddSaturate(Vector128<T>   left, Vector128<T>   right);
      public static Vector128<T>   SubtractSaturate(Vector128<T>   left, Vector128<T>   right);
}

public static partial class Vector256
{
      public static Vector256<T>   AddSaturate(Vector256<T>   left, Vector256<T>   right);
      public static Vector256<T>   SubtractSaturate(Vector256<T>   left, Vector256<T>   right);
}

public static partial class Vector512
{
      public static Vector512<T>   AddSaturate(Vector512<T>   left, Vector512<T>   right);
      public static Vector512<T>   SubtractSaturate(Vector512<T>   left, Vector512<T>   right);
}
```
## ArgumentOutOfRangeException.ThrowIfEqual and ThrowIfNotEqual

**Approved** | [#runtime/82667](https://github.com/dotnet/runtime/issues/82667#issuecomment-1481754130) | [Video](https://www.youtube.com/watch?v=NMYy4NSTSR0&t=1h24m40s)

We had a lively discussion around the utility of the NotEqual version, which largely came down to what the message will look like. We may want to consider our consistency with passing parameter-rooted-expressions vs parameter-expressions as the name of parameters, and expand it so we can do something akin to

```
$"The {param1Name} value ({value}) is not equal to {param2Name} ({other})."
ActualValue: {value}
ParamName: {param1Name}
```

But, for now, this looks consistent with GreaterThan and LessThan.  We may want to expand all of them to take a second parameter name for the message purposes.

```C#
namespace System;

public partial class ArgumentOutOfRangeException
{
    public static void ThrowIfEqual<T>(T value, T other, [CallerArgumentExpression("value")] string? paramName = null) where T : IEquatable<T>;
    public static void ThrowIfNotEqual<T>(T value, T other, [CallerArgumentExpression("value")] string? paramName = null) where T : IEquatable<T>;
}
```
