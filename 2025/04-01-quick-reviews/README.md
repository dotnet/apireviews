# API Review 04/01/2025

## Factory methods to create immutable dictionary instances from dictionary expressions

**Approved** | [#runtime/114090](https://github.com/dotnet/runtime/issues/114090#issuecomment-2770079633) | [Video](https://www.youtube.com/watch?v=nYUIv0I2908&t=0h0m0s)

Looks good as proposed

```C#
namespace System.Collections.Immutable
{
    [CollectionBuilder(typeof(ImmutableDictionary), nameof(ImmutableDictionary.CreateRange))]
    public sealed class ImmutableDictionary<TKey, TValue> : IDictionary<TKey, TValue> //, ...
    {
        // ...
    }

    public static class ImmutableDictionary
    {
        public static ImmutableDictionary<TKey, TValue> CreateRange<TKey, TValue>(
            ReadOnlySpan<KeyValuePair<TKey, TValue>> collection)
            where TKey : notnull;

        public static ImmutableDictionary<TKey, TValue> CreateRange<TKey, TValue>(
            IEqualityComparer<TKey> keyComparer,
            ReadOnlySpan<KeyValuePair<TKey, TValue>> collection)
            where TKey : notnull;
    }
}

namespace System.Collections.Frozen
{
    [CollectionBuilder(typeof(FrozenDictionary), nameof(FrozenDictionary.Create))]
    public abstract class FrozenDictionary<TKey, TValue> : IDictionary<TKey, TValue> //, ...
    {
        // ...
    }

    public static class FrozenDictionary
    {
        public static FrozenDictionary<TKey, TValue> Create<TKey, TValue>(
            ReadOnlySpan<KeyValuePair<TKey, TValue>> collection)
            where TKey : notnull;

        public static FrozenDictionary<TKey, TValue> Create<TKey, TValue>(
            IEqualityComparer<TKey> comparer,
            ReadOnlySpan<KeyValuePair<TKey, TValue>> collection)
            where TKey : notnull;
    }
}
```
## Tensor Dimensions Refining

**Approved** | [#runtime/113863](https://github.com/dotnet/runtime/issues/113863#issuecomment-2770264568) | [Video](https://www.youtube.com/watch?v=nYUIv0I2908&t=0h9m37s)

* TensorDimensionView => TensorDimensionSpan
  * Count => Length
  * GetSlice => indexer
* And GetDimension got renamed to GetDimensionSpan

```diff
namespace System.Numerics.Tensors;

public partial interface IReadOnlyTensor<TSelf, T> : IEnumerable<T>
{
-     TSelf SliceAlongDimension(int dimension, nint index);
}

public partial interface ITensor<TSelf, T> : IEnumerable<T>
{
-     TSelf SliceAlongDimension(int dimension, nint index);
}

public sealed partial class Tensor<T>
    : ITensor<Tensor<T>, T>
{
-      public DimensionCollection Dimensions {get;}

-     public sealed class DimensionCollection
-         : System.Collections.Generic.ICollection<Tensor<T>>,
-           System.Collections.Generic.IEnumerable<Tensor<T>>,
-           System.Collections.Generic.IReadOnlyCollection<Tensor<T>>,
-           System.Collections.ICollection
-     {
-         public int Count {get;}
-         public void CopyTo(Tensor<T>[] array , int index)
-         public Enumerator GetEnumerator();

-         public struct Enumerator : IEnumerator<Tensor<T>>
-         {
            
-             internal Enumerator(Tensor<T> tensor, int dimension);

-             public bool MoveNext();

-             public void Reset();

-             public void Dispose();

-             Tensor<T> IEnumerator<Tensor<T>>.Current;

-             object? IEnumerator.Current;
-         }     
-     }
}

public readonly ref partial struct ReadOnlyTensorSpan<T>
{
-     public DimensionCollection Dimensions {get;}
    
-     public ref struct DimensionCollection
-         : System.Collections.Generic.ICollection<Tensor<T>>,
-           System.Collections.Generic.IEnumerable<Tensor<T>>,
-           System.Collections.Generic.IReadOnlyCollection<Tensor<T>>,
-           System.Collections.ICollection
-     {
-         public int Count {get;}
-         public void CopyTo(Tensor<T>[] array , int index)
-         public Enumerator GetEnumerator();

-         //All else explicitly implemented and match what Dictionary does   

-         public ref struct Enumerator : IEnumerator<Tensor<T>>
-         {
            
-             internal Enumerator(Tensor<T> tensor, int dimension);

-             public bool MoveNext();

-             public ref readonly ReadOnlyTensorSpan<T> Current;
-         }     
-     }
}

public ref struct TensorSpan<T>
{
-     public DimensionCollection Dimensions {get;}
    
-     public ref struct DimensionCollection
-         : System.Collections.Generic.ICollection<Tensor<T>>,
-           System.Collections.Generic.IEnumerable<Tensor<T>>,
-           System.Collections.Generic.IReadOnlyCollection<Tensor<T>>,
-           System.Collections.ICollection
-     {
-         public int Count {get;}
-         public void CopyTo(Tensor<T>[] array , int index)
-         public Enumerator GetEnumerator();

-         //All else explicitly implemented and match what Dictionary does   

-         public ref struct Enumerator : IEnumerator<Tensor<T>>
-         {
            
-             internal Enumerator(Tensor<T> tensor, int dimension);

-             public bool MoveNext();

-             public ref TensorSpan<T> Current;
-         }     
-     }   
}
```

```C#
namespace System.Numerics.Tensors;

public readonly ref struct TensorDimensionSpan<T>
{
    public TensorDimensionSpan(TensorSpan<T> tensor, int dimension);
    public nint Length { get; }
    public TensorSpan<T> this[nint index];
    public Enumerator GetEnumerator();

    public ref struct Enumerator
#if NET9_0_OR_GREATER
        : IEnumerator<TensorSpan<T>>
#endif
    {
        internal Enumerator(TensorSpan<T> tensor, int dimension);
        public bool MoveNext();
        public void Reset();
        public void Dispose();
        public TensorSpan<T> Current { get; }
#if NET9_0_OR_GREATER
        // This will always just throw but needs to be here.
        object? IEnumerator.Current { get; }
#endif
    }
}

public readonly ref struct ReadOnlyTensorDimensionSpan<T>
{
    public ReadOnlyTensorDimensionSpan(ReadOnlyTensorSpan<T> tensor, int dimension);
    public nint Length { get; }
    public ReadOnlyTensorSpan<T> this[nint index];
    public Enumerator GetEnumerator();

    public ref struct Enumerator
#if NET9_0_OR_GREATER
        : IEnumerator<ReadOnlyTensorSpan<T>>
#endif
    {
        internal Enumerator(ReadOnlyTensorSpan<T> tensor, int dimension);
        public bool MoveNext();
        public void Reset();
        public void Dispose();
        public ReadOnlyTensorSpan<T> Current { get; }
#if NET9_0_OR_GREATER
        // This will always just throw but needs to be here.
        object? IEnumerator.Current { get; }
#endif
    }
}

public sealed partial class Tensor<T>
{
    public TensorDimensionSpan<T> GetDimensionSpan(int dimension);
}

public sealed partial class ReadOnlyTensor<T>
{
    public ReadOnlyTensorDimensionSpan<T> GetDimensionSpan(int dimension);
}

public partial interface ITensor<T>
{
    public TensorDimensionSpan<T> GetDimensionSpan(int dimension);
}

public partial interface IReadOnlyTensor<T>
{
    public ReadOnlyTensorDimensionSpan<T> GetDimensionSpan(int dimension);
}
```
## Tensor.LengthsAsInt/Long

**Rejected** | [#runtime/111963](https://github.com/dotnet/runtime/issues/111963#issuecomment-2770309254) | [Video](https://www.youtube.com/watch?v=nYUIv0I2908&t=0h51m51s)

We're not convinced that there's enough need to justify this right now, especially as it is a one-liner (`TensorPrimitives.ConvertChecked`) to do this from the Lengths property.
## Allow the user to provide state for a ComWrappers.GetOrCreateObjectForComInstance operation

**Approved** | [#runtime/113622](https://github.com/dotnet/runtime/issues/113622#issuecomment-2770382217) | [Video](https://www.youtube.com/watch?v=nYUIv0I2908&t=1h7m42s)

* Looks good as proposed
* It was asked if userState should be generic instead of object, and the answer was "no".

```C#
namespace System.Runtime.InteropServices;

[Flags]
public enum CreatedWrapperFlags
{
    None = 0,
    TrackerObject = 0x1,
    NonWrapping = 0x2,
}

public abstract partial class ComWrappers
{
    protected virtual object? CreateObject(IntPtr externalComObject, CreateObjectFlags flags, object? userState, out CreatedWrapperFlags wrapperFlags);

    public object GetOrCreateObjectForComInstance(IntPtr externalComObject, CreateObjectFlags flags, object? userState);
}
```
## VMOVD / VMOVW

**Approved** | [#runtime/113090](https://github.com/dotnet/runtime/issues/113090#issuecomment-2770399394) | [Video](https://www.youtube.com/watch?v=nYUIv0I2908&t=1h39m12s)

* Looks good as proposed

```C#
// VMOVD xmm1, xmm2/m32
public static Vector128<uint> MoveScalar(Vector128<uint> value);

// VMOVD xmm1, xmm2/m32
public static Vector128<int> MoveScalar(Vector128<int> value);

// VMOVW xmm1, xmm2/m16
public static Vector128<ushort> MoveScalar(Vector128<ushort> value);

// VMOVW xmm1, xmm2/m16
public static Vector128<short> MoveScalar(Vector128<short> value);

// VMOVW xmm1/m16, xmm2
public static unsafe void StoreScalar(Int16* address, Vector128<Int16> source);

// VMOVW xmm1/m16, xmm2
public static unsafe void StoreScalar(UInt16* address, Vector128<UInt16> source);
```
