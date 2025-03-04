# API Review 03/04/2025

## Non-generic Tensor base type

**Approved** | [#runtime/111968](https://github.com/dotnet/runtime/issues/111968#issuecomment-2698517753) | [Video](https://www.youtube.com/watch?v=VspXm71aI5k&t=0h0m0s)

Looks good as proposed

```diff
namespace System.Numerics.Tensors;

+ public interface IReadOnlyTensor
+ {
+     bool IsEmpty { get; }
+     bool IsPinned { get; }
+     nint FlattenedLength { get; }
+     int Rank { get; }

+     [UnscopedRef]
+     ReadOnlySpan<nint> Lengths { get; }
    
+     [UnscopedRef]
+     ReadOnlySpan<nint> Strides { get; }
    
+     object this[params scoped ReadOnlySpan<nint> indexes] { get; }
+     object this[params scoped ReadOnlySpan<NIndex> indexes] { get; }
+ }

+ public interface ITensor : IReadOnlyTensor
+ {
+     object this[params scoped ReadOnlySpan<nint> indexes] { get; set; }
+     object this[params scoped ReadOnlySpan<NIndex> indexes] { get; set; }

+     bool IsReadOnly { get; }
+     void Clear();
+     void Fill(object value);
+ }

public interface IReadOnlyTensor<TSelf, T> : IReadOnlyTensor, IEnumerable<T>
    where TSelf : IReadOnlyTensor<TSelf, T>
{
-     bool IsEmpty { get; }
-     bool IsPinned { get; }
-     nint FlattenedLength { get; }
-     int Rank { get; }

-     [UnscopedRef]
-     ReadOnlySpan<nint> Lengths { get; }
    
-     [UnscopedRef]
-     ReadOnlySpan<nint> Strides { get; }
}

public interface ITensor<TSelf, T>  : ITensor, IReadOnlyTensor<TSelf, T>
        where TSelf : ITensor<TSelf, T>
{
-     bool IsReadOnly { get; }
-     void Clear();
}
```
## GetPinnedHandle for tensors

**Approved** | [#runtime/111966](https://github.com/dotnet/runtime/issues/111966#issuecomment-2698528967) | [Video](https://www.youtube.com/watch?v=VspXm71aI5k&t=0h9m56s)

Looks good as proposed

```C#
namespace System.Numerics.Tensors;

public partial interface IReadOnlyTensor
{
    MemoryHandle GetPinnedHandle();
}
```
## Tensor Operations Per Dimension

**Approved** | [#runtime/113068](https://github.com/dotnet/runtime/issues/113068#issuecomment-2698776329) | [Video](https://www.youtube.com/watch?v=VspXm71aI5k&t=0h14m51s)

* Tensor.Aggregate(Many) isn't performing the same accumulation-based logic as Enumerable.Aggregate, so it shouldn't use the same verb.
  * The logic being performed by Tensor.AggregateMany here is really just Select, where the selector reduces dimensionality.
* The "Many" here is doing the opposite of the meaning of Linq, so we should drop it.
* The delegate types were renamed to TensorScalarSelector and TensorSelector to follow up with the renames of AggregateMany and SelectMany
* SelectMany and AggregateMany (now both select) changed defaults from -1 to 0.

```C#
namespace System.Numerics.Tensors;

public partial interface IReadOnlyTensor<TSelf, T>
{
    TSelf SliceAlongDimension(int dimension, nint index);
}

public static partial class Tensor
{
    public delegate T TensorScalarSelector<T>(in ReadOnlyTensorSpan<T> tensor);
    public delegate Tensor<T> TensorSelector<T>(in ReadOnlyTensorSpan<T> tensor);

    public static Tensor<T> Select<T>(this Tensor<T> tensor, TensorScalarSelector<T> func, int dimension = 0);
    public static Tensor<T> Select<T>(this Tensor<T> tensor, TensorSelector<T> func, int dimension = 0);

    public static Tensor<T> ToTensor<T>(this IEnumerable<Tensor<T>> source);
}

public sealed class Tensor<T>
    : ITensor<Tensor<T>, T>
{
    public DimensionCollection Dimensions {get;}
    
    public sealed class DimensionCollection
        : System.Collections.Generic.ICollection<Tensor<T>>,
          System.Collections.Generic.IEnumerable<Tensor<T>>,
          System.Collections.Generic.IReadOnlyCollection<Tensor<T>>,
          System.Collections.ICollection
    {
        public int Count {get;}
        public void CopyTo(Tensor<T>[] array , int index);
        public Enumerator GetEnumerator();

        //All else explicitly implemented and match what Dictionary does   

        public struct Enumerator : IEnumerator<Tensor<T>>
        {
            internal Enumerator(Tensor<T> tensor, int dimension);
            public bool MoveNext();
            public void Reset();
            public void Dispose();
            Tensor<T> IEnumerator<Tensor<T>>.Current {get; }
            object? IEnumerator.Current { get; }
        }     
    }
}

public readonly ref struct ReadOnlyTensorSpan<T>
{
    public DimensionCollection Dimensions {get;}
    
    public ref struct DimensionCollection
        : System.Collections.Generic.ICollection<Tensor<T>>,
          System.Collections.Generic.IEnumerable<Tensor<T>>,
          System.Collections.Generic.IReadOnlyCollection<Tensor<T>>,
          System.Collections.ICollection
    {
        public int Count {get;}
        public void CopyTo(Tensor<T>[] array , int index);
        public Enumerator GetEnumerator();

        //All else explicitly implemented and match what Dictionary does   

        public ref struct Enumerator : IEnumerator<Tensor<T>>
        {
            internal Enumerator(Tensor<T> tensor, int dimension);
            public bool MoveNext();
            public ref readonly ReadOnlyTensorSpan<T> Current { get; }
        }     
    }
}

public ref struct TensorSpan<T>
{
    public DimensionCollection Dimensions {get;}
    
    public ref struct DimensionCollection
        : System.Collections.Generic.ICollection<Tensor<T>>,
          System.Collections.Generic.IEnumerable<Tensor<T>>,
          System.Collections.Generic.IReadOnlyCollection<Tensor<T>>,
          System.Collections.ICollection
    {
        public int Count {get;}
        public void CopyTo(Tensor<T>[] array , int index);
        public Enumerator GetEnumerator();

        //All else explicitly implemented and match what Dictionary does   

        public ref struct Enumerator : IEnumerator<Tensor<T>>
        {
            internal Enumerator(Tensor<T> tensor, int dimension);
            public bool MoveNext();
            public ref TensorSpan<T> Current { get; }
        }     
    }   
}
```
