```C#
   namespace System.Numerics {
  ^ Eric St. John: System.Numerics.Tensors
  public static class CompressedSparseTensor {
    public static CompressedSparseTensor<T> FromSparseTensor<T>(SparseTensor<T> tensor);
    public static CompressedSparseTensor<T> FromSparseTensor<T>(SparseTensor<T> tensor, bool compressColumn);
    public static CompressedSparseTensor<T> FromTensor<T>(Tensor<T> tensor);
    public static CompressedSparseTensor<T> FromTensor<T>(Tensor<T> tensor, bool compressColumn);
    public static CompressedSparseTensor<T> OverCompressedSparseColumns<T>(T[] values, int[] columnCounts, int[] remainingIndices, params int[] dimensions);
    ^ Eric St. John: Buffer<T>
    public static CompressedSparseTensor<T> OverCompressedSparseRows<T>(T[] values, int[] rowCounts, int[] remainingIndices, params int[] dimensions);
  }
  public class CompressedSparseTensor<T> : Tensor<T> {
    public CompressedSparseTensor(T[] values, int[] compressedCounts, int[] remainingIndices, bool compressColumn, params int[] dimensions);
    public CompressedSparseTensor(T[] values, int[] compressedCounts, int[] remainingIndices, params int[] dimensions);
    public bool CompressedColumn { get; }
    public int[] CompressedCounts { get; }
    ^ Eric St. John: Buffer<T>
    public int[] RemainingIndices { get; }
    public override T this[int indexRow, int indexColumn] { get; set; }
    public override T this[params int[] indices] { get; set; }
    public override T this[int index] { get; set; }
    public T[] Values { get; }
    ^ Eric St. John: Buffer<T>
    public override Tensor<T> Clone();
    public override Tensor<T> CloneEmpty();
    public override Tensor<TResult> CloneEmpty<TResult>();
    public DenseTensor<T> ToDenseTensor();
    ^ Eric St. John: Doesn't allow caller to control allocation.
    | Eric St. John: 
    | Eric St. John: Consider adding Tensor.CopyTo
    public DenseTensor<T> ToSparseTensor();
  }
  public static class DenseTensor {
    public static DenseTensor<T> CreateFromDiagonal<T>(Tensor<T> diagonal);
    ^ Eric St. John: Is there a reverse?
    | Eric St. John: 
    | Eric St. John: Should we factor out these?
    public static DenseTensor<T> CreateFromDiagonal<T>(Tensor<T> diagonal, int offset);
    public static DenseTensor<T> CreateIdentity<T>(int size);
    public static DenseTensor<T> CreateIdentity<T>(int size, bool columMajor);
    public static DenseTensor<T> CreateIdentity<T>(int size, bool columMajor, T oneValue);
    public static DenseTensor<T> FromArray<T>(Array array);
    public static DenseTensor<T> FromArray<T>(Array array, bool columnMajor);
    public static DenseTensor<T> FromArray<T>(T[,,] array);
    public static DenseTensor<T> FromArray<T>(T[,,] array, bool columnMajor);
    public static DenseTensor<T> FromArray<T>(T[,] array);
    public static DenseTensor<T> FromArray<T>(T[,] array, bool columnMajor);
    ^ Eric St. John: Default parameter?
    public static DenseTensor<T> FromArray<T>(T[] array);
    public static DenseTensor<T> FromArray<T>(T[] array, bool columnMajor);
    public static DenseTensor<T> OverArray<T>(T[] array, bool columnMajor, params int[] dimensions);
    ^ Eric St. John: Test default parameter
    ^ Eric St. John: As instead of Over
    | Eric St. John: 
    | Eric St. John: Consider ArrayExtension.AsTensor(this T[] ...
    | Eric St. John: 
    | Eric St. John: Consider BufferExtensions.AsTensor
    public static DenseTensor<T> OverArray<T>(T[] array, params int[] dimensions);
    public static DenseTensor<T> OverBuffer<T>(Buffer<T> buffer, bool columnMajor, params int[] dimensions);
    public static DenseTensor<T> OverBuffer<T>(Buffer<T> buffer, params int[] dimensions);
  }
  public class DenseTensor<T> : Tensor<T> {
    ^ Eric St. John: sealed
    public DenseTensor(bool columnMajor, params int[] dimensions);
    public DenseTensor(Buffer<T> buffer, bool columnMajor, params int[] dimensions);
    public DenseTensor(Buffer<T> buffer, params int[] dimensions);
    public DenseTensor(params int[] dimensions);
    public Buffer<T> Buffer { get; }
    public bool IsColumnMajor { get; }
    public bool IsRowMajor { get; }
    public override T this[int indexRow, int indexColumn] { get; set; }
    public override T this[params int[] indices] { get; set; }
    public override T this[int index] { get; set; }
    public override Tensor<T> Clone();
    public override Tensor<T> CloneEmpty();
    public override Tensor<TResult> CloneEmpty<TResult>();
  }
  public static class SparseTensor {
    public static SparseTensor<T> FromTensor<T>(Tensor<T> tensor);
    public static SparseTensor<T> FromTensor<T>(Tensor<T> tensor, bool compressColumn);
  }
  public class SparseTensor<T> : Tensor<T> {
    ^ Eric St. John: Need some abstraction for sparseness, shared with CompressedSparseTensor.
    public SparseTensor(int initialCapacity, params int[] dimensions);
    public SparseTensor(params int[] dimensions);
    public override T this[int indexRow, int indexColumn] { get; set; }
    public override T this[params int[] indices] { get; set; }
    public override T this[int index] { get; set; }
    public override Tensor<T> Clone();
    public override Tensor<T> CloneEmpty();
    public override Tensor<TResult> CloneEmpty<TResult>();
    public DenseTensor<T> ToDenseTensor();
  }
  public static class Tensor {
    ^ Eric St. John: Backing interface: 
    | Eric St. John: 
    | Eric St. John: Consider exposing?  If exposed should it be a class?
    | Eric St. John: 
    | Eric St. John: Not v1
    | Eric St. John: 
    | Eric St. John: For v1:
    | Eric St. John: built in understands the set of T we support
    | Eric St. John: built in understands efficient layouts for what we support
    | Eric St. John: 
    | Eric St. John: If in future we want to extend operators we can expose the abstraction
    public static Tensor<T> Add<T>(Tensor<T> left, Tensor<T> right);
    public static void Add<T>(Tensor<T> left, Tensor<T> right, Tensor<T> result);
    public static Tensor<T> Add<T>(Tensor<T> tensor, T scalar);
    public static void Add<T>(Tensor<T> tensor, T scalar, Tensor<T> result);
    public static Tensor<T> And<T>(Tensor<T> left, Tensor<T> right);
    public static void And<T>(Tensor<T> left, Tensor<T> right, Tensor<T> result);
    public static Tensor<T> And<T>(Tensor<T> tensor, T scalar);
    public static void And<T>(Tensor<T> tensor, T scalar, Tensor<T> result);
    public static Tensor<T> Contract<T>(Tensor<T> left, Tensor<T> right, int[] leftAxes, int[] rightAxes);
    public static void Contract<T>(Tensor<T> left, Tensor<T> right, int[] leftAxes, int[] rightAxes, Tensor<T> result);
    public static Tensor<T> Decrement<T>(Tensor<T> tensor);
    public static void Decrement<T>(Tensor<T> tensor, Tensor<T> result);
    public static void DecrementInPlace<T>(Tensor<T> tensor);
    public static Tensor<T> Divide<T>(Tensor<T> left, Tensor<T> right);
    public static void Divide<T>(Tensor<T> left, Tensor<T> right, Tensor<T> result);
    public static Tensor<T> Divide<T>(Tensor<T> tensor, T scalar);
    public static void Divide<T>(Tensor<T> tensor, T scalar, Tensor<T> result);
    public static Tensor<Boolean> Equals<T>(Tensor<T> left, Tensor<T> right);
    public static void Equals<T>(Tensor<T> left, Tensor<T> right, Tensor<Boolean> result);
    public static Tensor<Boolean> GreaterThan<T>(Tensor<T> left, Tensor<T> right);
    public static void GreaterThan<T>(Tensor<T> left, Tensor<T> right, Tensor<Boolean> result);
    public static Tensor<Boolean> GreaterThanOrEqual<T>(Tensor<T> left, Tensor<T> right);
    public static void GreaterThanOrEqual<T>(Tensor<T> left, Tensor<T> right, Tensor<Boolean> result);
    public static Tensor<T> Increment<T>(Tensor<T> tensor);
    public static void Increment<T>(Tensor<T> tensor, Tensor<T> result);
    public static void IncrementInPlace<T>(Tensor<T> tensor);
    public static Tensor<T> LeftShift<T>(Tensor<T> tensor, int value);
    public static void LeftShift<T>(Tensor<T> tensor, int value, Tensor<T> result);
    public static Tensor<Boolean> LessThan<T>(Tensor<T> left, Tensor<T> right);
    public static void LessThan<T>(Tensor<T> left, Tensor<T> right, Tensor<Boolean> result);
    public static Tensor<Boolean> LessThanOrEqual<T>(Tensor<T> left, Tensor<T> right);
    public static void LessThanOrEqual<T>(Tensor<T> left, Tensor<T> right, Tensor<Boolean> result);
    public static Tensor<T> Modulo<T>(Tensor<T> left, Tensor<T> right);
    public static void Modulo<T>(Tensor<T> left, Tensor<T> right, Tensor<T> result);
    public static Tensor<T> Modulo<T>(Tensor<T> tensor, T scalar);
    public static void Modulo<T>(Tensor<T> tensor, T scalar, Tensor<T> result);
    public static Tensor<T> Multiply<T>(Tensor<T> left, Tensor<T> right);
    public static void Multiply<T>(Tensor<T> left, Tensor<T> right, Tensor<T> result);
    public static Tensor<T> Multiply<T>(Tensor<T> tensor, T scalar);
    public static void Multiply<T>(Tensor<T> tensor, T scalar, Tensor<T> result);
    public static Tensor<Boolean> NotEquals<T>(Tensor<T> left, Tensor<T> right);
    public static void NotEquals<T>(Tensor<T> left, Tensor<T> right, Tensor<Boolean> result);
    public static Tensor<T> Or<T>(Tensor<T> left, Tensor<T> right);
    public static void Or<T>(Tensor<T> left, Tensor<T> right, Tensor<T> result);
    public static Tensor<T> Or<T>(Tensor<T> tensor, T scalar);
    public static void Or<T>(Tensor<T> tensor, T scalar, Tensor<T> result);
    public static Tensor<T> RightShift<T>(Tensor<T> tensor, int value);
    public static void RightShift<T>(Tensor<T> tensor, int value, Tensor<T> result);
    public static Tensor<T> Subtract<T>(Tensor<T> left, Tensor<T> right);
    public static void Subtract<T>(Tensor<T> left, Tensor<T> right, Tensor<T> result);
    public static Tensor<T> Subtract<T>(Tensor<T> tensor, T scalar);
    public static void Subtract<T>(Tensor<T> tensor, T scalar, Tensor<T> result);
    public static Tensor<T> UnaryMinus<T>(Tensor<T> tensor);
    public static void UnaryMinus<T>(Tensor<T> tensor, Tensor<T> result);
    public static Tensor<T> UnaryPlus<T>(Tensor<T> tensor);
    public static void UnaryPlus<T>(Tensor<T> tensor, Tensor<T> result);
    public static Tensor<T> Xor<T>(Tensor<T> left, Tensor<T> right);
    public static void Xor<T>(Tensor<T> left, Tensor<T> right, Tensor<T> result);
    public static Tensor<T> Xor<T>(Tensor<T> tensor, T scalar);
    public static void Xor<T>(Tensor<T> tensor, T scalar, Tensor<T> result);
  }
  public abstract class Tensor<T> : ICollection, IEnumerable, IList, IStructuralComparable, IStructuralEquatable {
    public Tensor(params int[] dimensions);
    ^ Eric St. John: Consider T allowlist
    public IReadOnlyList<int> Dimensions { get; }
    public bool IsFixedSize { get; }
    public bool IsReadOnly { get; }
    public int Length { get; }
    public int Rank { get; }
    int System.Collections.ICollection.Count { get; }
    bool System.Collections.ICollection.IsSynchronized { get; }
    object System.Collections.ICollection.SyncRoot { get; }
    object System.Collections.IList.this[int index] { get; set; }
    public abstract T this[int indexRow, int indexColumn] { get; set; }
    public abstract T this[params int[] indices] { get; set; }
    ^ Eric St. John: Make cascading overloads virtual with abstract [Span<int>]
    public abstract T this[int index] { get; set; }
    public abstract Tensor<T> Clone();
    public abstract Tensor<T> CloneEmpty();
    public abstract Tensor<TResult> CloneEmpty<TResult>();
    public static int Compare(Tensor<T> left, Tensor<T> right);
    public Tensor<T> Contract(Tensor<T> right, int[] axes, int[] rightAxes);
    public static bool Equals(Tensor<T> left, Tensor<T> right);
    public override bool Equals(object obj);
    public virtual void Fill(T value);
    public virtual Tensor<T> GetDiagonal();
    public virtual Tensor<T> GetDiagonal(int offset);
    public IEnumerator GetEnumerator();
    public override int GetHashCode();
    public virtual Tensor<T> GetTriangle();
    public virtual Tensor<T> GetTriangle(int offset);
    public virtual Tensor<T> GetUpperTriangle();
    public virtual Tensor<T> GetUpperTriangle(int offset);
    public Tensor<T> MatrixMultiply(Tensor<T> right);
    public static Tensor<T> operator +(Tensor<T> left, Tensor<T> right);
    public static Tensor<T> operator +(Tensor<T> tensor, T scalar);
    public static Tensor<T> operator &(Tensor<T> left, Tensor<T> right);
    public static Tensor<T> operator &(Tensor<T> tensor, T scalar);
    public static Tensor<T> operator |(Tensor<T> left, Tensor<T> right);
    public static Tensor<T> operator |(Tensor<T> tensor, T scalar);
    public static Tensor<T> operator --(Tensor<T> tensor);
    public static Tensor<T> operator /(Tensor<T> left, Tensor<T> right);
    public static Tensor<T> operator /(Tensor<T> tensor, T scalar);
    public static Tensor<T> operator ^(Tensor<T> left, Tensor<T> right);
    public static Tensor<T> operator ^(Tensor<T> tensor, T scalar);
    public static Tensor<T> operator ++(Tensor<T> tensor);
    public static Tensor<T> operator <<(Tensor<T> tensor, int value);
    public static Tensor<T> operator %(Tensor<T> left, Tensor<T> right);
    public static Tensor<T> operator %(Tensor<T> tensor, T scalar);
    public static Tensor<T> operator *(Tensor<T> left, Tensor<T> right);
    public static Tensor<T> operator *(Tensor<T> tensor, T scalar);
    public static Tensor<T> operator >>(Tensor<T> tensor, int value);
    public static Tensor<T> operator -(Tensor<T> left, Tensor<T> right);
    public static Tensor<T> operator -(Tensor<T> tensor, T scalar);
    public static Tensor<T> operator -(Tensor<T> tensor);
    public static Tensor<T> operator +(Tensor<T> tensor);
    public virtual Tensor<T> Reshape(params int[] dimensions);
    void System.Collections.ICollection.CopyTo(Array array, int index);
    int System.Collections.IList.Add(object value);
    void System.Collections.IList.Clear();
    bool System.Collections.IList.Contains(object value);
    int System.Collections.IList.IndexOf(object value);
    void System.Collections.IList.Insert(int index, object value);
    void System.Collections.IList.Remove(object value);
    void System.Collections.IList.RemoveAt(int index);
    int System.Collections.IStructuralComparable.CompareTo(object other, IComparer comparer);
    bool System.Collections.IStructuralEquatable.Equals(object other, IEqualityComparer comparer);
    int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
  }
 }
```