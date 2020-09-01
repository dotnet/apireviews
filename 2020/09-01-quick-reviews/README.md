# Quick Reviews 09/01/2020

## Add System.BinaryData

**NeedsWork** | [#runtime/41686](https://github.com/dotnet/runtime/issues/41686#issuecomment-685061045) | [Video](https://www.youtube.com/watch?v=1p9HLT6O7G8&t=0h0m0s)

* The type is very opinionated in order to achieve usability:
    - Serialization is done with `System.Text.Json`, with specific options
    - String encoding is always UTF8
* We're concerned with hard coding the type to `System.Text.Json`, but we acknowledge that defaults are very valuable. If we need to support other serializers, then we can add an enum
    - We could decide to make the constructor that performs JSON serialization as a factory method. Combined with a hypothetical feature `extension statics` we could make the type more low level than serialization
    - An alternative design is to replace constructors and factories with instance members (`SetBytes`, `SetObject`) which will throw when the underlying array is non-null. That prevents mutation and uses a simple pattern `var x = new BinaryData(); x.SetBytes(...);`. And on top, we can use regular extension methods to split out JSON serialization.
* This type can't live in `corlib` because it would depend on `System.Text.Json` (and whatever future serializers it needs to support). Hence, most areas in the BCL wouldn't be able to take `BinaryData`, but technologies above the BCL could (`Microsoft.Extensions`, ASP.NET Core, Azure SDK etc)
* Thus, we shouldn't think of this as an exchange type in the BCL. We already have exchange types; the point of this type is unify them by providing conversions; the point isn't to provide yet another type. Thus, we don't see the need to, for example, provide new virtual methods on `Stream`.

```C#
namespace System
{
    public readonly struct BinaryData
    {
        public BinaryData(ReadOnlySpan<byte> data);
        public BinaryData(byte[] data);
        public BinaryData(object jsonSerializable, Type? type = null);
        public BinaryData(ReadOnlyMemory<byte> data);
        public BinaryData(string data);
        public static BinaryData FromBytes(ReadOnlyMemory<byte> data);
        public static BinaryData FromBytes(ReadOnlySpan<byte> data);
        public static BinaryData FromBytes(byte[] data);
        public static BinaryData FromObject<T>(T jsonSerializable, CancellationToken cancellationToken = default);
        public static Task<BinaryData> FromObjectAsync<T>(T jsonSerializable, CancellationToken cancellationToken = default);
        public static BinaryData FromStream(Stream stream);
        public static Task<BinaryData> FromStreamAsync(Stream stream, CancellationToken cancellationToken = default);
        public static BinaryData FromString(string data);
        public static implicit operator ReadOnlyMemory<byte>(BinaryData data);
        public ReadOnlyMemory<byte> ToBytes();
        public T ToObject<T>(CancellationToken cancellationToken = default);
        public ValueTask<T> ToObjectAsync<T>(CancellationToken cancellationToken = default);
        public Stream ToStream();
        [EditorBrowsable(EditorBrowsableState.Never)]
        public override bool Equals(object? obj);
        [EditorBrowsable(EditorBrowsableState.Never)]
        public override int GetHashCode();
        public override string ToString();
    }
}
```

## Consider adding more Half methods to BitConverter

**Approved** | [#runtime/41318](https://github.com/dotnet/runtime/issues/41318#issuecomment-685063700) | [Video](https://www.youtube.com/watch?v=1p9HLT6O7G8&t=1h36m56s)

* Looks good as proposed

```C#
namespace System
{
    public class BitConverter
    {
        public static Half ToHalf(byte[] value, int startIndex);
        public static Half ToHalf(ReadOnlySpan<byte> value);
        public static byte[] GetBytes(Half value);
        public static bool TryWriteBytes(Span<byte> destination, Half value);
    }
}
```

