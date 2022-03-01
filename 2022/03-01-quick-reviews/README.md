# API Review 03/01/2022

## Attributes and Analyzer Diagnostics to support custom struct marshalling in the DllImport Source Generator

**Approved** | [#runtime/46838](https://github.com/dotnet/runtime/issues/46838#issuecomment-1055795416) | [Video](https://www.youtube.com/watch?v=5PprVe5nX9U&t=0h0m0s)

* We changed all of the properties in the "shapes" to be methods, and made up names that made sense with that conversion.
* ElementIndirectionLevel => ElementIndirectionDepth for clarity on if 0 is the collection type (yes) or the element type (no).
* We dropped RequiresStackBuffer
* We renamed CustomTypeMarshallerKind.SpanCollection to CustomTypeMarshallerKind.LinearCollection to avoid a misunderstanding of "a collection of spans"
* We discussed adding a CustomTypeMarshallerDirection/CustomTypeMarshallerFeatures flags enum to help be declarative on what sub-shape is desired

```C#
namespace System.Runtime.InteropServices
{
    /// <summary>
    /// Indicates the default marshalling when the attributed type is used in source-generated interop.
    /// </summary>
    [AttributeUsage(AttributeTargets.Struct | AttributeTargets.Class | AttributeTargets.Type | AttributeTargets.Delegate)]
    public sealed class NativeMarshallingAttribute : Attribute
    {
        /// <summary>
        /// Initializes a new instance of the <see cref="NativeMarshallingAttribute"/>
        /// </summary>
        /// <param name="nativeType">Type that should be used when marshalling the attributed type. The type must not require marshalling.</param>
        public NativeMarshallingAttribute(Type nativeType) {}
    }


    /// <summary>
    /// Indicates the marshalling to use in source-generated interop.
    /// </summary>
    [AttributeUsage(AttributeTargets.Parameter | AttributeTargets.ReturnValue, AllowMultiple = true)]
    public sealed class MarshalUsingAttribute : Attribute
    {
        /// <summary>
        /// Initializes a new instance of the <see cref="MarshalUsingAttribute"/>
        /// </summary>
        public MarshalUsingAttribute() {}

        /// <summary>
        /// Initializes a new instance of the <see cref="MarshalUsingAttribute"/>
        /// </summary>
        /// <param name="nativeType">Type that should be used when marshalling the attributed parameter or field. The type must not require marshalling.</param>
        public MarshalUsingAttribute(Type nativeType) {}

        /// <summary>
        /// Type that should be used when marshalling the attributed parameter, field or, when ElementIndirectionLevel is specified, collection element on the attributed item.
        /// </summary>
        public Type? NativeType { get; }

        /// <summary>
        /// Name of the parameter that contains the number of native collection elements. A value of <see cref="ReturnsCountValue"/> indicates the return value of the p/invoke should be used.
        /// </summary>
        public string CountElementName { get; set; }

        /// <summary>
        /// Number of elements in the native collection.
        /// </summary>
        public int ConstantElementCount { get; set; }

        /// <summary>
        /// The degree of indirection in a collection to which this attribute should apply.
        /// </summary>
        public int ElementIndirectionDepth { get; set; }

        /// <summary>
        /// When returned by <see cref="CountElementName" />, indicates the return value of the p/invoke should be used as the count.
        /// </summary>
        public const string ReturnsCountValue = "return-value";
    }

    /// <summary>
    /// Indicates this type is a marshaller type to be used in source-generated interop.
    /// </summary>
    [AttributeUsage(AttributeTargets.Struct)]
    public sealed class CustomTypeMarshallerAttribute : Attribute
    {
        public CustomTypeMarshallerAttribute(Type managedType, CustomTypeMarshallerKind marshallerKind = CustomTypeMarshallerKind.Value)
        {
            ManagedType = managedType;
            MarshallerKind = marshallerKind;
        }

        /// <summary>
        /// The managed type that the attributed type is a marshaller for.
        /// </summary>
        public Type ManagedType { get; }

        /// <summary>
        /// The expected shape of this marshaller type
        /// </summary>
        public CustomTypeMarshallerKind MarshallerKind { get; }

        /// <summary>
        /// The size of the caller-allocated buffer in scenarios where using caller-allocated buffer is support.
        /// </summary>
        public int BufferSize { get; set; }

        /// <summary>
        /// This type is used as a placeholder for the first generic parameter when generic parameters cannot be used
        /// to identify the managed type (i.e. when the marshaller type is generic over T and the managed type is T[])
        /// </summary>
        public struct GenericPlaceholder
        {
        }
    }

    /// <summary>
    /// The kind (or shape) of the custom marshaller type.
    /// </summary>
    public enum CustomTypeMarshallerKind
    {
        /// <summary>
        /// This marshaller represents marshalling a single value.
        /// </summary>
        Value,

        /// <summary>
        /// This marshaller represents marshalling a Span-like collection of values
        /// </summary>
        LinearCollection,
    }
}
````

And for the shapes:

```C#
// Shape for Value kind
[CustomTypeMarshaller(typeof(ManagedType))]
public struct Marshaller
{
    // Support for marshalling from managed to native.
    // Required if ToManaged() is not defined.
    // May be omitted if marshalling from managed to native is not required.
    public Marshaller(ManagedType managed) {}

    // Optional.
    // Support for marshalling from managed to native using a caller-allocated buffer
    // Requires the BufferSize field to be set on the CustomTypeMarshaller attribute.
    public Marshaller(ManagedType managed, Span<byte> buffer) {}

    // Support for marshalling from native to managed.
    // Required if constructor taking TManaged is not defined.
    // May be omitted if marshalling from native to managed is not required.
    public ManagedType ToManaged() {}

    // Optional.
    // If defined, the return value will be passed to native instead of the Marshaller itself
    // NativeType must not require marshalling
    public NativeType ToNativeValue();

    // Optional
    // If defined, will be called when marshalling from native to managed.
    // NativeType must not require marshalling, and must match ToNativeValue (if it exists)
    public void FromNativeValue(NativeType nativeValue);

    // Optional.
    // If defined and ToNativeValue or FromNativeValue is defined, will be called before Value property getter and its return value will be pinned
    public ref NativeType GetPinnableReference() {}

    // Optional.
    // Release native resources.
    public void FreeNative() {}
}

[CustomTypeMarshaller(typeof(GenericCollection<>), CustomTypeMarshallerKind.SpanCollection)]
public struct GenericContiguousCollectionMarshallerImpl<T>
{
    // Support for marshalling from native to managed.
    // May be omitted if marshalling from native to managed is not required.
    public GenericContiguousCollectionMarshallerImpl(int nativeSizeOfElement);

    // Support for marshalling from managed to native.
    // May be omitted if marshalling from managed to native is not required.
    public GenericContiguousCollectionMarshallerImpl(GenericCollection<T> collection, int nativeSizeOfElement);

    // Optional.
    // Support for marshalling from managed to native using a caller-allocated buffer
    // Requires the BufferSize field to be set on the CustomTypeMarshaller attribute.
    public GenericContiguousCollectionMarshallerImpl(GenericCollection<T> collection, Span<byte> buffer, int nativeSizeOfElement);


    public Span<ManagedType> GetManagedValuesDestination(int length);
    public ReadOnlySpan<ManagedType> GetManagedValuesSource();

    public ReadOnlySpan<byte> GetNativeValuesSource(int length);
    public Span<byte> GetNativeValuesDestination();

    // Required.
    // NativeType must not require marshalling
    public NativeType ToNativeValue();

    // Required.
    // NativeType must not require marshalling, and must match ToNativeValue (if it exists)
    public void FromNativeValue(NativeType nativeValue);

    // Support for marshalling from native to managed.
    // Required if constructor taking TManaged is not defined.
    // May be omitted if marshalling from native to managed is not required.
    public ManagedType ToManaged() {}

    // Optional.
    // If defined and ToNativeValue or FromNativeValue is defined, will be called before Value property getter and its return value will be pinned
    public ref NativeType GetPinnableReference() {}

    // Optional.
    // Release native resources.
    public void FreeNative() {}
}
```
## Let consumers of MemoryCache access metrics

**Approved** | [#runtime/50406](https://github.com/dotnet/runtime/issues/50406#issuecomment-1055813163) | [Video](https://www.youtube.com/watch?v=5PprVe5nX9U&t=1h38m56s)

* We changed the extension method to an instance method (not virtual at this time)
* We changed MemoryCacheStatistics to readonly with get+init
* We renamed the properties on MemoryCacheStatistics to reduce redundancy, and then to enhance clarity.

```C#
namespace Microsoft.Extensions.Caching.Memory
{
    public partial class MemoryCache
    {
        public MemoryCacheStatistics GetCurrentStatistics() { throw null; }
    }

    public partial readonly struct MemoryCacheStatistics
    {
        public long TotalRequests { get; init; } 
        public long TotalHits { get; init; }
        public long CurrentEntryCount { get; init; }
    }
}
```
