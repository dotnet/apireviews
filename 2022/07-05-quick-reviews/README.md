# API Review 07/05/2022

## Update to the `LibraryImport` custom marshalling shape

**Approved** | [#runtime/70859](https://github.com/dotnet/runtime/issues/70859#issuecomment-1175408309) | [Video](https://www.youtube.com/watch?v=_P6_hkQzmi0&t=0h0m0s)

* We think that forcing the MarshalUsing target type to be a static class is overly limiting, it should allow a static class (stateless) or a struct (stateful) so that someone could just implement all the patterns on the flat type, if desired.
* "Scenario" is a fairly broad term that might conflict with user types (due to namespace/using-import order resolution).  We recommend "MarshalMode"
* CustomMarshallerAttribute should expose the ctor values as get-only properties
* Replace ElementUnmanagedTypeAttribute with a ContiguousCollectionMarshallerAttribute on the marshalling type
* For the nested types that we're producing, we should name them after the MarshalMode they are representing.
* Rename NotifyInvokeSucceeded() to OnInvoked()
* ToManagedGuaranteed => ToManagedFinally()
* In the collection marshallers, such as SpanMarshaller, be consistent with `numElements` instead of mixing `numElements` and `length`.

```C#
namespace System.Runtime.InteropServices.Marshalling;

/// <summary>
/// An enumeration representing the different marshalling scenarios in our marshalling model
/// </summary>
public enum MarshalMode
{
    /// <summary>
    /// All scenarios. A marshaller specified with this scenario will be used if there is not a specific
    /// marshaller specified for a given usage scenario.
    /// </summary>
    Default,
    /// <summary>
    /// By-value and <c>in</c> parameters in managed-to-unmanaged scenarios, like P/Invoke.
    /// </summary>
    ManagedToUnmanagedIn,
    /// <summary>
    /// <c>ref</c> parameters in managed-to-unmanaged scenarios, like P/Invoke.
    /// </summary>
    ManagedToUnmanagedRef,
    /// <summary>
    /// <c>out</c> parameters in managed-to-unmanaged scenarios, like P/Invoke.
    /// </summary>
    ManagedToUnmanagedOut,
    /// <summary>
    /// By-value and <c>in</c> parameters in unmanaged-to-managed scenarios, like Reverse P/Invoke.
    /// </summary>
    UnmanagedToManagedIn,
    /// <summary>
    /// <c>ref</c> parameters in unmanaged-to-managed scenarios, like Reverse P/Invoke.
    /// </summary>
    UnmanagedToManagedRef,
    /// <summary>
    /// <c>out</c> parameters in unmanaged-to-managed scenarios, like Reverse P/Invoke.
    /// </summary>
    UnmanagedToManagedOut,
    /// <summary>
    /// Elements of arrays passed with <c>in</c> or by-value in interop scenarios.
    /// </summary>
    ElementIn,
    /// <summary>
    /// Elements of arrays passed with <c>ref</c> or passed by-value with both <see cref="InAttribute"/> and <see cref="OutAttribute" /> in interop scenarios.
    /// </summary>
    ElementRef,
    /// <summary>
    /// Elements of arrays passed with <c>out</c> or passed by-value with only <see cref="OutAttribute" /> in interop scenarios.
    /// </summary>
    ElementOut
}

/// <summary>
/// Specifies which marshaller to use for a given managed type and marshalling scenario.
/// </summary>
/// <remarks>
/// This attribute should be placed on a marshaller entry-point type.
/// </remarks>
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Struct, AllowMultiple = true)]
public sealed class CustomMarshallerAttribute : Attribute
{
    public CustomMarshallerAttribute(Type managedType, MarshalMode marshalMode, Type marshallerType);

    public Type ManagedType { get; }
    public MarshalMode MarshalMode { get; }
    public Type MarshallerType { get; }

    /// <summary>
    /// Placeholder type for generic parameter
    /// </summary>
    public struct GenericPlaceholder
    {
    }
}

/// <summary>
/// Specifies that the attributed custom marshaller marshals a linear/contiguous collection.
/// </summary>
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Struct)]
public sealed class ContiguousCollectionMarshallerAttribute : Attribute
{
}

[CustomMarshaller(typeof(string), Scenario.Default, typeof(Utf8StringMarshaller))]
[CustomMarshaller(typeof(string), Scenario.ManagedToUnmanagedIn, typeof(ManagedToUnmanagedIn))]
public static class Utf8StringMarshaller
{
    public static byte* ConvertToUnmanaged(string managed);

    public static string ConvertToManaged(byte* unmanaged);

    public static void Free(byte* unmanaged);

    public ref struct ManagedToUnmanagedIn
    {
        public static int BufferSize { get; }

        public void FromManaged(string managed, Span<byte> buffer);

        public byte* ToUnmanaged();

        public void FromUnmanaged(byte* unmanaged);

        public string ToManaged();

        public void Free();
    }
}

[CustomMarshaller(typeof(string), Scenario.Default, typeof(AnsiStringMarshaller))]
[CustomMarshaller(typeof(string), Scenario.ManagedToUnmanagedIn, typeof(ManagedToUnmanagedIn))]
public static class AnsiStringMarshaller
{
    public static byte* ConvertToUnmanaged(string managed);

    public static string ConvertToManaged(byte* unmanaged);

    public static void Free(byte* unmanaged);

    public ref struct ManagedToUnmanagedIn
    {
        public static int BufferSize { get; }

        public void FromManaged(string managed, Span<byte> buffer);

        public byte* ToUnmanaged();

        public void FromUnmanaged(byte* unmanaged);

        public string ToManaged();

        public void Free();
    }
}

[CustomMarshaller(typeof(string), Scenario.Default, typeof(BStrStringMarshaller))]
[CustomMarshaller(typeof(string), Scenario.ManagedToUnmanagedIn, typeof(ManagedToUnmanagedIn))]
public static class BStrStringMarshaller
{
    public static ushort* ConvertToUnmanaged(string managed);

    public static string ConvertToManaged(ushort* unmanaged);

    public static void Free(ushort* unmanaged);

    public ref struct ManagedToUnmanagedIn
    {
        public static int BufferSize { get; }

        public void FromManaged(string managed, Span<ushort> buffer);

        public ushort* ToUnmanaged();

        public void FromUnmanaged(ushort* unmanaged);

        public string ToManaged();

        public void Free();
    }
}

[CustomMarshaller(typeof(string), Scenario.Default, typeof(Utf16StringMarshaller))]
public static class Utf16StringMarshaller
{
    public static ushort* ConvertToUnmanaged(string managed);

    public static string ConvertToManaged(ushort* unmanaged);

    public static void Free(ushort* unmanaged);

    public static ref char GetPinnableReference(string str);
}

[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder[]), Scenario.Default, typeof(ArrayMarshaller<,>))]
[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder[]), Scenario.ManagedToUnmanagedIn, typeof(ArrayMarshaller<,>.In))]
[ContiguousCollectionMarshaller]
public unsafe static class ArrayMarshaller<T, TUnmanagedElement>
    where TUnmanagedElement : unmanaged
{
    public static byte* AllocateContainerForUnmanagedElements(T[]? managed, out int numElements);

    public static ReadOnlySpan<T> GetManagedValuesSource(T[] managed);

    public static Span<T> GetManagedValuesDestination(T[] managed);

    public static Span<TUnmanagedElement> GetUnmanagedValuesDestination(byte* nativeValue, int numElements);

    public static ReadOnlySpan<TUnmanagedElement> GetUnmanagedValuesSource(byte* nativeValue, int numElements);

    public static T[] AllocateContainerForManagedElements(byte* nativeValue, int length);
    public static void Free(byte* native);

    public unsafe ref struct ManagedToUnmanagedIn
    {
        public static int BufferSize { get; }

        public void FromManaged(T[]? array, Span<TUnmanagedElement> buffer);

        public ReadOnlySpan<T> GetManagedValuesSource();

        public Span<TUnmanagedElement> GetUnmanagedValuesDestination();

        public ref TUnmanagedElement GetPinnableReference();

        public byte* ToUnmanaged();

        public void Free();

        public static ref T GetPinnableReference(T[] managed);
    }
}

[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder*[]), Scenario.Default, typeof(PointerArrayMarshaller<,>))]
[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder*[]), Scenario.ManagedToUnmanagedIn, typeof(PointerArrayMarshaller<,>.In))]
[ContiguousCollectionMarshaller]
public unsafe static class PointerArrayMarshaller<T, TUnmanagedElement>
    where T : unmanaged
    where TUnmanagedElement : unmanaged
{
    public static byte* AllocateContainerForUnmanagedElements(T*[]? managed, out int numElements);

    public static ReadOnlySpan<IntPtr> GetManagedValuesSource(T*[] managed);

    public static Span<IntPtr> GetManagedValuesDestination(T*[] managed);

    public static Span<TUnmanagedElement> GetUnmanagedValuesDestination(byte* nativeValue, int numElements);

    public static ReadOnlySpan<TUnmanagedElement> GetUnmanagedValuesSource(byte* nativeValue, int numElements);

    public static T*[] AllocateContainerForManagedElements(byte* nativeValue, int length);
    public static void Free(byte* native);

    public unsafe ref struct ManagedToUnmanagedIn
    {
        public static int BufferSize { get; }

        public void FromManaged(T*[]? array, Span<TUnmanagedElement> buffer);

        public ReadOnlySpan<IntPtr> GetManagedValuesSource();

        public Span<TUnmanagedElement> GetUnmanagedValuesDestination();

        public ref TUnmanagedElement GetPinnableReference();

        public byte* ToUnmanaged();

        public void Free();

        public static ref T* GetPinnableReference(T*[] managed);
    }
}

[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder*[]), Scenario.Default, typeof(SpanMarshaller<,>))]
[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder*[]), Scenario.ManagedToUnmanagedIn, typeof(SpanMarshaller<,>.In))]
[ContiguousCollectionMarshaller]
public unsafe static class SpanMarshaller<T, TUnmanagedElement>
    where TUnmanagedElement : unmanaged
{
    public static byte* AllocateContainerForUnmanagedElements(Span<T> managed, out int numElements);

    public static ReadOnlySpan<T> GetManagedValuesSource(Span<T> managed);

    public static Span<T> GetManagedValuesDestination(Span<T> managed);

    public static Span<TUnmanagedElement> GetUnmanagedValuesDestination(byte* nativeValue, int numElements);

    public static ReadOnlySpan<TUnmanagedElement> GetUnmanagedValuesSource(byte* nativeValue, int numElements);

    public static Span<T> AllocateContainerForManagedElements(byte* nativeValue, int length);
    public static void Free(byte* native);

    public unsafe ref struct ManagedToUnmanagedIn
    {
        public static int BufferSize { get; }

        public void FromManaged(Span<T> array, Span<TUnmanagedElement> buffer);

        public ReadOnlySpan<T> GetManagedValuesSource();

        public Span<TUnmanagedElement> GetUnmanagedValuesDestination();

        public ref TUnmanagedElement GetPinnableReference();

        public byte* ToUnmanaged();

        public void Free();

        public static ref T GetPinnableReference(Span<T> managed);
    }
}


[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder*[]), Scenario.ManagedToUnmanagedIn, typeof(SpanMarshaller<,>.ManagedToUnmanaged))]
[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder*[]), Scenario.UnmanagedToManagedOut, typeof(SpanMarshaller<,>.UnmanagedToManaged))]
[ContiguousCollectionMarshaller]
public unsafe static class ReadOnlySpanMarshaller<T, TUnmanagedElement>
    where TUnmanagedElement : unmanaged
{
    public unsafe ref struct ManagedToUnmanagedIn
    {
        public static int BufferSize { get; }

        public void FromManaged(ReadOnlySpan<T> array, Span<TUnmanagedElement> buffer);

        public ReadOnlySpan<T> GetManagedValuesSource();

        public Span<TUnmanagedElement> GetUnmanagedValuesDestination();

        public ref TUnmanagedElement GetPinnableReference();

        public byte* ToUnmanaged();

        public void Free();

        public static ref T GetPinnableReference(ReadOnlySpan<T> managed);
    }

    public unsafe static class UnmanagedToManagedOut
    {
        public static byte* AllocateContainerForUnmanagedElements(ReadOnlySpan<T> managed, out int numElements);

        public static ReadOnlySpan<T> GetManagedValuesSource(ReadOnlySpan<T> managed);

        public static Span<TUnmanagedElement> GetUnmanagedValuesDestination(byte* nativeValue, int numElements);
    }
}
```

Shapes

```C#
[CustomMarshaller(typeof(TManaged), Scenario.ManagedToUnmanagedIn, typeof(ManagedToUnmanaged))]
[CustomMarshaller(typeof(TManaged), Scenario.ManagedToUnmanagedRef, typeof(Bidirectional))]
[CustomMarshaller(typeof(TManaged), Scenario.ManagedToUnmanagedOut, typeof(UnmanagedToManaged))]
[CustomMarshaller(typeof(TManaged), Scenario.UnmanagedToManagedIn, typeof(UnmanagedToManaged))]
[CustomMarshaller(typeof(TManaged), Scenario.UnmanagedToManagedRef, typeof(Bidirectional))]
[CustomMarshaller(typeof(TManaged), Scenario.UnmanagedToManagedOut, typeof(ManagedToUnmanaged))]
[CustomMarshaller(typeof(TManaged), Scenario.ElementIn, typeof(Element))]
[CustomMarshaller(typeof(TManaged), Scenario.ElementRef, typeof(Element))]
[CustomMarshaller(typeof(TManaged), Scenario.ElementOut, typeof(Element))]
public unsafe static class Marshaller // Must be static class
{
    public unsafe ref struct ManagedToUnmanaged
    {
        public void FromManaged(TManaged managed) => throw null; // Optional caller allocation, Span<T>
        public ref byte GetPinnableReference() => throw null; // Optional, allowed on all "stateful" shapes
        public TUnmanaged ToUnmanaged() => throw null;
        public void Free() => throw null; // Should not throw exceptions. Use Free instead of Dispose to avoid issues with marshallers needing to follow the Dispose pattern guidance.

        // We will pattern-match this method and use it when available.
        // Canonical case is for GC.KeepAlive().
        public void OnInvoked() => throw null;
    }

    public unsafe ref struct Bidirectional
    {
        public void FromManaged(TManaged managed) => throw null; // Optional caller allocation, Span<T>
        public ref byte GetPinnableReference() => throw null; // Optional, allowed on all "stateful" shapes.
        public TUnmanaged ToUnmanaged() => throw null; // Should not throw exceptions.
        public void FromUnmanaged(TUnmanaged native) => throw null; // Should not throw exceptions.
        public TManaged ToManaged() => throw null;
        // ToManagedFinally requests the generator to move the unmarshalling method calls to be called as part of the GuaranteedUnmarshalling stage.
        // The generator will pattern-match this method as an alternative to ToManaged().
        public TManaged ToManagedFinally() => throw null;
        public void Free() => throw null; // Should not throw exceptions.

        public void OnInvoked() => throw null; // See notes in ManagedToUnmanaged on usage.
    }

    public unsafe struct UnmanagedToManaged
    {
        public void FromUnmanaged(TUnmanaged native) => throw null;
        public TManaged ToManaged() => throw null; // Should not throw exceptions.
        public void Free() => throw null; // Should not throw exceptions.
    }

    // Currently only support stateless. May support stateful in the future
    public static class Element
    {
        // Defined by public interface IMarshaller<TManaged, TUnmanaged> where TUnmanaged : unmanaged
        public static TUnmanaged ConvertToUnmanaged(TManaged managed) => throw null;
        public static TManaged ConvertToManaged(TUnmanaged native) => throw null;
        public static void Free(TUnmanaged native) => throw null; // Should not throw exceptions.
    }
}


[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder[]), Scenario.Default, typeof(ArrayMarshaller<,>))]
[CustomMarshaller(typeof(CustomMarshallerAttribute.GenericPlaceholder[]), Scenario.ManagedToUnmanagedIn, typeof(ArrayMarshaller<,>.In))]
public unsafe static class ArrayMarshaller<T, [ElementUnmanagedType] TUnmanagedElement> // Must be static class
    where TUnmanagedElement : unmanaged
{
    // Defined by public interface IMarshaller<TManaged, TUnmanaged> where TUnmanaged : unmanaged
    public static byte* AllocateContainerForUnmanagedElements(T[]? managed, out int numElements)
    {
        if (managed is null)
        {
            numElements = 0;
            return null;
        }
        numElements = managed.Length;
        return (byte*)Marshal.AllocCoTaskMem(checked(sizeof(TUnmanagedElement) * numElements));
    }

    public static ReadOnlySpan<T> GetManagedValuesSource(T[] managed) => managed;

    public static Span<T> GetManagedValuesDestination(T[] managed) => managed;

    public static Span<TUnmanagedElement> GetUnmanagedValuesDestination(byte* nativeValue, int numElements)
        => new Span<TUnmanagedElement>(nativeValue, numElements);

    public static ReadOnlySpan<TUnmanagedElement> GetUnmanagedValuesSource(byte* nativeValue, int numElements)
        => new Span<TUnmanagedElement>(nativeValue, numElements);

    public static T[] AllocateContainerForManagedElements(byte* nativeValue, int length) => new T[length];
    public static void Free(byte* native) => Marshal.FreeCoTaskMem((IntPtr)native);

    public unsafe ref struct In
    {
        private T[]? _managedArray;
        private IntPtr _allocatedMemory;
        private Span<TUnmanagedElement> _span;

        public static int BufferSize { get; } = 20;

        /// <summary>
        /// Initializes a new instance of the <see cref="ArrayMarshaller{T}"/>.
        /// </summary>
        /// <param name="array">Array to be marshalled.</param>
        /// <param name="buffer">Buffer that may be used for marshalling.</param>
        /// <param name="sizeOfUnmanagedElement">Size of the native element in bytes.</param>
        /// <remarks>
        /// The <paramref name="buffer"/> must not be movable - that is, it should not be
        /// on the managed heap or it should be pinned.
        /// <seealso cref="CustomTypeMarshallerFeatures.CallerAllocatedBuffer"/>
        /// </remarks>
        public void FromManaged(T[]? array, Span<TUnmanagedElement> buffer)
        {
            _allocatedMemory = default;
            if (array is null)
            {
                _managedArray = null;
                _span = default;
                return;
            }

            _managedArray = array;

            // Always allocate at least one byte when the array is zero-length.
            int bufferSize = checked(array.Length * sizeof(TUnmanagedElement));
            int spaceToAllocate = Math.Max(bufferSize, 1);
            if (spaceToAllocate <= buffer.Length)
            {
                _span = buffer[0..spaceToAllocate];
            }
            else
            {
                _allocatedMemory = Marshal.AllocCoTaskMem(spaceToAllocate);
                _span = new Span<TUnmanagedElement>((void*)_allocatedMemory, spaceToAllocate);
            }
        }

        /// <summary>
        /// Gets a span that points to the memory where the managed values of the array are stored.
        /// </summary>
        /// <returns>Span over managed values of the array.</returns>
        /// <remarks>
        /// <seealso cref="CustomTypeMarshallerDirection.In"/>
        /// </remarks>
        public ReadOnlySpan<T> GetManagedValuesSource() => _managedArray;

        /// <summary>
        /// Returns a span that points to the memory where the native values of the array should be stored.
        /// </summary>
        /// <returns>Span where native values of the array should be stored.</returns>
        public Span<TUnmanagedElement> GetUnmanagedValuesDestination() => _span;

        /// <summary>
        /// Returns a reference to the marshalled array.
        /// </summary>
        public ref TUnmanagedElement GetPinnableReference() => ref MemoryMarshal.GetReference(_span);

        /// <summary>
        /// Returns the native value representing the array.
        /// </summary>
        public byte* ToUnmanaged() => (byte*)Unsafe.AsPointer(ref GetPinnableReference());

        /// <summary>
        /// Frees native resources.
        /// </summary>
        public void Free()
        {
            Marshal.FreeCoTaskMem(_allocatedMemory);
        }

        public static ref T GetPinnableReference(T[] managed) // Optional, allowed on all shapes
        {
            if (managed is null)
            {
                return ref Unsafe.NullRef<T>();
            }
            return ref MemoryMarshal.GetArrayDataReference(managed);
        }
    }

    public unsafe ref struct Out
    {
        private T[]? _managedArray;
        private IntPtr _allocatedMemory;
        private Span<TUnmanagedElement> _span;

        /// <summary>
        /// Gets a span that points to the memory where the unmarshalled managed values of the array should be stored.
        /// </summary>
        /// <param name="length">Length of the array.</param>
        /// <returns>Span where managed values of the array should be stored.</returns>
        public Span<T> GetManagedValuesDestination(int length) => _allocatedMemory == IntPtr.Zero ? null : _managedArray = new T[length];

        /// <summary>
        /// Returns a span that points to the memory where the native values of the array are stored after the native call.
        /// </summary>
        /// <param name="length">Length of the array.</param>
        /// <returns>Span over the native values of the array.</returns>
        public ReadOnlySpan<TUnmanagedElement> GetUnmanagedValuesSource(int length)
        {
            if (_allocatedMemory == IntPtr.Zero)
                return default;

            return _span = new Span<TUnmanagedElement>((void*)_allocatedMemory, length);
        }

        /// <summary>
        /// Returns a reference to the marshalled array.
        /// </summary>
        public ref TUnmanagedElement GetPinnableReference() => ref MemoryMarshal.GetReference(_span);

        /// <summary>
        /// Sets the native value representing the array.
        /// </summary>
        /// <param name="value">The native value.</param>
        public void FromUnmanaged(byte* value)
        {
            _allocatedMemory = (IntPtr)value;
        }

        /// <summary>
        /// Returns the managed array.
        /// </summary>
        public T[]? ToManaged() => _managedArray;

        /// <summary>
        /// Frees native resources.
        /// </summary>
        public void Free()
        {
            Marshal.FreeCoTaskMem(_allocatedMemory);
        }
    }
}
```
