# System.Runtime

The diff below shows all the cases where we marked a type as nullable. All types
that aren't marked as nullable are now considered non-null.

```diff
---C:\DDrive\Temp\nullable\before
+++C:\DDrive\Temp\nullable\after
 assembly System.Runtime {
  namespace Microsoft.Win32.SafeHandles {
    public abstract class CriticalHandleMinusOneIsInvalid : CriticalHandle {
      protected CriticalHandleMinusOneIsInvalid();
      public override bool IsInvalid { get; }
    }
    public abstract class CriticalHandleZeroOrMinusOneIsInvalid : CriticalHandle {
      protected CriticalHandleZeroOrMinusOneIsInvalid();
      public override bool IsInvalid { get; }
    }
    public sealed class SafeFileHandle : SafeHandleZeroOrMinusOneIsInvalid {
      public SafeFileHandle(IntPtr preexistingHandle, bool ownsHandle);
      public override bool IsInvalid { get; }
      protected override bool ReleaseHandle();
    }
    public abstract class SafeHandleMinusOneIsInvalid : SafeHandle {
      protected SafeHandleMinusOneIsInvalid(bool ownsHandle);
      public override bool IsInvalid { get; }
    }
    public abstract class SafeHandleZeroOrMinusOneIsInvalid : SafeHandle {
      protected SafeHandleZeroOrMinusOneIsInvalid(bool ownsHandle);
      public override bool IsInvalid { get; }
    }
    public sealed class SafeWaitHandle : SafeHandleZeroOrMinusOneIsInvalid {
      public SafeWaitHandle(IntPtr existingHandle, bool ownsHandle);
      protected override bool ReleaseHandle();
    }
  }
  namespace System {
    public class AccessViolationException : SystemException {
      public AccessViolationException();
      protected AccessViolationException(SerializationInfo info, StreamingContext context);
      public AccessViolationException(string? message);
      public AccessViolationException(string? message, Exception? innerException);
    }
    public delegate void Action();
    public delegate void Action<in T>(T obj);
    public delegate void Action<in T1, in T2>(T1 arg1, T2 arg2);
    public delegate void Action<in T1, in T2, in T3>(T1 arg1, T2 arg2, T3 arg3);
    public delegate void Action<in T1, in T2, in T3, in T4>(T1 arg1, T2 arg2, T3 arg3, T4 arg4);
    public delegate void Action<in T1, in T2, in T3, in T4, in T5>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5);
    public delegate void Action<in T1, in T2, in T3, in T4, in T5, in T6>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6);
    public delegate void Action<in T1, in T2, in T3, in T4, in T5, in T6, in T7>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7);
    public delegate void Action<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8);
    public delegate void Action<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9);
    public delegate void Action<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10);
    public delegate void Action<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, in T11>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10, T11 arg11);
    public delegate void Action<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, in T11, in T12>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10, T11 arg11, T12 arg12);
    public delegate void Action<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, in T11, in T12, in T13>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10, T11 arg11, T12 arg12, T13 arg13);
    public delegate void Action<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, in T11, in T12, in T13, in T14>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10, T11 arg11, T12 arg12, T13 arg13, T14 arg14);
    public delegate void Action<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, in T11, in T12, in T13, in T14, in T15>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10, T11 arg11, T12 arg12, T13 arg13, T14 arg14, T15 arg15);
    public delegate void Action<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, in T11, in T12, in T13, in T14, in T15, in T16>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10, T11 arg11, T12 arg12, T13 arg13, T14 arg14, T15 arg15, T16 arg16);
    public static class Activator {
      public static ObjectHandle? CreateInstance(string assemblyName, string typeName);
      public static ObjectHandle? CreateInstance(string assemblyName, string typeName, bool ignoreCase, BindingFlags bindingAttr, Binder? binder, object?[]? args, CultureInfo? culture, object?[]? activationAttributes);
      public static ObjectHandle? CreateInstance(string assemblyName, string typeName, object?[]? activationAttributes);
      public static object? CreateInstance(Type type);
      public static object? CreateInstance(Type type, bool nonPublic);
      public static object? CreateInstance(Type type, params object?[]? args);
      public static object? CreateInstance(Type type, object?[]? args, object?[]? activationAttributes);
      public static object? CreateInstance(Type type, BindingFlags bindingAttr, Binder? binder, object?[]? args, CultureInfo? culture);
      public static object? CreateInstance(Type type, BindingFlags bindingAttr, Binder? binder, object?[]? args, CultureInfo? culture, object?[]? activationAttributes);
      public static T CreateInstance<T>();
      public static ObjectHandle? CreateInstanceFrom(string assemblyFile, string typeName);
      public static ObjectHandle? CreateInstanceFrom(string assemblyFile, string typeName, bool ignoreCase, BindingFlags bindingAttr, Binder? binder, object?[]? args, CultureInfo? culture, object?[]? activationAttributes);
      public static ObjectHandle? CreateInstanceFrom(string assemblyFile, string typeName, object?[]? activationAttributes);
    }
    public class AggregateException : Exception {
      public AggregateException();
      public AggregateException(IEnumerable<Exception> innerExceptions);
      public AggregateException(params Exception[] innerExceptions);
      protected AggregateException(SerializationInfo info, StreamingContext context);
      public AggregateException(string? message);
      public AggregateException(string? message, IEnumerable<Exception> innerExceptions);
      public AggregateException(string? message, Exception innerException);
      public AggregateException(string? message, params Exception[] innerExceptions);
      public ReadOnlyCollection<Exception> InnerExceptions { get; }
      public override string Message { get; }
      public AggregateException Flatten();
      public override Exception GetBaseException();
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
      public void Handle(Func<Exception, bool> predicate);
      public override string ToString();
    }
    public static class AppContext {
      public static string? BaseDirectory { get; }
      public static string? TargetFrameworkName { get; }
      public static object? GetData(string name);
      public static void SetSwitch(string switchName, bool isEnabled);
      public static bool TryGetSwitch(string switchName, out bool isEnabled);
    }
    public class ApplicationException : Exception {
      public ApplicationException();
      protected ApplicationException(SerializationInfo info, StreamingContext context);
      public ApplicationException(string? message);
      public ApplicationException(string? message, Exception? innerException);
    }
    public ref struct ArgIterator {
      public ArgIterator(RuntimeArgumentHandle arglist);
      public unsafe ArgIterator(RuntimeArgumentHandle arglist, void* ptr);
      public void End();
      public override bool Equals(object? o);
      public override int GetHashCode();
      public TypedReference GetNextArg();
      public TypedReference GetNextArg(RuntimeTypeHandle rth);
      public RuntimeTypeHandle GetNextArgType();
      public int GetRemainingCount();
    }
    public class ArgumentException : SystemException {
      public ArgumentException();
      protected ArgumentException(SerializationInfo info, StreamingContext context);
      public ArgumentException(string? message);
      public ArgumentException(string? message, Exception? innerException);
      public ArgumentException(string? message, string? paramName);
      public ArgumentException(string? message, string? paramName, Exception? innerException);
      public override string Message { get; }
      public virtual string? ParamName { get; }
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
    }
    public class ArgumentNullException : ArgumentException {
      public ArgumentNullException();
      protected ArgumentNullException(SerializationInfo info, StreamingContext context);
      public ArgumentNullException(string? paramName);
      public ArgumentNullException(string? message, Exception? innerException);
      public ArgumentNullException(string? paramName, string? message);
    }
    public class ArgumentOutOfRangeException : ArgumentException {
      public ArgumentOutOfRangeException();
      protected ArgumentOutOfRangeException(SerializationInfo info, StreamingContext context);
      public ArgumentOutOfRangeException(string? paramName);
      public ArgumentOutOfRangeException(string? message, Exception? innerException);
      public ArgumentOutOfRangeException(string? paramName, object? actualValue, string? message);
      public ArgumentOutOfRangeException(string? paramName, string? message);
      public virtual object? ActualValue { get; }
      public override string Message { get; }
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
    }
    public class ArithmeticException : SystemException {
      public ArithmeticException();
      protected ArithmeticException(SerializationInfo info, StreamingContext context);
      public ArithmeticException(string? message);
      public ArithmeticException(string? message, Exception? innerException);
    }
    public abstract class Array : ICloneable, ICollection, IEnumerable, IList, IStructuralComparable, IStructuralEquatable {
      public bool IsFixedSize { get; }
      public bool IsReadOnly { get; }
      public bool IsSynchronized { get; }
      public int Length { get; }
      public long LongLength { get; }
      public int Rank { get; }
      public object SyncRoot { get; }
      int System.Collections.ICollection.Count { get; }
      object? System.Collections.IList.this[int index] { get; set; }
      public static ReadOnlyCollection<T> AsReadOnly<T>(T[] array);
      public static int BinarySearch(Array array, int index, int length, object? value);
      public static int BinarySearch(Array array, int index, int length, object? value, IComparer? comparer);
      public static int BinarySearch(Array array, object? value);
      public static int BinarySearch(Array array, object? value, IComparer? comparer);
      public static int BinarySearch<T>(T[] array, int index, int length, T value);
      public static int BinarySearch<T>(T[] array, int index, int length, T value, IComparer<T>? comparer);
      public static int BinarySearch<T>(T[] array, T value);
      public static int BinarySearch<T>(T[] array, T value, IComparer<T>? comparer);
      public static void Clear(Array array, int index, int length);
      public object Clone();
      public static void ConstrainedCopy(Array sourceArray, int sourceIndex, Array destinationArray, int destinationIndex, int length);
      public static TOutput[] ConvertAll<TInput, TOutput>(TInput[] array, Converter<TInput, TOutput> converter);
      public static void Copy(Array sourceArray, Array destinationArray, int length);
      public static void Copy(Array sourceArray, Array destinationArray, long length);
      public static void Copy(Array sourceArray, int sourceIndex, Array destinationArray, int destinationIndex, int length);
      public static void Copy(Array sourceArray, long sourceIndex, Array destinationArray, long destinationIndex, long length);
      public void CopyTo(Array array, int index);
      public void CopyTo(Array array, long index);
      public static Array CreateInstance(Type elementType, int length);
      public static Array CreateInstance(Type elementType, int length1, int length2);
      public static Array CreateInstance(Type elementType, int length1, int length2, int length3);
      public static Array CreateInstance(Type elementType, params int[] lengths);
      public static Array CreateInstance(Type elementType, int[] lengths, int[] lowerBounds);
      public static Array CreateInstance(Type elementType, params long[] lengths);
      public static T[] Empty<T>();
      public static bool Exists<T>(T[] array, Predicate<T> match);
      public static void Fill<T>(T[] array, T value);
      public static void Fill<T>(T[] array, T value, int startIndex, int count);
      public static T Find<T>(T[] array, Predicate<T> match);
      public static T[] FindAll<T>(T[] array, Predicate<T> match);
      public static int FindIndex<T>(T[] array, int startIndex, int count, Predicate<T> match);
      public static int FindIndex<T>(T[] array, int startIndex, Predicate<T> match);
      public static int FindIndex<T>(T[] array, Predicate<T> match);
      public static T FindLast<T>(T[] array, Predicate<T> match);
      public static int FindLastIndex<T>(T[] array, int startIndex, int count, Predicate<T> match);
      public static int FindLastIndex<T>(T[] array, int startIndex, Predicate<T> match);
      public static int FindLastIndex<T>(T[] array, Predicate<T> match);
      public static void ForEach<T>(T[] array, Action<T> action);
      public IEnumerator GetEnumerator();
      public int GetLength(int dimension);
      public long GetLongLength(int dimension);
      public int GetLowerBound(int dimension);
      public int GetUpperBound(int dimension);
      public object? GetValue(int index);
      public object? GetValue(int index1, int index2);
      public object? GetValue(int index1, int index2, int index3);
      public object? GetValue(params int[] indices);
      public object? GetValue(long index);
      public object? GetValue(long index1, long index2);
      public object? GetValue(long index1, long index2, long index3);
      public object? GetValue(params long[] indices);
      public static int IndexOf(Array array, object? value);
      public static int IndexOf(Array array, object? value, int startIndex);
      public static int IndexOf(Array array, object? value, int startIndex, int count);
      public static int IndexOf<T>(T[] array, T value);
      public static int IndexOf<T>(T[] array, T value, int startIndex);
      public static int IndexOf<T>(T[] array, T value, int startIndex, int count);
      public void Initialize();
      public static int LastIndexOf(Array array, object? value);
      public static int LastIndexOf(Array array, object? value, int startIndex);
      public static int LastIndexOf(Array array, object? value, int startIndex, int count);
      public static int LastIndexOf<T>(T[] array, T value);
      public static int LastIndexOf<T>(T[] array, T value, int startIndex);
      public static int LastIndexOf<T>(T[] array, T value, int startIndex, int count);
      public static void Resize<T>(ref T[]? array, int newSize);
      public static void Reverse(Array array);
      public static void Reverse(Array array, int index, int length);
      public static void Reverse<T>(T[] array);
      public static void Reverse<T>(T[] array, int index, int length);
      public void SetValue(object? value, int index);
      public void SetValue(object? value, int index1, int index2);
      public void SetValue(object? value, int index1, int index2, int index3);
      public void SetValue(object? value, params int[] indices);
      public void SetValue(object? value, long index);
      public void SetValue(object? value, long index1, long index2);
      public void SetValue(object value, long index1, long index2, long index3);
      public void SetValue(object? value, params long[] indices);
      public static void Sort(Array array);
      public static void Sort(Array keys, Array? items);
      public static void Sort(Array keys, Array? items, IComparer? comparer);
      public static void Sort(Array keys, Array? items, int index, int length);
      public static void Sort(Array keys, Array? items, int index, int length, IComparer? comparer);
      public static void Sort(Array array, IComparer? comparer);
      public static void Sort(Array array, int index, int length);
      public static void Sort(Array array, int index, int length, IComparer? comparer);
      public static void Sort<T>(T[] array);
      public static void Sort<T>(T[] array, IComparer<T>? comparer);
      public static void Sort<T>(T[] array, Comparison<T> comparison);
      public static void Sort<T>(T[] array, int index, int length);
      public static void Sort<T>(T[] array, int index, int length, IComparer<T>? comparer);
      public static void Sort<TKey, TValue>(TKey[] keys, TValue[]? items);
      public static void Sort<TKey, TValue>(TKey[] keys, TValue[]? items, IComparer<TKey>? comparer);
      public static void Sort<TKey, TValue>(TKey[] keys, TValue[]? items, int index, int length);
      public static void Sort<TKey, TValue>(TKey[] keys, TValue[]? items, int index, int length, IComparer<TKey>? comparer);
      int System.Collections.IList.Add(object? value);
      void System.Collections.IList.Clear();
      bool System.Collections.IList.Contains(object? value);
      int System.Collections.IList.IndexOf(object? value);
      void System.Collections.IList.Insert(int index, object? value);
      void System.Collections.IList.Remove(object? value);
      void System.Collections.IList.RemoveAt(int index);
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      public static bool TrueForAll<T>(T[] array, Predicate<T> match);
    }
    public readonly struct ArraySegment<T> : ICollection<T>, IEnumerable, IEnumerable<T>, IList<T>, IReadOnlyCollection<T>, IReadOnlyList<T> {
      public ArraySegment(T[] array);
      public ArraySegment(T[] array, int offset, int count);
      public T[]? Array { get; }
      public int Count { get; }
      public static ArraySegment<T> Empty { get; }
      public int Offset { get; }
      bool System.Collections.Generic.ICollection<T>.IsReadOnlyICollection<T>.IsReadOnly { get; }
      T System.Collections.Generic.IList<T>.thisIList<T>.this[int index] { get; set; }
      T System.Collections.Generic.IReadOnlyList<T>.thisIReadOnlyList<T>.this[int index] { get; }
      public T this[int index] { get; set; }
      public void CopyTo(ArraySegment<T> destination);
      public void CopyTo(T[] destination);
      public void CopyTo(T[] destination, int destinationIndex);
      public bool Equals(ArraySegment<T> obj);
      public override bool Equals(object? obj);
      public ArraySegment<T>.Enumerator GetEnumerator();
      public override int GetHashCode();
      public static bool operator ==(ArraySegment<T> a, ArraySegment<T> b);
      public static implicit operator ArraySegment<T> (T[] array);
      public static bool operator !=(ArraySegment<T> a, ArraySegment<T> b);
      public ArraySegment<T> Slice(int index);
      public ArraySegment<T> Slice(int index, int count);
      void System.Collections.Generic.ICollection<T>.AddICollection<T>.Add(T item);
      void System.Collections.Generic.ICollection<T>.ClearICollection<T>.Clear();
      bool System.Collections.Generic.ICollection<T>.ContainsICollection<T>.Contains(T item);
      bool System.Collections.Generic.ICollection<T>.RemoveICollection<T>.Remove(T item);
      IEnumerator<T> System.Collections.Generic.IEnumerable<T>.GetEnumeratorIEnumerable<T>.GetEnumerator();
      int System.Collections.Generic.IList<T>.IndexOfIList<T>.IndexOf(T item);
      void System.Collections.Generic.IList<T>.InsertIList<T>.Insert(int index, T item);
      void System.Collections.Generic.IList<T>.RemoveAtIList<T>.RemoveAt(int index);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      public T[] ToArray();
      public struct Enumerator : IDisposable, IEnumerator, IEnumerator<T> {
        public T Current { get; }
        object? System.Collections.IEnumerator.Current { get; }
        public void Dispose();
        public bool MoveNext();
        void System.Collections.IEnumerator.Reset();
      }
    }
    public class ArrayTypeMismatchException : SystemException {
      public ArrayTypeMismatchException();
      protected ArrayTypeMismatchException(SerializationInfo info, StreamingContext context);
      public ArrayTypeMismatchException(string? message);
      public ArrayTypeMismatchException(string? message, Exception? innerException);
    }
    public delegate void AsyncCallback(IAsyncResult ar);
    public abstract class Attribute {
      protected Attribute();
      public virtual object TypeId { get; }
      public override bool Equals(object? obj);
      public static Attribute? GetCustomAttribute(Assembly element, Type attributeType);
      public static Attribute? GetCustomAttribute(Assembly element, Type attributeType, bool inherit);
      public static Attribute? GetCustomAttribute(MemberInfo element, Type attributeType);
      public static Attribute? GetCustomAttribute(MemberInfo element, Type attributeType, bool inherit);
      public static Attribute? GetCustomAttribute(Module element, Type attributeType);
      public static Attribute? GetCustomAttribute(Module element, Type attributeType, bool inherit);
      public static Attribute? GetCustomAttribute(ParameterInfo element, Type attributeType);
      public static Attribute? GetCustomAttribute(ParameterInfo element, Type attributeType, bool inherit);
      public static Attribute[] GetCustomAttributes(Assembly element);
      public static Attribute[] GetCustomAttributes(Assembly element, bool inherit);
      public static Attribute[] GetCustomAttributes(Assembly element, Type attributeType);
      public static Attribute[] GetCustomAttributes(Assembly element, Type attributeType, bool inherit);
      public static Attribute[] GetCustomAttributes(MemberInfo element);
      public static Attribute[] GetCustomAttributes(MemberInfo element, bool inherit);
      public static Attribute[] GetCustomAttributes(MemberInfo element, Type type);
      public static Attribute[] GetCustomAttributes(MemberInfo element, Type type, bool inherit);
      public static Attribute[] GetCustomAttributes(Module element);
      public static Attribute[] GetCustomAttributes(Module element, bool inherit);
      public static Attribute[] GetCustomAttributes(Module element, Type attributeType);
      public static Attribute[] GetCustomAttributes(Module element, Type attributeType, bool inherit);
      public static Attribute[] GetCustomAttributes(ParameterInfo element);
      public static Attribute[] GetCustomAttributes(ParameterInfo element, bool inherit);
      public static Attribute[] GetCustomAttributes(ParameterInfo element, Type attributeType);
      public static Attribute[] GetCustomAttributes(ParameterInfo element, Type attributeType, bool inherit);
      public override int GetHashCode();
      public virtual bool IsDefaultAttribute();
      public static bool IsDefined(Assembly element, Type attributeType);
      public static bool IsDefined(Assembly element, Type attributeType, bool inherit);
      public static bool IsDefined(MemberInfo element, Type attributeType);
      public static bool IsDefined(MemberInfo element, Type attributeType, bool inherit);
      public static bool IsDefined(Module element, Type attributeType);
      public static bool IsDefined(Module element, Type attributeType, bool inherit);
      public static bool IsDefined(ParameterInfo element, Type attributeType);
      public static bool IsDefined(ParameterInfo element, Type attributeType, bool inherit);
      public virtual bool Match(object? obj);
    }
    public enum AttributeTargets {
      All = 32767,
      Assembly = 1,
      Class = 4,
      Constructor = 32,
      Delegate = 4096,
      Enum = 16,
      Event = 512,
      Field = 256,
      GenericParameter = 16384,
      Interface = 1024,
      Method = 64,
      Module = 2,
      Parameter = 2048,
      Property = 128,
      ReturnValue = 8192,
      Struct = 8,
    }
    public sealed class AttributeUsageAttribute : Attribute {
      public AttributeUsageAttribute(AttributeTargets validOn);
      public bool AllowMultiple { get; set; }
      public bool Inherited { get; set; }
      public AttributeTargets ValidOn { get; }
    }
    public class BadImageFormatException : SystemException {
      public BadImageFormatException();
      protected BadImageFormatException(SerializationInfo info, StreamingContext context);
      public BadImageFormatException(string? message);
      public BadImageFormatException(string? message, Exception? inner);
      public BadImageFormatException(string? message, string? fileName);
      public BadImageFormatException(string? message, string? fileName, Exception? inner);
      public string? FileName { get; }
      public string? FusionLog { get; }
      public override string Message { get; }
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
      public override string ToString();
    }
    public readonly struct Boolean : IComparable, IComparable<bool>, IConvertible, IEquatable<bool> {
      public static readonly string FalseString;
      public static readonly string TrueString;
      public int CompareTo(Boolean value);
      public int CompareTo(object? obj);
      public Boolean Equals(Boolean obj);
      public override Boolean Equals(object? obj);
      public override int GetHashCode();
      public TypeCode GetTypeCode();
      public static Boolean Parse(ReadOnlySpan<char> value);
      public static Boolean Parse(string value);
      Boolean System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      public override string ToString();
      public string ToString(IFormatProvider? provider);
      public Boolean TryFormat(Span<char> destination, out int charsWritten);
      public static Boolean TryParse(ReadOnlySpan<char> value, out Boolean result);
      public static Boolean TryParse(string? value, out Boolean result);
    }
    public static class Buffer {
      public static void BlockCopy(Array src, int srcOffset, Array dst, int dstOffset, int count);
      public static int ByteLength(Array array);
      public static byte GetByte(Array array, int index);
      public unsafe static void MemoryCopy(void* source, void* destination, long destinationSizeInBytes, long sourceBytesToCopy);
      public unsafe static void MemoryCopy(void* source, void* destination, ulong destinationSizeInBytes, ulong sourceBytesToCopy);
      public static void SetByte(Array array, int index, byte value);
    }
    public readonly struct Byte : IComparable, IComparable<byte>, IConvertible, IEquatable<byte>, IFormattable {
      public const byte MaxValue = (byte)255;
      public const byte MinValue = (byte)0;
      public int CompareTo(Byte value);
      public int CompareTo(object? value);
      public bool Equals(Byte obj);
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public TypeCode GetTypeCode();
      public static Byte Parse(ReadOnlySpan<char> s, NumberStyles style = NumberStyles.Integer, IFormatProvider? provider = null);
      public static Byte Parse(string s);
      public static Byte Parse(string s, NumberStyles style);
      public static Byte Parse(string s, NumberStyles style, IFormatProvider? provider);
      public static Byte Parse(string s, IFormatProvider? provider);
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      Byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      public override string ToString();
      public string ToString(IFormatProvider? provider);
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? provider);
      public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? provider = null);
      public static bool TryParse(ReadOnlySpan<char> s, out Byte result);
      public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out Byte result);
      public static bool TryParse(string? s, out Byte result);
      public static bool TryParse(string? s, NumberStyles style, IFormatProvider? provider, out Byte result);
    }
    public readonly struct Char : IComparable, IComparable<char>, IConvertible, IEquatable<char> {
      public const char MaxValue = '\uFFFF';
      public const char MinValue = '\0';
      public int CompareTo(Char value);
      public int CompareTo(object? value);
      public static string ConvertFromUtf32(int utf32);
      public static int ConvertToUtf32(Char highSurrogate, Char lowSurrogate);
      public static int ConvertToUtf32(string s, int index);
      public bool Equals(Char obj);
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public static double GetNumericValue(Char c);
      public static double GetNumericValue(string s, int index);
      public TypeCode GetTypeCode();
      public static UnicodeCategory GetUnicodeCategory(Char c);
      public static UnicodeCategory GetUnicodeCategory(string s, int index);
      public static bool IsControl(Char c);
      public static bool IsControl(string s, int index);
      public static bool IsDigit(Char c);
      public static bool IsDigit(string s, int index);
      public static bool IsHighSurrogate(Char c);
      public static bool IsHighSurrogate(string s, int index);
      public static bool IsLetter(Char c);
      public static bool IsLetter(string s, int index);
      public static bool IsLetterOrDigit(Char c);
      public static bool IsLetterOrDigit(string s, int index);
      public static bool IsLower(Char c);
      public static bool IsLower(string s, int index);
      public static bool IsLowSurrogate(Char c);
      public static bool IsLowSurrogate(string s, int index);
      public static bool IsNumber(Char c);
      public static bool IsNumber(string s, int index);
      public static bool IsPunctuation(Char c);
      public static bool IsPunctuation(string s, int index);
      public static bool IsSeparator(Char c);
      public static bool IsSeparator(string s, int index);
      public static bool IsSurrogate(Char c);
      public static bool IsSurrogate(string s, int index);
      public static bool IsSurrogatePair(Char highSurrogate, Char lowSurrogate);
      public static bool IsSurrogatePair(string s, int index);
      public static bool IsSymbol(Char c);
      public static bool IsSymbol(string s, int index);
      public static bool IsUpper(Char c);
      public static bool IsUpper(string s, int index);
      public static bool IsWhiteSpace(Char c);
      public static bool IsWhiteSpace(string s, int index);
      public static Char Parse(string s);
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      Char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      public static Char ToLower(Char c);
      public static Char ToLower(Char c, CultureInfo culture);
      public static Char ToLowerInvariant(Char c);
      public override string ToString();
      public static string ToString(Char c);
      public string ToString(IFormatProvider? provider);
      public static Char ToUpper(Char c);
      public static Char ToUpper(Char c, CultureInfo culture);
      public static Char ToUpperInvariant(Char c);
      public static bool TryParse(string? s, out Char result);
    }
    public sealed class CharEnumerator : ICloneable, IDisposable, IEnumerator, IEnumerator<char> {
      public char Current { get; }
      object? System.Collections.IEnumerator.Current { get; }
      public object Clone();
      public void Dispose();
      public bool MoveNext();
      public void Reset();
    }
    public sealed class CLSCompliantAttribute : Attribute {
      public CLSCompliantAttribute(bool isCompliant);
      public bool IsCompliant { get; }
    }
    public delegate int Comparison<in T>(T x, T y);
    public delegate TOutput Converter<in TInput, out TOutput>(TInput input);
    public readonly struct DateTime : IComparable, IComparable<DateTime>, IConvertible, IEquatable<DateTime>, IFormattable, ISerializable {
      public static readonly DateTime MaxValue;
      public static readonly DateTime MinValue;
      public static readonly DateTime UnixEpoch;
      public DateTime(int year, int month, int day);
      public DateTime(int year, int month, int day, Calendar calendar);
      public DateTime(int year, int month, int day, int hour, int minute, int second);
      public DateTime(int year, int month, int day, int hour, int minute, int second, DateTimeKind kind);
      public DateTime(int year, int month, int day, int hour, int minute, int second, Calendar calendar);
      public DateTime(int year, int month, int day, int hour, int minute, int second, int millisecond);
      public DateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, DateTimeKind kind);
      public DateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, Calendar calendar);
      public DateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, Calendar calendar, DateTimeKind kind);
      public DateTime(long ticks);
      public DateTime(long ticks, DateTimeKind kind);
      public DateTime Date { get; }
      public int Day { get; }
      public DayOfWeek DayOfWeek { get; }
      public int DayOfYear { get; }
      public int Hour { get; }
      public DateTimeKind Kind { get; }
      public int Millisecond { get; }
      public int Minute { get; }
      public int Month { get; }
      public static DateTime Now { get; }
      public int Second { get; }
      public long Ticks { get; }
      public TimeSpan TimeOfDay { get; }
      public static DateTime Today { get; }
      public static DateTime UtcNow { get; }
      public int Year { get; }
      public DateTime Add(TimeSpan value);
      public DateTime AddDays(double value);
      public DateTime AddHours(double value);
      public DateTime AddMilliseconds(double value);
      public DateTime AddMinutes(double value);
      public DateTime AddMonths(int months);
      public DateTime AddSeconds(double value);
      public DateTime AddTicks(long value);
      public DateTime AddYears(int value);
      public static int Compare(DateTime t1, DateTime t2);
      public int CompareTo(DateTime value);
      public int CompareTo(object? value);
      public static int DaysInMonth(int year, int month);
      public bool Equals(DateTime value);
      public static bool Equals(DateTime t1, DateTime t2);
      public override bool Equals(object? value);
      public static DateTime FromBinary(long dateData);
      public static DateTime FromFileTime(long fileTime);
      public static DateTime FromFileTimeUtc(long fileTime);
      public static DateTime FromOADate(double d);
      public string[] GetDateTimeFormats();
      public string[] GetDateTimeFormats(char format);
      public string[] GetDateTimeFormats(char format, IFormatProvider? provider);
      public string[] GetDateTimeFormats(IFormatProvider? provider);
      public override int GetHashCode();
      public TypeCode GetTypeCode();
      public bool IsDaylightSavingTime();
      public static bool IsLeapYear(int year);
      public static DateTime operator +(DateTime d, TimeSpan t);
      public static bool operator ==(DateTime d1, DateTime d2);
      public static bool operator >(DateTime t1, DateTime t2);
      public static bool operator >=(DateTime t1, DateTime t2);
      public static bool operator !=(DateTime d1, DateTime d2);
      public static bool operator <(DateTime t1, DateTime t2);
      public static bool operator <=(DateTime t1, DateTime t2);
      public static TimeSpan operator -(DateTime d1, DateTime d2);
      public static DateTime operator -(DateTime d, TimeSpan t);
      public static DateTime Parse(ReadOnlySpan<char> s, IFormatProvider? provider = null, DateTimeStyles styles = DateTimeStyles.None);
      public static DateTime Parse(string s);
      public static DateTime Parse(string s, IFormatProvider? provider);
      public static DateTime Parse(string s, IFormatProvider? provider, DateTimeStyles styles);
      public static DateTime ParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);
      public static DateTime ParseExact(ReadOnlySpan<char> s, string[] formats, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);
      public static DateTime ParseExact(string s, string format, IFormatProvider? provider);
      public static DateTime ParseExact(string s, string format, IFormatProvider? provider, DateTimeStyles style);
      public static DateTime ParseExact(string s, string[] formats, IFormatProvider? provider, DateTimeStyles style);
      public static DateTime SpecifyKind(DateTime value, DateTimeKind kind);
      public TimeSpan Subtract(DateTime value);
      public DateTime Subtract(TimeSpan value);
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
      public long ToBinary();
      public long ToFileTime();
      public long ToFileTimeUtc();
      public DateTime ToLocalTime();
      public string ToLongDateString();
      public string ToLongTimeString();
      public double ToOADate();
      public string ToShortDateString();
      public string ToShortTimeString();
      public override string ToString();
      public string ToString(IFormatProvider? provider);
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? provider);
      public DateTime ToUniversalTime();
      public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? provider = null);
      public static bool TryParse(ReadOnlySpan<char> s, out DateTime result);
      public static bool TryParse(ReadOnlySpan<char> s, IFormatProvider? provider, DateTimeStyles styles, out DateTime result);
      public static bool TryParse(string? s, out DateTime result);
      public static bool TryParse(string? s, IFormatProvider? provider, DateTimeStyles styles, out DateTime result);
      public static bool TryParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, IFormatProvider? provider, DateTimeStyles style, out DateTime result);
      public static bool TryParseExact(ReadOnlySpan<char> s, string?[]? formats, IFormatProvider? provider, DateTimeStyles style, out DateTime result);
      public static bool TryParseExact(string? s, string? format, IFormatProvider? provider, DateTimeStyles style, out DateTime result);
      public static bool TryParseExact(string? s, string?[]? formats, IFormatProvider? provider, DateTimeStyles style, out DateTime result);
    }
    public enum DateTimeKind {
      Local = 2,
      Unspecified = 0,
      Utc = 1,
    }
    public readonly struct DateTimeOffset : IComparable, IComparable<DateTimeOffset>, IDeserializationCallback, IEquatable<DateTimeOffset>, IFormattable, ISerializable {
      public static readonly DateTimeOffset MaxValue;
      public static readonly DateTimeOffset MinValue;
      public static readonly DateTimeOffset UnixEpoch;
      public DateTimeOffset(DateTime dateTime);
      public DateTimeOffset(DateTime dateTime, TimeSpan offset);
      public DateTimeOffset(int year, int month, int day, int hour, int minute, int second, int millisecond, Calendar calendar, TimeSpan offset);
      public DateTimeOffset(int year, int month, int day, int hour, int minute, int second, int millisecond, TimeSpan offset);
      public DateTimeOffset(int year, int month, int day, int hour, int minute, int second, TimeSpan offset);
      public DateTimeOffset(long ticks, TimeSpan offset);
      public DateTime Date { get; }
      public DateTime DateTime { get; }
      public int Day { get; }
      public DayOfWeek DayOfWeek { get; }
      public int DayOfYear { get; }
      public int Hour { get; }
      public DateTime LocalDateTime { get; }
      public int Millisecond { get; }
      public int Minute { get; }
      public int Month { get; }
      public static DateTimeOffset Now { get; }
      public TimeSpan Offset { get; }
      public int Second { get; }
      public long Ticks { get; }
      public TimeSpan TimeOfDay { get; }
      public DateTime UtcDateTime { get; }
      public static DateTimeOffset UtcNow { get; }
      public long UtcTicks { get; }
      public int Year { get; }
      public DateTimeOffset Add(TimeSpan timeSpan);
      public DateTimeOffset AddDays(double days);
      public DateTimeOffset AddHours(double hours);
      public DateTimeOffset AddMilliseconds(double milliseconds);
      public DateTimeOffset AddMinutes(double minutes);
      public DateTimeOffset AddMonths(int months);
      public DateTimeOffset AddSeconds(double seconds);
      public DateTimeOffset AddTicks(long ticks);
      public DateTimeOffset AddYears(int years);
      public static int Compare(DateTimeOffset first, DateTimeOffset second);
      public int CompareTo(DateTimeOffset other);
      public bool Equals(DateTimeOffset other);
      public static bool Equals(DateTimeOffset first, DateTimeOffset second);
      public override bool Equals(object? obj);
      public bool EqualsExact(DateTimeOffset other);
      public static DateTimeOffset FromFileTime(long fileTime);
      public static DateTimeOffset FromUnixTimeMilliseconds(long milliseconds);
      public static DateTimeOffset FromUnixTimeSeconds(long seconds);
      public override int GetHashCode();
      public static DateTimeOffset operator +(DateTimeOffset dateTimeOffset, TimeSpan timeSpan);
      public static bool operator ==(DateTimeOffset left, DateTimeOffset right);
      public static bool operator >(DateTimeOffset left, DateTimeOffset right);
      public static bool operator >=(DateTimeOffset left, DateTimeOffset right);
      public static implicit operator DateTimeOffset (DateTime dateTime);
      public static bool operator !=(DateTimeOffset left, DateTimeOffset right);
      public static bool operator <(DateTimeOffset left, DateTimeOffset right);
      public static bool operator <=(DateTimeOffset left, DateTimeOffset right);
      public static TimeSpan operator -(DateTimeOffset left, DateTimeOffset right);
      public static DateTimeOffset operator -(DateTimeOffset dateTimeOffset, TimeSpan timeSpan);
      public static DateTimeOffset Parse(ReadOnlySpan<char> input, IFormatProvider? formatProvider = null, DateTimeStyles styles = DateTimeStyles.None);
      public static DateTimeOffset Parse(string input);
      public static DateTimeOffset Parse(string input, IFormatProvider? formatProvider);
      public static DateTimeOffset Parse(string input, IFormatProvider? formatProvider, DateTimeStyles styles);
      public static DateTimeOffset ParseExact(ReadOnlySpan<char> input, ReadOnlySpan<char> format, IFormatProvider? formatProvider, DateTimeStyles styles = DateTimeStyles.None);
      public static DateTimeOffset ParseExact(ReadOnlySpan<char> input, string[] formats, IFormatProvider? formatProvider, DateTimeStyles styles = DateTimeStyles.None);
      public static DateTimeOffset ParseExact(string input, string format, IFormatProvider? formatProvider);
      public static DateTimeOffset ParseExact(string input, string format, IFormatProvider? formatProvider, DateTimeStyles styles);
      public static DateTimeOffset ParseExact(string input, string[] formats, IFormatProvider? formatProvider, DateTimeStyles styles);
      public TimeSpan Subtract(DateTimeOffset value);
      public DateTimeOffset Subtract(TimeSpan value);
      int System.IComparable.CompareTo(object? obj);
      void System.Runtime.Serialization.IDeserializationCallback.OnDeserialization(object sender);
      void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
      public long ToFileTime();
      public DateTimeOffset ToLocalTime();
      public DateTimeOffset ToOffset(TimeSpan offset);
      public override string ToString();
      public string ToString(IFormatProvider? formatProvider);
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? formatProvider);
      public DateTimeOffset ToUniversalTime();
      public long ToUnixTimeMilliseconds();
      public long ToUnixTimeSeconds();
      public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? formatProvider = null);
      public static bool TryParse(ReadOnlySpan<char> input, out DateTimeOffset result);
      public static bool TryParse(ReadOnlySpan<char> input, IFormatProvider? formatProvider, DateTimeStyles styles, out DateTimeOffset result);
      public static bool TryParse(string? input, out DateTimeOffset result);
      public static bool TryParse(string? input, IFormatProvider? formatProvider, DateTimeStyles styles, out DateTimeOffset result);
      public static bool TryParseExact(ReadOnlySpan<char> input, ReadOnlySpan<char> format, IFormatProvider? formatProvider, DateTimeStyles styles, out DateTimeOffset result);
      public static bool TryParseExact(ReadOnlySpan<char> input, string?[]? formats, IFormatProvider? formatProvider, DateTimeStyles styles, out DateTimeOffset result);
      public static bool TryParseExact(string? input, string? format, IFormatProvider? formatProvider, DateTimeStyles styles, out DateTimeOffset result);
      public static bool TryParseExact(string? input, string?[]? formats, IFormatProvider? formatProvider, DateTimeStyles styles, out DateTimeOffset result);
    }
    public enum DayOfWeek {
      Friday = 5,
      Monday = 1,
      Saturday = 6,
      Sunday = 0,
      Thursday = 4,
      Tuesday = 2,
      Wednesday = 3,
    }
    public sealed class DBNull : IConvertible, ISerializable {
      public static readonly DBNull Value;
      public void GetObjectData(SerializationInfo info, StreamingContext context);
      public TypeCode GetTypeCode();
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      public override string ToString();
      public string ToString(IFormatProvider? provider);
    }
    public readonly struct Decimal : IComparable, IComparable<decimal>, IConvertible, IDeserializationCallback, IEquatable<decimal>, IFormattable {
      public static readonly decimal MaxValue;
      public static readonly decimal MinusOne;
      public static readonly decimal MinValue;
      public static readonly decimal One;
      public static readonly decimal Zero;
      public Decimal(double value);
      public Decimal(int value);
      public Decimal(int lo, int mid, int hi, bool isNegative, byte scale);
      public Decimal(int[] bits);
      public Decimal(long value);
      public Decimal(float value);
      public Decimal(uint value);
      public Decimal(ulong value);
      public static Decimal Add(Decimal d1, Decimal d2);
      public static Decimal Ceiling(Decimal d);
      public static int Compare(Decimal d1, Decimal d2);
      public int CompareTo(Decimal value);
      public int CompareTo(object? value);
      public static Decimal Divide(Decimal d1, Decimal d2);
      public bool Equals(Decimal value);
      public static bool Equals(Decimal d1, Decimal d2);
      public override bool Equals(object? value);
      public static Decimal Floor(Decimal d);
      public static Decimal FromOACurrency(long cy);
      public static int[] GetBits(Decimal d);
      public override int GetHashCode();
      public TypeCode GetTypeCode();
      public static Decimal Multiply(Decimal d1, Decimal d2);
      public static Decimal Negate(Decimal d);
      public static Decimal operator +(Decimal d1, Decimal d2);
      public static Decimal operator --(Decimal d);
      public static Decimal operator /(Decimal d1, Decimal d2);
      public static bool operator ==(Decimal d1, Decimal d2);
      public static explicit operator byte (Decimal value);
      public static explicit operator char (Decimal value);
      public static explicit operator double (Decimal value);
      public static explicit operator short (Decimal value);
      public static explicit operator int (Decimal value);
      public static explicit operator long (Decimal value);
      public static explicit operator sbyte (Decimal value);
      public static explicit operator float (Decimal value);
      public static explicit operator ushort (Decimal value);
      public static explicit operator uint (Decimal value);
      public static explicit operator ulong (Decimal value);
      public static explicit operator Decimal (double value);
      public static explicit operator Decimal (float value);
      public static bool operator >(Decimal d1, Decimal d2);
      public static bool operator >=(Decimal d1, Decimal d2);
      public static implicit operator Decimal (byte value);
      public static implicit operator Decimal (char value);
      public static implicit operator Decimal (short value);
      public static implicit operator Decimal (int value);
      public static implicit operator Decimal (long value);
      public static implicit operator Decimal (sbyte value);
      public static implicit operator Decimal (ushort value);
      public static implicit operator Decimal (uint value);
      public static implicit operator Decimal (ulong value);
      public static Decimal operator ++(Decimal d);
      public static bool operator !=(Decimal d1, Decimal d2);
      public static bool operator <(Decimal d1, Decimal d2);
      public static bool operator <=(Decimal d1, Decimal d2);
      public static Decimal operator %(Decimal d1, Decimal d2);
      public static Decimal operator *(Decimal d1, Decimal d2);
      public static Decimal operator -(Decimal d1, Decimal d2);
      public static Decimal operator -(Decimal d);
      public static Decimal operator +(Decimal d);
      public static Decimal Parse(ReadOnlySpan<char> s, NumberStyles style = NumberStyles.Number, IFormatProvider? provider = null);
      public static Decimal Parse(string s);
      public static Decimal Parse(string s, NumberStyles style);
      public static Decimal Parse(string s, NumberStyles style, IFormatProvider? provider);
      public static Decimal Parse(string s, IFormatProvider? provider);
      public static Decimal Remainder(Decimal d1, Decimal d2);
      public static Decimal Round(Decimal d);
      public static Decimal Round(Decimal d, int decimals);
      public static Decimal Round(Decimal d, int decimals, MidpointRounding mode);
      public static Decimal Round(Decimal d, MidpointRounding mode);
      public static Decimal Subtract(Decimal d1, Decimal d2);
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      Decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      void System.Runtime.Serialization.IDeserializationCallback.OnDeserialization(object sender);
      public static byte ToByte(Decimal value);
      public static double ToDouble(Decimal d);
      public static short ToInt16(Decimal value);
      public static int ToInt32(Decimal d);
      public static long ToInt64(Decimal d);
      public static long ToOACurrency(Decimal value);
      public static sbyte ToSByte(Decimal value);
      public static float ToSingle(Decimal d);
      public override string ToString();
      public string ToString(IFormatProvider? provider);
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? provider);
      public static ushort ToUInt16(Decimal value);
      public static uint ToUInt32(Decimal d);
      public static ulong ToUInt64(Decimal d);
      public static Decimal Truncate(Decimal d);
      public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? provider = null);
      public static bool TryParse(ReadOnlySpan<char> s, out Decimal result);
      public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out Decimal result);
      public static bool TryParse(string? s, out Decimal result);
      public static bool TryParse(string? s, NumberStyles style, IFormatProvider? provider, out Decimal result);
    }
    public abstract class Delegate : ICloneable, ISerializable {
      protected Delegate(object target, string method);
      protected Delegate(Type target, string method);
      public MethodInfo Method { get; }
      public object? Target { get; }
      public virtual object Clone();
      public static Delegate? Combine(Delegate? a, Delegate? b);
      public static Delegate? Combine(params Delegate?[]? delegates);
      protected virtual Delegate CombineImpl(Delegate? d);
      public static Delegate CreateDelegate(Type type, object? firstArgument, MethodInfo method);
      public static Delegate? CreateDelegate(Type type, object? firstArgument, MethodInfo method, bool throwOnBindFailure);
      public static Delegate CreateDelegate(Type type, object target, string method);
      public static Delegate CreateDelegate(Type type, object target, string method, bool ignoreCase);
      public static Delegate? CreateDelegate(Type type, object target, string method, bool ignoreCase, bool throwOnBindFailure);
      public static Delegate CreateDelegate(Type type, MethodInfo method);
      public static Delegate? CreateDelegate(Type type, MethodInfo method, bool throwOnBindFailure);
      public static Delegate CreateDelegate(Type type, Type target, string method);
      public static Delegate CreateDelegate(Type type, Type target, string method, bool ignoreCase);
      public static Delegate? CreateDelegate(Type type, Type target, string method, bool ignoreCase, bool throwOnBindFailure);
      public object? DynamicInvoke(params object?[]? args);
      protected virtual object? DynamicInvokeImpl(object?[]? args);
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public virtual Delegate[] GetInvocationList();
      protected virtual MethodInfo GetMethodImpl();
      public virtual void GetObjectData(SerializationInfo info, StreamingContext context);
      public static bool operator ==(Delegate? d1, Delegate? d2);
      public static bool operator !=(Delegate? d1, Delegate? d2);
      public static Delegate? Remove(Delegate? source, Delegate? value);
      public static Delegate? RemoveAll(Delegate? source, Delegate? value);
      protected virtual Delegate? RemoveImpl(Delegate d);
    }
    public class DivideByZeroException : ArithmeticException {
      public DivideByZeroException();
      protected DivideByZeroException(SerializationInfo info, StreamingContext context);
      public DivideByZeroException(string? message);
      public DivideByZeroException(string? message, Exception? innerException);
    }
    public readonly struct Double : IComparable, IComparable<double>, IConvertible, IEquatable<double>, IFormattable {
      public const double Epsilon = 4.94065645841247E-324;
      public const double MaxValue = 1.7976931348623157E+308;
      public const double MinValue = -1.7976931348623157E+308;
      public const double NaN = 0.0 / 0.0;
      public const double NegativeInfinity = -1.0 / 0.0;
      public const double PositiveInfinity = 1.0 / 0.0;
      public int CompareTo(Double value);
      public int CompareTo(object? value);
      public bool Equals(Double obj);
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public TypeCode GetTypeCode();
      public static bool IsFinite(Double d);
      public static bool IsInfinity(Double d);
      public static bool IsNaN(Double d);
      public static bool IsNegative(Double d);
      public static bool IsNegativeInfinity(Double d);
      public static bool IsNormal(Double d);
      public static bool IsPositiveInfinity(Double d);
      public static bool IsSubnormal(Double d);
      public static bool operator ==(Double left, Double right);
      public static bool operator >(Double left, Double right);
      public static bool operator >=(Double left, Double right);
      public static bool operator !=(Double left, Double right);
      public static bool operator <(Double left, Double right);
      public static bool operator <=(Double left, Double right);
      public static Double Parse(ReadOnlySpan<char> s, NumberStyles style = NumberStyles.AllowDecimalPoint | NumberStyles.AllowExponent | NumberStyles.AllowLeadingSign | NumberStyles.AllowLeadingWhite | NumberStyles.AllowThousands | NumberStyles.AllowTrailingWhite, IFormatProvider? provider = null);
      public static Double Parse(string s);
      public static Double Parse(string s, NumberStyles style);
      public static Double Parse(string s, NumberStyles style, IFormatProvider? provider);
      public static Double Parse(string s, IFormatProvider? provider);
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      Double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      public override string ToString();
      public string ToString(IFormatProvider? provider);
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? provider);
      public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? provider = null);
      public static bool TryParse(ReadOnlySpan<char> s, out Double result);
      public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out Double result);
      public static bool TryParse(string? s, out Double result);
      public static bool TryParse(string? s, NumberStyles style, IFormatProvider? provider, out Double result);
    }
    public class DuplicateWaitObjectException : ArgumentException {
      public DuplicateWaitObjectException();
      protected DuplicateWaitObjectException(SerializationInfo info, StreamingContext context);
      public DuplicateWaitObjectException(string? parameterName);
      public DuplicateWaitObjectException(string? message, Exception? innerException);
      public DuplicateWaitObjectException(string? parameterName, string? message);
    }
    public class EntryPointNotFoundException : TypeLoadException {
      public EntryPointNotFoundException();
      protected EntryPointNotFoundException(SerializationInfo info, StreamingContext context);
      public EntryPointNotFoundException(string? message);
      public EntryPointNotFoundException(string? message, Exception? inner);
    }
    public abstract class Enum : ValueType, IComparable, IConvertible, IFormattable {
      protected Enum();
      public int CompareTo(object? target);
      public override bool Equals(object? obj);
      public static string Format(Type enumType, object value, string format);
      public override int GetHashCode();
      public static string? GetName(Type enumType, object value);
      public static string[] GetNames(Type enumType);
      public TypeCode GetTypeCode();
      public static Type GetUnderlyingType(Type enumType);
      public static Array GetValues(Type enumType);
      public bool HasFlag(Enum flag);
      public static bool IsDefined(Type enumType, object value);
      public static object Parse(Type enumType, string value);
      public static object Parse(Type enumType, string value, bool ignoreCase);
      public static TEnum Parse<TEnum>(string value) where TEnum : struct;
      public static TEnum Parse<TEnum>(string value, bool ignoreCase) where TEnum : struct;
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      public static object ToObject(Type enumType, byte value);
      public static object ToObject(Type enumType, short value);
      public static object ToObject(Type enumType, int value);
      public static object ToObject(Type enumType, long value);
      public static object ToObject(Type enumType, object value);
      public static object ToObject(Type enumType, sbyte value);
      public static object ToObject(Type enumType, ushort value);
      public static object ToObject(Type enumType, uint value);
      public static object ToObject(Type enumType, ulong value);
      public override string ToString();
      public string ToString(IFormatProvider? provider);
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? provider);
      public static bool TryParse(Type enumType, string? value, bool ignoreCase, out object? result);
      public static bool TryParse(Type enumType, string? value, out object? result);
      public static bool TryParse<TEnum>(string? value, bool ignoreCase, out TEnum result) where TEnum : struct;
      public static bool TryParse<TEnum>(string? value, out TEnum result) where TEnum : struct;
    }
    public class EventArgs {
      public static readonly EventArgs Empty;
      public EventArgs();
    }
    public delegate void EventHandler(object? sender, EventArgs e);
    public delegate void EventHandler<TEventArgs>(object? sender, TEventArgs e);
    public class Exception : ISerializable {
      public Exception();
      protected Exception(SerializationInfo info, StreamingContext context);
      public Exception(string? message);
      public Exception(string? message, Exception? innerException);
      public virtual IDictionary Data { get; }
      public virtual string? HelpLink { get; set; }
      public int HResult { get; set; }
      public Exception? InnerException { get; }
      public virtual string Message { get; }
      public virtual string? Source { get; set; }
      public virtual string? StackTrace { get; }
      public MethodBase? TargetSite { get; }
      protected event EventHandler<SafeSerializationEventArgs> SerializeObjectState;
      public virtual Exception GetBaseException();
      public virtual void GetObjectData(SerializationInfo info, StreamingContext context);
      public new Type GetType();
      public override string ToString();
    }
    public sealed class ExecutionEngineException : SystemException {
      public ExecutionEngineException();
      public ExecutionEngineException(string? message);
      public ExecutionEngineException(string? message, Exception? innerException);
    }
    public class FieldAccessException : MemberAccessException {
      public FieldAccessException();
      protected FieldAccessException(SerializationInfo info, StreamingContext context);
      public FieldAccessException(string? message);
      public FieldAccessException(string? message, Exception? inner);
    }
    public class FileStyleUriParser : UriParser {
      public FileStyleUriParser();
    }
    public class FlagsAttribute : Attribute {
      public FlagsAttribute();
    }
    public class FormatException : SystemException {
      public FormatException();
      protected FormatException(SerializationInfo info, StreamingContext context);
      public FormatException(string? message);
      public FormatException(string? message, Exception? innerException);
    }
    public abstract class FormattableString : IFormattable {
      protected FormattableString();
      public abstract int ArgumentCount { get; }
      public abstract string Format { get; }
      public static string CurrentCulture(FormattableString formattable);
      public abstract object? GetArgument(int index);
      public abstract object?[] GetArguments();
      public static string Invariant(FormattableString formattable);
      string System.IFormattable.ToString(string? ignored, IFormatProvider? formatProvider);
      public override string ToString();
      public abstract string ToString(IFormatProvider? formatProvider);
    }
    public class FtpStyleUriParser : UriParser {
      public FtpStyleUriParser();
    }
    public delegate TResult Func<out TResult>();
    public delegate TResult Func<in T, out TResult>(T arg);
    public delegate TResult Func<in T1, in T2, out TResult>(T1 arg1, T2 arg2);
    public delegate TResult Func<in T1, in T2, in T3, out TResult>(T1 arg1, T2 arg2, T3 arg3);
    public delegate TResult Func<in T1, in T2, in T3, in T4, out TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4);
    public delegate TResult Func<in T1, in T2, in T3, in T4, in T5, out TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5);
    public delegate TResult Func<in T1, in T2, in T3, in T4, in T5, in T6, out TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6);
    public delegate TResult Func<in T1, in T2, in T3, in T4, in T5, in T6, in T7, out TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7);
    public delegate TResult Func<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, out TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8);
    public delegate TResult Func<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, out TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9);
    public delegate TResult Func<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, out TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10);
    public delegate TResult Func<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, in T11, out TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10, T11 arg11);
    public delegate TResult Func<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, in T11, in T12, out TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10, T11 arg11, T12 arg12);
    public delegate TResult Func<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, in T11, in T12, in T13, out TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10, T11 arg11, T12 arg12, T13 arg13);
    public delegate TResult Func<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, in T11, in T12, in T13, in T14, out TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10, T11 arg11, T12 arg12, T13 arg13, T14 arg14);
    public delegate TResult Func<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, in T11, in T12, in T13, in T14, in T15, out TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10, T11 arg11, T12 arg12, T13 arg13, T14 arg14, T15 arg15);
    public delegate TResult Func<in T1, in T2, in T3, in T4, in T5, in T6, in T7, in T8, in T9, in T10, in T11, in T12, in T13, in T14, in T15, in T16, out TResult>(T1 arg1, T2 arg2, T3 arg3, T4 arg4, T5 arg5, T6 arg6, T7 arg7, T8 arg8, T9 arg9, T10 arg10, T11 arg11, T12 arg12, T13 arg13, T14 arg14, T15 arg15, T16 arg16);
    public static class GC {
      public static int MaxGeneration { get; }
      public static void AddMemoryPressure(long bytesAllocated);
      public static void CancelFullGCNotification();
      public static void Collect();
      public static void Collect(int generation);
      public static void Collect(int generation, GCCollectionMode mode);
      public static void Collect(int generation, GCCollectionMode mode, bool blocking);
      public static void Collect(int generation, GCCollectionMode mode, bool blocking, bool compacting);
      public static int CollectionCount(int generation);
      public static void EndNoGCRegion();
      public static long GetAllocatedBytesForCurrentThread();
      public static GCMemoryInfo GetGCMemoryInfo();
      public static int GetGeneration(object obj);
      public static int GetGeneration(WeakReference wo);
      public static long GetTotalAllocatedBytes(bool precise = false);
      public static long GetTotalMemory(bool forceFullCollection);
      public static void KeepAlive(object? obj);
      public static void RegisterForFullGCNotification(int maxGenerationThreshold, int largeObjectHeapThreshold);
      public static void RemoveMemoryPressure(long bytesAllocated);
      public static void ReRegisterForFinalize(object obj);
      public static void SuppressFinalize(object obj);
      public static bool TryStartNoGCRegion(long totalSize);
      public static bool TryStartNoGCRegion(long totalSize, bool disallowFullBlockingGC);
      public static bool TryStartNoGCRegion(long totalSize, long lohSize);
      public static bool TryStartNoGCRegion(long totalSize, long lohSize, bool disallowFullBlockingGC);
      public static GCNotificationStatus WaitForFullGCApproach();
      public static GCNotificationStatus WaitForFullGCApproach(int millisecondsTimeout);
      public static GCNotificationStatus WaitForFullGCComplete();
      public static GCNotificationStatus WaitForFullGCComplete(int millisecondsTimeout);
      public static void WaitForPendingFinalizers();
    }
    public enum GCCollectionMode {
      Default = 0,
      Forced = 1,
      Optimized = 2,
    }
    public readonly struct GCMemoryInfo {
      public long FragmentedBytes { get; }
      public long HeapSizeBytes { get; }
      public long HighMemoryLoadThresholdBytes { get; }
      public long MemoryLoadBytes { get; }
      public long TotalAvailableMemoryBytes { get; }
    }
    public enum GCNotificationStatus {
      Canceled = 2,
      Failed = 1,
      NotApplicable = 4,
      Succeeded = 0,
      Timeout = 3,
    }
    public class GenericUriParser : UriParser {
      public GenericUriParser(GenericUriParserOptions options);
    }
    public enum GenericUriParserOptions {
      AllowEmptyAuthority = 2,
      Default = 0,
      DontCompressPath = 128,
      DontConvertPathBackslashes = 64,
      DontUnescapePathDotsAndSlashes = 256,
      GenericAuthority = 1,
      Idn = 512,
      IriParsing = 1024,
      NoFragment = 32,
      NoPort = 8,
      NoQuery = 16,
      NoUserInfo = 4,
    }
    public class GopherStyleUriParser : UriParser {
      public GopherStyleUriParser();
    }
    public struct Guid : IComparable, IComparable<Guid>, IEquatable<Guid>, IFormattable {
      public static readonly Guid Empty;
      public Guid(byte[] b);
      public Guid(int a, short b, short c, byte d, byte e, byte f, byte g, byte h, byte i, byte j, byte k);
      public Guid(int a, short b, short c, byte[] d);
      public Guid(ReadOnlySpan<byte> b);
      public Guid(string g);
      public Guid(uint a, ushort b, ushort c, byte d, byte e, byte f, byte g, byte h, byte i, byte j, byte k);
      public int CompareTo(Guid value);
      public int CompareTo(object? value);
      public bool Equals(Guid g);
      public override bool Equals(object? o);
      public override int GetHashCode();
      public static Guid NewGuid();
      public static bool operator ==(Guid a, Guid b);
      public static bool operator !=(Guid a, Guid b);
      public static Guid Parse(ReadOnlySpan<char> input);
      public static Guid Parse(string input);
      public static Guid ParseExact(ReadOnlySpan<char> input, ReadOnlySpan<char> format);
      public static Guid ParseExact(string input, string format);
      public byte[] ToByteArray();
      public override string ToString();
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? provider);
      public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>));
      public static bool TryParse(ReadOnlySpan<char> input, out Guid result);
      public static bool TryParse(string input, out Guid result);
      public static bool TryParseExact(ReadOnlySpan<char> input, ReadOnlySpan<char> format, out Guid result);
      public static bool TryParseExact(string? input, string? format, out Guid result);
      public bool TryWriteBytes(Span<byte> destination);
    }
    public struct HashCode {
      public void Add<T>(T value);
      public void Add<T>(T value, IEqualityComparer<T>? comparer);
      public static int Combine<T1, T2, T3, T4, T5, T6, T7, T8>(T1 value1, T2 value2, T3 value3, T4 value4, T5 value5, T6 value6, T7 value7, T8 value8);
      public static int Combine<T1, T2, T3, T4, T5, T6, T7>(T1 value1, T2 value2, T3 value3, T4 value4, T5 value5, T6 value6, T7 value7);
      public static int Combine<T1, T2, T3, T4, T5, T6>(T1 value1, T2 value2, T3 value3, T4 value4, T5 value5, T6 value6);
      public static int Combine<T1, T2, T3, T4, T5>(T1 value1, T2 value2, T3 value3, T4 value4, T5 value5);
      public static int Combine<T1, T2, T3, T4>(T1 value1, T2 value2, T3 value3, T4 value4);
      public static int Combine<T1, T2, T3>(T1 value1, T2 value2, T3 value3);
      public static int Combine<T1, T2>(T1 value1, T2 value2);
      public static int Combine<T1>(T1 value1);
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public int ToHashCode();
    }
    public class HttpStyleUriParser : UriParser {
      public HttpStyleUriParser();
    }
    public interface IAsyncDisposable {
      ValueTask DisposeAsync();
    }
    public interface IAsyncResult {
      object? AsyncState { get; }
      WaitHandle AsyncWaitHandle { get; }
      bool CompletedSynchronously { get; }
      bool IsCompleted { get; }
    }
    public interface ICloneable {
      object Clone();
    }
    public interface IComparable {
      int CompareTo(object? obj);
    }
    public interface IComparable<in T> {
      int CompareTo(T other);
    }
    public interface IConvertible {
      TypeCode GetTypeCode();
      bool ToBoolean(IFormatProvider? provider);
      byte ToByte(IFormatProvider? provider);
      char ToChar(IFormatProvider? provider);
      DateTime ToDateTime(IFormatProvider? provider);
      decimal ToDecimal(IFormatProvider? provider);
      double ToDouble(IFormatProvider? provider);
      short ToInt16(IFormatProvider? provider);
      int ToInt32(IFormatProvider? provider);
      long ToInt64(IFormatProvider? provider);
      sbyte ToSByte(IFormatProvider? provider);
      float ToSingle(IFormatProvider? provider);
      string ToString(IFormatProvider? provider);
      object ToType(Type conversionType, IFormatProvider? provider);
      ushort ToUInt16(IFormatProvider? provider);
      uint ToUInt32(IFormatProvider? provider);
      ulong ToUInt64(IFormatProvider? provider);
    }
    public interface ICustomFormatter {
      string Format(string? format, object? arg, IFormatProvider? formatProvider);
    }
    public interface IDisposable {
      void Dispose();
    }
    public interface IEquatable<T> {
      bool Equals(T other);
    }
    public interface IFormatProvider {
      object? GetFormat(Type? formatType);
    }
    public interface IFormattable {
      string ToString(string? format, IFormatProvider? formatProvider);
    }
    public readonly struct Index : IEquatable<Index> {
      public Index(int value, bool fromEnd = false);
      public static Index End { get; }
      public bool IsFromEnd { get; }
      public static Index Start { get; }
      public int Value { get; }
      public bool Equals(Index other);
      public override bool Equals(object? value);
      public static Index FromEnd(int value);
      public static Index FromStart(int value);
      public override int GetHashCode();
      public int GetOffset(int length);
      public static implicit operator Index (int value);
      public override string ToString();
    }
    public sealed class IndexOutOfRangeException : SystemException {
      public IndexOutOfRangeException();
      public IndexOutOfRangeException(string? message);
      public IndexOutOfRangeException(string? message, Exception? innerException);
    }
    public sealed class InsufficientExecutionStackException : SystemException {
      public InsufficientExecutionStackException();
      public InsufficientExecutionStackException(string? message);
      public InsufficientExecutionStackException(string? message, Exception? innerException);
    }
    public sealed class InsufficientMemoryException : OutOfMemoryException {
      public InsufficientMemoryException();
      public InsufficientMemoryException(string? message);
      public InsufficientMemoryException(string? message, Exception? innerException);
    }
    public readonly struct Int16 : IComparable, IComparable<short>, IConvertible, IEquatable<short>, IFormattable {
      public const short MaxValue = (short)32767;
      public const short MinValue = (short)-32768;
      public int CompareTo(Int16 value);
      public int CompareTo(object? value);
      public bool Equals(Int16 obj);
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public TypeCode GetTypeCode();
      public static Int16 Parse(ReadOnlySpan<char> s, NumberStyles style = NumberStyles.Integer, IFormatProvider? provider = null);
      public static Int16 Parse(string s);
      public static Int16 Parse(string s, NumberStyles style);
      public static Int16 Parse(string s, NumberStyles style, IFormatProvider? provider);
      public static Int16 Parse(string s, IFormatProvider? provider);
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      Int16 System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      public override string ToString();
      public string ToString(IFormatProvider? provider);
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? provider);
      public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? provider = null);
      public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out Int16 result);
      public static bool TryParse(ReadOnlySpan<char> s, out Int16 result);
      public static bool TryParse(string? s, NumberStyles style, IFormatProvider? provider, out Int16 result);
      public static bool TryParse(string? s, out Int16 result);
    }
    public readonly struct Int32 : IComparable, IComparable<int>, IConvertible, IEquatable<int>, IFormattable {
      public const int MaxValue = 2147483647;
      public const int MinValue = -2147483648;
      public Int32 CompareTo(Int32 value);
      public Int32 CompareTo(object? value);
      public bool Equals(Int32 obj);
      public override bool Equals(object? obj);
      public override Int32 GetHashCode();
      public TypeCode GetTypeCode();
      public static Int32 Parse(ReadOnlySpan<char> s, NumberStyles style = NumberStyles.Integer, IFormatProvider? provider = null);
      public static Int32 Parse(string s);
      public static Int32 Parse(string s, NumberStyles style);
      public static Int32 Parse(string s, NumberStyles style, IFormatProvider? provider);
      public static Int32 Parse(string s, IFormatProvider? provider);
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      Int32 System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      public override string ToString();
      public string ToString(IFormatProvider? provider);
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? provider);
      public bool TryFormat(Span<char> destination, out Int32 charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? provider = null);
      public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out Int32 result);
      public static bool TryParse(ReadOnlySpan<char> s, out Int32 result);
      public static bool TryParse(string? s, NumberStyles style, IFormatProvider? provider, out Int32 result);
      public static bool TryParse(string? s, out Int32 result);
    }
    public readonly struct Int64 : IComparable, IComparable<long>, IConvertible, IEquatable<long>, IFormattable {
      public const long MaxValue = (long)9223372036854775807;
      public const long MinValue = (long)-9223372036854775808;
      public int CompareTo(Int64 value);
      public int CompareTo(object? value);
      public bool Equals(Int64 obj);
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public TypeCode GetTypeCode();
      public static Int64 Parse(ReadOnlySpan<char> s, NumberStyles style = NumberStyles.Integer, IFormatProvider? provider = null);
      public static Int64 Parse(string s);
      public static Int64 Parse(string s, NumberStyles style);
      public static Int64 Parse(string s, NumberStyles style, IFormatProvider? provider);
      public static Int64 Parse(string s, IFormatProvider? provider);
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      Int64 System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      public override string ToString();
      public string ToString(IFormatProvider? provider);
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? provider);
      public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? provider = null);
      public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out Int64 result);
      public static bool TryParse(ReadOnlySpan<char> s, out Int64 result);
      public static bool TryParse(string? s, NumberStyles style, IFormatProvider? provider, out Int64 result);
      public static bool TryParse(string? s, out Int64 result);
    }
    public readonly struct IntPtr : IEquatable<IntPtr>, ISerializable {
      public static readonly IntPtr Zero;
      public IntPtr(int value);
      public IntPtr(long value);
      public unsafe IntPtr(void* value);
      public static int Size { get; }
      public static IntPtr Add(IntPtr pointer, int offset);
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public static IntPtr operator +(IntPtr pointer, int offset);
      public static bool operator ==(IntPtr value1, IntPtr value2);
      public static explicit operator IntPtr (int value);
      public static explicit operator IntPtr (long value);
      public static explicit operator int (IntPtr value);
      public static explicit operator long (IntPtr value);
      public unsafe static explicit operator void* (IntPtr value);
      public unsafe static explicit operator IntPtr (void* value);
      public static bool operator !=(IntPtr value1, IntPtr value2);
      public static IntPtr operator -(IntPtr pointer, int offset);
      public static IntPtr Subtract(IntPtr pointer, int offset);
      bool System.IEquatable<System.IntPtr>.Equals(IntPtr other);
      void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
      public int ToInt32();
      public long ToInt64();
      public unsafe void* ToPointer();
      public override string ToString();
      public string ToString(string format);
    }
    public class InvalidCastException : SystemException {
      public InvalidCastException();
      protected InvalidCastException(SerializationInfo info, StreamingContext context);
      public InvalidCastException(string? message);
      public InvalidCastException(string? message, Exception? innerException);
      public InvalidCastException(string? message, int errorCode);
    }
    public class InvalidOperationException : SystemException {
      public InvalidOperationException();
      protected InvalidOperationException(SerializationInfo info, StreamingContext context);
      public InvalidOperationException(string? message);
      public InvalidOperationException(string? message, Exception? innerException);
    }
    public sealed class InvalidProgramException : SystemException {
      public InvalidProgramException();
      public InvalidProgramException(string? message);
      public InvalidProgramException(string? message, Exception? inner);
    }
    public class InvalidTimeZoneException : Exception {
      public InvalidTimeZoneException();
      protected InvalidTimeZoneException(SerializationInfo info, StreamingContext context);
      public InvalidTimeZoneException(string? message);
      public InvalidTimeZoneException(string? message, Exception? innerException);
    }
    public interface IObservable<out T> {
      IDisposable Subscribe(IObserver<T> observer);
    }
    public interface IObserver<in T> {
      void OnCompleted();
      void OnError(Exception error);
      void OnNext(T value);
    }
    public interface IProgress<in T> {
      void Report(T value);
    }
    public class Lazy<T> {
      public Lazy();
      public Lazy(bool isThreadSafe);
      public Lazy(Func<T> valueFactory);
      public Lazy(Func<T> valueFactory, bool isThreadSafe);
      public Lazy(Func<T> valueFactory, LazyThreadSafetyMode mode);
      public Lazy(LazyThreadSafetyMode mode);
      public Lazy(T value);
      public bool IsValueCreated { get; }
      public T Value { get; }
      public override string? ToString();
    }
    public class Lazy<T, TMetadata> : Lazy<T> {
      public Lazy(Func<T> valueFactory, TMetadata metadata);
      public Lazy(Func<T> valueFactory, TMetadata metadata, bool isThreadSafe);
      public Lazy(Func<T> valueFactory, TMetadata metadata, LazyThreadSafetyMode mode);
      public Lazy(TMetadata metadata);
      public Lazy(TMetadata metadata, bool isThreadSafe);
      public Lazy(TMetadata metadata, LazyThreadSafetyMode mode);
      public TMetadata Metadata { get; }
    }
    public class LdapStyleUriParser : UriParser {
      public LdapStyleUriParser();
    }
    public abstract class MarshalByRefObject {
      protected MarshalByRefObject();
      public object GetLifetimeService();
      public virtual object InitializeLifetimeService();
      protected MarshalByRefObject MemberwiseClone(bool cloneIdentity);
    }
    public class MemberAccessException : SystemException {
      public MemberAccessException();
      protected MemberAccessException(SerializationInfo info, StreamingContext context);
      public MemberAccessException(string? message);
      public MemberAccessException(string? message, Exception? inner);
    }
    public readonly struct Memory<T> : IEquatable<Memory<T>> {
      public Memory(T[]? array);
      public Memory(T[]? array, int start, int length);
      public static Memory<T> Empty { get; }
      public bool IsEmpty { get; }
      public int Length { get; }
      public Span<T> Span { get; }
      public void CopyTo(Memory<T> destination);
      public bool Equals(Memory<T> other);
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public static implicit operator Memory<T> (ArraySegment<T> segment);
      public static implicit operator ReadOnlyMemory<T> (Memory<T> memory);
      public static implicit operator Memory<T> (T[]? array);
      public MemoryHandle Pin();
      public Memory<T> Slice(int start);
      public Memory<T> Slice(int start, int length);
      public T[] ToArray();
      public override string ToString();
      public bool TryCopyTo(Memory<T> destination);
    }
    public class MethodAccessException : MemberAccessException {
      public MethodAccessException();
      protected MethodAccessException(SerializationInfo info, StreamingContext context);
      public MethodAccessException(string? message);
      public MethodAccessException(string? message, Exception? inner);
    }
    public enum MidpointRounding {
      AwayFromZero = 1,
      ToEven = 0,
      ToNegativeInfinity = 3,
      ToPositiveInfinity = 4,
      ToZero = 2,
    }
    public class MissingFieldException : MissingMemberException, ISerializable {
      public MissingFieldException();
      protected MissingFieldException(SerializationInfo info, StreamingContext context);
      public MissingFieldException(string? message);
      public MissingFieldException(string? message, Exception? inner);
      public MissingFieldException(string? className, string? fieldName);
      public override string Message { get; }
    }
    public class MissingMemberException : MemberAccessException, ISerializable {
      protected byte[]? Signature;
      protected string? ClassName;
      protected string? MemberName;
      public MissingMemberException();
      protected MissingMemberException(SerializationInfo info, StreamingContext context);
      public MissingMemberException(string? message);
      public MissingMemberException(string? message, Exception? inner);
      public MissingMemberException(string? className, string? memberName);
      public override string Message { get; }
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
    }
    public class MissingMethodException : MissingMemberException {
      public MissingMethodException();
      protected MissingMethodException(SerializationInfo info, StreamingContext context);
      public MissingMethodException(string? message);
      public MissingMethodException(string? message, Exception? inner);
      public MissingMethodException(string? className, string? methodName);
      public override string Message { get; }
    }
    public struct ModuleHandle {
      public static readonly ModuleHandle EmptyHandle;
      public int MDStreamVersion { get; }
      public bool Equals(ModuleHandle handle);
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public RuntimeFieldHandle GetRuntimeFieldHandleFromMetadataToken(int fieldToken);
      public RuntimeMethodHandle GetRuntimeMethodHandleFromMetadataToken(int methodToken);
      public RuntimeTypeHandle GetRuntimeTypeHandleFromMetadataToken(int typeToken);
      public static bool operator ==(ModuleHandle left, ModuleHandle right);
      public static bool operator !=(ModuleHandle left, ModuleHandle right);
      public RuntimeFieldHandle ResolveFieldHandle(int fieldToken);
      public RuntimeFieldHandle ResolveFieldHandle(int fieldToken, RuntimeTypeHandle[]? typeInstantiationContext, RuntimeTypeHandle[]? methodInstantiationContext);
      public RuntimeMethodHandle ResolveMethodHandle(int methodToken);
      public RuntimeMethodHandle ResolveMethodHandle(int methodToken, RuntimeTypeHandle[]? typeInstantiationContext, RuntimeTypeHandle[]? methodInstantiationContext);
      public RuntimeTypeHandle ResolveTypeHandle(int typeToken);
      public RuntimeTypeHandle ResolveTypeHandle(int typeToken, RuntimeTypeHandle[]? typeInstantiationContext, RuntimeTypeHandle[]? methodInstantiationContext);
    }
    public sealed class MTAThreadAttribute : Attribute {
      public MTAThreadAttribute();
    }
    public abstract class MulticastDelegate : Delegate {
      protected MulticastDelegate(object target, string method);
      protected MulticastDelegate(Type target, string method);
      protected sealed override Delegate CombineImpl(Delegate? follow);
      public sealed override bool Equals(object? obj);
      public sealed override int GetHashCode();
      public sealed override Delegate[] GetInvocationList();
      protected override MethodInfo GetMethodImpl();
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
      public static bool operator ==(MulticastDelegate? d1, MulticastDelegate? d2);
      public static bool operator !=(MulticastDelegate? d1, MulticastDelegate? d2);
      protected sealed override Delegate? RemoveImpl(Delegate value);
    }
    public sealed class MulticastNotSupportedException : SystemException {
      public MulticastNotSupportedException();
      public MulticastNotSupportedException(string? message);
      public MulticastNotSupportedException(string? message, Exception? inner);
    }
    public class NetPipeStyleUriParser : UriParser {
      public NetPipeStyleUriParser();
    }
    public class NetTcpStyleUriParser : UriParser {
      public NetTcpStyleUriParser();
    }
    public class NewsStyleUriParser : UriParser {
      public NewsStyleUriParser();
    }
    public sealed class NonSerializedAttribute : Attribute {
      public NonSerializedAttribute();
    }
    public class NotFiniteNumberException : ArithmeticException {
      public NotFiniteNumberException();
      public NotFiniteNumberException(double offendingNumber);
      protected NotFiniteNumberException(SerializationInfo info, StreamingContext context);
      public NotFiniteNumberException(string? message);
      public NotFiniteNumberException(string? message, double offendingNumber);
      public NotFiniteNumberException(string? message, double offendingNumber, Exception? innerException);
      public NotFiniteNumberException(string? message, Exception? innerException);
      public double OffendingNumber { get; }
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
    }
    public class NotImplementedException : SystemException {
      public NotImplementedException();
      protected NotImplementedException(SerializationInfo info, StreamingContext context);
      public NotImplementedException(string? message);
      public NotImplementedException(string? message, Exception? inner);
    }
    public class NotSupportedException : SystemException {
      public NotSupportedException();
      protected NotSupportedException(SerializationInfo info, StreamingContext context);
      public NotSupportedException(string? message);
      public NotSupportedException(string? message, Exception? innerException);
    }
    public static class Nullable {
      public static int Compare<T>(T? n1, T? n2) where T : struct;
      public static bool Equals<T>(T? n1, T? n2) where T : struct;
      public static Type? GetUnderlyingType(Type nullableType);
    }
    public struct Nullable<T> where T : struct {
      public Nullable(T value);
      public bool HasValue { get; }
      public T Value { get; }
      public override bool Equals(object? other);
      public override int GetHashCode();
      public T GetValueOrDefault();
      public T GetValueOrDefault(T defaultValue);
      public static explicit operator T (T? value);
      public static implicit operator T? (T value);
      public override string? ToString();
    }
    public class NullReferenceException : SystemException {
      public NullReferenceException();
      protected NullReferenceException(SerializationInfo info, StreamingContext context);
      public NullReferenceException(string? message);
      public NullReferenceException(string? message, Exception? innerException);
    }
    public class Object {
      public Object();
      public virtual bool Equals(Object? obj);
      public static bool Equals(Object? objA, Object? objB);
      ~Object();
      public virtual int GetHashCode();
      public Type GetType();
      protected Object MemberwiseClone();
      public static bool ReferenceEquals(Object? objA, Object? objB);
      public virtual string? ToString();
    }
    public class ObjectDisposedException : InvalidOperationException {
      protected ObjectDisposedException(SerializationInfo info, StreamingContext context);
      public ObjectDisposedException(string? objectName);
      public ObjectDisposedException(string? message, Exception? innerException);
      public ObjectDisposedException(string? objectName, string? message);
      public override string Message { get; }
      public string ObjectName { get; }
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
    }
    public sealed class ObsoleteAttribute : Attribute {
      public ObsoleteAttribute();
      public ObsoleteAttribute(string? message);
      public ObsoleteAttribute(string? message, bool error);
      public bool IsError { get; }
      public string? Message { get; }
    }
    public class OutOfMemoryException : SystemException {
      public OutOfMemoryException();
      protected OutOfMemoryException(SerializationInfo info, StreamingContext context);
      public OutOfMemoryException(string? message);
      public OutOfMemoryException(string? message, Exception? innerException);
    }
    public class OverflowException : ArithmeticException {
      public OverflowException();
      protected OverflowException(SerializationInfo info, StreamingContext context);
      public OverflowException(string? message);
      public OverflowException(string? message, Exception? innerException);
    }
    public sealed class ParamArrayAttribute : Attribute {
      public ParamArrayAttribute();
    }
    public class PlatformNotSupportedException : NotSupportedException {
      public PlatformNotSupportedException();
      protected PlatformNotSupportedException(SerializationInfo info, StreamingContext context);
      public PlatformNotSupportedException(string? message);
      public PlatformNotSupportedException(string? message, Exception? inner);
    }
    public delegate bool Predicate<in T>(T obj);
    public readonly struct Range : IEquatable<Range> {
      public Range(Index start, Index end);
      public static Range All { get; }
      public Index End { get; }
      public Index Start { get; }
      public static Range EndAt(Index end);
      public override bool Equals(object? value);
      public bool Equals(Range other);
      public override int GetHashCode();
      public (int Offset, int Length) GetOffsetAndLength(int length);
      public static Range StartAt(Index start);
      public override string ToString();
    }
    public class RankException : SystemException {
      public RankException();
      protected RankException(SerializationInfo info, StreamingContext context);
      public RankException(string? message);
      public RankException(string? message, Exception? innerException);
    }
    public readonly struct ReadOnlyMemory<T> : IEquatable<ReadOnlyMemory<T>> {
      public ReadOnlyMemory(T[]? array);
      public ReadOnlyMemory(T[]? array, int start, int length);
      public static ReadOnlyMemory<T> Empty { get; }
      public bool IsEmpty { get; }
      public int Length { get; }
      public ReadOnlySpan<T> Span { get; }
      public void CopyTo(Memory<T> destination);
      public override bool Equals(object? obj);
      public bool Equals(ReadOnlyMemory<T> other);
      public override int GetHashCode();
      public static implicit operator ReadOnlyMemory<T> (ArraySegment<T> segment);
      public static implicit operator ReadOnlyMemory<T> (T[]? array);
      public MemoryHandle Pin();
      public ReadOnlyMemory<T> Slice(int start);
      public ReadOnlyMemory<T> Slice(int start, int length);
      public T[] ToArray();
      public override string ToString();
      public bool TryCopyTo(Memory<T> destination);
    }
    public readonly ref struct ReadOnlySpan<T> {
      public unsafe ReadOnlySpan(void* pointer, int length);
      public ReadOnlySpan(T[]? array);
      public ReadOnlySpan(T[]? array, int start, int length);
      public static ReadOnlySpan<T> Empty { get; }
      public bool IsEmpty { get; }
      public int Length { get; }
      public ref readonly T this[int index] { get; }
      public void CopyTo(Span<T> destination);
      public override bool Equals(object? obj);
      public ReadOnlySpan<T>.Enumerator GetEnumerator();
      public override int GetHashCode();
      public ref readonly T GetPinnableReference();
      public static bool operator ==(ReadOnlySpan<T> left, ReadOnlySpan<T> right);
      public static implicit operator ReadOnlySpan<T> (ArraySegment<T> segment);
      public static implicit operator ReadOnlySpan<T> (T[]? array);
      public static bool operator !=(ReadOnlySpan<T> left, ReadOnlySpan<T> right);
      public ReadOnlySpan<T> Slice(int start);
      public ReadOnlySpan<T> Slice(int start, int length);
      public T[] ToArray();
      public override string ToString();
      public bool TryCopyTo(Span<T> destination);
      public ref struct Enumerator {
        public ref readonly T Current { get; }
        public bool MoveNext();
      }
    }
    public class ResolveEventArgs : EventArgs {
      public ResolveEventArgs(string? name);
      public ResolveEventArgs(string? name, Assembly? requestingAssembly);
      public string? Name { get; }
      public Assembly? RequestingAssembly { get; }
    }
    public ref struct RuntimeArgumentHandle {
    }
    public struct RuntimeFieldHandle : ISerializable {
      public IntPtr Value { get; }
      public override bool Equals(object? obj);
      public bool Equals(RuntimeFieldHandle handle);
      public override int GetHashCode();
      public void GetObjectData(SerializationInfo info, StreamingContext context);
      public static bool operator ==(RuntimeFieldHandle left, RuntimeFieldHandle right);
      public static bool operator !=(RuntimeFieldHandle left, RuntimeFieldHandle right);
    }
    public struct RuntimeMethodHandle : ISerializable {
      public IntPtr Value { get; }
      public override bool Equals(object? obj);
      public bool Equals(RuntimeMethodHandle handle);
      public IntPtr GetFunctionPointer();
      public override int GetHashCode();
      public void GetObjectData(SerializationInfo info, StreamingContext context);
      public static bool operator ==(RuntimeMethodHandle left, RuntimeMethodHandle right);
      public static bool operator !=(RuntimeMethodHandle left, RuntimeMethodHandle right);
    }
    public struct RuntimeTypeHandle : ISerializable {
      public IntPtr Value { get; }
      public override bool Equals(object? obj);
      public bool Equals(RuntimeTypeHandle handle);
      public override int GetHashCode();
      public ModuleHandle GetModuleHandle();
      public void GetObjectData(SerializationInfo info, StreamingContext context);
      public static bool operator ==(object left, RuntimeTypeHandle right);
      public static bool operator ==(RuntimeTypeHandle left, object right);
      public static bool operator !=(object left, RuntimeTypeHandle right);
      public static bool operator !=(RuntimeTypeHandle left, object right);
    }
    public readonly struct SByte : IComparable, IComparable<sbyte>, IConvertible, IEquatable<sbyte>, IFormattable {
      public const sbyte MaxValue = (sbyte)127;
      public const sbyte MinValue = (sbyte)-128;
      public int CompareTo(object? obj);
      public int CompareTo(SByte value);
      public override bool Equals(object? obj);
      public bool Equals(SByte obj);
      public override int GetHashCode();
      public TypeCode GetTypeCode();
      public static SByte Parse(ReadOnlySpan<char> s, NumberStyles style = NumberStyles.Integer, IFormatProvider? provider = null);
      public static SByte Parse(string s);
      public static SByte Parse(string s, NumberStyles style);
      public static SByte Parse(string s, NumberStyles style, IFormatProvider? provider);
      public static SByte Parse(string s, IFormatProvider? provider);
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      SByte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      public override string ToString();
      public string ToString(IFormatProvider? provider);
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? provider);
      public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? provider = null);
      public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out SByte result);
      public static bool TryParse(ReadOnlySpan<char> s, out SByte result);
      public static bool TryParse(string? s, NumberStyles style, IFormatProvider? provider, out SByte result);
      public static bool TryParse(string? s, out SByte result);
    }
    public sealed class SerializableAttribute : Attribute {
      public SerializableAttribute();
    }
    public readonly struct Single : IComparable, IComparable<float>, IConvertible, IEquatable<float>, IFormattable {
      public const float Epsilon = 1.401298E-45f;
      public const float MaxValue = 3.40282347E+38f;
      public const float MinValue = -3.40282347E+38f;
      public const float NaN = 0.0f / 0.0f;
      public const float NegativeInfinity = -1.0f / 0.0f;
      public const float PositiveInfinity = 1.0f / 0.0f;
      public int CompareTo(object? value);
      public int CompareTo(Single value);
      public override bool Equals(object? obj);
      public bool Equals(Single obj);
      public override int GetHashCode();
      public TypeCode GetTypeCode();
      public static bool IsFinite(Single f);
      public static bool IsInfinity(Single f);
      public static bool IsNaN(Single f);
      public static bool IsNegative(Single f);
      public static bool IsNegativeInfinity(Single f);
      public static bool IsNormal(Single f);
      public static bool IsPositiveInfinity(Single f);
      public static bool IsSubnormal(Single f);
      public static bool operator ==(Single left, Single right);
      public static bool operator >(Single left, Single right);
      public static bool operator >=(Single left, Single right);
      public static bool operator !=(Single left, Single right);
      public static bool operator <(Single left, Single right);
      public static bool operator <=(Single left, Single right);
      public static Single Parse(ReadOnlySpan<char> s, NumberStyles style = NumberStyles.AllowDecimalPoint | NumberStyles.AllowExponent | NumberStyles.AllowLeadingSign | NumberStyles.AllowLeadingWhite | NumberStyles.AllowThousands | NumberStyles.AllowTrailingWhite, IFormatProvider? provider = null);
      public static Single Parse(string s);
      public static Single Parse(string s, NumberStyles style);
      public static Single Parse(string s, NumberStyles style, IFormatProvider? provider);
      public static Single Parse(string s, IFormatProvider? provider);
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      Single System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      public override string ToString();
      public string ToString(IFormatProvider? provider);
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? provider);
      public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? provider = null);
      public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out Single result);
      public static bool TryParse(ReadOnlySpan<char> s, out Single result);
      public static bool TryParse(string? s, NumberStyles style, IFormatProvider? provider, out Single result);
      public static bool TryParse(string? s, out Single result);
    }
    public readonly ref struct Span<T> {
      public unsafe Span(void* pointer, int length);
      public Span(T[]? array);
      public Span(T[]? array, int start, int length);
      public static Span<T> Empty { get; }
      public bool IsEmpty { get; }
      public int Length { get; }
      public ref T this[int index] { get; }
      public void Clear();
      public void CopyTo(Span<T> destination);
      public override bool Equals(object? obj);
      public void Fill(T value);
      public Span<T>.Enumerator GetEnumerator();
      public override int GetHashCode();
      public ref T GetPinnableReference();
      public static bool operator ==(Span<T> left, Span<T> right);
      public static implicit operator Span<T> (ArraySegment<T> segment);
      public static implicit operator ReadOnlySpan<T> (Span<T> span);
      public static implicit operator Span<T> (T[]? array);
      public static bool operator !=(Span<T> left, Span<T> right);
      public Span<T> Slice(int start);
      public Span<T> Slice(int start, int length);
      public T[] ToArray();
      public override string ToString();
      public bool TryCopyTo(Span<T> destination);
      public ref struct Enumerator {
        public ref T Current { get; }
        public bool MoveNext();
      }
    }
    public sealed class StackOverflowException : SystemException {
      public StackOverflowException();
      public StackOverflowException(string? message);
      public StackOverflowException(string? message, Exception? innerException);
    }
    public sealed class STAThreadAttribute : Attribute {
      public STAThreadAttribute();
    }
    public sealed class String : ICloneable, IComparable, IComparable<string?>, IConvertible, IEnumerable, IEnumerable<char>, IEquatable<string?> {
      public static readonly string Empty;
      public unsafe String(char* value);
      public unsafe String(char* value, int startIndex, int length);
      public String(char c, int count);
      public String(char[] value);
      public String(char[] value, int startIndex, int length);
      public String(ReadOnlySpan<char> value);
      public unsafe String(sbyte* value);
      public unsafe String(sbyte* value, int startIndex, int length);
      public unsafe String(sbyte* value, int startIndex, int length, Encoding enc);
      public int Length { get; }
      public char this[int index] { get; }
      public object Clone();
      public static int Compare(String? strA, int indexA, String? strB, int indexB, int length);
      public static int Compare(String? strA, int indexA, String? strB, int indexB, int length, bool ignoreCase);
      public static int Compare(String? strA, int indexA, String? strB, int indexB, int length, bool ignoreCase, CultureInfo culture);
      public static int Compare(String? strA, int indexA, String? strB, int indexB, int length, CultureInfo culture, CompareOptions options);
      public static int Compare(String? strA, int indexA, String? strB, int indexB, int length, StringComparison comparisonType);
      public static int Compare(String? strA, String? strB);
      public static int Compare(String? strA, String? strB, bool ignoreCase);
      public static int Compare(String? strA, String? strB, bool ignoreCase, CultureInfo culture);
      public static int Compare(String? strA, String? strB, CultureInfo culture, CompareOptions options);
      public static int Compare(String? strA, String? strB, StringComparison comparisonType);
      public static int CompareOrdinal(String? strA, int indexA, String? strB, int indexB, int length);
      public static int CompareOrdinal(String? strA, String? strB);
      public int CompareTo(object? value);
      public int CompareTo(String? strB);
      public static String Concat(IEnumerable<string?> values);
      public static String Concat(object? arg0);
      public static String Concat(object? arg0, object? arg1);
      public static String Concat(object? arg0, object? arg1, object? arg2);
      public static String Concat(params object?[] args);
      public static String Concat(ReadOnlySpan<char> str0, ReadOnlySpan<char> str1);
      public static String Concat(ReadOnlySpan<char> str0, ReadOnlySpan<char> str1, ReadOnlySpan<char> str2);
      public static String Concat(ReadOnlySpan<char> str0, ReadOnlySpan<char> str1, ReadOnlySpan<char> str2, ReadOnlySpan<char> str3);
      public static String Concat(String? str0, String? str1);
      public static String Concat(String? str0, String? str1, String? str2);
      public static String Concat(String? str0, String? str1, String? str2, String? str3);
      public static String Concat(params string?[] values);
      public static String Concat<T>(IEnumerable<T> values);
      public bool Contains(char value);
      public bool Contains(char value, StringComparison comparisonType);
      public bool Contains(String value);
      public bool Contains(String value, StringComparison comparisonType);
      public static String Copy(String str);
      public void CopyTo(int sourceIndex, char[] destination, int destinationIndex, int count);
      public static String Create<TState>(int length, TState state, SpanAction<char, TState> action);
      public bool EndsWith(char value);
      public bool EndsWith(String value);
      public bool EndsWith(String value, bool ignoreCase, CultureInfo? culture);
      public bool EndsWith(String value, StringComparison comparisonType);
      public StringRuneEnumerator EnumerateRunes();
      public override bool Equals(object? obj);
      public bool Equals(String? value);
      public static bool Equals(String? a, String? b);
      public static bool Equals(String? a, String? b, StringComparison comparisonType);
      public bool Equals(String? value, StringComparison comparisonType);
      public static String Format(IFormatProvider? provider, String format, object? arg0);
      public static String Format(IFormatProvider? provider, String format, object? arg0, object? arg1);
      public static String Format(IFormatProvider? provider, String format, object? arg0, object? arg1, object? arg2);
      public static String Format(IFormatProvider? provider, String format, params object?[] args);
      public static String Format(String format, object? arg0);
      public static String Format(String format, object? arg0, object? arg1);
      public static String Format(String format, object? arg0, object? arg1, object? arg2);
      public static String Format(String format, params object?[] args);
      public CharEnumerator GetEnumerator();
      public override int GetHashCode();
      public static int GetHashCode(ReadOnlySpan<char> value);
      public static int GetHashCode(ReadOnlySpan<char> value, StringComparison comparisonType);
      public int GetHashCode(StringComparison comparisonType);
      public ref readonly char GetPinnableReference();
      public TypeCode GetTypeCode();
      public int IndexOf(char value);
      public int IndexOf(char value, int startIndex);
      public int IndexOf(char value, int startIndex, int count);
      public int IndexOf(char value, StringComparison comparisonType);
      public int IndexOf(String value);
      public int IndexOf(String value, int startIndex);
      public int IndexOf(String value, int startIndex, int count);
      public int IndexOf(String value, int startIndex, int count, StringComparison comparisonType);
      public int IndexOf(String value, int startIndex, StringComparison comparisonType);
      public int IndexOf(String value, StringComparison comparisonType);
      public int IndexOfAny(char[] anyOf);
      public int IndexOfAny(char[] anyOf, int startIndex);
      public int IndexOfAny(char[] anyOf, int startIndex, int count);
      public String Insert(int startIndex, String value);
      public static String Intern(String str);
      public static String? IsInterned(String str);
      public bool IsNormalized();
      public bool IsNormalized(NormalizationForm normalizationForm);
      public static bool IsNullOrEmpty(String? value);
      public static bool IsNullOrWhiteSpace(String? value);
      public static String Join(char separator, params object?[] values);
      public static String Join(char separator, params string?[] value);
      public static String Join(char separator, string?[] value, int startIndex, int count);
      public static String Join(String? separator, IEnumerable<string> values);
      public static String Join(String? separator, params object?[] values);
      public static String Join(String? separator, params string?[] value);
      public static String Join(String? separator, string?[] value, int startIndex, int count);
      public static String Join<T>(char separator, IEnumerable<T> values);
      public static String Join<T>(String? separator, IEnumerable<T> values);
      public int LastIndexOf(char value);
      public int LastIndexOf(char value, int startIndex);
      public int LastIndexOf(char value, int startIndex, int count);
      public int LastIndexOf(String value);
      public int LastIndexOf(String value, int startIndex);
      public int LastIndexOf(String value, int startIndex, int count);
      public int LastIndexOf(String value, int startIndex, int count, StringComparison comparisonType);
      public int LastIndexOf(String value, int startIndex, StringComparison comparisonType);
      public int LastIndexOf(String value, StringComparison comparisonType);
      public int LastIndexOfAny(char[] anyOf);
      public int LastIndexOfAny(char[] anyOf, int startIndex);
      public int LastIndexOfAny(char[] anyOf, int startIndex, int count);
      public String Normalize();
      public String Normalize(NormalizationForm normalizationForm);
      public static bool operator ==(String? a, String? b);
      public static implicit operator ReadOnlySpan<char> (String? value);
      public static bool operator !=(String? a, String? b);
      public String PadLeft(int totalWidth);
      public String PadLeft(int totalWidth, char paddingChar);
      public String PadRight(int totalWidth);
      public String PadRight(int totalWidth, char paddingChar);
      public String Remove(int startIndex);
      public String Remove(int startIndex, int count);
      public String Replace(char oldChar, char newChar);
      public String Replace(String oldValue, String? newValue);
      public String Replace(String oldValue, String? newValue, bool ignoreCase, CultureInfo? culture);
      public String Replace(String oldValue, String? newValue, StringComparison comparisonType);
      public string[] Split(char separator, int count, StringSplitOptions options = StringSplitOptions.None);
      public string[] Split(char separator, StringSplitOptions options = StringSplitOptions.None);
      public string[] Split(params char[]? separator);
      public string[] Split(char[]? separator, int count);
      public string[] Split(char[]? separator, int count, StringSplitOptions options);
      public string[] Split(char[]? separator, StringSplitOptions options);
      public string[] Split(String? separator, int count, StringSplitOptions options = StringSplitOptions.None);
      public string[] Split(String? separator, StringSplitOptions options = StringSplitOptions.None);
      public string[] Split(string[]? separator, int count, StringSplitOptions options);
      public string[] Split(string[]? separator, StringSplitOptions options);
      public bool StartsWith(char value);
      public bool StartsWith(String value);
      public bool StartsWith(String value, bool ignoreCase, CultureInfo? culture);
      public bool StartsWith(String value, StringComparison comparisonType);
      public String Substring(int startIndex);
      public String Substring(int startIndex, int length);
      IEnumerator<char> System.Collections.Generic.IEnumerable<System.Char>.GetEnumerator();
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      public char[] ToCharArray();
      public char[] ToCharArray(int startIndex, int length);
      public String ToLower();
      public String ToLower(CultureInfo culture);
      public String ToLowerInvariant();
      public override String ToString();
      public String ToString(IFormatProvider? provider);
      public String ToUpper();
      public String ToUpper(CultureInfo culture);
      public String ToUpperInvariant();
      public String Trim();
      public String Trim(char trimChar);
      public String Trim(params char[]? trimChars);
      public String TrimEnd();
      public String TrimEnd(char trimChar);
      public String TrimEnd(params char[]? trimChars);
      public String TrimStart();
      public String TrimStart(char trimChar);
      public String TrimStart(params char[]? trimChars);
    }
    public enum StringComparison {
      CurrentCulture = 0,
      CurrentCultureIgnoreCase = 1,
      InvariantCulture = 2,
      InvariantCultureIgnoreCase = 3,
      Ordinal = 4,
      OrdinalIgnoreCase = 5,
    }
    public enum StringSplitOptions {
      None = 0,
      RemoveEmptyEntries = 1,
    }
    public class SystemException : Exception {
      public SystemException();
      protected SystemException(SerializationInfo info, StreamingContext context);
      public SystemException(string? message);
      public SystemException(string? message, Exception? innerException);
    }
    public class ThreadStaticAttribute : Attribute {
      public ThreadStaticAttribute();
    }
    public class TimeoutException : SystemException {
      public TimeoutException();
      protected TimeoutException(SerializationInfo info, StreamingContext context);
      public TimeoutException(string? message);
      public TimeoutException(string? message, Exception? innerException);
    }
    public readonly struct TimeSpan : IComparable, IComparable<TimeSpan>, IEquatable<TimeSpan>, IFormattable {
      public const long TicksPerDay = (long)864000000000;
      public const long TicksPerHour = (long)36000000000;
      public const long TicksPerMillisecond = (long)10000;
      public const long TicksPerMinute = (long)600000000;
      public const long TicksPerSecond = (long)10000000;
      public static readonly TimeSpan MaxValue;
      public static readonly TimeSpan MinValue;
      public static readonly TimeSpan Zero;
      public TimeSpan(int hours, int minutes, int seconds);
      public TimeSpan(int days, int hours, int minutes, int seconds);
      public TimeSpan(int days, int hours, int minutes, int seconds, int milliseconds);
      public TimeSpan(long ticks);
      public int Days { get; }
      public int Hours { get; }
      public int Milliseconds { get; }
      public int Minutes { get; }
      public int Seconds { get; }
      public long Ticks { get; }
      public double TotalDays { get; }
      public double TotalHours { get; }
      public double TotalMilliseconds { get; }
      public double TotalMinutes { get; }
      public double TotalSeconds { get; }
      public TimeSpan Add(TimeSpan ts);
      public static int Compare(TimeSpan t1, TimeSpan t2);
      public int CompareTo(object? value);
      public int CompareTo(TimeSpan value);
      public TimeSpan Divide(double divisor);
      public double Divide(TimeSpan ts);
      public TimeSpan Duration();
      public override bool Equals(object? value);
      public bool Equals(TimeSpan obj);
      public static bool Equals(TimeSpan t1, TimeSpan t2);
      public static TimeSpan FromDays(double value);
      public static TimeSpan FromHours(double value);
      public static TimeSpan FromMilliseconds(double value);
      public static TimeSpan FromMinutes(double value);
      public static TimeSpan FromSeconds(double value);
      public static TimeSpan FromTicks(long value);
      public override int GetHashCode();
      public TimeSpan Multiply(double factor);
      public TimeSpan Negate();
      public static TimeSpan operator +(TimeSpan t1, TimeSpan t2);
      public static TimeSpan operator /(TimeSpan timeSpan, double divisor);
      public static double operator /(TimeSpan t1, TimeSpan t2);
      public static bool operator ==(TimeSpan t1, TimeSpan t2);
      public static bool operator >(TimeSpan t1, TimeSpan t2);
      public static bool operator >=(TimeSpan t1, TimeSpan t2);
      public static bool operator !=(TimeSpan t1, TimeSpan t2);
      public static bool operator <(TimeSpan t1, TimeSpan t2);
      public static bool operator <=(TimeSpan t1, TimeSpan t2);
      public static TimeSpan operator *(double factor, TimeSpan timeSpan);
      public static TimeSpan operator *(TimeSpan timeSpan, double factor);
      public static TimeSpan operator -(TimeSpan t1, TimeSpan t2);
      public static TimeSpan operator -(TimeSpan t);
      public static TimeSpan operator +(TimeSpan t);
      public static TimeSpan Parse(ReadOnlySpan<char> input, IFormatProvider? formatProvider = null);
      public static TimeSpan Parse(string s);
      public static TimeSpan Parse(string input, IFormatProvider? formatProvider);
      public static TimeSpan ParseExact(ReadOnlySpan<char> input, ReadOnlySpan<char> format, IFormatProvider? formatProvider, TimeSpanStyles styles = TimeSpanStyles.None);
      public static TimeSpan ParseExact(ReadOnlySpan<char> input, string[] formats, IFormatProvider? formatProvider, TimeSpanStyles styles = TimeSpanStyles.None);
      public static TimeSpan ParseExact(string input, string format, IFormatProvider? formatProvider);
      public static TimeSpan ParseExact(string input, string format, IFormatProvider? formatProvider, TimeSpanStyles styles);
      public static TimeSpan ParseExact(string input, string[] formats, IFormatProvider? formatProvider);
      public static TimeSpan ParseExact(string input, string[] formats, IFormatProvider? formatProvider, TimeSpanStyles styles);
      public TimeSpan Subtract(TimeSpan ts);
      public override string ToString();
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? formatProvider);
      public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? formatProvider = null);
      public static bool TryParse(ReadOnlySpan<char> input, IFormatProvider? formatProvider, out TimeSpan result);
      public static bool TryParse(ReadOnlySpan<char> s, out TimeSpan result);
      public static bool TryParse(string? input, IFormatProvider? formatProvider, out TimeSpan result);
      public static bool TryParse(string? s, out TimeSpan result);
      public static bool TryParseExact(ReadOnlySpan<char> input, ReadOnlySpan<char> format, IFormatProvider? formatProvider, TimeSpanStyles styles, out TimeSpan result);
      public static bool TryParseExact(ReadOnlySpan<char> input, ReadOnlySpan<char> format, IFormatProvider? formatProvider, out TimeSpan result);
      public static bool TryParseExact(ReadOnlySpan<char> input, string?[]? formats, IFormatProvider? formatProvider, TimeSpanStyles styles, out TimeSpan result);
      public static bool TryParseExact(ReadOnlySpan<char> input, string?[]? formats, IFormatProvider? formatProvider, out TimeSpan result);
      public static bool TryParseExact(string? input, string? format, IFormatProvider? formatProvider, TimeSpanStyles styles, out TimeSpan result);
      public static bool TryParseExact(string? input, string? format, IFormatProvider? formatProvider, out TimeSpan result);
      public static bool TryParseExact(string? input, string?[]? formats, IFormatProvider? formatProvider, TimeSpanStyles styles, out TimeSpan result);
      public static bool TryParseExact(string? input, string?[]? formats, IFormatProvider? formatProvider, out TimeSpan result);
    }
    public abstract class TimeZone {
      protected TimeZone();
      public static TimeZone CurrentTimeZone { get; }
      public abstract string DaylightName { get; }
      public abstract string StandardName { get; }
      public abstract DaylightTime GetDaylightChanges(int year);
      public abstract TimeSpan GetUtcOffset(DateTime time);
      public virtual bool IsDaylightSavingTime(DateTime time);
      public static bool IsDaylightSavingTime(DateTime time, DaylightTime daylightTimes);
      public virtual DateTime ToLocalTime(DateTime time);
      public virtual DateTime ToUniversalTime(DateTime time);
    }
    public sealed class TimeZoneInfo : IDeserializationCallback, IEquatable<TimeZoneInfo?>, ISerializable {
      public TimeSpan BaseUtcOffset { get; }
      public string DaylightName { get; }
      public string DisplayName { get; }
      public string Id { get; }
      public static TimeZoneInfo Local { get; }
      public string StandardName { get; }
      public bool SupportsDaylightSavingTime { get; }
      public static TimeZoneInfo Utc { get; }
      public static void ClearCachedData();
      public static DateTime ConvertTime(DateTime dateTime, TimeZoneInfo destinationTimeZone);
      public static DateTime ConvertTime(DateTime dateTime, TimeZoneInfo sourceTimeZone, TimeZoneInfo destinationTimeZone);
      public static DateTimeOffset ConvertTime(DateTimeOffset dateTimeOffset, TimeZoneInfo destinationTimeZone);
      public static DateTime ConvertTimeBySystemTimeZoneId(DateTime dateTime, string destinationTimeZoneId);
      public static DateTime ConvertTimeBySystemTimeZoneId(DateTime dateTime, string sourceTimeZoneId, string destinationTimeZoneId);
      public static DateTimeOffset ConvertTimeBySystemTimeZoneId(DateTimeOffset dateTimeOffset, string destinationTimeZoneId);
      public static DateTime ConvertTimeFromUtc(DateTime dateTime, TimeZoneInfo destinationTimeZone);
      public static DateTime ConvertTimeToUtc(DateTime dateTime);
      public static DateTime ConvertTimeToUtc(DateTime dateTime, TimeZoneInfo sourceTimeZone);
      public static TimeZoneInfo CreateCustomTimeZone(string id, TimeSpan baseUtcOffset, string? displayName, string? standardDisplayName);
      public static TimeZoneInfo CreateCustomTimeZone(string id, TimeSpan baseUtcOffset, string? displayName, string? standardDisplayName, string? daylightDisplayName, TimeZoneInfo.AdjustmentRule[]? adjustmentRules);
      public static TimeZoneInfo CreateCustomTimeZone(string id, TimeSpan baseUtcOffset, string? displayName, string? standardDisplayName, string? daylightDisplayName, TimeZoneInfo.AdjustmentRule[]? adjustmentRules, bool disableDaylightSavingTime);
      public override bool Equals(object? obj);
      public bool Equals(TimeZoneInfo? other);
      public static TimeZoneInfo FindSystemTimeZoneById(string id);
      public static TimeZoneInfo FromSerializedString(string source);
      public TimeZoneInfo.AdjustmentRule[] GetAdjustmentRules();
      public TimeSpan[] GetAmbiguousTimeOffsets(DateTime dateTime);
      public TimeSpan[] GetAmbiguousTimeOffsets(DateTimeOffset dateTimeOffset);
      public override int GetHashCode();
      public static ReadOnlyCollection<TimeZoneInfo> GetSystemTimeZones();
      public TimeSpan GetUtcOffset(DateTime dateTime);
      public TimeSpan GetUtcOffset(DateTimeOffset dateTimeOffset);
      public bool HasSameRules(TimeZoneInfo other);
      public bool IsAmbiguousTime(DateTime dateTime);
      public bool IsAmbiguousTime(DateTimeOffset dateTimeOffset);
      public bool IsDaylightSavingTime(DateTime dateTime);
      public bool IsDaylightSavingTime(DateTimeOffset dateTimeOffset);
      public bool IsInvalidTime(DateTime dateTime);
      void System.Runtime.Serialization.IDeserializationCallback.OnDeserialization(object sender);
      void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
      public string ToSerializedString();
      public override string ToString();
      public sealed class AdjustmentRule : IDeserializationCallback, IEquatable<TimeZoneInfo.AdjustmentRule?>, ISerializable {
        public DateTime DateEnd { get; }
        public DateTime DateStart { get; }
        public TimeSpan DaylightDelta { get; }
        public TimeZoneInfo.TransitionTime DaylightTransitionEnd { get; }
        public TimeZoneInfo.TransitionTime DaylightTransitionStart { get; }
        public static TimeZoneInfo.AdjustmentRule CreateAdjustmentRule(DateTime dateStart, DateTime dateEnd, TimeSpan daylightDelta, TimeZoneInfo.TransitionTime daylightTransitionStart, TimeZoneInfo.TransitionTime daylightTransitionEnd);
        public bool Equals(TimeZoneInfo.AdjustmentRule? other);
        public override int GetHashCode();
        void System.Runtime.Serialization.IDeserializationCallback.OnDeserialization(object sender);
        void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
      }
      public readonly struct TransitionTime : IDeserializationCallback, IEquatable<TimeZoneInfo.TransitionTime>, ISerializable {
        public int Day { get; }
        public DayOfWeek DayOfWeek { get; }
        public bool IsFixedDateRule { get; }
        public int Month { get; }
        public DateTime TimeOfDay { get; }
        public int Week { get; }
        public static TimeZoneInfo.TransitionTime CreateFixedDateRule(DateTime timeOfDay, int month, int day);
        public static TimeZoneInfo.TransitionTime CreateFloatingDateRule(DateTime timeOfDay, int month, int week, DayOfWeek dayOfWeek);
        public override bool Equals(object? obj);
        public bool Equals(TimeZoneInfo.TransitionTime other);
        public override int GetHashCode();
        public static bool operator ==(TimeZoneInfo.TransitionTime t1, TimeZoneInfo.TransitionTime t2);
        public static bool operator !=(TimeZoneInfo.TransitionTime t1, TimeZoneInfo.TransitionTime t2);
        void System.Runtime.Serialization.IDeserializationCallback.OnDeserialization(object sender);
        void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
      }
    }
    public class TimeZoneNotFoundException : Exception {
      public TimeZoneNotFoundException();
      protected TimeZoneNotFoundException(SerializationInfo info, StreamingContext context);
      public TimeZoneNotFoundException(string? message);
      public TimeZoneNotFoundException(string? message, Exception? innerException);
    }
    public static class Tuple {
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8>> Create<T1, T2, T3, T4, T5, T6, T7, T8>(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5, T6 item6, T7 item7, T8 item8);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7> Create<T1, T2, T3, T4, T5, T6, T7>(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5, T6 item6, T7 item7);
      public static Tuple<T1, T2, T3, T4, T5, T6> Create<T1, T2, T3, T4, T5, T6>(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5, T6 item6);
      public static Tuple<T1, T2, T3, T4, T5> Create<T1, T2, T3, T4, T5>(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5);
      public static Tuple<T1, T2, T3, T4> Create<T1, T2, T3, T4>(T1 item1, T2 item2, T3 item3, T4 item4);
      public static Tuple<T1, T2, T3> Create<T1, T2, T3>(T1 item1, T2 item2, T3 item3);
      public static Tuple<T1, T2> Create<T1, T2>(T1 item1, T2 item2);
      public static Tuple<T1> Create<T1>(T1 item1);
    }
    public class Tuple<T1> : IComparable, IStructuralComparable, IStructuralEquatable, ITuple {
      public Tuple(T1 item1);
      public T1 Item1 { get; }
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public override bool Equals(object? obj);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? obj);
      public override string ToString();
    }
    public class Tuple<T1, T2> : IComparable, IStructuralComparable, IStructuralEquatable, ITuple {
      public Tuple(T1 item1, T2 item2);
      public T1 Item1 { get; }
      public T2 Item2 { get; }
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public override bool Equals(object? obj);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? obj);
      public override string ToString();
    }
    public class Tuple<T1, T2, T3> : IComparable, IStructuralComparable, IStructuralEquatable, ITuple {
      public Tuple(T1 item1, T2 item2, T3 item3);
      public T1 Item1 { get; }
      public T2 Item2 { get; }
      public T3 Item3 { get; }
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public override bool Equals(object? obj);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? obj);
      public override string ToString();
    }
    public class Tuple<T1, T2, T3, T4> : IComparable, IStructuralComparable, IStructuralEquatable, ITuple {
      public Tuple(T1 item1, T2 item2, T3 item3, T4 item4);
      public T1 Item1 { get; }
      public T2 Item2 { get; }
      public T3 Item3 { get; }
      public T4 Item4 { get; }
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public override bool Equals(object? obj);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? obj);
      public override string ToString();
    }
    public class Tuple<T1, T2, T3, T4, T5> : IComparable, IStructuralComparable, IStructuralEquatable, ITuple {
      public Tuple(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5);
      public T1 Item1 { get; }
      public T2 Item2 { get; }
      public T3 Item3 { get; }
      public T4 Item4 { get; }
      public T5 Item5 { get; }
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public override bool Equals(object? obj);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? obj);
      public override string ToString();
    }
    public class Tuple<T1, T2, T3, T4, T5, T6> : IComparable, IStructuralComparable, IStructuralEquatable, ITuple {
      public Tuple(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5, T6 item6);
      public T1 Item1 { get; }
      public T2 Item2 { get; }
      public T3 Item3 { get; }
      public T4 Item4 { get; }
      public T5 Item5 { get; }
      public T6 Item6 { get; }
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public override bool Equals(object? obj);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? obj);
      public override string ToString();
    }
    public class Tuple<T1, T2, T3, T4, T5, T6, T7> : IComparable, IStructuralComparable, IStructuralEquatable, ITuple {
      public Tuple(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5, T6 item6, T7 item7);
      public T1 Item1 { get; }
      public T2 Item2 { get; }
      public T3 Item3 { get; }
      public T4 Item4 { get; }
      public T5 Item5 { get; }
      public T6 Item6 { get; }
      public T7 Item7 { get; }
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public override bool Equals(object? obj);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? obj);
      public override string ToString();
    }
    public class Tuple<T1, T2, T3, T4, T5, T6, T7, TRest> : IComparable, IStructuralComparable, IStructuralEquatable, ITuple {
      public Tuple(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5, T6 item6, T7 item7, TRest rest);
      public T1 Item1 { get; }
      public T2 Item2 { get; }
      public T3 Item3 { get; }
      public T4 Item4 { get; }
      public T5 Item5 { get; }
      public T6 Item6 { get; }
      public T7 Item7 { get; }
      public TRest Rest { get; }
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public override bool Equals(object? obj);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? obj);
      public override string ToString();
    }
    public static class TupleExtensions {
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19, T20, T21>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17, T18, T19, T20, T21>>> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7, out T8 item8, out T9 item9, out T10 item10, out T11 item11, out T12 item12, out T13 item13, out T14 item14, out T15 item15, out T16 item16, out T17 item17, out T18 item18, out T19 item19, out T20 item20, out T21 item21);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19, T20>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17, T18, T19, T20>>> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7, out T8 item8, out T9 item9, out T10 item10, out T11 item11, out T12 item12, out T13 item13, out T14 item14, out T15 item15, out T16 item16, out T17 item17, out T18 item18, out T19 item19, out T20 item20);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17, T18, T19>>> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7, out T8 item8, out T9 item9, out T10 item10, out T11 item11, out T12 item12, out T13 item13, out T14 item14, out T15 item15, out T16 item16, out T17 item17, out T18 item18, out T19 item19);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17, T18>>> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7, out T8 item8, out T9 item9, out T10 item10, out T11 item11, out T12 item12, out T13 item13, out T14 item14, out T15 item15, out T16 item16, out T17 item17, out T18 item18);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17>>> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7, out T8 item8, out T9 item9, out T10 item10, out T11 item11, out T12 item12, out T13 item13, out T14 item14, out T15 item15, out T16 item16, out T17 item17);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16>>> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7, out T8 item8, out T9 item9, out T10 item10, out T11 item11, out T12 item12, out T13 item13, out T14 item14, out T15 item15, out T16 item16);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15>>> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7, out T8 item8, out T9 item9, out T10 item10, out T11 item11, out T12 item12, out T13 item13, out T14 item14, out T15 item15);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14>> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7, out T8 item8, out T9 item9, out T10 item10, out T11 item11, out T12 item12, out T13 item13, out T14 item14);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13>> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7, out T8 item8, out T9 item9, out T10 item10, out T11 item11, out T12 item12, out T13 item13);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12>> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7, out T8 item8, out T9 item9, out T10 item10, out T11 item11, out T12 item12);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11>> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7, out T8 item8, out T9 item9, out T10 item10, out T11 item11);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10>> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7, out T8 item8, out T9 item9, out T10 item10);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7, T8, T9>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9>> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7, out T8 item8, out T9 item9);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7, T8>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8>> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7, out T8 item8);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6, T7>(this Tuple<T1, T2, T3, T4, T5, T6, T7> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6, out T7 item7);
      public static void Deconstruct<T1, T2, T3, T4, T5, T6>(this Tuple<T1, T2, T3, T4, T5, T6> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5, out T6 item6);
      public static void Deconstruct<T1, T2, T3, T4, T5>(this Tuple<T1, T2, T3, T4, T5> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4, out T5 item5);
      public static void Deconstruct<T1, T2, T3, T4>(this Tuple<T1, T2, T3, T4> value, out T1 item1, out T2 item2, out T3 item3, out T4 item4);
      public static void Deconstruct<T1, T2, T3>(this Tuple<T1, T2, T3> value, out T1 item1, out T2 item2, out T3 item3);
      public static void Deconstruct<T1, T2>(this Tuple<T1, T2> value, out T1 item1, out T2 item2);
      public static void Deconstruct<T1>(this Tuple<T1> value, out T1 item1);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17, T18, T19, T20, T21>>> ToTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19, T20, T21>(this (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19, T20, T21) value);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17, T18, T19, T20>>> ToTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19, T20>(this (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19, T20) value);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17, T18, T19>>> ToTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19>(this (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19) value);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17, T18>>> ToTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18>(this (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18) value);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17>>> ToTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17>(this (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17) value);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16>>> ToTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16>(this (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16) value);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15>>> ToTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15>(this (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15) value);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14>> ToTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14>(this (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14) value);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13>> ToTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13>(this (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13) value);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12>> ToTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12>(this (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12) value);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11>> ToTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11>(this (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11) value);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10>> ToTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10>(this (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10) value);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9>> ToTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9>(this (T1, T2, T3, T4, T5, T6, T7, T8, T9) value);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8>> ToTuple<T1, T2, T3, T4, T5, T6, T7, T8>(this (T1, T2, T3, T4, T5, T6, T7, T8) value);
      public static Tuple<T1, T2, T3, T4, T5, T6, T7> ToTuple<T1, T2, T3, T4, T5, T6, T7>(this (T1, T2, T3, T4, T5, T6, T7) value);
      public static Tuple<T1, T2, T3, T4, T5, T6> ToTuple<T1, T2, T3, T4, T5, T6>(this (T1, T2, T3, T4, T5, T6) value);
      public static Tuple<T1, T2, T3, T4, T5> ToTuple<T1, T2, T3, T4, T5>(this (T1, T2, T3, T4, T5) value);
      public static Tuple<T1, T2, T3, T4> ToTuple<T1, T2, T3, T4>(this (T1, T2, T3, T4) value);
      public static Tuple<T1, T2, T3> ToTuple<T1, T2, T3>(this (T1, T2, T3) value);
      public static Tuple<T1, T2> ToTuple<T1, T2>(this (T1, T2) value);
      public static Tuple<T1> ToTuple<T1>(this ValueTuple<T1> value);
      public static (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19, T20, T21) ToValueTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19, T20, T21>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17, T18, T19, T20, T21>>> value);
      public static (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19, T20) ToValueTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19, T20>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17, T18, T19, T20>>> value);
      public static (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19) ToValueTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18, T19>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17, T18, T19>>> value);
      public static (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18) ToValueTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17, T18>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17, T18>>> value);
      public static (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17) ToValueTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16, T17>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16, T17>>> value);
      public static (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16) ToValueTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15, T16>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15, T16>>> value);
      public static (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15) ToValueTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14, T15>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14, Tuple<T15>>> value);
      public static (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14) ToValueTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13, T14>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13, T14>> value);
      public static (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13) ToValueTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12, T13>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12, T13>> value);
      public static (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12) ToValueTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11, T12>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11, T12>> value);
      public static (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11) ToValueTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10, T11>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10, T11>> value);
      public static (T1, T2, T3, T4, T5, T6, T7, T8, T9, T10) ToValueTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9, T10>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9, T10>> value);
      public static (T1, T2, T3, T4, T5, T6, T7, T8, T9) ToValueTuple<T1, T2, T3, T4, T5, T6, T7, T8, T9>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8, T9>> value);
      public static (T1, T2, T3, T4, T5, T6, T7, T8) ToValueTuple<T1, T2, T3, T4, T5, T6, T7, T8>(this Tuple<T1, T2, T3, T4, T5, T6, T7, Tuple<T8>> value);
      public static (T1, T2, T3, T4, T5, T6, T7) ToValueTuple<T1, T2, T3, T4, T5, T6, T7>(this Tuple<T1, T2, T3, T4, T5, T6, T7> value);
      public static (T1, T2, T3, T4, T5, T6) ToValueTuple<T1, T2, T3, T4, T5, T6>(this Tuple<T1, T2, T3, T4, T5, T6> value);
      public static (T1, T2, T3, T4, T5) ToValueTuple<T1, T2, T3, T4, T5>(this Tuple<T1, T2, T3, T4, T5> value);
      public static (T1, T2, T3, T4) ToValueTuple<T1, T2, T3, T4>(this Tuple<T1, T2, T3, T4> value);
      public static (T1, T2, T3) ToValueTuple<T1, T2, T3>(this Tuple<T1, T2, T3> value);
      public static (T1, T2) ToValueTuple<T1, T2>(this Tuple<T1, T2> value);
      public static ValueTuple<T1> ToValueTuple<T1>(this Tuple<T1> value);
    }
    public abstract class Type : MemberInfo, IReflect {
      public static readonly char Delimiter;
      public static readonly object Missing;
      public static readonly MemberFilter FilterAttribute;
      public static readonly MemberFilter FilterName;
      public static readonly MemberFilter FilterNameIgnoreCase;
      public static readonly Type[] EmptyTypes;
      protected Type();
      public abstract Assembly Assembly { get; }
      public abstract string? AssemblyQualifiedName { get; }
      public TypeAttributes Attributes { get; }
      public abstract Type? BaseType { get; }
      public virtual bool ContainsGenericParameters { get; }
      public virtual MethodBase? DeclaringMethod { get; }
      public override Type? DeclaringType { get; }
      public static Binder DefaultBinder { get; }
      public abstract string? FullName { get; }
      public virtual GenericParameterAttributes GenericParameterAttributes { get; }
      public virtual int GenericParameterPosition { get; }
      public virtual Type[] GenericTypeArguments { get; }
      public abstract Guid GUID { get; }
      public bool HasElementType { get; }
      public bool IsAbstract { get; }
      public bool IsAnsiClass { get; }
      public bool IsArray { get; }
      public bool IsAutoClass { get; }
      public bool IsAutoLayout { get; }
      public bool IsByRef { get; }
      public virtual bool IsByRefLike { get; }
      public bool IsClass { get; }
      public bool IsCOMObject { get; }
      public virtual bool IsConstructedGenericType { get; }
      public bool IsContextful { get; }
      public virtual bool IsEnum { get; }
      public bool IsExplicitLayout { get; }
      public virtual bool IsGenericMethodParameter { get; }
      public virtual bool IsGenericParameter { get; }
      public virtual bool IsGenericType { get; }
      public virtual bool IsGenericTypeDefinition { get; }
      public virtual bool IsGenericTypeParameter { get; }
      public bool IsImport { get; }
      public bool IsInterface { get; }
      public bool IsLayoutSequential { get; }
      public bool IsMarshalByRef { get; }
      public bool IsNested { get; }
      public bool IsNestedAssembly { get; }
      public bool IsNestedFamANDAssem { get; }
      public bool IsNestedFamily { get; }
      public bool IsNestedFamORAssem { get; }
      public bool IsNestedPrivate { get; }
      public bool IsNestedPublic { get; }
      public bool IsNotPublic { get; }
      public bool IsPointer { get; }
      public bool IsPrimitive { get; }
      public bool IsPublic { get; }
      public bool IsSealed { get; }
      public virtual bool IsSecurityCritical { get; }
      public virtual bool IsSecuritySafeCritical { get; }
      public virtual bool IsSecurityTransparent { get; }
      public virtual bool IsSerializable { get; }
      public virtual bool IsSignatureType { get; }
      public bool IsSpecialName { get; }
      public virtual bool IsSZArray { get; }
      public virtual bool IsTypeDefinition { get; }
      public bool IsUnicodeClass { get; }
      public bool IsValueType { get; }
      public virtual bool IsVariableBoundArray { get; }
      public bool IsVisible { get; }
      public override MemberTypes MemberType { get; }
      public abstract new Module Module { get; }
      public abstract string? Namespace { get; }
      public override Type? ReflectedType { get; }
      public virtual StructLayoutAttribute? StructLayoutAttribute { get; }
      public virtual RuntimeTypeHandle TypeHandle { get; }
      public ConstructorInfo? TypeInitializer { get; }
      public abstract Type UnderlyingSystemType { get; }
      public override bool Equals(object? o);
      public virtual bool Equals(Type? o);
      public virtual Type[] FindInterfaces(TypeFilter filter, object filterCriteria);
      public virtual MemberInfo[] FindMembers(MemberTypes memberType, BindingFlags bindingAttr, MemberFilter? filter, object filterCriteria);
      public virtual int GetArrayRank();
      protected abstract TypeAttributes GetAttributeFlagsImpl();
      public ConstructorInfo? GetConstructor(BindingFlags bindingAttr, Binder? binder, CallingConventions callConvention, Type[] types, ParameterModifier[]? modifiers);
      public ConstructorInfo? GetConstructor(BindingFlags bindingAttr, Binder? binder, Type[] types, ParameterModifier[]? modifiers);
      public ConstructorInfo? GetConstructor(Type[] types);
      protected abstract ConstructorInfo? GetConstructorImpl(BindingFlags bindingAttr, Binder? binder, CallingConventions callConvention, Type[] types, ParameterModifier[]? modifiers);
      public ConstructorInfo[] GetConstructors();
      public abstract ConstructorInfo[] GetConstructors(BindingFlags bindingAttr);
      public virtual MemberInfo[] GetDefaultMembers();
      public abstract Type? GetElementType();
      public virtual string? GetEnumName(object value);
      public virtual string[] GetEnumNames();
      public virtual Type GetEnumUnderlyingType();
      public virtual Array GetEnumValues();
      public EventInfo? GetEvent(string name);
      public abstract EventInfo? GetEvent(string name, BindingFlags bindingAttr);
      public virtual EventInfo[] GetEvents();
      public abstract EventInfo[] GetEvents(BindingFlags bindingAttr);
      public FieldInfo? GetField(string name);
      public abstract FieldInfo? GetField(string name, BindingFlags bindingAttr);
      public FieldInfo[] GetFields();
      public abstract FieldInfo[] GetFields(BindingFlags bindingAttr);
      public virtual Type[] GetGenericArguments();
      public virtual Type[] GetGenericParameterConstraints();
      public virtual Type GetGenericTypeDefinition();
      public override int GetHashCode();
      public Type? GetInterface(string name);
      public abstract Type? GetInterface(string name, bool ignoreCase);
      public virtual InterfaceMapping GetInterfaceMap(Type interfaceType);
      public abstract Type[] GetInterfaces();
      public MemberInfo[] GetMember(string name);
      public virtual MemberInfo[] GetMember(string name, BindingFlags bindingAttr);
      public virtual MemberInfo[] GetMember(string name, MemberTypes type, BindingFlags bindingAttr);
      public MemberInfo[] GetMembers();
      public abstract MemberInfo[] GetMembers(BindingFlags bindingAttr);
      public MethodInfo? GetMethod(string name);
      public MethodInfo? GetMethod(string name, int genericParameterCount, BindingFlags bindingAttr, Binder? binder, CallingConventions callConvention, Type[] types, ParameterModifier[]? modifiers);
      public MethodInfo? GetMethod(string name, int genericParameterCount, BindingFlags bindingAttr, Binder? binder, Type[] types, ParameterModifier[]? modifiers);
      public MethodInfo? GetMethod(string name, int genericParameterCount, Type[] types);
      public MethodInfo? GetMethod(string name, int genericParameterCount, Type[] types, ParameterModifier[]? modifiers);
      public MethodInfo? GetMethod(string name, BindingFlags bindingAttr);
      public MethodInfo? GetMethod(string name, BindingFlags bindingAttr, Binder? binder, CallingConventions callConvention, Type[] types, ParameterModifier[]? modifiers);
      public MethodInfo? GetMethod(string name, BindingFlags bindingAttr, Binder? binder, Type[] types, ParameterModifier[]? modifiers);
      public MethodInfo? GetMethod(string name, Type[] types);
      public MethodInfo? GetMethod(string name, Type[] types, ParameterModifier[]? modifiers);
      protected virtual MethodInfo? GetMethodImpl(string name, int genericParameterCount, BindingFlags bindingAttr, Binder? binder, CallingConventions callConvention, Type[]? types, ParameterModifier[]? modifiers);
      protected abstract MethodInfo? GetMethodImpl(string name, BindingFlags bindingAttr, Binder? binder, CallingConventions callConvention, Type[]? types, ParameterModifier[]? modifiers);
      public MethodInfo[] GetMethods();
      public abstract MethodInfo[] GetMethods(BindingFlags bindingAttr);
      public Type? GetNestedType(string name);
      public abstract Type? GetNestedType(string name, BindingFlags bindingAttr);
      public Type[] GetNestedTypes();
      public abstract Type[] GetNestedTypes(BindingFlags bindingAttr);
      public PropertyInfo[] GetProperties();
      public abstract PropertyInfo[] GetProperties(BindingFlags bindingAttr);
      public PropertyInfo? GetProperty(string name);
      public PropertyInfo? GetProperty(string name, BindingFlags bindingAttr);
      public PropertyInfo? GetProperty(string name, BindingFlags bindingAttr, Binder? binder, Type? returnType, Type[] types, ParameterModifier[]? modifiers);
      public PropertyInfo? GetProperty(string name, Type returnType);
      public PropertyInfo? GetProperty(string name, Type? returnType, Type[] types);
      public PropertyInfo? GetProperty(string name, Type? returnType, Type[] types, ParameterModifier[]? modifiers);
      public PropertyInfo? GetProperty(string name, Type[] types);
      protected abstract PropertyInfo? GetPropertyImpl(string name, BindingFlags bindingAttr, Binder? binder, Type? returnType, Type[]? types, ParameterModifier[]? modifiers);
      public new Type GetType();
      public static Type? GetType(string typeName);
      public static Type? GetType(string typeName, bool throwOnError);
      public static Type? GetType(string typeName, bool throwOnError, bool ignoreCase);
      public static Type? GetType(string typeName, Func<AssemblyName, Assembly?>? assemblyResolver, Func<Assembly?, string, bool, Type?>? typeResolver);
      public static Type? GetType(string typeName, Func<AssemblyName, Assembly?>? assemblyResolver, Func<Assembly?, string, bool, Type?>? typeResolver, bool throwOnError);
      public static Type? GetType(string typeName, Func<AssemblyName, Assembly?>? assemblyResolver, Func<Assembly?, string, bool, Type?>? typeResolver, bool throwOnError, bool ignoreCase);
      public static Type[] GetTypeArray(object[] args);
      public static TypeCode GetTypeCode(Type? type);
      protected virtual TypeCode GetTypeCodeImpl();
      public static Type GetTypeFromCLSID(Guid clsid);
      public static Type GetTypeFromCLSID(Guid clsid, bool throwOnError);
      public static Type GetTypeFromCLSID(Guid clsid, string? server);
      public static Type GetTypeFromCLSID(Guid clsid, string? server, bool throwOnError);
      public static Type GetTypeFromHandle(RuntimeTypeHandle handle);
      public static Type GetTypeFromProgID(string progID);
      public static Type GetTypeFromProgID(string progID, bool throwOnError);
      public static Type GetTypeFromProgID(string progID, string? server);
      public static Type GetTypeFromProgID(string progID, string? server, bool throwOnError);
      public static RuntimeTypeHandle GetTypeHandle(object o);
      protected abstract bool HasElementTypeImpl();
      public object? InvokeMember(string name, BindingFlags invokeAttr, Binder? binder, object? target, object[]? args);
      public object? InvokeMember(string name, BindingFlags invokeAttr, Binder? binder, object? target, object[]? args, CultureInfo? culture);
      public abstract object? InvokeMember(string name, BindingFlags invokeAttr, Binder? binder, object? target, object[]? args, ParameterModifier[]? modifiers, CultureInfo? culture, string[]? namedParameters);
      protected abstract bool IsArrayImpl();
      public virtual bool IsAssignableFrom(Type? c);
      protected abstract bool IsByRefImpl();
      protected abstract bool IsCOMObjectImpl();
      protected virtual bool IsContextfulImpl();
      public virtual bool IsEnumDefined(object value);
      public virtual bool IsEquivalentTo(Type? other);
      public virtual bool IsInstanceOfType(object? o);
      protected virtual bool IsMarshalByRefImpl();
      protected abstract bool IsPointerImpl();
      protected abstract bool IsPrimitiveImpl();
      public virtual bool IsSubclassOf(Type c);
      protected virtual bool IsValueTypeImpl();
      public virtual Type MakeArrayType();
      public virtual Type MakeArrayType(int rank);
      public virtual Type MakeByRefType();
      public static Type MakeGenericMethodParameter(int position);
      public static Type MakeGenericSignatureType(Type genericTypeDefinition, params Type[] typeArguments);
      public virtual Type MakeGenericType(params Type[] typeArguments);
      public virtual Type MakePointerType();
      public static bool operator ==(Type? left, Type? right);
      public static bool operator !=(Type? left, Type? right);
      public static Type ReflectionOnlyGetType(string typeName, bool throwIfNotFound, bool ignoreCase);
      public override string ToString();
    }
    public class TypeAccessException : TypeLoadException {
      public TypeAccessException();
      protected TypeAccessException(SerializationInfo info, StreamingContext context);
      public TypeAccessException(string? message);
      public TypeAccessException(string? message, Exception? inner);
    }
    public enum TypeCode {
      Boolean = 3,
      Byte = 6,
      Char = 4,
      DateTime = 16,
      DBNull = 2,
      Decimal = 15,
      Double = 14,
      Empty = 0,
      Int16 = 7,
      Int32 = 9,
      Int64 = 11,
      Object = 1,
      SByte = 5,
      Single = 13,
      String = 18,
      UInt16 = 8,
      UInt32 = 10,
      UInt64 = 12,
    }
    public ref struct TypedReference {
      public override bool Equals(object? o);
      public override int GetHashCode();
      public static Type GetTargetType(TypedReference value);
      public static TypedReference MakeTypedReference(object target, FieldInfo[] flds);
      public static void SetTypedReference(TypedReference target, object? value);
      public static RuntimeTypeHandle TargetTypeToken(TypedReference value);
      public static object ToObject(TypedReference value);
    }
    public sealed class TypeInitializationException : SystemException {
      public TypeInitializationException(string? fullTypeName, Exception? innerException);
      public string TypeName { get; }
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
    }
    public class TypeLoadException : SystemException, ISerializable {
      public TypeLoadException();
      protected TypeLoadException(SerializationInfo info, StreamingContext context);
      public TypeLoadException(string? message);
      public TypeLoadException(string? message, Exception? inner);
      public override string Message { get; }
      public string TypeName { get; }
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
    }
    public class TypeUnloadedException : SystemException {
      public TypeUnloadedException();
      protected TypeUnloadedException(SerializationInfo info, StreamingContext context);
      public TypeUnloadedException(string? message);
      public TypeUnloadedException(string? message, Exception? innerException);
    }
    public readonly struct UInt16 : IComparable, IComparable<ushort>, IConvertible, IEquatable<ushort>, IFormattable {
      public const ushort MaxValue = (ushort)65535;
      public const ushort MinValue = (ushort)0;
      public int CompareTo(object? value);
      public int CompareTo(UInt16 value);
      public override bool Equals(object? obj);
      public bool Equals(UInt16 obj);
      public override int GetHashCode();
      public TypeCode GetTypeCode();
      public static UInt16 Parse(ReadOnlySpan<char> s, NumberStyles style = NumberStyles.Integer, IFormatProvider? provider = null);
      public static UInt16 Parse(string s);
      public static UInt16 Parse(string s, NumberStyles style);
      public static UInt16 Parse(string s, NumberStyles style, IFormatProvider? provider);
      public static UInt16 Parse(string s, IFormatProvider? provider);
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      UInt16 System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      public override string ToString();
      public string ToString(IFormatProvider? provider);
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? provider);
      public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? provider = null);
      public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out UInt16 result);
      public static bool TryParse(ReadOnlySpan<char> s, out UInt16 result);
      public static bool TryParse(string? s, NumberStyles style, IFormatProvider? provider, out UInt16 result);
      public static bool TryParse(string? s, out UInt16 result);
    }
    public readonly struct UInt32 : IComparable, IComparable<uint>, IConvertible, IEquatable<uint>, IFormattable {
      public const uint MaxValue = (uint)4294967295;
      public const uint MinValue = (uint)0;
      public int CompareTo(object? value);
      public int CompareTo(UInt32 value);
      public override bool Equals(object? obj);
      public bool Equals(UInt32 obj);
      public override int GetHashCode();
      public TypeCode GetTypeCode();
      public static UInt32 Parse(ReadOnlySpan<char> s, NumberStyles style = NumberStyles.Integer, IFormatProvider? provider = null);
      public static UInt32 Parse(string s);
      public static UInt32 Parse(string s, NumberStyles style);
      public static UInt32 Parse(string s, NumberStyles style, IFormatProvider? provider);
      public static UInt32 Parse(string s, IFormatProvider? provider);
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      UInt32 System.IConvertible.ToUInt32(IFormatProvider? provider);
      ulong System.IConvertible.ToUInt64(IFormatProvider? provider);
      public override string ToString();
      public string ToString(IFormatProvider? provider);
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? provider);
      public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? provider = null);
      public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out UInt32 result);
      public static bool TryParse(ReadOnlySpan<char> s, out UInt32 result);
      public static bool TryParse(string? s, NumberStyles style, IFormatProvider? provider, out UInt32 result);
      public static bool TryParse(string? s, out UInt32 result);
    }
    public readonly struct UInt64 : IComparable, IComparable<ulong>, IConvertible, IEquatable<ulong>, IFormattable {
      public const ulong MaxValue = (ulong)18446744073709551615;
      public const ulong MinValue = (ulong)0;
      public int CompareTo(object? value);
      public int CompareTo(UInt64 value);
      public override bool Equals(object? obj);
      public bool Equals(UInt64 obj);
      public override int GetHashCode();
      public TypeCode GetTypeCode();
      public static UInt64 Parse(ReadOnlySpan<char> s, NumberStyles style = NumberStyles.Integer, IFormatProvider? provider = null);
      public static UInt64 Parse(string s);
      public static UInt64 Parse(string s, NumberStyles style);
      public static UInt64 Parse(string s, NumberStyles style, IFormatProvider? provider);
      public static UInt64 Parse(string s, IFormatProvider? provider);
      bool System.IConvertible.ToBoolean(IFormatProvider? provider);
      byte System.IConvertible.ToByte(IFormatProvider? provider);
      char System.IConvertible.ToChar(IFormatProvider? provider);
      DateTime System.IConvertible.ToDateTime(IFormatProvider? provider);
      decimal System.IConvertible.ToDecimal(IFormatProvider? provider);
      double System.IConvertible.ToDouble(IFormatProvider? provider);
      short System.IConvertible.ToInt16(IFormatProvider? provider);
      int System.IConvertible.ToInt32(IFormatProvider? provider);
      long System.IConvertible.ToInt64(IFormatProvider? provider);
      sbyte System.IConvertible.ToSByte(IFormatProvider? provider);
      float System.IConvertible.ToSingle(IFormatProvider? provider);
      object System.IConvertible.ToType(Type type, IFormatProvider? provider);
      ushort System.IConvertible.ToUInt16(IFormatProvider? provider);
      uint System.IConvertible.ToUInt32(IFormatProvider? provider);
      UInt64 System.IConvertible.ToUInt64(IFormatProvider? provider);
      public override string ToString();
      public string ToString(IFormatProvider? provider);
      public string ToString(string? format);
      public string ToString(string? format, IFormatProvider? provider);
      public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? provider = null);
      public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider? provider, out UInt64 result);
      public static bool TryParse(ReadOnlySpan<char> s, out UInt64 result);
      public static bool TryParse(string? s, NumberStyles style, IFormatProvider? provider, out UInt64 result);
      public static bool TryParse(string? s, out UInt64 result);
    }
    public readonly struct UIntPtr : IEquatable<UIntPtr>, ISerializable {
      public static readonly UIntPtr Zero;
      public UIntPtr(uint value);
      public UIntPtr(ulong value);
      public unsafe UIntPtr(void* value);
      public static int Size { get; }
      public static UIntPtr Add(UIntPtr pointer, int offset);
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public static UIntPtr operator +(UIntPtr pointer, int offset);
      public static bool operator ==(UIntPtr value1, UIntPtr value2);
      public static explicit operator UIntPtr (uint value);
      public static explicit operator UIntPtr (ulong value);
      public static explicit operator uint (UIntPtr value);
      public static explicit operator ulong (UIntPtr value);
      public unsafe static explicit operator void* (UIntPtr value);
      public unsafe static explicit operator UIntPtr (void* value);
      public static bool operator !=(UIntPtr value1, UIntPtr value2);
      public static UIntPtr operator -(UIntPtr pointer, int offset);
      public static UIntPtr Subtract(UIntPtr pointer, int offset);
      bool System.IEquatable<System.UIntPtr>.Equals(UIntPtr other);
      void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
      public unsafe void* ToPointer();
      public override string ToString();
      public uint ToUInt32();
      public ulong ToUInt64();
    }
    public class UnauthorizedAccessException : SystemException {
      public UnauthorizedAccessException();
      protected UnauthorizedAccessException(SerializationInfo info, StreamingContext context);
      public UnauthorizedAccessException(string? message);
      public UnauthorizedAccessException(string? message, Exception? inner);
    }
    public class UnhandledExceptionEventArgs : EventArgs {
      public UnhandledExceptionEventArgs(object exception, bool isTerminating);
      public object ExceptionObject { get; }
      public bool IsTerminating { get; }
    }
    public delegate void UnhandledExceptionEventHandler(object sender, UnhandledExceptionEventArgs e);
    public class Uri : ISerializable {
      public static readonly string SchemeDelimiter;
      public static readonly string UriSchemeFile;
      public static readonly string UriSchemeFtp;
      public static readonly string UriSchemeGopher;
      public static readonly string UriSchemeHttp;
      public static readonly string UriSchemeHttps;
      public static readonly string UriSchemeMailto;
      public static readonly string UriSchemeNetPipe;
      public static readonly string UriSchemeNetTcp;
      public static readonly string UriSchemeNews;
      public static readonly string UriSchemeNntp;
      protected Uri(SerializationInfo serializationInfo, StreamingContext streamingContext);
      public Uri(string uriString);
      public Uri(string uriString, bool dontEscape);
      public Uri(string uriString, UriKind uriKind);
      public Uri(Uri baseUri, string relativeUri);
      public Uri(Uri baseUri, string relativeUri, bool dontEscape);
      public Uri(Uri baseUri, Uri relativeUri);
      public string AbsolutePath { get; }
      public string AbsoluteUri { get; }
      public string Authority { get; }
      public string DnsSafeHost { get; }
      public string Fragment { get; }
      public string Host { get; }
      public UriHostNameType HostNameType { get; }
      public string IdnHost { get; }
      public bool IsAbsoluteUri { get; }
      public bool IsDefaultPort { get; }
      public bool IsFile { get; }
      public bool IsLoopback { get; }
      public bool IsUnc { get; }
      public string LocalPath { get; }
      public string OriginalString { get; }
      public string PathAndQuery { get; }
      public int Port { get; }
      public string Query { get; }
      public string Scheme { get; }
      public string[] Segments { get; }
      public bool UserEscaped { get; }
      public string UserInfo { get; }
      protected virtual void Canonicalize();
      public static UriHostNameType CheckHostName(string name);
      public static bool CheckSchemeName(string schemeName);
      protected virtual void CheckSecurity();
      public static int Compare(Uri uri1, Uri uri2, UriComponents partsToCompare, UriFormat compareFormat, StringComparison comparisonType);
      public override bool Equals(object comparand);
      protected virtual void Escape();
      public static string EscapeDataString(string stringToEscape);
      protected static string EscapeString(string str);
      public static string EscapeUriString(string stringToEscape);
      public static int FromHex(char digit);
      public string GetComponents(UriComponents components, UriFormat format);
      public override int GetHashCode();
      public string GetLeftPart(UriPartial part);
      protected void GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
      public static string HexEscape(char character);
      public static char HexUnescape(string pattern, ref int index);
      protected virtual bool IsBadFileSystemCharacter(char character);
      public bool IsBaseOf(Uri uri);
      protected static bool IsExcludedCharacter(char character);
      public static bool IsHexDigit(char character);
      public static bool IsHexEncoding(string pattern, int index);
      protected virtual bool IsReservedCharacter(char character);
      public bool IsWellFormedOriginalString();
      public static bool IsWellFormedUriString(string uriString, UriKind uriKind);
      public string MakeRelative(Uri toUri);
      public Uri MakeRelativeUri(Uri uri);
      public static bool operator ==(Uri uri1, Uri uri2);
      public static bool operator !=(Uri uri1, Uri uri2);
      protected virtual void Parse();
      void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
      public override string ToString();
      public static bool TryCreate(string uriString, UriKind uriKind, out Uri result);
      public static bool TryCreate(Uri baseUri, string relativeUri, out Uri result);
      public static bool TryCreate(Uri baseUri, Uri relativeUri, out Uri result);
      protected virtual string Unescape(string path);
      public static string UnescapeDataString(string stringToUnescape);
    }
    public enum UriComponents {
      AbsoluteUri = 127,
      Fragment = 64,
      Host = 4,
      HostAndPort = 132,
      HttpRequestUrl = 61,
      KeepDelimiter = 1073741824,
      NormalizedHost = 256,
      Path = 16,
      PathAndQuery = 48,
      Port = 8,
      Query = 32,
      Scheme = 1,
      SchemeAndServer = 13,
      SerializationInfoString = -2147483648,
      StrongAuthority = 134,
      StrongPort = 128,
      UserInfo = 2,
    }
    public enum UriFormat {
      SafeUnescaped = 3,
      Unescaped = 2,
      UriEscaped = 1,
    }
    public class UriFormatException : FormatException, ISerializable {
      public UriFormatException();
      protected UriFormatException(SerializationInfo serializationInfo, StreamingContext streamingContext);
      public UriFormatException(string textString);
      public UriFormatException(string textString, Exception e);
      void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
    }
    public enum UriHostNameType {
      Basic = 1,
      Dns = 2,
      IPv4 = 3,
      IPv6 = 4,
      Unknown = 0,
    }
    public enum UriKind {
      Absolute = 1,
      Relative = 2,
      RelativeOrAbsolute = 0,
    }
    public abstract class UriParser {
      protected UriParser();
      protected virtual string GetComponents(Uri uri, UriComponents components, UriFormat format);
      protected virtual void InitializeAndValidate(Uri uri, out UriFormatException parsingError);
      protected virtual bool IsBaseOf(Uri baseUri, Uri relativeUri);
      public static bool IsKnownScheme(string schemeName);
      protected virtual bool IsWellFormedOriginalString(Uri uri);
      protected virtual UriParser OnNewUri();
      protected virtual void OnRegister(string schemeName, int defaultPort);
      public static void Register(UriParser uriParser, string schemeName, int defaultPort);
      protected virtual string Resolve(Uri baseUri, Uri relativeUri, out UriFormatException parsingError);
    }
    public enum UriPartial {
      Authority = 1,
      Path = 2,
      Query = 3,
      Scheme = 0,
    }
    public struct ValueTuple : IComparable, IComparable<ValueTuple>, IEquatable<ValueTuple>, IStructuralComparable, IStructuralEquatable, ITuple {
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public int CompareTo(ValueTuple other);
      public static ValueTuple Create();
      public static (T1, T2, T3, T4, T5, T6, T7, T8) Create<T1, T2, T3, T4, T5, T6, T7, T8>(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5, T6 item6, T7 item7, T8 item8);
      public static (T1, T2, T3, T4, T5, T6, T7) Create<T1, T2, T3, T4, T5, T6, T7>(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5, T6 item6, T7 item7);
      public static (T1, T2, T3, T4, T5, T6) Create<T1, T2, T3, T4, T5, T6>(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5, T6 item6);
      public static (T1, T2, T3, T4, T5) Create<T1, T2, T3, T4, T5>(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5);
      public static (T1, T2, T3, T4) Create<T1, T2, T3, T4>(T1 item1, T2 item2, T3 item3, T4 item4);
      public static (T1, T2, T3) Create<T1, T2, T3>(T1 item1, T2 item2, T3 item3);
      public static (T1, T2) Create<T1, T2>(T1 item1, T2 item2);
      public static ValueTuple<T1> Create<T1>(T1 item1);
      public override bool Equals(object? obj);
      public bool Equals(ValueTuple other);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? other);
      public override string ToString();
    }
    public struct ValueTuple<T1> : IComparable, IComparable<ValueTuple<T1>>, IEquatable<ValueTuple<T1>>, IStructuralComparable, IStructuralEquatable, ITuple {
      public T1 Item1;
      public ValueTuple(T1 item1);
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public int CompareTo(ValueTuple<T1> other);
      public override bool Equals(object? obj);
      public bool Equals(ValueTuple<T1> other);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? other);
      public override string ToString();
    }
    public struct ValueTuple<T1, T2> : IComparable, IComparable<(T1, T2)>, IEquatable<(T1, T2)>, IStructuralComparable, IStructuralEquatable, ITuple {
      public T1 Item1;
      public T2 Item2;
      public ValueTuple(T1 item1, T2 item2);
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public int CompareTo((T1, T2) other);
      public override bool Equals(object? obj);
      public bool Equals((T1, T2) other);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? other);
      public override string ToString();
    }
    public struct ValueTuple<T1, T2, T3> : IComparable, IComparable<(T1, T2, T3)>, IEquatable<(T1, T2, T3)>, IStructuralComparable, IStructuralEquatable, ITuple {
      public T1 Item1;
      public T2 Item2;
      public T3 Item3;
      public ValueTuple(T1 item1, T2 item2, T3 item3);
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public int CompareTo((T1, T2, T3) other);
      public override bool Equals(object? obj);
      public bool Equals((T1, T2, T3) other);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? other);
      public override string ToString();
    }
    public struct ValueTuple<T1, T2, T3, T4> : IComparable, IComparable<(T1, T2, T3, T4)>, IEquatable<(T1, T2, T3, T4)>, IStructuralComparable, IStructuralEquatable, ITuple {
      public T1 Item1;
      public T2 Item2;
      public T3 Item3;
      public T4 Item4;
      public ValueTuple(T1 item1, T2 item2, T3 item3, T4 item4);
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public int CompareTo((T1, T2, T3, T4) other);
      public override bool Equals(object? obj);
      public bool Equals((T1, T2, T3, T4) other);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? other);
      public override string ToString();
    }
    public struct ValueTuple<T1, T2, T3, T4, T5> : IComparable, IComparable<(T1, T2, T3, T4, T5)>, IEquatable<(T1, T2, T3, T4, T5)>, IStructuralComparable, IStructuralEquatable, ITuple {
      public T1 Item1;
      public T2 Item2;
      public T3 Item3;
      public T4 Item4;
      public T5 Item5;
      public ValueTuple(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5);
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public int CompareTo((T1, T2, T3, T4, T5) other);
      public override bool Equals(object? obj);
      public bool Equals((T1, T2, T3, T4, T5) other);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? other);
      public override string ToString();
    }
    public struct ValueTuple<T1, T2, T3, T4, T5, T6> : IComparable, IComparable<(T1, T2, T3, T4, T5, T6)>, IEquatable<(T1, T2, T3, T4, T5, T6)>, IStructuralComparable, IStructuralEquatable, ITuple {
      public T1 Item1;
      public T2 Item2;
      public T3 Item3;
      public T4 Item4;
      public T5 Item5;
      public T6 Item6;
      public ValueTuple(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5, T6 item6);
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public int CompareTo((T1, T2, T3, T4, T5, T6) other);
      public override bool Equals(object? obj);
      public bool Equals((T1, T2, T3, T4, T5, T6) other);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? other);
      public override string ToString();
    }
    public struct ValueTuple<T1, T2, T3, T4, T5, T6, T7> : IComparable, IComparable<(T1, T2, T3, T4, T5, T6, T7)>, IEquatable<(T1, T2, T3, T4, T5, T6, T7)>, IStructuralComparable, IStructuralEquatable, ITuple {
      public T1 Item1;
      public T2 Item2;
      public T3 Item3;
      public T4 Item4;
      public T5 Item5;
      public T6 Item6;
      public T7 Item7;
      public ValueTuple(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5, T6 item6, T7 item7);
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public int CompareTo((T1, T2, T3, T4, T5, T6, T7) other);
      public override bool Equals(object? obj);
      public bool Equals((T1, T2, T3, T4, T5, T6, T7) other);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? other);
      public override string ToString();
    }
    public struct ValueTuple<T1, T2, T3, T4, T5, T6, T7, TRest> : IComparable, IComparable<(T1, T2, T3, T4, T5, T6, T7, TRest)>, IEquatable<(T1, T2, T3, T4, T5, T6, T7, TRest)>, IStructuralComparable, IStructuralEquatable, ITuple where TRest : struct {
      public T1 Item1;
      public T2 Item2;
      public T3 Item3;
      public T4 Item4;
      public T5 Item5;
      public T6 Item6;
      public T7 Item7;
      public TRest Rest;
      public ValueTuple(T1 item1, T2 item2, T3 item3, T4 item4, T5 item5, T6 item6, T7 item7, TRest rest);
      object? System.Runtime.CompilerServices.ITuple.this[int index] { get; }
      int System.Runtime.CompilerServices.ITuple.Length { get; }
      public int CompareTo((T1, T2, T3, T4, T5, T6, T7, TRest) other);
      public override bool Equals(object? obj);
      public bool Equals((T1, T2, T3, T4, T5, T6, T7, TRest) other);
      public override int GetHashCode();
      int System.Collections.IStructuralComparable.CompareTo(object? other, IComparer comparer);
      bool System.Collections.IStructuralEquatable.Equals(object? other, IEqualityComparer comparer);
      int System.Collections.IStructuralEquatable.GetHashCode(IEqualityComparer comparer);
      int System.IComparable.CompareTo(object? other);
      public override string ToString();
    }
    public abstract class ValueType {
      protected ValueType();
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public override string ToString();
    }
    public sealed class Version : ICloneable, IComparable, IComparable<Version?>, IEquatable<Version?> {
      public Version();
      public Version(int major, int minor);
      public Version(int major, int minor, int build);
      public Version(int major, int minor, int build, int revision);
      public Version(string version);
      public int Build { get; }
      public int Major { get; }
      public short MajorRevision { get; }
      public int Minor { get; }
      public short MinorRevision { get; }
      public int Revision { get; }
      public object Clone();
      public int CompareTo(object? version);
      public int CompareTo(Version? value);
      public override bool Equals(object? obj);
      public bool Equals(Version? obj);
      public override int GetHashCode();
      public static bool operator ==(Version? v1, Version? v2);
      public static bool operator >(Version? v1, Version? v2);
      public static bool operator >=(Version? v1, Version? v2);
      public static bool operator !=(Version? v1, Version? v2);
      public static bool operator <(Version? v1, Version? v2);
      public static bool operator <=(Version? v1, Version? v2);
      public static Version Parse(ReadOnlySpan<char> input);
      public static Version Parse(string input);
      public override string ToString();
      public string ToString(int fieldCount);
      public bool TryFormat(Span<char> destination, out int charsWritten);
      public bool TryFormat(Span<char> destination, int fieldCount, out int charsWritten);
      public static bool TryParse(ReadOnlySpan<char> input, out Version? result);
      public static bool TryParse(string? input, out Version? result);
    }
    public struct Void {
    }
    public class WeakReference : ISerializable {
      public WeakReference(object? target);
      public WeakReference(object? target, bool trackResurrection);
      protected WeakReference(SerializationInfo info, StreamingContext context);
      public virtual bool IsAlive { get; }
      public virtual object? Target { get; set; }
      public virtual bool TrackResurrection { get; }
      ~WeakReference();
      public virtual void GetObjectData(SerializationInfo info, StreamingContext context);
    }
    public sealed class WeakReference<T> : ISerializable where T : class? {
      public WeakReference(T target);
      public WeakReference(T target, bool trackResurrection);
      ~WeakReference();
      public void GetObjectData(SerializationInfo info, StreamingContext context);
      public void SetTarget(T target);
      public bool TryGetTarget(out T target);
    }
  }
  namespace System.Buffers {
    public interface IMemoryOwner<T> : IDisposable {
      Memory<T> Memory { get; }
    }
    public interface IPinnable {
      MemoryHandle Pin(int elementIndex);
      void Unpin();
    }
    public struct MemoryHandle : IDisposable {
      public unsafe MemoryHandle(void* pointer, GCHandle handle = default(GCHandle), IPinnable? pinnable = null);
      public unsafe void* Pointer { get; }
      public void Dispose();
    }
    public abstract class MemoryManager<T> : IDisposable, IMemoryOwner<T>, IPinnable {
      protected MemoryManager();
      public virtual Memory<T> Memory { get; }
      protected Memory<T> CreateMemory(int length);
      protected Memory<T> CreateMemory(int start, int length);
      protected abstract void Dispose(bool disposing);
      public abstract Span<T> GetSpan();
      public abstract MemoryHandle Pin(int elementIndex = 0);
      void System.IDisposable.Dispose();
      protected internal virtual bool TryGetArray(out ArraySegment<T> segment);
      public abstract void Unpin();
    }
    public enum OperationStatus {
      DestinationTooSmall = 1,
      Done = 0,
      InvalidData = 3,
      NeedMoreData = 2,
    }
    public delegate void ReadOnlySpanAction<T, in TArg>(ReadOnlySpan<T> span, TArg arg);
    public delegate void SpanAction<T, in TArg>(Span<T> span, TArg arg);
  }
  namespace System.Collections {
    public struct DictionaryEntry {
      public DictionaryEntry(object key, object? value);
      public object Key { get; set; }
      public object? Value { get; set; }
      public void Deconstruct(out object key, out object? value);
    }
    public interface ICollection : IEnumerable {
      int Count { get; }
      bool IsSynchronized { get; }
      object SyncRoot { get; }
      void CopyTo(Array array, int index);
    }
    public interface IComparer {
      int Compare(object? x, object? y);
    }
    public interface IDictionary : ICollection, IEnumerable {
      bool IsFixedSize { get; }
      bool IsReadOnly { get; }
      ICollection Keys { get; }
      object? this[object key] { get; set; }
      ICollection Values { get; }
      void Add(object key, object? value);
      void Clear();
      bool Contains(object key);
      new IDictionaryEnumerator GetEnumerator();
      void Remove(object key);
    }
    public interface IDictionaryEnumerator : IEnumerator {
      DictionaryEntry Entry { get; }
      object Key { get; }
      object? Value { get; }
    }
    public interface IEnumerable {
      IEnumerator GetEnumerator();
    }
    public interface IEnumerator {
      object? Current { get; }
      bool MoveNext();
      void Reset();
    }
    public interface IEqualityComparer {
      bool Equals(object? x, object? y);
      int GetHashCode(object obj);
    }
    public interface IList : ICollection, IEnumerable {
      bool IsFixedSize { get; }
      bool IsReadOnly { get; }
      object? this[int index] { get; set; }
      int Add(object? value);
      void Clear();
      bool Contains(object? value);
      int IndexOf(object? value);
      void Insert(int index, object? value);
      void Remove(object? value);
      void RemoveAt(int index);
    }
    public interface IStructuralComparable {
      int CompareTo(object? other, IComparer comparer);
    }
    public interface IStructuralEquatable {
      bool Equals(object? other, IEqualityComparer comparer);
      int GetHashCode(IEqualityComparer comparer);
    }
  }
  namespace System.Collections.Generic {
    public interface IAsyncEnumerable<out T> {
      IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default(CancellationToken));
    }
    public interface IAsyncEnumerator<out T> : IAsyncDisposable {
      T Current { get; }
      ValueTask<bool> MoveNextAsync();
    }
    public interface ICollection<T> : IEnumerable, IEnumerable<T> {
      int Count { get; }
      bool IsReadOnly { get; }
      void Add(T item);
      void Clear();
      bool Contains(T item);
      void CopyTo(T[] array, int arrayIndex);
      bool Remove(T item);
    }
    public interface IComparer<in T> {
      int Compare(T x, T y);
    }
    public interface IDictionary<TKey, TValue> : ICollection<KeyValuePair<TKey, TValue>>, IEnumerable, IEnumerable<KeyValuePair<TKey, TValue>> where TKey : object {
      ICollection<TKey> Keys { get; }
      TValue this[TKey key] { get; set; }
      ICollection<TValue> Values { get; }
      void Add(TKey key, TValue value);
      bool ContainsKey(TKey key);
      bool Remove(TKey key);
      bool TryGetValue(TKey key, out TValue value);
    }
    public interface IEnumerable<out T> : IEnumerable {
      new IEnumerator<T> GetEnumerator();
    }
    public interface IEnumerator<out T> : IDisposable, IEnumerator {
      new T Current { get; }
    }
    public interface IEqualityComparer<in T> {
      bool Equals(T x, T y);
      int GetHashCode(T obj);
    }
    public interface IList<T> : ICollection<T>, IEnumerable, IEnumerable<T> {
      T this[int index] { get; set; }
      int IndexOf(T item);
      void Insert(int index, T item);
      void RemoveAt(int index);
    }
    public interface IReadOnlyCollection<out T> : IEnumerable, IEnumerable<T> {
      int Count { get; }
    }
    public interface IReadOnlyDictionary<TKey, TValue> : IEnumerable, IEnumerable<KeyValuePair<TKey, TValue>>, IReadOnlyCollection<KeyValuePair<TKey, TValue>> where TKey : object {
      IEnumerable<TKey> Keys { get; }
      TValue this[TKey key] { get; }
      IEnumerable<TValue> Values { get; }
      bool ContainsKey(TKey key);
      bool TryGetValue(TKey key, out TValue value);
    }
    public interface IReadOnlyList<out T> : IEnumerable, IEnumerable<T>, IReadOnlyCollection<T> {
      T this[int index] { get; }
    }
    public interface ISet<T> : ICollection<T>, IEnumerable, IEnumerable<T> {
      new bool Add(T item);
      void ExceptWith(IEnumerable<T> other);
      void IntersectWith(IEnumerable<T> other);
      bool IsProperSubsetOf(IEnumerable<T> other);
      bool IsProperSupersetOf(IEnumerable<T> other);
      bool IsSubsetOf(IEnumerable<T> other);
      bool IsSupersetOf(IEnumerable<T> other);
      bool Overlaps(IEnumerable<T> other);
      bool SetEquals(IEnumerable<T> other);
      void SymmetricExceptWith(IEnumerable<T> other);
      void UnionWith(IEnumerable<T> other);
    }
    public class KeyNotFoundException : SystemException {
      public KeyNotFoundException();
      protected KeyNotFoundException(SerializationInfo info, StreamingContext context);
      public KeyNotFoundException(string? message);
      public KeyNotFoundException(string? message, Exception? innerException);
    }
    public static class KeyValuePair {
      public static KeyValuePair<TKey, TValue> Create<TKey, TValue>(TKey key, TValue value);
    }
    public readonly struct KeyValuePair<TKey, TValue> {
      public KeyValuePair(TKey key, TValue value);
      public TKey Key { get; }
      public TValue Value { get; }
      public void Deconstruct(out TKey key, out TValue value);
      public override string ToString();
    }
  }
  namespace System.Collections.ObjectModel {
    public class Collection<T> : ICollection, ICollection<T>, IEnumerable, IEnumerable<T>, IList, IList<T>, IReadOnlyCollection<T>, IReadOnlyList<T> {
      public Collection();
      public Collection(IList<T> list);
      public int Count { get; }
      protected IList<T> Items { get; }
      bool System.Collections.Generic.ICollection<T>.IsReadOnlyICollection<T>.IsReadOnly { get; }
      bool System.Collections.ICollection.IsSynchronized { get; }
      object System.Collections.ICollection.SyncRoot { get; }
      bool System.Collections.IList.IsFixedSize { get; }
      bool System.Collections.IList.IsReadOnly { get; }
      object? System.Collections.IList.this[int index] { get; set; }
      public T this[int index] { get; set; }
      public void Add(T item);
      public void AddRange(IEnumerable<T> collection);
      public void Clear();
      protected virtual void ClearItems();
      public bool Contains(T item);
      public void CopyTo(T[] array, int index);
      public IEnumerator<T> GetEnumerator();
      public int IndexOf(T item);
      public void Insert(int index, T item);
      protected virtual void InsertItem(int index, T item);
      protected virtual void InsertItemsRange(int index, IEnumerable<T> collection);
      public void InsertRange(int index, IEnumerable<T> collection);
      public bool Remove(T item);
      public void RemoveAt(int index);
      protected virtual void RemoveItem(int index);
      protected virtual void RemoveItemsRange(int index, int count);
      public void RemoveRange(int index, int count);
      protected virtual void ReplaceItemsRange(int index, int count, IEnumerable<T> collection);
      public void ReplaceRange(int index, int count, IEnumerable<T> collection);
      protected virtual void SetItem(int index, T item);
      void System.Collections.ICollection.CopyTo(Array array, int index);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      int System.Collections.IList.Add(object? value);
      bool System.Collections.IList.Contains(object? value);
      int System.Collections.IList.IndexOf(object? value);
      void System.Collections.IList.Insert(int index, object? value);
      void System.Collections.IList.Remove(object? value);
    }
    public class ReadOnlyCollection<T> : ICollection, ICollection<T>, IEnumerable, IEnumerable<T>, IList, IList<T>, IReadOnlyCollection<T>, IReadOnlyList<T> {
      public ReadOnlyCollection(IList<T> list);
      public int Count { get; }
      protected IList<T> Items { get; }
      bool System.Collections.Generic.ICollection<T>.IsReadOnlyICollection<T>.IsReadOnly { get; }
      T System.Collections.Generic.IList<T>.thisIList<T>.this[int index] { get; set; }
      bool System.Collections.ICollection.IsSynchronized { get; }
      object System.Collections.ICollection.SyncRoot { get; }
      bool System.Collections.IList.IsFixedSize { get; }
      bool System.Collections.IList.IsReadOnly { get; }
      object? System.Collections.IList.this[int index] { get; set; }
      public T this[int index] { get; }
      public bool Contains(T value);
      public void CopyTo(T[] array, int index);
      public IEnumerator<T> GetEnumerator();
      public int IndexOf(T value);
      void System.Collections.Generic.ICollection<T>.AddICollection<T>.Add(T value);
      void System.Collections.Generic.ICollection<T>.ClearICollection<T>.Clear();
      bool System.Collections.Generic.ICollection<T>.RemoveICollection<T>.Remove(T value);
      void System.Collections.Generic.IList<T>.InsertIList<T>.Insert(int index, T value);
      void System.Collections.Generic.IList<T>.RemoveAtIList<T>.RemoveAt(int index);
      void System.Collections.ICollection.CopyTo(Array array, int index);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      int System.Collections.IList.Add(object? value);
      void System.Collections.IList.Clear();
      bool System.Collections.IList.Contains(object? value);
      int System.Collections.IList.IndexOf(object? value);
      void System.Collections.IList.Insert(int index, object? value);
      void System.Collections.IList.Remove(object? value);
      void System.Collections.IList.RemoveAt(int index);
    }
  }
  namespace System.ComponentModel {
    public class DefaultValueAttribute : Attribute {
      public DefaultValueAttribute(bool value);
      public DefaultValueAttribute(byte value);
      public DefaultValueAttribute(char value);
      public DefaultValueAttribute(double value);
      public DefaultValueAttribute(short value);
      public DefaultValueAttribute(int value);
      public DefaultValueAttribute(long value);
      public DefaultValueAttribute(object? value);
      public DefaultValueAttribute(sbyte value);
      public DefaultValueAttribute(float value);
      public DefaultValueAttribute(string? value);
      public DefaultValueAttribute(Type? type, string? value);
      public DefaultValueAttribute(ushort value);
      public DefaultValueAttribute(uint value);
      public DefaultValueAttribute(ulong value);
      public virtual object? Value { get; }
      public override bool Equals(object? obj);
      public override int GetHashCode();
      protected void SetValue(object? value);
    }
    public sealed class EditorBrowsableAttribute : Attribute {
      public EditorBrowsableAttribute();
      public EditorBrowsableAttribute(EditorBrowsableState state);
      public EditorBrowsableState State { get; }
      public override bool Equals(object? obj);
      public override int GetHashCode();
    }
    public enum EditorBrowsableState {
      Advanced = 2,
      Always = 0,
      Never = 1,
    }
  }
  namespace System.Configuration.Assemblies {
    public enum AssemblyHashAlgorithm {
      MD5 = 32771,
      None = 0,
      SHA1 = 32772,
      SHA256 = 32780,
      SHA384 = 32781,
      SHA512 = 32782,
    }
    public enum AssemblyVersionCompatibility {
      SameDomain = 3,
      SameMachine = 1,
      SameProcess = 2,
    }
  }
  namespace System.Diagnostics {
    public sealed class ConditionalAttribute : Attribute {
      public ConditionalAttribute(string conditionString);
      public string ConditionString { get; }
    }
    public sealed class DebuggableAttribute : Attribute {
      public DebuggableAttribute(DebuggableAttribute.DebuggingModes modes);
      public DebuggableAttribute(bool isJITTrackingEnabled, bool isJITOptimizerDisabled);
      public DebuggableAttribute.DebuggingModes DebuggingFlags { get; }
      public bool IsJITOptimizerDisabled { get; }
      public bool IsJITTrackingEnabled { get; }
      public enum DebuggingModes {
        Default = 1,
        DisableOptimizations = 256,
        EnableEditAndContinue = 4,
        IgnoreSymbolStoreSequencePoints = 2,
        None = 0,
      }
    }
  }
  namespace System.Globalization {
    public abstract class Calendar : ICloneable {
      public const int CurrentEra = 0;
      protected Calendar();
      public virtual CalendarAlgorithmType AlgorithmType { get; }
      protected virtual int DaysInYearBeforeMinSupportedYear { get; }
      public abstract int[] Eras { get; }
      public bool IsReadOnly { get; }
      public virtual DateTime MaxSupportedDateTime { get; }
      public virtual DateTime MinSupportedDateTime { get; }
      public virtual int TwoDigitYearMax { get; set; }
      public virtual DateTime AddDays(DateTime time, int days);
      public virtual DateTime AddHours(DateTime time, int hours);
      public virtual DateTime AddMilliseconds(DateTime time, double milliseconds);
      public virtual DateTime AddMinutes(DateTime time, int minutes);
      public abstract DateTime AddMonths(DateTime time, int months);
      public virtual DateTime AddSeconds(DateTime time, int seconds);
      public virtual DateTime AddWeeks(DateTime time, int weeks);
      public abstract DateTime AddYears(DateTime time, int years);
      public virtual object Clone();
      public abstract int GetDayOfMonth(DateTime time);
      public abstract DayOfWeek GetDayOfWeek(DateTime time);
      public abstract int GetDayOfYear(DateTime time);
      public virtual int GetDaysInMonth(int year, int month);
      public abstract int GetDaysInMonth(int year, int month, int era);
      public virtual int GetDaysInYear(int year);
      public abstract int GetDaysInYear(int year, int era);
      public abstract int GetEra(DateTime time);
      public virtual int GetHour(DateTime time);
      public virtual int GetLeapMonth(int year);
      public virtual int GetLeapMonth(int year, int era);
      public virtual double GetMilliseconds(DateTime time);
      public virtual int GetMinute(DateTime time);
      public abstract int GetMonth(DateTime time);
      public virtual int GetMonthsInYear(int year);
      public abstract int GetMonthsInYear(int year, int era);
      public virtual int GetSecond(DateTime time);
      public virtual int GetWeekOfYear(DateTime time, CalendarWeekRule rule, DayOfWeek firstDayOfWeek);
      public abstract int GetYear(DateTime time);
      public virtual bool IsLeapDay(int year, int month, int day);
      public abstract bool IsLeapDay(int year, int month, int day, int era);
      public virtual bool IsLeapMonth(int year, int month);
      public abstract bool IsLeapMonth(int year, int month, int era);
      public virtual bool IsLeapYear(int year);
      public abstract bool IsLeapYear(int year, int era);
      public static Calendar ReadOnly(Calendar calendar);
      public virtual DateTime ToDateTime(int year, int month, int day, int hour, int minute, int second, int millisecond);
      public abstract DateTime ToDateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int era);
      public virtual int ToFourDigitYear(int year);
    }
    public enum CalendarAlgorithmType {
      LunarCalendar = 2,
      LunisolarCalendar = 3,
      SolarCalendar = 1,
      Unknown = 0,
    }
    public enum CalendarWeekRule {
      FirstDay = 0,
      FirstFourDayWeek = 2,
      FirstFullWeek = 1,
    }
    public static class CharUnicodeInfo {
      public static int GetDecimalDigitValue(char ch);
      public static int GetDecimalDigitValue(string s, int index);
      public static int GetDigitValue(char ch);
      public static int GetDigitValue(string s, int index);
      public static double GetNumericValue(char ch);
      public static double GetNumericValue(string s, int index);
      public static UnicodeCategory GetUnicodeCategory(char ch);
      public static UnicodeCategory GetUnicodeCategory(int codePoint);
      public static UnicodeCategory GetUnicodeCategory(string s, int index);
    }
    public class ChineseLunisolarCalendar : EastAsianLunisolarCalendar {
      public const int ChineseEra = 1;
      public ChineseLunisolarCalendar();
      protected override int DaysInYearBeforeMinSupportedYear { get; }
      public override int[] Eras { get; }
      public override DateTime MaxSupportedDateTime { get; }
      public override DateTime MinSupportedDateTime { get; }
      public override int GetEra(DateTime time);
    }
    public class CompareInfo : IDeserializationCallback {
      public int LCID { get; }
      public virtual string Name { get; }
      public SortVersion Version { get; }
      public virtual int Compare(string? string1, int offset1, int length1, string? string2, int offset2, int length2);
      public virtual int Compare(string? string1, int offset1, int length1, string? string2, int offset2, int length2, CompareOptions options);
      public virtual int Compare(string? string1, int offset1, string? string2, int offset2);
      public virtual int Compare(string? string1, int offset1, string? string2, int offset2, CompareOptions options);
      public virtual int Compare(string? string1, string? string2);
      public virtual int Compare(string? string1, string? string2, CompareOptions options);
      public override bool Equals(object? value);
      public static CompareInfo GetCompareInfo(int culture);
      public static CompareInfo GetCompareInfo(int culture, Assembly assembly);
      public static CompareInfo GetCompareInfo(string name);
      public static CompareInfo GetCompareInfo(string name, Assembly assembly);
      public override int GetHashCode();
      public int GetHashCode(ReadOnlySpan<char> source, CompareOptions options);
      public virtual int GetHashCode(string source, CompareOptions options);
      public virtual SortKey GetSortKey(string source);
      public virtual SortKey GetSortKey(string source, CompareOptions options);
      public virtual int IndexOf(string source, char value);
      public virtual int IndexOf(string source, char value, CompareOptions options);
      public virtual int IndexOf(string source, char value, int startIndex);
      public virtual int IndexOf(string source, char value, int startIndex, CompareOptions options);
      public virtual int IndexOf(string source, char value, int startIndex, int count);
      public virtual int IndexOf(string source, char value, int startIndex, int count, CompareOptions options);
      public virtual int IndexOf(string source, string value);
      public virtual int IndexOf(string source, string value, CompareOptions options);
      public virtual int IndexOf(string source, string value, int startIndex);
      public virtual int IndexOf(string source, string value, int startIndex, CompareOptions options);
      public virtual int IndexOf(string source, string value, int startIndex, int count);
      public virtual int IndexOf(string source, string value, int startIndex, int count, CompareOptions options);
      public virtual bool IsPrefix(string source, string prefix);
      public virtual bool IsPrefix(string source, string prefix, CompareOptions options);
      public static bool IsSortable(char ch);
      public static bool IsSortable(string text);
      public virtual bool IsSuffix(string source, string suffix);
      public virtual bool IsSuffix(string source, string suffix, CompareOptions options);
      public virtual int LastIndexOf(string source, char value);
      public virtual int LastIndexOf(string source, char value, CompareOptions options);
      public virtual int LastIndexOf(string source, char value, int startIndex);
      public virtual int LastIndexOf(string source, char value, int startIndex, CompareOptions options);
      public virtual int LastIndexOf(string source, char value, int startIndex, int count);
      public virtual int LastIndexOf(string source, char value, int startIndex, int count, CompareOptions options);
      public virtual int LastIndexOf(string source, string value);
      public virtual int LastIndexOf(string source, string value, CompareOptions options);
      public virtual int LastIndexOf(string source, string value, int startIndex);
      public virtual int LastIndexOf(string source, string value, int startIndex, CompareOptions options);
      public virtual int LastIndexOf(string source, string value, int startIndex, int count);
      public virtual int LastIndexOf(string source, string value, int startIndex, int count, CompareOptions options);
      void System.Runtime.Serialization.IDeserializationCallback.OnDeserialization(object sender);
      public override string ToString();
    }
    public enum CompareOptions {
      IgnoreCase = 1,
      IgnoreKanaType = 8,
      IgnoreNonSpace = 2,
      IgnoreSymbols = 4,
      IgnoreWidth = 16,
      None = 0,
      Ordinal = 1073741824,
      OrdinalIgnoreCase = 268435456,
      StringSort = 536870912,
    }
    public class CultureInfo : ICloneable, IFormatProvider {
      public CultureInfo(int culture);
      public CultureInfo(int culture, bool useUserOverride);
      public CultureInfo(string name);
      public CultureInfo(string name, bool useUserOverride);
      public virtual Calendar Calendar { get; }
      public virtual CompareInfo CompareInfo { get; }
      public CultureTypes CultureTypes { get; }
      public static CultureInfo CurrentCulture { get; set; }
      public static CultureInfo CurrentUICulture { get; set; }
      public virtual DateTimeFormatInfo DateTimeFormat { get; set; }
      public static CultureInfo? DefaultThreadCurrentCulture { get; set; }
      public static CultureInfo? DefaultThreadCurrentUICulture { get; set; }
      public virtual string DisplayName { get; }
      public virtual string EnglishName { get; }
      public string IetfLanguageTag { get; }
      public static CultureInfo InstalledUICulture { get; }
      public static CultureInfo InvariantCulture { get; }
      public virtual bool IsNeutralCulture { get; }
      public bool IsReadOnly { get; }
      public virtual int KeyboardLayoutId { get; }
      public virtual int LCID { get; }
      public virtual string Name { get; }
      public virtual string NativeName { get; }
      public virtual NumberFormatInfo NumberFormat { get; set; }
      public virtual Calendar[] OptionalCalendars { get; }
      public virtual CultureInfo Parent { get; }
      public virtual TextInfo TextInfo { get; }
      public virtual string ThreeLetterISOLanguageName { get; }
      public virtual string ThreeLetterWindowsLanguageName { get; }
      public virtual string TwoLetterISOLanguageName { get; }
      public bool UseUserOverride { get; }
      public void ClearCachedData();
      public virtual object Clone();
      public static CultureInfo CreateSpecificCulture(string name);
      public override bool Equals(object? value);
      public CultureInfo GetConsoleFallbackUICulture();
      public static CultureInfo GetCultureInfo(int culture);
      public static CultureInfo GetCultureInfo(string name);
      public static CultureInfo GetCultureInfo(string name, string altName);
      public static CultureInfo GetCultureInfoByIetfLanguageTag(string name);
      public static CultureInfo[] GetCultures(CultureTypes types);
      public virtual object? GetFormat(Type? formatType);
      public override int GetHashCode();
      public static CultureInfo ReadOnly(CultureInfo ci);
      public override string ToString();
    }
    public class CultureNotFoundException : ArgumentException {
      public CultureNotFoundException();
      protected CultureNotFoundException(SerializationInfo info, StreamingContext context);
      public CultureNotFoundException(string? message);
      public CultureNotFoundException(string? message, Exception? innerException);
      public CultureNotFoundException(string? message, int invalidCultureId, Exception? innerException);
      public CultureNotFoundException(string? paramName, int invalidCultureId, string? message);
      public CultureNotFoundException(string? paramName, string? message);
      public CultureNotFoundException(string? message, string? invalidCultureName, Exception? innerException);
      public CultureNotFoundException(string? paramName, string? invalidCultureName, string? message);
      public virtual int? InvalidCultureId { get; }
      public virtual string? InvalidCultureName { get; }
      public override string Message { get; }
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
    }
    public enum CultureTypes {
      AllCultures = 7,
      FrameworkCultures = 64,
      InstalledWin32Cultures = 4,
      NeutralCultures = 1,
      ReplacementCultures = 16,
      SpecificCultures = 2,
      UserCustomCulture = 8,
      WindowsOnlyCultures = 32,
    }
    public sealed class DateTimeFormatInfo : ICloneable, IFormatProvider {
      public DateTimeFormatInfo();
      public string[] AbbreviatedDayNames { get; set; }
      public string[] AbbreviatedMonthGenitiveNames { get; set; }
      public string[] AbbreviatedMonthNames { get; set; }
      public string AMDesignator { get; set; }
      public Calendar Calendar { get; set; }
      public CalendarWeekRule CalendarWeekRule { get; set; }
      public static DateTimeFormatInfo CurrentInfo { get; }
      public string DateSeparator { get; set; }
      public string[] DayNames { get; set; }
      public DayOfWeek FirstDayOfWeek { get; set; }
      public string FullDateTimePattern { get; set; }
      public static DateTimeFormatInfo InvariantInfo { get; }
      public bool IsReadOnly { get; }
      public string LongDatePattern { get; set; }
      public string LongTimePattern { get; set; }
      public string MonthDayPattern { get; set; }
      public string[] MonthGenitiveNames { get; set; }
      public string[] MonthNames { get; set; }
      public string NativeCalendarName { get; }
      public string PMDesignator { get; set; }
      public string RFC1123Pattern { get; }
      public string ShortDatePattern { get; set; }
      public string[] ShortestDayNames { get; set; }
      public string ShortTimePattern { get; set; }
      public string SortableDateTimePattern { get; }
      public string TimeSeparator { get; set; }
      public string UniversalSortableDateTimePattern { get; }
      public string YearMonthPattern { get; set; }
      public object Clone();
      public string GetAbbreviatedDayName(DayOfWeek dayofweek);
      public string GetAbbreviatedEraName(int era);
      public string GetAbbreviatedMonthName(int month);
      public string[] GetAllDateTimePatterns();
      public string[] GetAllDateTimePatterns(char format);
      public string GetDayName(DayOfWeek dayofweek);
      public int GetEra(string eraName);
      public string GetEraName(int era);
      public object? GetFormat(Type? formatType);
      public static DateTimeFormatInfo GetInstance(IFormatProvider? provider);
      public string GetMonthName(int month);
      public string GetShortestDayName(DayOfWeek dayOfWeek);
      public static DateTimeFormatInfo ReadOnly(DateTimeFormatInfo dtfi);
      public void SetAllDateTimePatterns(string[] patterns, char format);
    }
    public enum DateTimeStyles {
      AdjustToUniversal = 16,
      AllowInnerWhite = 4,
      AllowLeadingWhite = 1,
      AllowTrailingWhite = 2,
      AllowWhiteSpaces = 7,
      AssumeLocal = 32,
      AssumeUniversal = 64,
      NoCurrentDateDefault = 8,
      None = 0,
      RoundtripKind = 128,
    }
    public class DaylightTime {
      public DaylightTime(DateTime start, DateTime end, TimeSpan delta);
      public TimeSpan Delta { get; }
      public DateTime End { get; }
      public DateTime Start { get; }
    }
    public enum DigitShapes {
      Context = 0,
      NativeNational = 2,
      None = 1,
    }
    public abstract class EastAsianLunisolarCalendar : Calendar {
      public override CalendarAlgorithmType AlgorithmType { get; }
      public override int TwoDigitYearMax { get; set; }
      public override DateTime AddMonths(DateTime time, int months);
      public override DateTime AddYears(DateTime time, int years);
      public int GetCelestialStem(int sexagenaryYear);
      public override int GetDayOfMonth(DateTime time);
      public override DayOfWeek GetDayOfWeek(DateTime time);
      public override int GetDayOfYear(DateTime time);
      public override int GetDaysInMonth(int year, int month, int era);
      public override int GetDaysInYear(int year, int era);
      public override int GetLeapMonth(int year, int era);
      public override int GetMonth(DateTime time);
      public override int GetMonthsInYear(int year, int era);
      public virtual int GetSexagenaryYear(DateTime time);
      public int GetTerrestrialBranch(int sexagenaryYear);
      public override int GetYear(DateTime time);
      public override bool IsLeapDay(int year, int month, int day, int era);
      public override bool IsLeapMonth(int year, int month, int era);
      public override bool IsLeapYear(int year, int era);
      public override DateTime ToDateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int era);
      public override int ToFourDigitYear(int year);
    }
    public class GregorianCalendar : Calendar {
      public const int ADEra = 1;
      public GregorianCalendar();
      public GregorianCalendar(GregorianCalendarTypes type);
      public override CalendarAlgorithmType AlgorithmType { get; }
      public virtual GregorianCalendarTypes CalendarType { get; set; }
      public override int[] Eras { get; }
      public override DateTime MaxSupportedDateTime { get; }
      public override DateTime MinSupportedDateTime { get; }
      public override int TwoDigitYearMax { get; set; }
      public override DateTime AddMonths(DateTime time, int months);
      public override DateTime AddYears(DateTime time, int years);
      public override int GetDayOfMonth(DateTime time);
      public override DayOfWeek GetDayOfWeek(DateTime time);
      public override int GetDayOfYear(DateTime time);
      public override int GetDaysInMonth(int year, int month, int era);
      public override int GetDaysInYear(int year, int era);
      public override int GetEra(DateTime time);
      public override int GetLeapMonth(int year, int era);
      public override int GetMonth(DateTime time);
      public override int GetMonthsInYear(int year, int era);
      public override int GetYear(DateTime time);
      public override bool IsLeapDay(int year, int month, int day, int era);
      public override bool IsLeapMonth(int year, int month, int era);
      public override bool IsLeapYear(int year, int era);
      public override DateTime ToDateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int era);
      public override int ToFourDigitYear(int year);
    }
    public enum GregorianCalendarTypes {
      Arabic = 10,
      Localized = 1,
      MiddleEastFrench = 9,
      TransliteratedEnglish = 11,
      TransliteratedFrench = 12,
      USEnglish = 2,
    }
    public class HebrewCalendar : Calendar {
      public static readonly int HebrewEra;
      public HebrewCalendar();
      public override CalendarAlgorithmType AlgorithmType { get; }
      public override int[] Eras { get; }
      public override DateTime MaxSupportedDateTime { get; }
      public override DateTime MinSupportedDateTime { get; }
      public override int TwoDigitYearMax { get; set; }
      public override DateTime AddMonths(DateTime time, int months);
      public override DateTime AddYears(DateTime time, int years);
      public override int GetDayOfMonth(DateTime time);
      public override DayOfWeek GetDayOfWeek(DateTime time);
      public override int GetDayOfYear(DateTime time);
      public override int GetDaysInMonth(int year, int month, int era);
      public override int GetDaysInYear(int year, int era);
      public override int GetEra(DateTime time);
      public override int GetLeapMonth(int year, int era);
      public override int GetMonth(DateTime time);
      public override int GetMonthsInYear(int year, int era);
      public override int GetYear(DateTime time);
      public override bool IsLeapDay(int year, int month, int day, int era);
      public override bool IsLeapMonth(int year, int month, int era);
      public override bool IsLeapYear(int year, int era);
      public override DateTime ToDateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int era);
      public override int ToFourDigitYear(int year);
    }
    public class HijriCalendar : Calendar {
      public static readonly int HijriEra;
      public HijriCalendar();
      public override CalendarAlgorithmType AlgorithmType { get; }
      protected override int DaysInYearBeforeMinSupportedYear { get; }
      public override int[] Eras { get; }
      public int HijriAdjustment { get; set; }
      public override DateTime MaxSupportedDateTime { get; }
      public override DateTime MinSupportedDateTime { get; }
      public override int TwoDigitYearMax { get; set; }
      public override DateTime AddMonths(DateTime time, int months);
      public override DateTime AddYears(DateTime time, int years);
      public override int GetDayOfMonth(DateTime time);
      public override DayOfWeek GetDayOfWeek(DateTime time);
      public override int GetDayOfYear(DateTime time);
      public override int GetDaysInMonth(int year, int month, int era);
      public override int GetDaysInYear(int year, int era);
      public override int GetEra(DateTime time);
      public override int GetLeapMonth(int year, int era);
      public override int GetMonth(DateTime time);
      public override int GetMonthsInYear(int year, int era);
      public override int GetYear(DateTime time);
      public override bool IsLeapDay(int year, int month, int day, int era);
      public override bool IsLeapMonth(int year, int month, int era);
      public override bool IsLeapYear(int year, int era);
      public override DateTime ToDateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int era);
      public override int ToFourDigitYear(int year);
    }
    public sealed class IdnMapping {
      public IdnMapping();
      public bool AllowUnassigned { get; set; }
      public bool UseStd3AsciiRules { get; set; }
      public override bool Equals(object? obj);
      public string GetAscii(string unicode);
      public string GetAscii(string unicode, int index);
      public string GetAscii(string unicode, int index, int count);
      public override int GetHashCode();
      public string GetUnicode(string ascii);
      public string GetUnicode(string ascii, int index);
      public string GetUnicode(string ascii, int index, int count);
    }
    public static class ISOWeek {
      public static int GetWeekOfYear(DateTime date);
      public static int GetWeeksInYear(int year);
      public static int GetYear(DateTime date);
      public static DateTime GetYearEnd(int year);
      public static DateTime GetYearStart(int year);
      public static DateTime ToDateTime(int year, int week, DayOfWeek dayOfWeek);
    }
    public class JapaneseCalendar : Calendar {
      public JapaneseCalendar();
      public override CalendarAlgorithmType AlgorithmType { get; }
      public override int[] Eras { get; }
      public override DateTime MaxSupportedDateTime { get; }
      public override DateTime MinSupportedDateTime { get; }
      public override int TwoDigitYearMax { get; set; }
      public override DateTime AddMonths(DateTime time, int months);
      public override DateTime AddYears(DateTime time, int years);
      public override int GetDayOfMonth(DateTime time);
      public override DayOfWeek GetDayOfWeek(DateTime time);
      public override int GetDayOfYear(DateTime time);
      public override int GetDaysInMonth(int year, int month, int era);
      public override int GetDaysInYear(int year, int era);
      public override int GetEra(DateTime time);
      public override int GetLeapMonth(int year, int era);
      public override int GetMonth(DateTime time);
      public override int GetMonthsInYear(int year, int era);
      public override int GetWeekOfYear(DateTime time, CalendarWeekRule rule, DayOfWeek firstDayOfWeek);
      public override int GetYear(DateTime time);
      public override bool IsLeapDay(int year, int month, int day, int era);
      public override bool IsLeapMonth(int year, int month, int era);
      public override bool IsLeapYear(int year, int era);
      public override DateTime ToDateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int era);
      public override int ToFourDigitYear(int year);
    }
    public class JapaneseLunisolarCalendar : EastAsianLunisolarCalendar {
      public const int JapaneseEra = 1;
      public JapaneseLunisolarCalendar();
      protected override int DaysInYearBeforeMinSupportedYear { get; }
      public override int[] Eras { get; }
      public override DateTime MaxSupportedDateTime { get; }
      public override DateTime MinSupportedDateTime { get; }
      public override int GetEra(DateTime time);
    }
    public class JulianCalendar : Calendar {
      public static readonly int JulianEra;
      public JulianCalendar();
      public override CalendarAlgorithmType AlgorithmType { get; }
      public override int[] Eras { get; }
      public override DateTime MaxSupportedDateTime { get; }
      public override DateTime MinSupportedDateTime { get; }
      public override int TwoDigitYearMax { get; set; }
      public override DateTime AddMonths(DateTime time, int months);
      public override DateTime AddYears(DateTime time, int years);
      public override int GetDayOfMonth(DateTime time);
      public override DayOfWeek GetDayOfWeek(DateTime time);
      public override int GetDayOfYear(DateTime time);
      public override int GetDaysInMonth(int year, int month, int era);
      public override int GetDaysInYear(int year, int era);
      public override int GetEra(DateTime time);
      public override int GetLeapMonth(int year, int era);
      public override int GetMonth(DateTime time);
      public override int GetMonthsInYear(int year, int era);
      public override int GetYear(DateTime time);
      public override bool IsLeapDay(int year, int month, int day, int era);
      public override bool IsLeapMonth(int year, int month, int era);
      public override bool IsLeapYear(int year, int era);
      public override DateTime ToDateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int era);
      public override int ToFourDigitYear(int year);
    }
    public class KoreanCalendar : Calendar {
      public const int KoreanEra = 1;
      public KoreanCalendar();
      public override CalendarAlgorithmType AlgorithmType { get; }
      public override int[] Eras { get; }
      public override DateTime MaxSupportedDateTime { get; }
      public override DateTime MinSupportedDateTime { get; }
      public override int TwoDigitYearMax { get; set; }
      public override DateTime AddMonths(DateTime time, int months);
      public override DateTime AddYears(DateTime time, int years);
      public override int GetDayOfMonth(DateTime time);
      public override DayOfWeek GetDayOfWeek(DateTime time);
      public override int GetDayOfYear(DateTime time);
      public override int GetDaysInMonth(int year, int month, int era);
      public override int GetDaysInYear(int year, int era);
      public override int GetEra(DateTime time);
      public override int GetLeapMonth(int year, int era);
      public override int GetMonth(DateTime time);
      public override int GetMonthsInYear(int year, int era);
      public override int GetWeekOfYear(DateTime time, CalendarWeekRule rule, DayOfWeek firstDayOfWeek);
      public override int GetYear(DateTime time);
      public override bool IsLeapDay(int year, int month, int day, int era);
      public override bool IsLeapMonth(int year, int month, int era);
      public override bool IsLeapYear(int year, int era);
      public override DateTime ToDateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int era);
      public override int ToFourDigitYear(int year);
    }
    public class KoreanLunisolarCalendar : EastAsianLunisolarCalendar {
      public const int GregorianEra = 1;
      public KoreanLunisolarCalendar();
      protected override int DaysInYearBeforeMinSupportedYear { get; }
      public override int[] Eras { get; }
      public override DateTime MaxSupportedDateTime { get; }
      public override DateTime MinSupportedDateTime { get; }
      public override int GetEra(DateTime time);
    }
    public sealed class NumberFormatInfo : ICloneable, IFormatProvider {
      public NumberFormatInfo();
      public int CurrencyDecimalDigits { get; set; }
      public string CurrencyDecimalSeparator { get; set; }
      public string CurrencyGroupSeparator { get; set; }
      public int[] CurrencyGroupSizes { get; set; }
      public int CurrencyNegativePattern { get; set; }
      public int CurrencyPositivePattern { get; set; }
      public string CurrencySymbol { get; set; }
      public static NumberFormatInfo CurrentInfo { get; }
      public DigitShapes DigitSubstitution { get; set; }
      public static NumberFormatInfo InvariantInfo { get; }
      public bool IsReadOnly { get; }
      public string NaNSymbol { get; set; }
      public string[] NativeDigits { get; set; }
      public string NegativeInfinitySymbol { get; set; }
      public string NegativeSign { get; set; }
      public int NumberDecimalDigits { get; set; }
      public string NumberDecimalSeparator { get; set; }
      public string NumberGroupSeparator { get; set; }
      public int[] NumberGroupSizes { get; set; }
      public int NumberNegativePattern { get; set; }
      public int PercentDecimalDigits { get; set; }
      public string PercentDecimalSeparator { get; set; }
      public string PercentGroupSeparator { get; set; }
      public int[] PercentGroupSizes { get; set; }
      public int PercentNegativePattern { get; set; }
      public int PercentPositivePattern { get; set; }
      public string PercentSymbol { get; set; }
      public string PerMilleSymbol { get; set; }
      public string PositiveInfinitySymbol { get; set; }
      public string PositiveSign { get; set; }
      public object Clone();
      public object? GetFormat(Type? formatType);
      public static NumberFormatInfo GetInstance(IFormatProvider? formatProvider);
      public static NumberFormatInfo ReadOnly(NumberFormatInfo nfi);
    }
    public enum NumberStyles {
      AllowCurrencySymbol = 256,
      AllowDecimalPoint = 32,
      AllowExponent = 128,
      AllowHexSpecifier = 512,
      AllowLeadingSign = 4,
      AllowLeadingWhite = 1,
      AllowParentheses = 16,
      AllowThousands = 64,
      AllowTrailingSign = 8,
      AllowTrailingWhite = 2,
      Any = 511,
      Currency = 383,
      Float = 167,
      HexNumber = 515,
      Integer = 7,
      None = 0,
      Number = 111,
    }
    public class PersianCalendar : Calendar {
      public static readonly int PersianEra;
      public PersianCalendar();
      public override CalendarAlgorithmType AlgorithmType { get; }
      public override int[] Eras { get; }
      public override DateTime MaxSupportedDateTime { get; }
      public override DateTime MinSupportedDateTime { get; }
      public override int TwoDigitYearMax { get; set; }
      public override DateTime AddMonths(DateTime time, int months);
      public override DateTime AddYears(DateTime time, int years);
      public override int GetDayOfMonth(DateTime time);
      public override DayOfWeek GetDayOfWeek(DateTime time);
      public override int GetDayOfYear(DateTime time);
      public override int GetDaysInMonth(int year, int month, int era);
      public override int GetDaysInYear(int year, int era);
      public override int GetEra(DateTime time);
      public override int GetLeapMonth(int year, int era);
      public override int GetMonth(DateTime time);
      public override int GetMonthsInYear(int year, int era);
      public override int GetYear(DateTime time);
      public override bool IsLeapDay(int year, int month, int day, int era);
      public override bool IsLeapMonth(int year, int month, int era);
      public override bool IsLeapYear(int year, int era);
      public override DateTime ToDateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int era);
      public override int ToFourDigitYear(int year);
    }
    public class RegionInfo {
      public RegionInfo(int culture);
      public RegionInfo(string name);
      public virtual string CurrencyEnglishName { get; }
      public virtual string CurrencyNativeName { get; }
      public virtual string CurrencySymbol { get; }
      public static RegionInfo CurrentRegion { get; }
      public virtual string DisplayName { get; }
      public virtual string EnglishName { get; }
      public virtual int GeoId { get; }
      public virtual bool IsMetric { get; }
      public virtual string ISOCurrencySymbol { get; }
      public virtual string Name { get; }
      public virtual string NativeName { get; }
      public virtual string ThreeLetterISORegionName { get; }
      public virtual string ThreeLetterWindowsRegionName { get; }
      public virtual string TwoLetterISORegionName { get; }
      public override bool Equals(object? value);
      public override int GetHashCode();
      public override string ToString();
    }
    public class SortKey {
      public virtual byte[] KeyData { get; }
      public virtual string OriginalString { get; }
      public static int Compare(SortKey sortkey1, SortKey sortkey2);
      public override bool Equals(object? value);
      public override int GetHashCode();
      public override string ToString();
    }
    public sealed class SortVersion : IEquatable<SortVersion?> {
      public SortVersion(int fullVersion, Guid sortId);
      public int FullVersion { get; }
      public Guid SortId { get; }
      public bool Equals(SortVersion? other);
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public static bool operator ==(SortVersion? left, SortVersion? right);
      public static bool operator !=(SortVersion? left, SortVersion? right);
    }
    public class StringInfo {
      public StringInfo();
      public StringInfo(string value);
      public int LengthInTextElements { get; }
      public string String { get; set; }
      public override bool Equals(object? value);
      public override int GetHashCode();
      public static string GetNextTextElement(string str);
      public static string GetNextTextElement(string str, int index);
      public static TextElementEnumerator GetTextElementEnumerator(string str);
      public static TextElementEnumerator GetTextElementEnumerator(string str, int index);
      public static int[] ParseCombiningCharacters(string str);
      public string SubstringByTextElements(int startingTextElement);
      public string SubstringByTextElements(int startingTextElement, int lengthInTextElements);
    }
    public class TaiwanCalendar : Calendar {
      public TaiwanCalendar();
      public override CalendarAlgorithmType AlgorithmType { get; }
      public override int[] Eras { get; }
      public override DateTime MaxSupportedDateTime { get; }
      public override DateTime MinSupportedDateTime { get; }
      public override int TwoDigitYearMax { get; set; }
      public override DateTime AddMonths(DateTime time, int months);
      public override DateTime AddYears(DateTime time, int years);
      public override int GetDayOfMonth(DateTime time);
      public override DayOfWeek GetDayOfWeek(DateTime time);
      public override int GetDayOfYear(DateTime time);
      public override int GetDaysInMonth(int year, int month, int era);
      public override int GetDaysInYear(int year, int era);
      public override int GetEra(DateTime time);
      public override int GetLeapMonth(int year, int era);
      public override int GetMonth(DateTime time);
      public override int GetMonthsInYear(int year, int era);
      public override int GetWeekOfYear(DateTime time, CalendarWeekRule rule, DayOfWeek firstDayOfWeek);
      public override int GetYear(DateTime time);
      public override bool IsLeapDay(int year, int month, int day, int era);
      public override bool IsLeapMonth(int year, int month, int era);
      public override bool IsLeapYear(int year, int era);
      public override DateTime ToDateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int era);
      public override int ToFourDigitYear(int year);
    }
    public class TaiwanLunisolarCalendar : EastAsianLunisolarCalendar {
      public TaiwanLunisolarCalendar();
      protected override int DaysInYearBeforeMinSupportedYear { get; }
      public override int[] Eras { get; }
      public override DateTime MaxSupportedDateTime { get; }
      public override DateTime MinSupportedDateTime { get; }
      public override int GetEra(DateTime time);
    }
    public class TextElementEnumerator : IEnumerator {
      public object? Current { get; }
      public int ElementIndex { get; }
      public string GetTextElement();
      public bool MoveNext();
      public void Reset();
    }
    public class TextInfo : ICloneable, IDeserializationCallback {
      public virtual int ANSICodePage { get; }
      public string CultureName { get; }
      public virtual int EBCDICCodePage { get; }
      public bool IsReadOnly { get; }
      public bool IsRightToLeft { get; }
      public int LCID { get; }
      public virtual string ListSeparator { get; set; }
      public virtual int MacCodePage { get; }
      public virtual int OEMCodePage { get; }
      public virtual object Clone();
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public static TextInfo ReadOnly(TextInfo textInfo);
      void System.Runtime.Serialization.IDeserializationCallback.OnDeserialization(object sender);
      public virtual char ToLower(char c);
      public virtual string ToLower(string str);
      public override string ToString();
      public string ToTitleCase(string str);
      public virtual char ToUpper(char c);
      public virtual string ToUpper(string str);
    }
    public class ThaiBuddhistCalendar : Calendar {
      public const int ThaiBuddhistEra = 1;
      public ThaiBuddhistCalendar();
      public override CalendarAlgorithmType AlgorithmType { get; }
      public override int[] Eras { get; }
      public override DateTime MaxSupportedDateTime { get; }
      public override DateTime MinSupportedDateTime { get; }
      public override int TwoDigitYearMax { get; set; }
      public override DateTime AddMonths(DateTime time, int months);
      public override DateTime AddYears(DateTime time, int years);
      public override int GetDayOfMonth(DateTime time);
      public override DayOfWeek GetDayOfWeek(DateTime time);
      public override int GetDayOfYear(DateTime time);
      public override int GetDaysInMonth(int year, int month, int era);
      public override int GetDaysInYear(int year, int era);
      public override int GetEra(DateTime time);
      public override int GetLeapMonth(int year, int era);
      public override int GetMonth(DateTime time);
      public override int GetMonthsInYear(int year, int era);
      public override int GetWeekOfYear(DateTime time, CalendarWeekRule rule, DayOfWeek firstDayOfWeek);
      public override int GetYear(DateTime time);
      public override bool IsLeapDay(int year, int month, int day, int era);
      public override bool IsLeapMonth(int year, int month, int era);
      public override bool IsLeapYear(int year, int era);
      public override DateTime ToDateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int era);
      public override int ToFourDigitYear(int year);
    }
    public enum TimeSpanStyles {
      AssumeNegative = 1,
      None = 0,
    }
    public class UmAlQuraCalendar : Calendar {
      public const int UmAlQuraEra = 1;
      public UmAlQuraCalendar();
      public override CalendarAlgorithmType AlgorithmType { get; }
      protected override int DaysInYearBeforeMinSupportedYear { get; }
      public override int[] Eras { get; }
      public override DateTime MaxSupportedDateTime { get; }
      public override DateTime MinSupportedDateTime { get; }
      public override int TwoDigitYearMax { get; set; }
      public override DateTime AddMonths(DateTime time, int months);
      public override DateTime AddYears(DateTime time, int years);
      public override int GetDayOfMonth(DateTime time);
      public override DayOfWeek GetDayOfWeek(DateTime time);
      public override int GetDayOfYear(DateTime time);
      public override int GetDaysInMonth(int year, int month, int era);
      public override int GetDaysInYear(int year, int era);
      public override int GetEra(DateTime time);
      public override int GetLeapMonth(int year, int era);
      public override int GetMonth(DateTime time);
      public override int GetMonthsInYear(int year, int era);
      public override int GetYear(DateTime time);
      public override bool IsLeapDay(int year, int month, int day, int era);
      public override bool IsLeapMonth(int year, int month, int era);
      public override bool IsLeapYear(int year, int era);
      public override DateTime ToDateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int era);
      public override int ToFourDigitYear(int year);
    }
    public enum UnicodeCategory {
      ClosePunctuation = 21,
      ConnectorPunctuation = 18,
      Control = 14,
      CurrencySymbol = 26,
      DashPunctuation = 19,
      DecimalDigitNumber = 8,
      EnclosingMark = 7,
      FinalQuotePunctuation = 23,
      Format = 15,
      InitialQuotePunctuation = 22,
      LetterNumber = 9,
      LineSeparator = 12,
      LowercaseLetter = 1,
      MathSymbol = 25,
      ModifierLetter = 3,
      ModifierSymbol = 27,
      NonSpacingMark = 5,
      OpenPunctuation = 20,
      OtherLetter = 4,
      OtherNotAssigned = 29,
      OtherNumber = 10,
      OtherPunctuation = 24,
      OtherSymbol = 28,
      ParagraphSeparator = 13,
      PrivateUse = 17,
      SpaceSeparator = 11,
      SpacingCombiningMark = 6,
      Surrogate = 16,
      TitlecaseLetter = 2,
      UppercaseLetter = 0,
    }
  }
  namespace System.IO {
    public class DirectoryNotFoundException : IOException {
      public DirectoryNotFoundException();
      protected DirectoryNotFoundException(SerializationInfo info, StreamingContext context);
      public DirectoryNotFoundException(string? message);
      public DirectoryNotFoundException(string? message, Exception? innerException);
    }
    public enum FileAccess {
      Read = 1,
      ReadWrite = 3,
      Write = 2,
    }
    public enum FileAttributes {
      Archive = 32,
      Compressed = 2048,
      Device = 64,
      Directory = 16,
      Encrypted = 16384,
      Hidden = 2,
      IntegrityStream = 32768,
      Normal = 128,
      NoScrubData = 131072,
      NotContentIndexed = 8192,
      Offline = 4096,
      ReadOnly = 1,
      ReparsePoint = 1024,
      SparseFile = 512,
      System = 4,
      Temporary = 256,
    }
    public class FileLoadException : IOException {
      public FileLoadException();
      protected FileLoadException(SerializationInfo info, StreamingContext context);
      public FileLoadException(string? message);
      public FileLoadException(string? message, Exception? inner);
      public FileLoadException(string? message, string? fileName);
      public FileLoadException(string? message, string? fileName, Exception? inner);
      public string? FileName { get; }
      public string? FusionLog { get; }
      public override string Message { get; }
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
      public override string ToString();
    }
    public enum FileMode {
      Append = 6,
      Create = 2,
      CreateNew = 1,
      Open = 3,
      OpenOrCreate = 4,
      Truncate = 5,
    }
    public class FileNotFoundException : IOException {
      public FileNotFoundException();
      protected FileNotFoundException(SerializationInfo info, StreamingContext context);
      public FileNotFoundException(string? message);
      public FileNotFoundException(string? message, Exception? innerException);
      public FileNotFoundException(string? message, string? fileName);
      public FileNotFoundException(string? message, string? fileName, Exception? innerException);
      public string? FileName { get; }
      public string? FusionLog { get; }
      public override string Message { get; }
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
      public override string ToString();
    }
    public enum FileOptions {
      Asynchronous = 1073741824,
      DeleteOnClose = 67108864,
      Encrypted = 16384,
      None = 0,
      RandomAccess = 268435456,
      SequentialScan = 134217728,
      WriteThrough = -2147483648,
    }
    public enum FileShare {
      Delete = 4,
      Inheritable = 16,
      None = 0,
      Read = 1,
      ReadWrite = 3,
      Write = 2,
    }
    public class FileStream : Stream {
      public FileStream(SafeFileHandle handle, FileAccess access);
      public FileStream(SafeFileHandle handle, FileAccess access, int bufferSize);
      public FileStream(SafeFileHandle handle, FileAccess access, int bufferSize, bool isAsync);
      public FileStream(IntPtr handle, FileAccess access);
      public FileStream(IntPtr handle, FileAccess access, bool ownsHandle);
      public FileStream(IntPtr handle, FileAccess access, bool ownsHandle, int bufferSize);
      public FileStream(IntPtr handle, FileAccess access, bool ownsHandle, int bufferSize, bool isAsync);
      public FileStream(string path, FileMode mode);
      public FileStream(string path, FileMode mode, FileAccess access);
      public FileStream(string path, FileMode mode, FileAccess access, FileShare share);
      public FileStream(string path, FileMode mode, FileAccess access, FileShare share, int bufferSize);
      public FileStream(string path, FileMode mode, FileAccess access, FileShare share, int bufferSize, bool useAsync);
      public FileStream(string path, FileMode mode, FileAccess access, FileShare share, int bufferSize, FileOptions options);
      public override bool CanRead { get; }
      public override bool CanSeek { get; }
      public override bool CanWrite { get; }
      public virtual IntPtr Handle { get; }
      public virtual bool IsAsync { get; }
      public override long Length { get; }
      public virtual string Name { get; }
      public override long Position { get; set; }
      public virtual SafeFileHandle SafeFileHandle { get; }
      public override IAsyncResult BeginRead(byte[] array, int offset, int numBytes, AsyncCallback callback, object? state);
      public override IAsyncResult BeginWrite(byte[] array, int offset, int numBytes, AsyncCallback callback, object? state);
      public override Task CopyToAsync(Stream destination, int bufferSize, CancellationToken cancellationToken);
      protected override void Dispose(bool disposing);
      public override ValueTask DisposeAsync();
      public override int EndRead(IAsyncResult asyncResult);
      public override void EndWrite(IAsyncResult asyncResult);
      ~FileStream();
      public override void Flush();
      public virtual void Flush(bool flushToDisk);
      public override Task FlushAsync(CancellationToken cancellationToken);
      public virtual void Lock(long position, long length);
      public override int Read(byte[] array, int offset, int count);
      public override int Read(Span<byte> buffer);
      public override Task<int> ReadAsync(byte[] buffer, int offset, int count, CancellationToken cancellationToken);
      public override ValueTask<int> ReadAsync(Memory<byte> buffer, CancellationToken cancellationToken = default(CancellationToken));
      public override int ReadByte();
      public override long Seek(long offset, SeekOrigin origin);
      public override void SetLength(long value);
      public virtual void Unlock(long position, long length);
      public override void Write(byte[] array, int offset, int count);
      public override void Write(ReadOnlySpan<byte> buffer);
      public override Task WriteAsync(byte[] buffer, int offset, int count, CancellationToken cancellationToken);
      public override ValueTask WriteAsync(ReadOnlyMemory<byte> buffer, CancellationToken cancellationToken = default(CancellationToken));
      public override void WriteByte(byte value);
    }
    public enum HandleInheritability {
      Inheritable = 1,
      None = 0,
    }
    public class IOException : SystemException {
      public IOException();
      protected IOException(SerializationInfo info, StreamingContext context);
      public IOException(string? message);
      public IOException(string? message, Exception? innerException);
      public IOException(string? message, int hresult);
    }
    public class PathTooLongException : IOException {
      public PathTooLongException();
      protected PathTooLongException(SerializationInfo info, StreamingContext context);
      public PathTooLongException(string? message);
      public PathTooLongException(string? message, Exception? innerException);
    }
    public enum SeekOrigin {
      Begin = 0,
      Current = 1,
      End = 2,
    }
    public abstract class Stream : MarshalByRefObject, IAsyncDisposable, IDisposable {
      public static readonly Stream Null;
      protected Stream();
      public abstract bool CanRead { get; }
      public abstract bool CanSeek { get; }
      public virtual bool CanTimeout { get; }
      public abstract bool CanWrite { get; }
      public abstract long Length { get; }
      public abstract long Position { get; set; }
      public virtual int ReadTimeout { get; set; }
      public virtual int WriteTimeout { get; set; }
      public virtual IAsyncResult BeginRead(byte[] buffer, int offset, int count, AsyncCallback callback, object? state);
      public virtual IAsyncResult BeginWrite(byte[] buffer, int offset, int count, AsyncCallback callback, object? state);
      public virtual void Close();
      public void CopyTo(Stream destination);
      public virtual void CopyTo(Stream destination, int bufferSize);
      public Task CopyToAsync(Stream destination);
      public Task CopyToAsync(Stream destination, int bufferSize);
      public virtual Task CopyToAsync(Stream destination, int bufferSize, CancellationToken cancellationToken);
      public Task CopyToAsync(Stream destination, CancellationToken cancellationToken);
      protected virtual WaitHandle CreateWaitHandle();
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      public virtual ValueTask DisposeAsync();
      public virtual int EndRead(IAsyncResult asyncResult);
      public virtual void EndWrite(IAsyncResult asyncResult);
      public abstract void Flush();
      public Task FlushAsync();
      public virtual Task FlushAsync(CancellationToken cancellationToken);
      protected virtual void ObjectInvariant();
      public abstract int Read(byte[] buffer, int offset, int count);
      public virtual int Read(Span<byte> buffer);
      public Task<int> ReadAsync(byte[] buffer, int offset, int count);
      public virtual Task<int> ReadAsync(byte[] buffer, int offset, int count, CancellationToken cancellationToken);
      public virtual ValueTask<int> ReadAsync(Memory<byte> buffer, CancellationToken cancellationToken = default(CancellationToken));
      public virtual int ReadByte();
      public abstract long Seek(long offset, SeekOrigin origin);
      public abstract void SetLength(long value);
      public static Stream Synchronized(Stream stream);
      public abstract void Write(byte[] buffer, int offset, int count);
      public virtual void Write(ReadOnlySpan<byte> buffer);
      public Task WriteAsync(byte[] buffer, int offset, int count);
      public virtual Task WriteAsync(byte[] buffer, int offset, int count, CancellationToken cancellationToken);
      public virtual ValueTask WriteAsync(ReadOnlyMemory<byte> buffer, CancellationToken cancellationToken = default(CancellationToken));
      public virtual void WriteByte(byte value);
    }
  }
  namespace System.Reflection {
    public sealed class AmbiguousMatchException : SystemException {
      public AmbiguousMatchException();
      public AmbiguousMatchException(string? message);
      public AmbiguousMatchException(string? message, Exception? inner);
    }
    public abstract class Assembly : ICustomAttributeProvider, ISerializable {
      protected Assembly();
      public virtual string? CodeBase { get; }
      public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
      public virtual IEnumerable<TypeInfo> DefinedTypes { get; }
      public virtual MethodInfo? EntryPoint { get; }
      public virtual string EscapedCodeBase { get; }
      public virtual IEnumerable<Type> ExportedTypes { get; }
      public virtual string? FullName { get; }
      public virtual bool GlobalAssemblyCache { get; }
      public virtual long HostContext { get; }
      public virtual string ImageRuntimeVersion { get; }
      public virtual bool IsCollectible { get; }
      public virtual bool IsDynamic { get; }
      public bool IsFullyTrusted { get; }
      public virtual string Location { get; }
      public virtual Module? ManifestModule { get; }
      public virtual IEnumerable<Module> Modules { get; }
      public virtual bool ReflectionOnly { get; }
      public virtual SecurityRuleSet SecurityRuleSet { get; }
      public virtual event ModuleResolveEventHandler ModuleResolve;
      public object? CreateInstance(string typeName);
      public object? CreateInstance(string typeName, bool ignoreCase);
      public virtual object? CreateInstance(string typeName, bool ignoreCase, BindingFlags bindingAttr, Binder? binder, object[]? args, CultureInfo? culture, object[]? activationAttributes);
      public static string CreateQualifiedName(string? assemblyName, string? typeName);
      public override bool Equals(object? o);
      public static Assembly? GetAssembly(Type type);
      public static Assembly GetCallingAssembly();
      public virtual object[] GetCustomAttributes(bool inherit);
      public virtual object[] GetCustomAttributes(Type attributeType, bool inherit);
      public virtual IList<CustomAttributeData> GetCustomAttributesData();
      public static Assembly? GetEntryAssembly();
      public static Assembly GetExecutingAssembly();
      public virtual Type[] GetExportedTypes();
      public virtual FileStream? GetFile(string name);
      public virtual FileStream[] GetFiles();
      public virtual FileStream[] GetFiles(bool getResourceModules);
      public virtual Type[] GetForwardedTypes();
      public override int GetHashCode();
      public Module[] GetLoadedModules();
      public virtual Module[] GetLoadedModules(bool getResourceModules);
      public virtual ManifestResourceInfo? GetManifestResourceInfo(string resourceName);
      public virtual string[] GetManifestResourceNames();
      public virtual Stream? GetManifestResourceStream(string name);
      public virtual Stream? GetManifestResourceStream(Type type, string name);
      public virtual Module GetModule(string name);
      public Module[] GetModules();
      public virtual Module[] GetModules(bool getResourceModules);
      public virtual AssemblyName GetName();
      public virtual AssemblyName GetName(bool copiedName);
      public virtual void GetObjectData(SerializationInfo info, StreamingContext context);
      public virtual AssemblyName[] GetReferencedAssemblies();
      public virtual Assembly GetSatelliteAssembly(CultureInfo culture);
      public virtual Assembly GetSatelliteAssembly(CultureInfo culture, Version? version);
      public virtual Type GetType(string name);
      public virtual Type GetType(string name, bool throwOnError);
      public virtual Type GetType(string name, bool throwOnError, bool ignoreCase);
      public virtual Type[] GetTypes();
      public virtual bool IsDefined(Type attributeType, bool inherit);
      public static Assembly Load(byte[] rawAssembly);
      public static Assembly Load(byte[] rawAssembly, byte[]? rawSymbolStore);
      public static Assembly Load(AssemblyName assemblyRef);
      public static Assembly Load(string assemblyString);
      public static Assembly LoadFile(string path);
      public static Assembly LoadFrom(string assemblyFile);
      public static Assembly LoadFrom(string? assemblyFile, byte[]? hashValue, AssemblyHashAlgorithm hashAlgorithm);
      public Module LoadModule(string? moduleName, byte[]? rawModule);
      public virtual Module LoadModule(string? moduleName, byte[]? rawModule, byte[]? rawSymbolStore);
      public static Assembly? LoadWithPartialName(string partialName);
      public static bool operator ==(Assembly? left, Assembly? right);
      public static bool operator !=(Assembly? left, Assembly? right);
      public static Assembly ReflectionOnlyLoad(byte[]? rawAssembly);
      public static Assembly ReflectionOnlyLoad(string? assemblyString);
      public static Assembly ReflectionOnlyLoadFrom(string? assemblyFile);
      public override string ToString();
      public static Assembly UnsafeLoadFrom(string assemblyFile);
    }
    public sealed class AssemblyAlgorithmIdAttribute : Attribute {
      public AssemblyAlgorithmIdAttribute(AssemblyHashAlgorithm algorithmId);
      public AssemblyAlgorithmIdAttribute(uint algorithmId);
      public uint AlgorithmId { get; }
    }
    public sealed class AssemblyCompanyAttribute : Attribute {
      public AssemblyCompanyAttribute(string company);
      public string Company { get; }
    }
    public sealed class AssemblyConfigurationAttribute : Attribute {
      public AssemblyConfigurationAttribute(string configuration);
      public string Configuration { get; }
    }
    public enum AssemblyContentType {
      Default = 0,
      WindowsRuntime = 1,
    }
    public sealed class AssemblyCopyrightAttribute : Attribute {
      public AssemblyCopyrightAttribute(string copyright);
      public string Copyright { get; }
    }
    public sealed class AssemblyCultureAttribute : Attribute {
      public AssemblyCultureAttribute(string culture);
      public string Culture { get; }
    }
    public sealed class AssemblyDefaultAliasAttribute : Attribute {
      public AssemblyDefaultAliasAttribute(string defaultAlias);
      public string DefaultAlias { get; }
    }
    public sealed class AssemblyDelaySignAttribute : Attribute {
      public AssemblyDelaySignAttribute(bool delaySign);
      public bool DelaySign { get; }
    }
    public sealed class AssemblyDescriptionAttribute : Attribute {
      public AssemblyDescriptionAttribute(string description);
      public string Description { get; }
    }
    public sealed class AssemblyFileVersionAttribute : Attribute {
      public AssemblyFileVersionAttribute(string version);
      public string Version { get; }
    }
    public sealed class AssemblyFlagsAttribute : Attribute {
      public AssemblyFlagsAttribute(int assemblyFlags);
      public AssemblyFlagsAttribute(AssemblyNameFlags assemblyFlags);
      public AssemblyFlagsAttribute(uint flags);
      public int AssemblyFlags { get; }
      public uint Flags { get; }
    }
    public sealed class AssemblyInformationalVersionAttribute : Attribute {
      public AssemblyInformationalVersionAttribute(string informationalVersion);
      public string InformationalVersion { get; }
    }
    public sealed class AssemblyKeyFileAttribute : Attribute {
      public AssemblyKeyFileAttribute(string keyFile);
      public string KeyFile { get; }
    }
    public sealed class AssemblyKeyNameAttribute : Attribute {
      public AssemblyKeyNameAttribute(string keyName);
      public string KeyName { get; }
    }
    public sealed class AssemblyMetadataAttribute : Attribute {
      public AssemblyMetadataAttribute(string key, string? value);
      public string Key { get; }
      public string? Value { get; }
    }
    public sealed class AssemblyName : ICloneable, IDeserializationCallback, ISerializable {
      public AssemblyName();
      public AssemblyName(string assemblyName);
      public string? CodeBase { get; set; }
      public AssemblyContentType ContentType { get; set; }
      public CultureInfo? CultureInfo { get; set; }
      public string? CultureName { get; set; }
      public string? EscapedCodeBase { get; }
      public AssemblyNameFlags Flags { get; set; }
      public string FullName { get; }
      public AssemblyHashAlgorithm HashAlgorithm { get; set; }
      public StrongNameKeyPair? KeyPair { get; set; }
      public string? Name { get; set; }
      public ProcessorArchitecture ProcessorArchitecture { get; set; }
      public Version? Version { get; set; }
      public AssemblyVersionCompatibility VersionCompatibility { get; set; }
      public object Clone();
      public static AssemblyName GetAssemblyName(string assemblyFile);
      public void GetObjectData(SerializationInfo info, StreamingContext context);
      public byte[]? GetPublicKey();
      public byte[] GetPublicKeyToken();
      public void OnDeserialization(object sender);
      public static bool ReferenceMatchesDefinition(AssemblyName? reference, AssemblyName? definition);
      public void SetPublicKey(byte[]? publicKey);
      public void SetPublicKeyToken(byte[]? publicKeyToken);
      public override string ToString();
    }
    public enum AssemblyNameFlags {
      EnableJITcompileOptimizer = 16384,
      EnableJITcompileTracking = 32768,
      None = 0,
      PublicKey = 1,
      Retargetable = 256,
    }
    public sealed class AssemblyProductAttribute : Attribute {
      public AssemblyProductAttribute(string product);
      public string Product { get; }
    }
    public sealed class AssemblySignatureKeyAttribute : Attribute {
      public AssemblySignatureKeyAttribute(string publicKey, string countersignature);
      public string Countersignature { get; }
      public string PublicKey { get; }
    }
    public sealed class AssemblyTitleAttribute : Attribute {
      public AssemblyTitleAttribute(string title);
      public string Title { get; }
    }
    public sealed class AssemblyTrademarkAttribute : Attribute {
      public AssemblyTrademarkAttribute(string trademark);
      public string Trademark { get; }
    }
    public sealed class AssemblyVersionAttribute : Attribute {
      public AssemblyVersionAttribute(string version);
      public string Version { get; }
    }
    public abstract class Binder {
      protected Binder();
      public abstract FieldInfo BindToField(BindingFlags bindingAttr, FieldInfo[] match, object value, CultureInfo? culture);
      public abstract MethodBase BindToMethod(BindingFlags bindingAttr, MethodBase[] match, ref object?[] args, ParameterModifier[]? modifiers, CultureInfo? culture, string[]? names, out object? state);
      public abstract object ChangeType(object value, Type type, CultureInfo? culture);
      public abstract void ReorderArgumentArray(ref object[] args, object state);
      public abstract MethodBase? SelectMethod(BindingFlags bindingAttr, MethodBase[] match, Type[] types, ParameterModifier[]? modifiers);
      public abstract PropertyInfo? SelectProperty(BindingFlags bindingAttr, PropertyInfo[] match, Type? returnType, Type[]? indexes, ParameterModifier[]? modifiers);
    }
    public enum BindingFlags {
      CreateInstance = 512,
      DeclaredOnly = 2,
      Default = 0,
      DoNotWrapExceptions = 33554432,
      ExactBinding = 65536,
      FlattenHierarchy = 64,
      GetField = 1024,
      GetProperty = 4096,
      IgnoreCase = 1,
      IgnoreReturn = 16777216,
      Instance = 4,
      InvokeMethod = 256,
      NonPublic = 32,
      OptionalParamBinding = 262144,
      Public = 16,
      PutDispProperty = 16384,
      PutRefDispProperty = 32768,
      SetField = 2048,
      SetProperty = 8192,
      Static = 8,
      SuppressChangeType = 131072,
    }
    public enum CallingConventions {
      Any = 3,
      ExplicitThis = 64,
      HasThis = 32,
      Standard = 1,
      VarArgs = 2,
    }
    public abstract class ConstructorInfo : MethodBase {
      public static readonly string ConstructorName;
      public static readonly string TypeConstructorName;
      protected ConstructorInfo();
      public override MemberTypes MemberType { get; }
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public object Invoke(object?[]? parameters);
      public abstract object Invoke(BindingFlags invokeAttr, Binder? binder, object?[]? parameters, CultureInfo? culture);
      public static bool operator ==(ConstructorInfo? left, ConstructorInfo? right);
      public static bool operator !=(ConstructorInfo? left, ConstructorInfo? right);
    }
    public class CustomAttributeData {
      protected CustomAttributeData();
      public virtual Type? AttributeType { get; }
      public virtual ConstructorInfo Constructor { get; }
      public virtual IList<CustomAttributeTypedArgument> ConstructorArguments { get; }
      public virtual IList<CustomAttributeNamedArgument> NamedArguments { get; }
      public override bool Equals(object? obj);
      public static IList<CustomAttributeData> GetCustomAttributes(Assembly target);
      public static IList<CustomAttributeData> GetCustomAttributes(MemberInfo target);
      public static IList<CustomAttributeData> GetCustomAttributes(Module target);
      public static IList<CustomAttributeData> GetCustomAttributes(ParameterInfo target);
      public override int GetHashCode();
      public override string ToString();
    }
    public static class CustomAttributeExtensions {
      public static Attribute? GetCustomAttribute(this Assembly element, Type attributeType);
      public static Attribute? GetCustomAttribute(this MemberInfo element, Type attributeType);
      public static Attribute? GetCustomAttribute(this MemberInfo element, Type attributeType, bool inherit);
      public static Attribute? GetCustomAttribute(this Module element, Type attributeType);
      public static Attribute? GetCustomAttribute(this ParameterInfo element, Type attributeType);
      public static Attribute? GetCustomAttribute(this ParameterInfo element, Type attributeType, bool inherit);
      public static T? GetCustomAttribute<T>(this Assembly element) where T : Attribute;
      public static T? GetCustomAttribute<T>(this MemberInfo element) where T : Attribute;
      public static T? GetCustomAttribute<T>(this MemberInfo element, bool inherit) where T : Attribute;
      public static T? GetCustomAttribute<T>(this Module element) where T : Attribute;
      public static T? GetCustomAttribute<T>(this ParameterInfo element) where T : Attribute;
      public static T? GetCustomAttribute<T>(this ParameterInfo element, bool inherit) where T : Attribute;
      public static IEnumerable<Attribute> GetCustomAttributes(this Assembly element);
      public static IEnumerable<Attribute> GetCustomAttributes(this Assembly element, Type attributeType);
      public static IEnumerable<Attribute> GetCustomAttributes(this MemberInfo element);
      public static IEnumerable<Attribute> GetCustomAttributes(this MemberInfo element, bool inherit);
      public static IEnumerable<Attribute> GetCustomAttributes(this MemberInfo element, Type attributeType);
      public static IEnumerable<Attribute> GetCustomAttributes(this MemberInfo element, Type attributeType, bool inherit);
      public static IEnumerable<Attribute> GetCustomAttributes(this Module element);
      public static IEnumerable<Attribute> GetCustomAttributes(this Module element, Type attributeType);
      public static IEnumerable<Attribute> GetCustomAttributes(this ParameterInfo element);
      public static IEnumerable<Attribute> GetCustomAttributes(this ParameterInfo element, bool inherit);
      public static IEnumerable<Attribute> GetCustomAttributes(this ParameterInfo element, Type attributeType);
      public static IEnumerable<Attribute> GetCustomAttributes(this ParameterInfo element, Type attributeType, bool inherit);
      public static IEnumerable<T> GetCustomAttributes<T>(this Assembly element) where T : Attribute;
      public static IEnumerable<T> GetCustomAttributes<T>(this MemberInfo element) where T : Attribute;
      public static IEnumerable<T> GetCustomAttributes<T>(this MemberInfo element, bool inherit) where T : Attribute;
      public static IEnumerable<T> GetCustomAttributes<T>(this Module element) where T : Attribute;
      public static IEnumerable<T> GetCustomAttributes<T>(this ParameterInfo element) where T : Attribute;
      public static IEnumerable<T> GetCustomAttributes<T>(this ParameterInfo element, bool inherit) where T : Attribute;
      public static bool IsDefined(this Assembly element, Type attributeType);
      public static bool IsDefined(this MemberInfo element, Type attributeType);
      public static bool IsDefined(this MemberInfo element, Type attributeType, bool inherit);
      public static bool IsDefined(this Module element, Type attributeType);
      public static bool IsDefined(this ParameterInfo element, Type attributeType);
      public static bool IsDefined(this ParameterInfo element, Type attributeType, bool inherit);
    }
    public class CustomAttributeFormatException : FormatException {
      public CustomAttributeFormatException();
      protected CustomAttributeFormatException(SerializationInfo info, StreamingContext context);
      public CustomAttributeFormatException(string? message);
      public CustomAttributeFormatException(string? message, Exception? inner);
    }
    public readonly struct CustomAttributeNamedArgument {
      public CustomAttributeNamedArgument(MemberInfo memberInfo, object? value);
      public CustomAttributeNamedArgument(MemberInfo memberInfo, CustomAttributeTypedArgument typedArgument);
      public bool IsField { get; }
      public MemberInfo MemberInfo { get; }
      public string MemberName { get; }
      public CustomAttributeTypedArgument TypedValue { get; }
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public static bool operator ==(CustomAttributeNamedArgument left, CustomAttributeNamedArgument right);
      public static bool operator !=(CustomAttributeNamedArgument left, CustomAttributeNamedArgument right);
      public override string ToString();
    }
    public readonly struct CustomAttributeTypedArgument {
      public CustomAttributeTypedArgument(object value);
      public CustomAttributeTypedArgument(Type argumentType, object? value);
      public Type ArgumentType { get; }
      public object? Value { get; }
      public override bool Equals(object? obj);
      public override int GetHashCode();
      public static bool operator ==(CustomAttributeTypedArgument left, CustomAttributeTypedArgument right);
      public static bool operator !=(CustomAttributeTypedArgument left, CustomAttributeTypedArgument right);
      public override string ToString();
    }
    public sealed class DefaultMemberAttribute : Attribute {
      public DefaultMemberAttribute(string memberName);
      public string MemberName { get; }
    }
    public enum EventAttributes {
      None = 0,
      ReservedMask = 1024,
      RTSpecialName = 1024,
      SpecialName = 512,
    }
    public abstract class EventInfo : MemberInfo {
      protected EventInfo();
      public virtual MethodInfo? AddMethod { get; }
      public abstract EventAttributes Attributes { get; }
      public virtual Type? EventHandlerType { get; }
      public virtual bool IsMulticast { get; }
      public bool IsSpecialName { get; }
      public override MemberTypes MemberType { get; }
      public virtual MethodInfo? RaiseMethod { get; }
      public virtual MethodInfo? RemoveMethod { get; }
      public virtual void AddEventHandler(object? target, Delegate? handler);
      public override bool Equals(object? obj);
      public MethodInfo? GetAddMethod();
      public abstract MethodInfo? GetAddMethod(bool nonPublic);
      public override int GetHashCode();
      public MethodInfo[] GetOtherMethods();
      public virtual MethodInfo[] GetOtherMethods(bool nonPublic);
      public MethodInfo? GetRaiseMethod();
      public abstract MethodInfo? GetRaiseMethod(bool nonPublic);
      public MethodInfo? GetRemoveMethod();
      public abstract MethodInfo? GetRemoveMethod(bool nonPublic);
      public static bool operator ==(EventInfo? left, EventInfo? right);
      public static bool operator !=(EventInfo? left, EventInfo? right);
      public virtual void RemoveEventHandler(object? target, Delegate? handler);
    }
    public class ExceptionHandlingClause {
      protected ExceptionHandlingClause();
      public virtual Type? CatchType { get; }
      public virtual int FilterOffset { get; }
      public virtual ExceptionHandlingClauseOptions Flags { get; }
      public virtual int HandlerLength { get; }
      public virtual int HandlerOffset { get; }
      public virtual int TryLength { get; }
      public virtual int TryOffset { get; }
      public override string ToString();
    }
    public enum ExceptionHandlingClauseOptions {
      Clause = 0,
      Fault = 4,
      Filter = 1,
      Finally = 2,
    }
    public enum FieldAttributes {
      Assembly = 3,
      FamANDAssem = 2,
      Family = 4,
      FamORAssem = 5,
      FieldAccessMask = 7,
      HasDefault = 32768,
      HasFieldMarshal = 4096,
      HasFieldRVA = 256,
      InitOnly = 32,
      Literal = 64,
      NotSerialized = 128,
      PinvokeImpl = 8192,
      Private = 1,
      PrivateScope = 0,
      Public = 6,
      ReservedMask = 38144,
      RTSpecialName = 1024,
      SpecialName = 512,
      Static = 16,
    }
    public abstract class FieldInfo : MemberInfo {
      protected FieldInfo();
      public abstract FieldAttributes Attributes { get; }
      public abstract RuntimeFieldHandle FieldHandle { get; }
      public abstract Type FieldType { get; }
      public bool IsAssembly { get; }
      public bool IsFamily { get; }
      public bool IsFamilyAndAssembly { get; }
      public bool IsFamilyOrAssembly { get; }
      public bool IsInitOnly { get; }
      public bool IsLiteral { get; }
      public bool IsNotSerialized { get; }
      public bool IsPinvokeImpl { get; }
      public bool IsPrivate { get; }
      public bool IsPublic { get; }
      public virtual bool IsSecurityCritical { get; }
      public virtual bool IsSecuritySafeCritical { get; }
      public virtual bool IsSecurityTransparent { get; }
      public bool IsSpecialName { get; }
      public bool IsStatic { get; }
      public override MemberTypes MemberType { get; }
      public override bool Equals(object? obj);
      public static FieldInfo GetFieldFromHandle(RuntimeFieldHandle handle);
      public static FieldInfo GetFieldFromHandle(RuntimeFieldHandle handle, RuntimeTypeHandle declaringType);
      public override int GetHashCode();
      public virtual Type[] GetOptionalCustomModifiers();
      public virtual object GetRawConstantValue();
      public virtual Type[] GetRequiredCustomModifiers();
      public abstract object? GetValue(object? obj);
      public virtual object GetValueDirect(TypedReference obj);
      public static bool operator ==(FieldInfo? left, FieldInfo? right);
      public static bool operator !=(FieldInfo? left, FieldInfo? right);
      public void SetValue(object? obj, object value);
      public abstract void SetValue(object? obj, object? value, BindingFlags invokeAttr, Binder? binder, CultureInfo? culture);
      public virtual void SetValueDirect(TypedReference obj, object value);
    }
    public enum GenericParameterAttributes {
      Contravariant = 2,
      Covariant = 1,
      DefaultConstructorConstraint = 16,
      None = 0,
      NotNullableValueTypeConstraint = 8,
      ReferenceTypeConstraint = 4,
      SpecialConstraintMask = 28,
      VarianceMask = 3,
    }
    public interface ICustomAttributeProvider {
      object[] GetCustomAttributes(bool inherit);
      object[] GetCustomAttributes(Type attributeType, bool inherit);
      bool IsDefined(Type attributeType, bool inherit);
    }
    public enum ImageFileMachine {
      AMD64 = 34404,
      ARM = 452,
      I386 = 332,
      IA64 = 512,
    }
    public struct InterfaceMapping {
      public MethodInfo[] InterfaceMethods;
      public MethodInfo[] TargetMethods;
      public Type InterfaceType;
      public Type TargetType;
    }
    public static class IntrospectionExtensions {
      public static TypeInfo GetTypeInfo(this Type type);
    }
    public class InvalidFilterCriteriaException : ApplicationException {
      public InvalidFilterCriteriaException();
      protected InvalidFilterCriteriaException(SerializationInfo info, StreamingContext context);
      public InvalidFilterCriteriaException(string? message);
      public InvalidFilterCriteriaException(string? message, Exception? inner);
    }
    public interface IReflect {
      Type UnderlyingSystemType { get; }
      FieldInfo? GetField(string name, BindingFlags bindingAttr);
      FieldInfo[] GetFields(BindingFlags bindingAttr);
      MemberInfo[] GetMember(string name, BindingFlags bindingAttr);
      MemberInfo[] GetMembers(BindingFlags bindingAttr);
      MethodInfo? GetMethod(string name, BindingFlags bindingAttr);
      MethodInfo? GetMethod(string name, BindingFlags bindingAttr, Binder? binder, Type[] types, ParameterModifier[]? modifiers);
      MethodInfo[] GetMethods(BindingFlags bindingAttr);
      PropertyInfo[] GetProperties(BindingFlags bindingAttr);
      PropertyInfo? GetProperty(string name, BindingFlags bindingAttr);
      PropertyInfo? GetProperty(string name, BindingFlags bindingAttr, Binder? binder, Type? returnType, Type[] types, ParameterModifier[]? modifiers);
      object? InvokeMember(string name, BindingFlags invokeAttr, Binder? binder, object? target, object[]? args, ParameterModifier[]? modifiers, CultureInfo? culture, string[]? namedParameters);
    }
    public interface IReflectableType {
      TypeInfo GetTypeInfo();
    }
    public class LocalVariableInfo {
      protected LocalVariableInfo();
      public virtual bool IsPinned { get; }
      public virtual int LocalIndex { get; }
      public virtual Type LocalType { get; }
      public override string ToString();
    }
    public class ManifestResourceInfo {
      public ManifestResourceInfo(Assembly containingAssembly, string containingFileName, ResourceLocation resourceLocation);
      public virtual string FileName { get; }
      public virtual Assembly ReferencedAssembly { get; }
      public virtual ResourceLocation ResourceLocation { get; }
    }
    public delegate bool MemberFilter(MemberInfo m, object filterCriteria);
    public abstract class MemberInfo : ICustomAttributeProvider {
      protected MemberInfo();
      public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
      public abstract Type? DeclaringType { get; }
      public virtual bool IsCollectible { get; }
      public abstract MemberTypes MemberType { get; }
      public virtual int MetadataToken { get; }
      public virtual Module Module { get; }
      public abstract string Name { get; }
      public abstract Type? ReflectedType { get; }
      public override bool Equals(object? obj);
      public abstract object[] GetCustomAttributes(bool inherit);
      public abstract object[] GetCustomAttributes(Type attributeType, bool inherit);
      public virtual IList<CustomAttributeData> GetCustomAttributesData();
      public override int GetHashCode();
      public virtual bool HasSameMetadataDefinitionAs(MemberInfo other);
      public abstract bool IsDefined(Type attributeType, bool inherit);
      public static bool operator ==(MemberInfo? left, MemberInfo? right);
      public static bool operator !=(MemberInfo? left, MemberInfo? right);
    }
    public enum MemberTypes {
      All = 191,
      Constructor = 1,
      Custom = 64,
      Event = 2,
      Field = 4,
      Method = 8,
      NestedType = 128,
      Property = 16,
      TypeInfo = 32,
    }
    public enum MethodAttributes {
      Abstract = 1024,
      Assembly = 3,
      CheckAccessOnOverride = 512,
      FamANDAssem = 2,
      Family = 4,
      FamORAssem = 5,
      Final = 32,
      HasSecurity = 16384,
      HideBySig = 128,
      MemberAccessMask = 7,
      NewSlot = 256,
      PinvokeImpl = 8192,
      Private = 1,
      PrivateScope = 0,
      Public = 6,
      RequireSecObject = 32768,
      ReservedMask = 53248,
      ReuseSlot = 0,
      RTSpecialName = 4096,
      SpecialName = 2048,
      Static = 16,
      UnmanagedExport = 8,
      Virtual = 64,
      VtableLayoutMask = 256,
    }
    public abstract class MethodBase : MemberInfo {
      protected MethodBase();
      public abstract MethodAttributes Attributes { get; }
      public virtual CallingConventions CallingConvention { get; }
      public virtual bool ContainsGenericParameters { get; }
      public bool IsAbstract { get; }
      public bool IsAssembly { get; }
      public virtual bool IsConstructedGenericMethod { get; }
      public bool IsConstructor { get; }
      public bool IsFamily { get; }
      public bool IsFamilyAndAssembly { get; }
      public bool IsFamilyOrAssembly { get; }
      public bool IsFinal { get; }
      public virtual bool IsGenericMethod { get; }
      public virtual bool IsGenericMethodDefinition { get; }
      public bool IsHideBySig { get; }
      public bool IsPrivate { get; }
      public bool IsPublic { get; }
      public virtual bool IsSecurityCritical { get; }
      public virtual bool IsSecuritySafeCritical { get; }
      public virtual bool IsSecurityTransparent { get; }
      public bool IsSpecialName { get; }
      public bool IsStatic { get; }
      public bool IsVirtual { get; }
      public abstract RuntimeMethodHandle MethodHandle { get; }
      public virtual MethodImplAttributes MethodImplementationFlags { get; }
      public override bool Equals(object? obj);
      public static MethodBase? GetCurrentMethod();
      public virtual Type[] GetGenericArguments();
      public override int GetHashCode();
      public virtual MethodBody GetMethodBody();
      public static MethodBase? GetMethodFromHandle(RuntimeMethodHandle handle);
      public static MethodBase? GetMethodFromHandle(RuntimeMethodHandle handle, RuntimeTypeHandle declaringType);
      public abstract MethodImplAttributes GetMethodImplementationFlags();
      public abstract ParameterInfo[] GetParameters();
      public object? Invoke(object? obj, object?[]? parameters);
      public abstract object? Invoke(object? obj, BindingFlags invokeAttr, Binder? binder, object?[]? parameters, CultureInfo? culture);
      public static bool operator ==(MethodBase? left, MethodBase? right);
      public static bool operator !=(MethodBase? left, MethodBase? right);
    }
    public class MethodBody {
      protected MethodBody();
      public virtual IList<ExceptionHandlingClause> ExceptionHandlingClauses { get; }
      public virtual bool InitLocals { get; }
      public virtual int LocalSignatureMetadataToken { get; }
      public virtual IList<LocalVariableInfo> LocalVariables { get; }
      public virtual int MaxStackSize { get; }
      public virtual byte[]? GetILAsByteArray();
    }
    public enum MethodImplAttributes {
      AggressiveInlining = 256,
      AggressiveOptimization = 512,
      CodeTypeMask = 3,
      ForwardRef = 16,
      IL = 0,
      InternalCall = 4096,
      Managed = 0,
      ManagedMask = 4,
      MaxMethodImplVal = 65535,
      Native = 1,
      NoInlining = 8,
      NoOptimization = 64,
      OPTIL = 2,
      PreserveSig = 128,
      Runtime = 3,
      Synchronized = 32,
      Unmanaged = 4,
    }
    public abstract class MethodInfo : MethodBase {
      protected MethodInfo();
      public override MemberTypes MemberType { get; }
      public virtual ParameterInfo? ReturnParameter { get; }
      public virtual Type? ReturnType { get; }
      public abstract ICustomAttributeProvider? ReturnTypeCustomAttributes { get; }
      public virtual Delegate CreateDelegate(Type delegateType);
      public virtual Delegate CreateDelegate(Type delegateType, object? target);
      public override bool Equals(object? obj);
      public abstract MethodInfo? GetBaseDefinition();
      public override Type[] GetGenericArguments();
      public virtual MethodInfo? GetGenericMethodDefinition();
      public override int GetHashCode();
      public virtual MethodInfo? MakeGenericMethod(params Type[] typeArguments);
      public static bool operator ==(MethodInfo? left, MethodInfo? right);
      public static bool operator !=(MethodInfo? left, MethodInfo? right);
    }
    public sealed class Missing : ISerializable {
      public static readonly Missing Value;
      void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
    }
    public abstract class Module : ICustomAttributeProvider, ISerializable {
      public static readonly TypeFilter FilterTypeName;
      public static readonly TypeFilter FilterTypeNameIgnoreCase;
      protected Module();
      public virtual Assembly Assembly { get; }
      public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
      public virtual string FullyQualifiedName { get; }
      public virtual int MDStreamVersion { get; }
      public virtual int MetadataToken { get; }
      public ModuleHandle ModuleHandle { get; }
      public virtual Guid ModuleVersionId { get; }
      public virtual string Name { get; }
      public virtual string ScopeName { get; }
      public override bool Equals(object? o);
      public virtual Type[] FindTypes(TypeFilter? filter, object filterCriteria);
      public virtual object[] GetCustomAttributes(bool inherit);
      public virtual object[] GetCustomAttributes(Type attributeType, bool inherit);
      public virtual IList<CustomAttributeData> GetCustomAttributesData();
      public FieldInfo? GetField(string name);
      public virtual FieldInfo? GetField(string name, BindingFlags bindingAttr);
      public FieldInfo[] GetFields();
      public virtual FieldInfo[] GetFields(BindingFlags bindingFlags);
      public override int GetHashCode();
      public MethodInfo? GetMethod(string name);
      public MethodInfo? GetMethod(string name, BindingFlags bindingAttr, Binder? binder, CallingConventions callConvention, Type[] types, ParameterModifier[]? modifiers);
      public MethodInfo? GetMethod(string name, Type[] types);
      protected virtual MethodInfo? GetMethodImpl(string name, BindingFlags bindingAttr, Binder? binder, CallingConventions callConvention, Type[]? types, ParameterModifier[]? modifiers);
      public MethodInfo[] GetMethods();
      public virtual MethodInfo[] GetMethods(BindingFlags bindingFlags);
      public virtual void GetObjectData(SerializationInfo info, StreamingContext context);
      public virtual void GetPEKind(out PortableExecutableKinds peKind, out ImageFileMachine machine);
      public virtual Type? GetType(string className);
      public virtual Type? GetType(string className, bool ignoreCase);
      public virtual Type? GetType(string className, bool throwOnError, bool ignoreCase);
      public virtual Type[] GetTypes();
      public virtual bool IsDefined(Type attributeType, bool inherit);
      public virtual bool IsResource();
      public static bool operator ==(Module? left, Module? right);
      public static bool operator !=(Module? left, Module? right);
      public FieldInfo? ResolveField(int metadataToken);
      public virtual FieldInfo? ResolveField(int metadataToken, Type[]? genericTypeArguments, Type[]? genericMethodArguments);
      public MemberInfo? ResolveMember(int metadataToken);
      public virtual MemberInfo? ResolveMember(int metadataToken, Type[]? genericTypeArguments, Type[]? genericMethodArguments);
      public MethodBase? ResolveMethod(int metadataToken);
      public virtual MethodBase? ResolveMethod(int metadataToken, Type[]? genericTypeArguments, Type[]? genericMethodArguments);
      public virtual byte[] ResolveSignature(int metadataToken);
      public virtual string ResolveString(int metadataToken);
      public Type ResolveType(int metadataToken);
      public virtual Type ResolveType(int metadataToken, Type[]? genericTypeArguments, Type[]? genericMethodArguments);
      public override string ToString();
    }
    public delegate Module ModuleResolveEventHandler(object sender, ResolveEventArgs e);
    public sealed class ObfuscateAssemblyAttribute : Attribute {
      public ObfuscateAssemblyAttribute(bool assemblyIsPrivate);
      public bool AssemblyIsPrivate { get; }
      public bool StripAfterObfuscation { get; set; }
    }
    public sealed class ObfuscationAttribute : Attribute {
      public ObfuscationAttribute();
      public bool ApplyToMembers { get; set; }
      public bool Exclude { get; set; }
      public string? Feature { get; set; }
      public bool StripAfterObfuscation { get; set; }
    }
    public enum ParameterAttributes {
      HasDefault = 4096,
      HasFieldMarshal = 8192,
      In = 1,
      Lcid = 4,
      None = 0,
      Optional = 16,
      Out = 2,
      Reserved3 = 16384,
      Reserved4 = 32768,
      ReservedMask = 61440,
      Retval = 8,
    }
    public class ParameterInfo : ICustomAttributeProvider, IObjectReference {
      protected int PositionImpl;
      protected object? DefaultValueImpl;
      protected MemberInfo MemberImpl;
      protected ParameterAttributes AttrsImpl;
      protected string? NameImpl;
      protected Type? ClassImpl;
      protected ParameterInfo();
      public virtual ParameterAttributes Attributes { get; }
      public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
      public virtual object? DefaultValue { get; }
      public virtual bool HasDefaultValue { get; }
      public bool IsIn { get; }
      public bool IsLcid { get; }
      public bool IsOptional { get; }
      public bool IsOut { get; }
      public bool IsRetval { get; }
      public virtual MemberInfo Member { get; }
      public virtual int MetadataToken { get; }
      public virtual string? Name { get; }
      public virtual Type ParameterType { get; }
      public virtual int Position { get; }
      public virtual object? RawDefaultValue { get; }
      public virtual object[] GetCustomAttributes(bool inherit);
      public virtual object[] GetCustomAttributes(Type attributeType, bool inherit);
      public virtual IList<CustomAttributeData> GetCustomAttributesData();
      public virtual Type[] GetOptionalCustomModifiers();
      public object GetRealObject(StreamingContext context);
      public virtual Type[] GetRequiredCustomModifiers();
      public virtual bool IsDefined(Type attributeType, bool inherit);
      public override string ToString();
    }
    public readonly struct ParameterModifier {
      public ParameterModifier(int parameterCount);
      public bool this[int index] { get; set; }
    }
    public sealed class Pointer : ISerializable {
      public unsafe static object Box(void* ptr, Type type);
      void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
      public unsafe static void* Unbox(object ptr);
    }
    public enum PortableExecutableKinds {
      ILOnly = 1,
      NotAPortableExecutableImage = 0,
      PE32Plus = 4,
      Preferred32Bit = 16,
      Required32Bit = 2,
      Unmanaged32Bit = 8,
    }
    public enum ProcessorArchitecture {
      Amd64 = 4,
      Arm = 5,
      IA64 = 3,
      MSIL = 1,
      None = 0,
      X86 = 2,
    }
    public enum PropertyAttributes {
      HasDefault = 4096,
      None = 0,
      Reserved2 = 8192,
      Reserved3 = 16384,
      Reserved4 = 32768,
      ReservedMask = 62464,
      RTSpecialName = 1024,
      SpecialName = 512,
    }
    public abstract class PropertyInfo : MemberInfo {
      protected PropertyInfo();
      public abstract PropertyAttributes Attributes { get; }
      public abstract bool CanRead { get; }
      public abstract bool CanWrite { get; }
      public virtual MethodInfo? GetMethod { get; }
      public bool IsSpecialName { get; }
      public override MemberTypes MemberType { get; }
      public abstract Type PropertyType { get; }
      public virtual MethodInfo? SetMethod { get; }
      public override bool Equals(object? obj);
      public MethodInfo[] GetAccessors();
      public abstract MethodInfo[] GetAccessors(bool nonPublic);
      public virtual object GetConstantValue();
      public MethodInfo? GetGetMethod();
      public abstract MethodInfo? GetGetMethod(bool nonPublic);
      public override int GetHashCode();
      public abstract ParameterInfo[] GetIndexParameters();
      public virtual Type[] GetOptionalCustomModifiers();
      public virtual object GetRawConstantValue();
      public virtual Type[] GetRequiredCustomModifiers();
      public MethodInfo? GetSetMethod();
      public abstract MethodInfo? GetSetMethod(bool nonPublic);
      public object? GetValue(object? obj);
      public virtual object? GetValue(object? obj, object?[]? index);
      public abstract object? GetValue(object? obj, BindingFlags invokeAttr, Binder? binder, object?[]? index, CultureInfo? culture);
      public static bool operator ==(PropertyInfo? left, PropertyInfo? right);
      public static bool operator !=(PropertyInfo? left, PropertyInfo? right);
      public void SetValue(object? obj, object? value);
      public virtual void SetValue(object? obj, object? value, object[]? index);
      public abstract void SetValue(object? obj, object? value, BindingFlags invokeAttr, Binder? binder, object[]? index, CultureInfo? culture);
    }
    public abstract class ReflectionContext {
      protected ReflectionContext();
      public virtual TypeInfo GetTypeForObject(object value);
      public abstract Assembly MapAssembly(Assembly assembly);
      public abstract TypeInfo MapType(TypeInfo type);
    }
    public sealed class ReflectionTypeLoadException : SystemException, ISerializable {
      public ReflectionTypeLoadException(Type[]? classes, Exception?[]? exceptions);
      public ReflectionTypeLoadException(Type[]? classes, Exception?[]? exceptions, string? message);
      public Exception?[]? LoaderExceptions { get; }
      public override string Message { get; }
      public Type[]? Types { get; }
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
      public override string ToString();
    }
    public enum ResourceAttributes {
      Private = 2,
      Public = 1,
    }
    public enum ResourceLocation {
      ContainedInAnotherAssembly = 2,
      ContainedInManifestFile = 4,
      Embedded = 1,
    }
    public static class RuntimeReflectionExtensions {
      public static MethodInfo? GetMethodInfo(this Delegate del);
      public static MethodInfo? GetRuntimeBaseDefinition(this MethodInfo method);
      public static EventInfo? GetRuntimeEvent(this Type type, string name);
      public static IEnumerable<EventInfo> GetRuntimeEvents(this Type type);
      public static FieldInfo? GetRuntimeField(this Type type, string name);
      public static IEnumerable<FieldInfo> GetRuntimeFields(this Type type);
      public static InterfaceMapping GetRuntimeInterfaceMap(this TypeInfo typeInfo, Type interfaceType);
      public static MethodInfo? GetRuntimeMethod(this Type type, string name, Type[] parameters);
      public static IEnumerable<MethodInfo> GetRuntimeMethods(this Type type);
      public static IEnumerable<PropertyInfo> GetRuntimeProperties(this Type type);
      public static PropertyInfo? GetRuntimeProperty(this Type type, string name);
    }
    public class StrongNameKeyPair : IDeserializationCallback, ISerializable {
      public StrongNameKeyPair(byte[] keyPairArray);
      public StrongNameKeyPair(FileStream keyPairFile);
      protected StrongNameKeyPair(SerializationInfo info, StreamingContext context);
      public StrongNameKeyPair(string keyPairContainer);
      public byte[] PublicKey { get; }
      void System.Runtime.Serialization.IDeserializationCallback.OnDeserialization(object sender);
      void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
    }
    public class TargetException : ApplicationException {
      public TargetException();
      protected TargetException(SerializationInfo info, StreamingContext context);
      public TargetException(string? message);
      public TargetException(string? message, Exception? inner);
    }
    public sealed class TargetInvocationException : ApplicationException {
      public TargetInvocationException(Exception? inner);
      public TargetInvocationException(string? message, Exception? inner);
    }
    public sealed class TargetParameterCountException : ApplicationException {
      public TargetParameterCountException();
      public TargetParameterCountException(string? message);
      public TargetParameterCountException(string? message, Exception? inner);
    }
    public enum TypeAttributes {
      Abstract = 128,
      AnsiClass = 0,
      AutoClass = 131072,
      AutoLayout = 0,
      BeforeFieldInit = 1048576,
      Class = 0,
      ClassSemanticsMask = 32,
      CustomFormatClass = 196608,
      CustomFormatMask = 12582912,
      ExplicitLayout = 16,
      HasSecurity = 262144,
      Import = 4096,
      Interface = 32,
      LayoutMask = 24,
      NestedAssembly = 5,
      NestedFamANDAssem = 6,
      NestedFamily = 4,
      NestedFamORAssem = 7,
      NestedPrivate = 3,
      NestedPublic = 2,
      NotPublic = 0,
      Public = 1,
      ReservedMask = 264192,
      RTSpecialName = 2048,
      Sealed = 256,
      SequentialLayout = 8,
      Serializable = 8192,
      SpecialName = 1024,
      StringFormatMask = 196608,
      UnicodeClass = 65536,
      VisibilityMask = 7,
      WindowsRuntime = 16384,
    }
    public class TypeDelegator : TypeInfo {
      protected Type typeImpl;
      protected TypeDelegator();
      public TypeDelegator(Type delegatingType);
      public override Assembly Assembly { get; }
      public override string? AssemblyQualifiedName { get; }
      public override Type? BaseType { get; }
      public override string? FullName { get; }
      public override Guid GUID { get; }
      public override bool IsByRefLike { get; }
      public override bool IsCollectible { get; }
      public override bool IsConstructedGenericType { get; }
      public override bool IsGenericMethodParameter { get; }
      public override bool IsGenericTypeParameter { get; }
      public override bool IsSZArray { get; }
      public override bool IsTypeDefinition { get; }
      public override bool IsVariableBoundArray { get; }
      public override int MetadataToken { get; }
      public override Module Module { get; }
      public override string Name { get; }
      public override string? Namespace { get; }
      public override RuntimeTypeHandle TypeHandle { get; }
      public override Type UnderlyingSystemType { get; }
      protected override TypeAttributes GetAttributeFlagsImpl();
      protected override ConstructorInfo? GetConstructorImpl(BindingFlags bindingAttr, Binder? binder, CallingConventions callConvention, Type[] types, ParameterModifier[]? modifiers);
      public override ConstructorInfo[] GetConstructors(BindingFlags bindingAttr);
      public override object[] GetCustomAttributes(bool inherit);
      public override object[] GetCustomAttributes(Type attributeType, bool inherit);
      public override Type? GetElementType();
      public override EventInfo? GetEvent(string name, BindingFlags bindingAttr);
      public override EventInfo[] GetEvents();
      public override EventInfo[] GetEvents(BindingFlags bindingAttr);
      public override FieldInfo? GetField(string name, BindingFlags bindingAttr);
      public override FieldInfo[] GetFields(BindingFlags bindingAttr);
      public override Type? GetInterface(string name, bool ignoreCase);
      public override InterfaceMapping GetInterfaceMap(Type interfaceType);
      public override Type[] GetInterfaces();
      public override MemberInfo[] GetMember(string name, MemberTypes type, BindingFlags bindingAttr);
      public override MemberInfo[] GetMembers(BindingFlags bindingAttr);
      protected override MethodInfo? GetMethodImpl(string name, BindingFlags bindingAttr, Binder? binder, CallingConventions callConvention, Type[]? types, ParameterModifier[]? modifiers);
      public override MethodInfo[] GetMethods(BindingFlags bindingAttr);
      public override Type? GetNestedType(string name, BindingFlags bindingAttr);
      public override Type[] GetNestedTypes(BindingFlags bindingAttr);
      public override PropertyInfo[] GetProperties(BindingFlags bindingAttr);
      protected override PropertyInfo? GetPropertyImpl(string name, BindingFlags bindingAttr, Binder? binder, Type? returnType, Type[]? types, ParameterModifier[]? modifiers);
      protected override bool HasElementTypeImpl();
      public override object? InvokeMember(string name, BindingFlags invokeAttr, Binder? binder, object? target, object[]? args, ParameterModifier[]? modifiers, CultureInfo? culture, string[]? namedParameters);
      protected override bool IsArrayImpl();
      public override bool IsAssignableFrom(TypeInfo? typeInfo);
      protected override bool IsByRefImpl();
      protected override bool IsCOMObjectImpl();
      public override bool IsDefined(Type attributeType, bool inherit);
      protected override bool IsPointerImpl();
      protected override bool IsPrimitiveImpl();
      protected override bool IsValueTypeImpl();
    }
    public delegate bool TypeFilter(Type m, object filterCriteria);
    public abstract class TypeInfo : Type, IReflectableType {
      protected TypeInfo();
      public virtual IEnumerable<ConstructorInfo> DeclaredConstructors { get; }
      public virtual IEnumerable<EventInfo> DeclaredEvents { get; }
      public virtual IEnumerable<FieldInfo> DeclaredFields { get; }
      public virtual IEnumerable<MemberInfo> DeclaredMembers { get; }
      public virtual IEnumerable<MethodInfo> DeclaredMethods { get; }
      public virtual IEnumerable<TypeInfo> DeclaredNestedTypes { get; }
      public virtual IEnumerable<PropertyInfo> DeclaredProperties { get; }
      public virtual Type[] GenericTypeParameters { get; }
      public virtual IEnumerable<Type> ImplementedInterfaces { get; }
      public virtual Type AsType();
      public virtual EventInfo? GetDeclaredEvent(string name);
      public virtual FieldInfo? GetDeclaredField(string name);
      public virtual MethodInfo? GetDeclaredMethod(string name);
      public virtual IEnumerable<MethodInfo> GetDeclaredMethods(string name);
      public virtual TypeInfo? GetDeclaredNestedType(string name);
      public virtual PropertyInfo? GetDeclaredProperty(string name);
      public virtual bool IsAssignableFrom(TypeInfo? typeInfo);
      TypeInfo System.Reflection.IReflectableType.GetTypeInfo();
    }
  }
  namespace System.Runtime {
    public sealed class AmbiguousImplementationException : Exception {
      public AmbiguousImplementationException();
      public AmbiguousImplementationException(string? message);
      public AmbiguousImplementationException(string? message, Exception? innerException);
    }
    public sealed class AssemblyTargetedPatchBandAttribute : Attribute {
      public AssemblyTargetedPatchBandAttribute(string? targetedPatchBand);
      public string? TargetedPatchBand { get; }
    }
    public enum GCLargeObjectHeapCompactionMode {
      CompactOnce = 2,
      Default = 1,
    }
    public enum GCLatencyMode {
      Batch = 0,
      Interactive = 1,
      LowLatency = 2,
      NoGCRegion = 4,
      SustainedLowLatency = 3,
    }
    public static class GCSettings {
      public static bool IsServerGC { get; }
      public static GCLargeObjectHeapCompactionMode LargeObjectHeapCompactionMode { get; set; }
      public static GCLatencyMode LatencyMode { get; set; }
    }
    public sealed class MemoryFailPoint : CriticalFinalizerObject, IDisposable {
      public MemoryFailPoint(int sizeInMegabytes);
      public void Dispose();
      ~MemoryFailPoint();
    }
    public sealed class TargetedPatchingOptOutAttribute : Attribute {
      public TargetedPatchingOptOutAttribute(string? reason);
      public string? Reason { get; }
    }
  }
  namespace System.Runtime.CompilerServices {
    public sealed class AccessedThroughPropertyAttribute : Attribute {
      public AccessedThroughPropertyAttribute(string propertyName);
      public string PropertyName { get; }
    }
    public sealed class AsyncIteratorStateMachineAttribute : StateMachineAttribute {
      public AsyncIteratorStateMachineAttribute(Type stateMachineType);
    }
    public sealed class AsyncMethodBuilderAttribute : Attribute {
      public AsyncMethodBuilderAttribute(Type builderType);
      public Type BuilderType { get; }
    }
    public sealed class AsyncStateMachineAttribute : StateMachineAttribute {
      public AsyncStateMachineAttribute(Type stateMachineType);
    }
    public struct AsyncValueTaskMethodBuilder {
      public ValueTask Task { get; }
      public void AwaitOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : INotifyCompletion where TStateMachine : IAsyncStateMachine;
      public void AwaitUnsafeOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : ICriticalNotifyCompletion where TStateMachine : IAsyncStateMachine;
      public static AsyncValueTaskMethodBuilder Create();
      public void SetException(Exception exception);
      public void SetResult();
      public void SetStateMachine(IAsyncStateMachine stateMachine);
      public void Start<TStateMachine>(ref TStateMachine stateMachine) where TStateMachine : IAsyncStateMachine;
    }
    public struct AsyncValueTaskMethodBuilder<TResult> {
      public ValueTask<TResult> Task { get; }
      public void AwaitOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : INotifyCompletion where TStateMachine : IAsyncStateMachine;
      public void AwaitUnsafeOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : ICriticalNotifyCompletion where TStateMachine : IAsyncStateMachine;
      public static AsyncValueTaskMethodBuilder<TResult> Create();
      public void SetException(Exception exception);
      public void SetResult(TResult result);
      public void SetStateMachine(IAsyncStateMachine stateMachine);
      public void Start<TStateMachine>(ref TStateMachine stateMachine) where TStateMachine : IAsyncStateMachine;
    }
    public sealed class CallerArgumentExpressionAttribute : Attribute {
      public CallerArgumentExpressionAttribute(string parameterName);
      public string ParameterName { get; }
    }
    public sealed class CallerFilePathAttribute : Attribute {
      public CallerFilePathAttribute();
    }
    public sealed class CallerLineNumberAttribute : Attribute {
      public CallerLineNumberAttribute();
    }
    public sealed class CallerMemberNameAttribute : Attribute {
      public CallerMemberNameAttribute();
    }
    public enum CompilationRelaxations {
      NoStringInterning = 8,
    }
    public class CompilationRelaxationsAttribute : Attribute {
      public CompilationRelaxationsAttribute(int relaxations);
      public CompilationRelaxationsAttribute(CompilationRelaxations relaxations);
      public int CompilationRelaxations { get; }
    }
    public sealed class CompilerGeneratedAttribute : Attribute {
      public CompilerGeneratedAttribute();
    }
    public class CompilerGlobalScopeAttribute : Attribute {
      public CompilerGlobalScopeAttribute();
    }
    public sealed class ConditionalWeakTable<TKey, TValue> : IEnumerable, IEnumerable<KeyValuePair<TKey, TValue>> where TKey : class? where TValue : class? {
      public ConditionalWeakTable();
      public void Add(TKey key, TValue value);
      public void AddOrUpdate(TKey key, TValue value);
      public void Clear();
      public TValue GetOrCreateValue(TKey key);
      public TValue GetValue(TKey key, ConditionalWeakTable<TKey, TValue>.CreateValueCallback createValueCallback);
      public bool Remove(TKey key);
      IEnumerator<KeyValuePair<TKey, TValue>> System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<TKey,TValue>>.GetEnumeratorIEnumerable<KeyValuePair<TKey, TValue>>.GetEnumerator();
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      public bool TryGetValue(TKey key, out TValue value);
      public delegate TValue CreateValueCallback(TKey key);
    }
    public readonly struct ConfiguredTaskAwaitable {
      public ConfiguredTaskAwaitable.ConfiguredTaskAwaiter GetAwaiter();
      public readonly struct ConfiguredTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion {
        public bool IsCompleted { get; }
        public void GetResult();
        public void OnCompleted(Action continuation);
        public void UnsafeOnCompleted(Action continuation);
      }
    }
    public readonly struct ConfiguredTaskAwaitable<TResult> {
      public ConfiguredTaskAwaitable<TResult>.ConfiguredTaskAwaiter GetAwaiter();
      public readonly struct ConfiguredTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion {
        public bool IsCompleted { get; }
        public TResult GetResult();
        public void OnCompleted(Action continuation);
        public void UnsafeOnCompleted(Action continuation);
      }
    }
    public readonly struct ConfiguredValueTaskAwaitable {
      public ConfiguredValueTaskAwaitable.ConfiguredValueTaskAwaiter GetAwaiter();
      public readonly struct ConfiguredValueTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion {
        public bool IsCompleted { get; }
        public void GetResult();
        public void OnCompleted(Action continuation);
        public void UnsafeOnCompleted(Action continuation);
      }
    }
    public readonly struct ConfiguredValueTaskAwaitable<TResult> {
      public ConfiguredValueTaskAwaitable<TResult>.ConfiguredValueTaskAwaiter GetAwaiter();
      public readonly struct ConfiguredValueTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion {
        public bool IsCompleted { get; }
        public TResult GetResult();
        public void OnCompleted(Action continuation);
        public void UnsafeOnCompleted(Action continuation);
      }
    }
    public abstract class CustomConstantAttribute : Attribute {
      protected CustomConstantAttribute();
      public abstract object? Value { get; }
    }
    public sealed class DateTimeConstantAttribute : CustomConstantAttribute {
      public DateTimeConstantAttribute(long ticks);
      public override object? Value { get; }
    }
    public sealed class DecimalConstantAttribute : Attribute {
      public DecimalConstantAttribute(byte scale, byte sign, int hi, int mid, int low);
      public DecimalConstantAttribute(byte scale, byte sign, uint hi, uint mid, uint low);
      public decimal Value { get; }
    }
    public sealed class DefaultDependencyAttribute : Attribute {
      public DefaultDependencyAttribute(LoadHint loadHintArgument);
      public LoadHint LoadHint { get; }
    }
    public sealed class DependencyAttribute : Attribute {
      public DependencyAttribute(string dependentAssemblyArgument, LoadHint loadHintArgument);
      public string DependentAssembly { get; }
      public LoadHint LoadHint { get; }
    }
    public sealed class DisablePrivateReflectionAttribute : Attribute {
      public DisablePrivateReflectionAttribute();
    }
    public class DiscardableAttribute : Attribute {
      public DiscardableAttribute();
    }
    public sealed class EnumeratorCancellationAttribute : Attribute {
      public EnumeratorCancellationAttribute();
    }
    public sealed class ExtensionAttribute : Attribute {
      public ExtensionAttribute();
    }
    public sealed class FixedAddressValueTypeAttribute : Attribute {
      public FixedAddressValueTypeAttribute();
    }
    public sealed class FixedBufferAttribute : Attribute {
      public FixedBufferAttribute(Type elementType, int length);
      public Type ElementType { get; }
      public int Length { get; }
    }
    public static class FormattableStringFactory {
      public static FormattableString Create(string format, params object?[] arguments);
    }
    public interface IAsyncStateMachine {
      void MoveNext();
      void SetStateMachine(IAsyncStateMachine stateMachine);
    }
    public interface ICriticalNotifyCompletion : INotifyCompletion {
      void UnsafeOnCompleted(Action continuation);
    }
    public sealed class IndexerNameAttribute : Attribute {
      public IndexerNameAttribute(string indexerName);
    }
    public interface INotifyCompletion {
      void OnCompleted(Action continuation);
    }
    public sealed class InternalsVisibleToAttribute : Attribute {
      public InternalsVisibleToAttribute(string assemblyName);
      public bool AllInternalsVisible { get; set; }
      public string AssemblyName { get; }
    }
    public sealed class IsByRefLikeAttribute : Attribute {
      public IsByRefLikeAttribute();
    }
    public static class IsConst {
    }
    public sealed class IsReadOnlyAttribute : Attribute {
      public IsReadOnlyAttribute();
    }
    public interface IStrongBox {
      object? Value { get; set; }
    }
    public static class IsVolatile {
    }
    public sealed class IteratorStateMachineAttribute : StateMachineAttribute {
      public IteratorStateMachineAttribute(Type stateMachineType);
    }
    public interface ITuple {
      int Length { get; }
      object? this[int index] { get; }
    }
    public enum LoadHint {
      Always = 1,
      Default = 0,
      Sometimes = 2,
    }
    public enum MethodCodeType {
      IL = 0,
      Native = 1,
      OPTIL = 2,
      Runtime = 3,
    }
    public sealed class MethodImplAttribute : Attribute {
      public MethodCodeType MethodCodeType;
      public MethodImplAttribute();
      public MethodImplAttribute(short value);
      public MethodImplAttribute(MethodImplOptions methodImplOptions);
      public MethodImplOptions Value { get; }
    }
    public enum MethodImplOptions {
      AggressiveInlining = 256,
      AggressiveOptimization = 512,
      ForwardRef = 16,
      InternalCall = 4096,
      NoInlining = 8,
      NoOptimization = 64,
      PreserveSig = 128,
      Synchronized = 32,
      Unmanaged = 4,
    }
    public sealed class ReferenceAssemblyAttribute : Attribute {
      public ReferenceAssemblyAttribute();
      public ReferenceAssemblyAttribute(string? description);
      public string? Description { get; }
    }
    public sealed class RuntimeCompatibilityAttribute : Attribute {
      public RuntimeCompatibilityAttribute();
      public bool WrapNonExceptionThrows { get; set; }
    }
    public static class RuntimeFeature {
      public const string DefaultImplementationsOfInterfaces = "DefaultImplementationsOfInterfaces";
      public const string PortablePdb = "PortablePdb";
      public static bool IsDynamicCodeCompiled { get; }
      public static bool IsDynamicCodeSupported { get; }
      public static bool IsSupported(string feature);
    }
    public static class RuntimeHelpers {
      public static int OffsetToStringData { get; }
      public static void EnsureSufficientExecutionStack();
      public static new bool Equals(object? o1, object? o2);
      public static void ExecuteCodeWithGuaranteedCleanup(RuntimeHelpers.TryCode code, RuntimeHelpers.CleanupCode backoutCode, object? userData);
      public static int GetHashCode(object o);
      public static object GetObjectValue(object obj);
      public static T[] GetSubArray<T>(T[] array, Range range);
      public static object GetUninitializedObject(Type type);
      public static void InitializeArray(Array array, RuntimeFieldHandle fldHandle);
      public static bool IsReferenceOrContainsReferences<T>();
      public static void PrepareConstrainedRegions();
      public static void PrepareConstrainedRegionsNoOP();
      public static void PrepareContractedDelegate(Delegate d);
      public static void PrepareDelegate(Delegate d);
      public static void PrepareMethod(RuntimeMethodHandle method);
      public static void PrepareMethod(RuntimeMethodHandle method, RuntimeTypeHandle[]? instantiation);
      public static void ProbeForSufficientStack();
      public static void RunClassConstructor(RuntimeTypeHandle type);
      public static void RunModuleConstructor(ModuleHandle module);
      public static bool TryEnsureSufficientExecutionStack();
      public delegate void CleanupCode(object? userData, bool exceptionThrown);
      public delegate void TryCode(object? userData);
    }
    public sealed class RuntimeWrappedException : Exception {
      public RuntimeWrappedException(object thrownObject);
      public object WrappedException { get; }
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
    }
    public sealed class SpecialNameAttribute : Attribute {
      public SpecialNameAttribute();
    }
    public class StateMachineAttribute : Attribute {
      public StateMachineAttribute(Type stateMachineType);
      public Type StateMachineType { get; }
    }
    public sealed class StringFreezingAttribute : Attribute {
      public StringFreezingAttribute();
    }
    public class StrongBox<T> : IStrongBox {
      public T Value;
      public StrongBox();
      public StrongBox(T value);
      object? System.Runtime.CompilerServices.IStrongBox.Value { get; set; }
    }
    public sealed class SuppressIldasmAttribute : Attribute {
      public SuppressIldasmAttribute();
    }
    public readonly struct TaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion {
      public bool IsCompleted { get; }
      public void GetResult();
      public void OnCompleted(Action continuation);
      public void UnsafeOnCompleted(Action continuation);
    }
    public readonly struct TaskAwaiter<TResult> : ICriticalNotifyCompletion, INotifyCompletion {
      public bool IsCompleted { get; }
      public TResult GetResult();
      public void OnCompleted(Action continuation);
      public void UnsafeOnCompleted(Action continuation);
    }
    public sealed class TupleElementNamesAttribute : Attribute {
      public TupleElementNamesAttribute(string?[] transformNames);
      public IList<string?> TransformNames { get; }
    }
    public sealed class TypeForwardedFromAttribute : Attribute {
      public TypeForwardedFromAttribute(string assemblyFullName);
      public string AssemblyFullName { get; }
    }
    public sealed class TypeForwardedToAttribute : Attribute {
      public TypeForwardedToAttribute(Type destination);
      public Type Destination { get; }
    }
    public sealed class UnsafeValueTypeAttribute : Attribute {
      public UnsafeValueTypeAttribute();
    }
    public readonly struct ValueTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion {
      public bool IsCompleted { get; }
      public void GetResult();
      public void OnCompleted(Action continuation);
      public void UnsafeOnCompleted(Action continuation);
    }
    public readonly struct ValueTaskAwaiter<TResult> : ICriticalNotifyCompletion, INotifyCompletion {
      public bool IsCompleted { get; }
      public TResult GetResult();
      public void OnCompleted(Action continuation);
      public void UnsafeOnCompleted(Action continuation);
    }
    public readonly struct YieldAwaitable {
      public YieldAwaitable.YieldAwaiter GetAwaiter();
      public readonly struct YieldAwaiter : ICriticalNotifyCompletion, INotifyCompletion {
        public bool IsCompleted { get; }
        public void GetResult();
        public void OnCompleted(Action continuation);
        public void UnsafeOnCompleted(Action continuation);
      }
    }
  }
  namespace System.Runtime.ConstrainedExecution {
    public enum Cer {
      MayFail = 1,
      None = 0,
      Success = 2,
    }
    public enum Consistency {
      MayCorruptAppDomain = 1,
      MayCorruptInstance = 2,
      MayCorruptProcess = 0,
      WillNotCorruptState = 3,
    }
    public abstract class CriticalFinalizerObject {
      protected CriticalFinalizerObject();
      ~CriticalFinalizerObject();
    }
    public sealed class PrePrepareMethodAttribute : Attribute {
      public PrePrepareMethodAttribute();
    }
    public sealed class ReliabilityContractAttribute : Attribute {
      public ReliabilityContractAttribute(Consistency consistencyGuarantee, Cer cer);
      public Cer Cer { get; }
      public Consistency ConsistencyGuarantee { get; }
    }
  }
  namespace System.Runtime.ExceptionServices {
    public sealed class ExceptionDispatchInfo {
      public Exception SourceException { get; }
      public static ExceptionDispatchInfo Capture(Exception source);
      public void Throw();
      public static void Throw(Exception source);
    }
    public class FirstChanceExceptionEventArgs : EventArgs {
      public FirstChanceExceptionEventArgs(Exception exception);
      public Exception Exception { get; }
    }
    public sealed class HandleProcessCorruptedStateExceptionsAttribute : Attribute {
      public HandleProcessCorruptedStateExceptionsAttribute();
    }
  }
  namespace System.Runtime.InteropServices {
    public enum CharSet {
      Ansi = 2,
      Auto = 4,
      None = 1,
      Unicode = 3,
    }
    public sealed class ComVisibleAttribute : Attribute {
      public ComVisibleAttribute(bool visibility);
      public bool Value { get; }
    }
    public abstract class CriticalHandle : CriticalFinalizerObject, IDisposable {
      protected IntPtr handle;
      protected CriticalHandle(IntPtr invalidHandleValue);
      public bool IsClosed { get; }
      public abstract bool IsInvalid { get; }
      public void Close();
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      ~CriticalHandle();
      protected abstract bool ReleaseHandle();
      protected void SetHandle(IntPtr handle);
      public void SetHandleAsInvalid();
    }
    public class ExternalException : SystemException {
      public ExternalException();
      protected ExternalException(SerializationInfo info, StreamingContext context);
      public ExternalException(string? message);
      public ExternalException(string? message, Exception? inner);
      public ExternalException(string? message, int errorCode);
      public virtual int ErrorCode { get; }
      public override string ToString();
    }
    public sealed class FieldOffsetAttribute : Attribute {
      public FieldOffsetAttribute(int offset);
      public int Value { get; }
    }
    public struct GCHandle {
      public bool IsAllocated { get; }
      public object? Target { get; set; }
      public IntPtr AddrOfPinnedObject();
      public static GCHandle Alloc(object? value);
      public static GCHandle Alloc(object? value, GCHandleType type);
      public override bool Equals(object? o);
      public void Free();
      public static GCHandle FromIntPtr(IntPtr value);
      public override int GetHashCode();
      public static bool operator ==(GCHandle a, GCHandle b);
      public static explicit operator GCHandle (IntPtr value);
      public static explicit operator IntPtr (GCHandle value);
      public static bool operator !=(GCHandle a, GCHandle b);
      public static IntPtr ToIntPtr(GCHandle value);
    }
    public enum GCHandleType {
      Normal = 2,
      Pinned = 3,
      Weak = 0,
      WeakTrackResurrection = 1,
    }
    public sealed class InAttribute : Attribute {
      public InAttribute();
    }
    public enum LayoutKind {
      Auto = 3,
      Explicit = 2,
      Sequential = 0,
    }
    public sealed class OutAttribute : Attribute {
      public OutAttribute();
    }
    public abstract class SafeHandle : CriticalFinalizerObject, IDisposable {
      protected IntPtr handle;
      protected SafeHandle(IntPtr invalidHandleValue, bool ownsHandle);
      public bool IsClosed { get; }
      public abstract bool IsInvalid { get; }
      public void Close();
      public void DangerousAddRef(ref bool success);
      public IntPtr DangerousGetHandle();
      public void DangerousRelease();
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      ~SafeHandle();
      protected abstract bool ReleaseHandle();
      protected void SetHandle(IntPtr handle);
      public void SetHandleAsInvalid();
    }
    public sealed class StructLayoutAttribute : Attribute {
      public int Pack;
      public int Size;
      public CharSet CharSet;
      public StructLayoutAttribute(short layoutKind);
      public StructLayoutAttribute(LayoutKind layoutKind);
      public LayoutKind Value { get; }
    }
  }
  namespace System.Runtime.Remoting {
    public class ObjectHandle : MarshalByRefObject {
      public ObjectHandle(object? o);
      public object? Unwrap();
    }
  }
  namespace System.Runtime.Serialization {
    public sealed class DeserializationBlockedException : Exception {
      public DeserializationBlockedException();
      public DeserializationBlockedException(Exception? innerException);
      public DeserializationBlockedException(string? message);
    }
    public interface IDeserializationCallback {
      void OnDeserialization(object sender);
    }
    public interface IFormatterConverter {
      object Convert(object value, Type type);
      object Convert(object value, TypeCode typeCode);
      bool ToBoolean(object value);
      byte ToByte(object value);
      char ToChar(object value);
      DateTime ToDateTime(object value);
      decimal ToDecimal(object value);
      double ToDouble(object value);
      short ToInt16(object value);
      int ToInt32(object value);
      long ToInt64(object value);
      sbyte ToSByte(object value);
      float ToSingle(object value);
      string? ToString(object value);
      ushort ToUInt16(object value);
      uint ToUInt32(object value);
      ulong ToUInt64(object value);
    }
    public interface IObjectReference {
      object GetRealObject(StreamingContext context);
    }
    public interface ISafeSerializationData {
      void CompleteDeserialization(object deserialized);
    }
    public interface ISerializable {
      void GetObjectData(SerializationInfo info, StreamingContext context);
    }
    public sealed class OnDeserializedAttribute : Attribute {
      public OnDeserializedAttribute();
    }
    public sealed class OnDeserializingAttribute : Attribute {
      public OnDeserializingAttribute();
    }
    public sealed class OnSerializedAttribute : Attribute {
      public OnSerializedAttribute();
    }
    public sealed class OnSerializingAttribute : Attribute {
      public OnSerializingAttribute();
    }
    public sealed class OptionalFieldAttribute : Attribute {
      public OptionalFieldAttribute();
      public int VersionAdded { get; set; }
    }
    public sealed class SafeSerializationEventArgs : EventArgs {
      public StreamingContext StreamingContext { get; }
      public void AddSerializedState(ISafeSerializationData serializedState);
    }
    public readonly struct SerializationEntry {
      public string Name { get; }
      public Type ObjectType { get; }
      public object? Value { get; }
    }
    public class SerializationException : SystemException {
      public SerializationException();
      protected SerializationException(SerializationInfo info, StreamingContext context);
      public SerializationException(string? message);
      public SerializationException(string? message, Exception? innerException);
    }
    public sealed class SerializationInfo {
      public SerializationInfo(Type type, IFormatterConverter converter);
      public SerializationInfo(Type type, IFormatterConverter converter, bool requireSameTokenInPartialTrust);
      public string AssemblyName { get; set; }
      public string FullTypeName { get; set; }
      public bool IsAssemblyNameSetExplicit { get; }
      public bool IsFullTypeNameSetExplicit { get; }
      public int MemberCount { get; }
      public Type ObjectType { get; }
      public void AddValue(string name, bool value);
      public void AddValue(string name, byte value);
      public void AddValue(string name, char value);
      public void AddValue(string name, DateTime value);
      public void AddValue(string name, decimal value);
      public void AddValue(string name, double value);
      public void AddValue(string name, short value);
      public void AddValue(string name, int value);
      public void AddValue(string name, long value);
      public void AddValue(string name, object? value);
      public void AddValue(string name, object? value, Type type);
      public void AddValue(string name, sbyte value);
      public void AddValue(string name, float value);
      public void AddValue(string name, ushort value);
      public void AddValue(string name, uint value);
      public void AddValue(string name, ulong value);
      public bool GetBoolean(string name);
      public byte GetByte(string name);
      public char GetChar(string name);
      public DateTime GetDateTime(string name);
      public decimal GetDecimal(string name);
      public double GetDouble(string name);
      public SerializationInfoEnumerator GetEnumerator();
      public short GetInt16(string name);
      public int GetInt32(string name);
      public long GetInt64(string name);
      public sbyte GetSByte(string name);
      public float GetSingle(string name);
      public string? GetString(string name);
      public ushort GetUInt16(string name);
      public uint GetUInt32(string name);
      public ulong GetUInt64(string name);
      public object? GetValue(string name, Type type);
      public void SetType(Type type);
    }
    public sealed class SerializationInfoEnumerator : IEnumerator {
      public SerializationEntry Current { get; }
      public string Name { get; }
      public Type ObjectType { get; }
      object? System.Collections.IEnumerator.Current { get; }
      public object? Value { get; }
      public bool MoveNext();
      public void Reset();
    }
    public readonly struct StreamingContext {
      public StreamingContext(StreamingContextStates state);
      public StreamingContext(StreamingContextStates state, object? additional);
      public object? Context { get; }
      public StreamingContextStates State { get; }
      public override bool Equals(object? obj);
      public override int GetHashCode();
    }
    public enum StreamingContextStates {
      All = 255,
      Clone = 64,
      CrossAppDomain = 128,
      CrossMachine = 2,
      CrossProcess = 1,
      File = 4,
      Other = 32,
      Persistence = 8,
      Remoting = 16,
    }
  }
  namespace System.Runtime.Versioning {
    public sealed class TargetFrameworkAttribute : Attribute {
      public TargetFrameworkAttribute(string frameworkName);
      public string? FrameworkDisplayName { get; set; }
      public string FrameworkName { get; }
    }
  }
  namespace System.Security {
    public sealed class AllowPartiallyTrustedCallersAttribute : Attribute {
      public AllowPartiallyTrustedCallersAttribute();
      public PartialTrustVisibilityLevel PartialTrustVisibilityLevel { get; set; }
    }
    public enum PartialTrustVisibilityLevel {
      NotVisibleByDefault = 1,
      VisibleToAllHosts = 0,
    }
    public sealed class SecurityCriticalAttribute : Attribute {
      public SecurityCriticalAttribute();
      public SecurityCriticalAttribute(SecurityCriticalScope scope);
      public SecurityCriticalScope Scope { get; }
    }
    public enum SecurityCriticalScope {
      Everything = 1,
      Explicit = 0,
    }
    public class SecurityException : SystemException {
      public SecurityException();
      protected SecurityException(SerializationInfo info, StreamingContext context);
      public SecurityException(string? message);
      public SecurityException(string? message, Exception? inner);
      public SecurityException(string? message, Type? type);
      public SecurityException(string? message, Type? type, string? state);
      public object? Demanded { get; set; }
      public object? DenySetInstance { get; set; }
      public AssemblyName? FailedAssemblyInfo { get; set; }
      public string? GrantedSet { get; set; }
      public MethodInfo? Method { get; set; }
      public string? PermissionState { get; set; }
      public Type? PermissionType { get; set; }
      public object? PermitOnlySetInstance { get; set; }
      public string? RefusedSet { get; set; }
      public string? Url { get; set; }
      public override void GetObjectData(SerializationInfo info, StreamingContext context);
      public override string ToString();
    }
    public sealed class SecurityRulesAttribute : Attribute {
      public SecurityRulesAttribute(SecurityRuleSet ruleSet);
      public SecurityRuleSet RuleSet { get; }
      public bool SkipVerificationInFullTrust { get; set; }
    }
    public enum SecurityRuleSet : byte {
      Level1 = (byte)1,
      Level2 = (byte)2,
      None = (byte)0,
    }
    public sealed class SecuritySafeCriticalAttribute : Attribute {
      public SecuritySafeCriticalAttribute();
    }
    public sealed class SecurityTransparentAttribute : Attribute {
      public SecurityTransparentAttribute();
    }
    public sealed class SecurityTreatAsSafeAttribute : Attribute {
      public SecurityTreatAsSafeAttribute();
    }
    public sealed class SuppressUnmanagedCodeSecurityAttribute : Attribute {
      public SuppressUnmanagedCodeSecurityAttribute();
    }
    public sealed class UnverifiableCodeAttribute : Attribute {
      public UnverifiableCodeAttribute();
    }
    public class VerificationException : SystemException {
      public VerificationException();
      protected VerificationException(SerializationInfo info, StreamingContext context);
      public VerificationException(string? message);
      public VerificationException(string? message, Exception? innerException);
    }
  }
  namespace System.Security.Cryptography {
    public class CryptographicException : SystemException {
      public CryptographicException();
      public CryptographicException(int hr);
      protected CryptographicException(SerializationInfo info, StreamingContext context);
      public CryptographicException(string? message);
      public CryptographicException(string? message, Exception? inner);
      public CryptographicException(string format, string? insert);
    }
  }
  namespace System.Text {
    public abstract class Decoder {
      protected Decoder();
      public DecoderFallback? Fallback { get; set; }
      public DecoderFallbackBuffer FallbackBuffer { get; }
      public unsafe virtual void Convert(byte* bytes, int byteCount, char* chars, int charCount, bool flush, out int bytesUsed, out int charsUsed, out bool completed);
      public virtual void Convert(byte[] bytes, int byteIndex, int byteCount, char[] chars, int charIndex, int charCount, bool flush, out int bytesUsed, out int charsUsed, out bool completed);
      public virtual void Convert(ReadOnlySpan<byte> bytes, Span<char> chars, bool flush, out int bytesUsed, out int charsUsed, out bool completed);
      public unsafe virtual int GetCharCount(byte* bytes, int count, bool flush);
      public abstract int GetCharCount(byte[] bytes, int index, int count);
      public virtual int GetCharCount(byte[] bytes, int index, int count, bool flush);
      public virtual int GetCharCount(ReadOnlySpan<byte> bytes, bool flush);
      public unsafe virtual int GetChars(byte* bytes, int byteCount, char* chars, int charCount, bool flush);
      public abstract int GetChars(byte[] bytes, int byteIndex, int byteCount, char[] chars, int charIndex);
      public virtual int GetChars(byte[] bytes, int byteIndex, int byteCount, char[] chars, int charIndex, bool flush);
      public virtual int GetChars(ReadOnlySpan<byte> bytes, Span<char> chars, bool flush);
      public virtual void Reset();
    }
    public sealed class DecoderExceptionFallback : DecoderFallback {
      public DecoderExceptionFallback();
      public override int MaxCharCount { get; }
      public override DecoderFallbackBuffer CreateFallbackBuffer();
      public override bool Equals(object? value);
      public override int GetHashCode();
    }
    public sealed class DecoderExceptionFallbackBuffer : DecoderFallbackBuffer {
      public DecoderExceptionFallbackBuffer();
      public override int Remaining { get; }
      public override bool Fallback(byte[] bytesUnknown, int index);
      public override char GetNextChar();
      public override bool MovePrevious();
    }
    public abstract class DecoderFallback {
      protected DecoderFallback();
      public static DecoderFallback ExceptionFallback { get; }
      public abstract int MaxCharCount { get; }
      public static DecoderFallback ReplacementFallback { get; }
      public abstract DecoderFallbackBuffer CreateFallbackBuffer();
    }
    public abstract class DecoderFallbackBuffer {
      protected DecoderFallbackBuffer();
      public abstract int Remaining { get; }
      public abstract bool Fallback(byte[] bytesUnknown, int index);
      public abstract char GetNextChar();
      public abstract bool MovePrevious();
      public virtual void Reset();
    }
    public sealed class DecoderFallbackException : ArgumentException {
      public DecoderFallbackException();
      public DecoderFallbackException(string? message);
      public DecoderFallbackException(string? message, byte[]? bytesUnknown, int index);
      public DecoderFallbackException(string? message, Exception? innerException);
      public byte[]? BytesUnknown { get; }
      public int Index { get; }
    }
    public sealed class DecoderReplacementFallback : DecoderFallback {
      public DecoderReplacementFallback();
      public DecoderReplacementFallback(string replacement);
      public string DefaultString { get; }
      public override int MaxCharCount { get; }
      public override DecoderFallbackBuffer CreateFallbackBuffer();
      public override bool Equals(object? value);
      public override int GetHashCode();
    }
    public sealed class DecoderReplacementFallbackBuffer : DecoderFallbackBuffer {
      public DecoderReplacementFallbackBuffer(DecoderReplacementFallback fallback);
      public override int Remaining { get; }
      public override bool Fallback(byte[] bytesUnknown, int index);
      public override char GetNextChar();
      public override bool MovePrevious();
      public override void Reset();
    }
    public abstract class Encoder {
      protected Encoder();
      public EncoderFallback? Fallback { get; set; }
      public EncoderFallbackBuffer FallbackBuffer { get; }
      public unsafe virtual void Convert(char* chars, int charCount, byte* bytes, int byteCount, bool flush, out int charsUsed, out int bytesUsed, out bool completed);
      public virtual void Convert(char[] chars, int charIndex, int charCount, byte[] bytes, int byteIndex, int byteCount, bool flush, out int charsUsed, out int bytesUsed, out bool completed);
      public virtual void Convert(ReadOnlySpan<char> chars, Span<byte> bytes, bool flush, out int charsUsed, out int bytesUsed, out bool completed);
      public unsafe virtual int GetByteCount(char* chars, int count, bool flush);
      public abstract int GetByteCount(char[] chars, int index, int count, bool flush);
      public virtual int GetByteCount(ReadOnlySpan<char> chars, bool flush);
      public unsafe virtual int GetBytes(char* chars, int charCount, byte* bytes, int byteCount, bool flush);
      public abstract int GetBytes(char[] chars, int charIndex, int charCount, byte[] bytes, int byteIndex, bool flush);
      public virtual int GetBytes(ReadOnlySpan<char> chars, Span<byte> bytes, bool flush);
      public virtual void Reset();
    }
    public sealed class EncoderExceptionFallback : EncoderFallback {
      public EncoderExceptionFallback();
      public override int MaxCharCount { get; }
      public override EncoderFallbackBuffer CreateFallbackBuffer();
      public override bool Equals(object? value);
      public override int GetHashCode();
    }
    public sealed class EncoderExceptionFallbackBuffer : EncoderFallbackBuffer {
      public EncoderExceptionFallbackBuffer();
      public override int Remaining { get; }
      public override bool Fallback(char charUnknownHigh, char charUnknownLow, int index);
      public override bool Fallback(char charUnknown, int index);
      public override char GetNextChar();
      public override bool MovePrevious();
    }
    public abstract class EncoderFallback {
      protected EncoderFallback();
      public static EncoderFallback ExceptionFallback { get; }
      public abstract int MaxCharCount { get; }
      public static EncoderFallback ReplacementFallback { get; }
      public abstract EncoderFallbackBuffer CreateFallbackBuffer();
    }
    public abstract class EncoderFallbackBuffer {
      protected EncoderFallbackBuffer();
      public abstract int Remaining { get; }
      public abstract bool Fallback(char charUnknownHigh, char charUnknownLow, int index);
      public abstract bool Fallback(char charUnknown, int index);
      public abstract char GetNextChar();
      public abstract bool MovePrevious();
      public virtual void Reset();
    }
    public sealed class EncoderFallbackException : ArgumentException {
      public EncoderFallbackException();
      public EncoderFallbackException(string? message);
      public EncoderFallbackException(string? message, Exception? innerException);
      public char CharUnknown { get; }
      public char CharUnknownHigh { get; }
      public char CharUnknownLow { get; }
      public int Index { get; }
      public bool IsUnknownSurrogate();
    }
    public sealed class EncoderReplacementFallback : EncoderFallback {
      public EncoderReplacementFallback();
      public EncoderReplacementFallback(string replacement);
      public string DefaultString { get; }
      public override int MaxCharCount { get; }
      public override EncoderFallbackBuffer CreateFallbackBuffer();
      public override bool Equals(object? value);
      public override int GetHashCode();
    }
    public sealed class EncoderReplacementFallbackBuffer : EncoderFallbackBuffer {
      public EncoderReplacementFallbackBuffer(EncoderReplacementFallback fallback);
      public override int Remaining { get; }
      public override bool Fallback(char charUnknownHigh, char charUnknownLow, int index);
      public override bool Fallback(char charUnknown, int index);
      public override char GetNextChar();
      public override bool MovePrevious();
      public override void Reset();
    }
    public abstract class Encoding : ICloneable {
      protected Encoding();
      protected Encoding(int codePage);
      protected Encoding(int codePage, EncoderFallback? encoderFallback, DecoderFallback? decoderFallback);
      public static Encoding ASCII { get; }
      public static Encoding BigEndianUnicode { get; }
      public virtual string? BodyName { get; }
      public virtual int CodePage { get; }
      public DecoderFallback DecoderFallback { get; set; }
      public static Encoding Default { get; }
      public EncoderFallback EncoderFallback { get; set; }
      public virtual string? EncodingName { get; }
      public virtual string? HeaderName { get; }
      public virtual bool IsBrowserDisplay { get; }
      public virtual bool IsBrowserSave { get; }
      public virtual bool IsMailNewsDisplay { get; }
      public virtual bool IsMailNewsSave { get; }
      public bool IsReadOnly { get; }
      public virtual bool IsSingleByte { get; }
      public virtual ReadOnlySpan<byte> Preamble { get; }
      public static Encoding Unicode { get; }
      public static Encoding UTF32 { get; }
      public static Encoding UTF7 { get; }
      public static Encoding UTF8 { get; }
      public virtual string? WebName { get; }
      public virtual int WindowsCodePage { get; }
      public virtual object Clone();
      public static byte[] Convert(Encoding srcEncoding, Encoding dstEncoding, byte[] bytes);
      public static byte[] Convert(Encoding srcEncoding, Encoding dstEncoding, byte[] bytes, int index, int count);
      public override bool Equals(object? value);
      public unsafe virtual int GetByteCount(char* chars, int count);
      public virtual int GetByteCount(char[] chars);
      public abstract int GetByteCount(char[] chars, int index, int count);
      public virtual int GetByteCount(ReadOnlySpan<char> chars);
      public virtual int GetByteCount(string s);
      public int GetByteCount(string s, int index, int count);
      public unsafe virtual int GetBytes(char* chars, int charCount, byte* bytes, int byteCount);
      public virtual byte[] GetBytes(char[] chars);
      public virtual byte[] GetBytes(char[] chars, int index, int count);
      public abstract int GetBytes(char[] chars, int charIndex, int charCount, byte[] bytes, int byteIndex);
      public virtual int GetBytes(ReadOnlySpan<char> chars, Span<byte> bytes);
      public virtual byte[] GetBytes(string s);
      public byte[] GetBytes(string s, int index, int count);
      public virtual int GetBytes(string s, int charIndex, int charCount, byte[] bytes, int byteIndex);
      public unsafe virtual int GetCharCount(byte* bytes, int count);
      public virtual int GetCharCount(byte[] bytes);
      public abstract int GetCharCount(byte[] bytes, int index, int count);
      public virtual int GetCharCount(ReadOnlySpan<byte> bytes);
      public unsafe virtual int GetChars(byte* bytes, int byteCount, char* chars, int charCount);
      public virtual char[] GetChars(byte[] bytes);
      public virtual char[] GetChars(byte[] bytes, int index, int count);
      public abstract int GetChars(byte[] bytes, int byteIndex, int byteCount, char[] chars, int charIndex);
      public virtual int GetChars(ReadOnlySpan<byte> bytes, Span<char> chars);
      public virtual Decoder GetDecoder();
      public virtual Encoder GetEncoder();
      public static Encoding GetEncoding(int codepage);
      public static Encoding GetEncoding(int codepage, EncoderFallback encoderFallback, DecoderFallback decoderFallback);
      public static Encoding GetEncoding(string name);
      public static Encoding GetEncoding(string name, EncoderFallback encoderFallback, DecoderFallback decoderFallback);
      public static EncodingInfo[] GetEncodings();
      public override int GetHashCode();
      public abstract int GetMaxByteCount(int charCount);
      public abstract int GetMaxCharCount(int byteCount);
      public virtual byte[] GetPreamble();
      public unsafe string GetString(byte* bytes, int byteCount);
      public virtual string GetString(byte[] bytes);
      public virtual string GetString(byte[] bytes, int index, int count);
      public string GetString(ReadOnlySpan<byte> bytes);
      public bool IsAlwaysNormalized();
      public virtual bool IsAlwaysNormalized(NormalizationForm form);
      public static void RegisterProvider(EncodingProvider provider);
    }
    public sealed class EncodingInfo {
      public int CodePage { get; }
      public string DisplayName { get; }
      public string Name { get; }
      public override bool Equals(object? value);
      public Encoding GetEncoding();
      public override int GetHashCode();
    }
    public abstract class EncodingProvider {
      public EncodingProvider();
      public abstract Encoding? GetEncoding(int codepage);
      public virtual Encoding? GetEncoding(int codepage, EncoderFallback encoderFallback, DecoderFallback decoderFallback);
      public abstract Encoding? GetEncoding(string name);
      public virtual Encoding? GetEncoding(string name, EncoderFallback encoderFallback, DecoderFallback decoderFallback);
    }
    public enum NormalizationForm {
      FormC = 1,
      FormD = 2,
      FormKC = 5,
      FormKD = 6,
    }
    public readonly struct Rune : IComparable<Rune>, IEquatable<Rune> {
      public Rune(char ch);
      public Rune(char highSurrogate, char lowSurrogate);
      public Rune(int value);
      public Rune(uint value);
      public bool IsAscii { get; }
      public bool IsBmp { get; }
      public int Plane { get; }
      public static Rune ReplacementChar { get; }
      public int Utf16SequenceLength { get; }
      public int Utf8SequenceLength { get; }
      public int Value { get; }
      public int CompareTo(Rune other);
      public static OperationStatus DecodeFromUtf16(ReadOnlySpan<char> source, out Rune result, out int charsConsumed);
      public static OperationStatus DecodeFromUtf8(ReadOnlySpan<byte> source, out Rune result, out int bytesConsumed);
      public static OperationStatus DecodeLastFromUtf16(ReadOnlySpan<char> source, out Rune result, out int charsConsumed);
      public static OperationStatus DecodeLastFromUtf8(ReadOnlySpan<byte> source, out Rune value, out int bytesConsumed);
      public int EncodeToUtf16(Span<char> destination);
      public int EncodeToUtf8(Span<byte> destination);
      public override bool Equals(object? obj);
      public bool Equals(Rune other);
      public override int GetHashCode();
      public static double GetNumericValue(Rune value);
      public static Rune GetRuneAt(string input, int index);
      public static UnicodeCategory GetUnicodeCategory(Rune value);
      public static bool IsControl(Rune value);
      public static bool IsDigit(Rune value);
      public static bool IsLetter(Rune value);
      public static bool IsLetterOrDigit(Rune value);
      public static bool IsLower(Rune value);
      public static bool IsNumber(Rune value);
      public static bool IsPunctuation(Rune value);
      public static bool IsSeparator(Rune value);
      public static bool IsSymbol(Rune value);
      public static bool IsUpper(Rune value);
      public static bool IsValid(int value);
      public static bool IsValid(uint value);
      public static bool IsWhiteSpace(Rune value);
      public static bool operator ==(Rune left, Rune right);
      public static explicit operator Rune (char ch);
      public static explicit operator Rune (int value);
      public static explicit operator Rune (uint value);
      public static bool operator >(Rune left, Rune right);
      public static bool operator >=(Rune left, Rune right);
      public static bool operator !=(Rune left, Rune right);
      public static bool operator <(Rune left, Rune right);
      public static bool operator <=(Rune left, Rune right);
      public static Rune ToLower(Rune value, CultureInfo culture);
      public static Rune ToLowerInvariant(Rune value);
      public override string ToString();
      public static Rune ToUpper(Rune value, CultureInfo culture);
      public static Rune ToUpperInvariant(Rune value);
      public static bool TryCreate(char highSurrogate, char lowSurrogate, out Rune result);
      public static bool TryCreate(char ch, out Rune result);
      public static bool TryCreate(int value, out Rune result);
      public static bool TryCreate(uint value, out Rune result);
      public bool TryEncodeToUtf16(Span<char> destination, out int charsWritten);
      public bool TryEncodeToUtf8(Span<byte> destination, out int bytesWritten);
      public static bool TryGetRuneAt(string input, int index, out Rune value);
    }
    public sealed class StringBuilder : ISerializable {
      public StringBuilder();
      public StringBuilder(int capacity);
      public StringBuilder(int capacity, int maxCapacity);
      public StringBuilder(string? value);
      public StringBuilder(string? value, int capacity);
      public StringBuilder(string? value, int startIndex, int length, int capacity);
      public int Capacity { get; set; }
      public int Length { get; set; }
      public int MaxCapacity { get; }
      public char this[int index] { get; set; }
      public StringBuilder Append(bool value);
      public StringBuilder Append(byte value);
      public StringBuilder Append(char value);
      public unsafe StringBuilder Append(char* value, int valueCount);
      public StringBuilder Append(char value, int repeatCount);
      public StringBuilder Append(char[]? value);
      public StringBuilder Append(char[]? value, int startIndex, int charCount);
      public StringBuilder Append(decimal value);
      public StringBuilder Append(double value);
      public StringBuilder Append(short value);
      public StringBuilder Append(int value);
      public StringBuilder Append(long value);
      public StringBuilder Append(object? value);
      public StringBuilder Append(ReadOnlyMemory<char> value);
      public StringBuilder Append(ReadOnlySpan<char> value);
      public StringBuilder Append(sbyte value);
      public StringBuilder Append(float value);
      public StringBuilder Append(string? value);
      public StringBuilder Append(string? value, int startIndex, int count);
      public StringBuilder Append(StringBuilder? value);
      public StringBuilder Append(StringBuilder? value, int startIndex, int count);
      public StringBuilder Append(ushort value);
      public StringBuilder Append(uint value);
      public StringBuilder Append(ulong value);
      public StringBuilder AppendFormat(IFormatProvider provider, string format, object? arg0);
      public StringBuilder AppendFormat(IFormatProvider provider, string format, object? arg0, object? arg1);
      public StringBuilder AppendFormat(IFormatProvider provider, string format, object? arg0, object? arg1, object? arg2);
      public StringBuilder AppendFormat(IFormatProvider provider, string format, params object?[] args);
      public StringBuilder AppendFormat(string format, object? arg0);
      public StringBuilder AppendFormat(string format, object? arg0, object? arg1);
      public StringBuilder AppendFormat(string format, object? arg0, object? arg1, object? arg2);
      public StringBuilder AppendFormat(string format, params object?[] args);
      public StringBuilder AppendJoin(char separator, params object?[] values);
      public StringBuilder AppendJoin(char separator, params string?[] values);
      public StringBuilder AppendJoin(string? separator, params object?[] values);
      public StringBuilder AppendJoin(string? separator, params string?[] values);
      public StringBuilder AppendJoin<T>(char separator, IEnumerable<T> values);
      public StringBuilder AppendJoin<T>(string? separator, IEnumerable<T> values);
      public StringBuilder AppendLine();
      public StringBuilder AppendLine(string? value);
      public StringBuilder Clear();
      public void CopyTo(int sourceIndex, char[] destination, int destinationIndex, int count);
      public void CopyTo(int sourceIndex, Span<char> destination, int count);
      public int EnsureCapacity(int capacity);
      public bool Equals(ReadOnlySpan<char> span);
      public bool Equals(StringBuilder? sb);
      public StringBuilder.ChunkEnumerator GetChunks();
      public StringBuilder Insert(int index, bool value);
      public StringBuilder Insert(int index, byte value);
      public StringBuilder Insert(int index, char value);
      public StringBuilder Insert(int index, char[]? value);
      public StringBuilder Insert(int index, char[]? value, int startIndex, int charCount);
      public StringBuilder Insert(int index, decimal value);
      public StringBuilder Insert(int index, double value);
      public StringBuilder Insert(int index, short value);
      public StringBuilder Insert(int index, int value);
      public StringBuilder Insert(int index, long value);
      public StringBuilder Insert(int index, object? value);
      public StringBuilder Insert(int index, ReadOnlySpan<char> value);
      public StringBuilder Insert(int index, sbyte value);
      public StringBuilder Insert(int index, float value);
      public StringBuilder Insert(int index, string? value);
      public StringBuilder Insert(int index, string? value, int count);
      public StringBuilder Insert(int index, ushort value);
      public StringBuilder Insert(int index, uint value);
      public StringBuilder Insert(int index, ulong value);
      public StringBuilder Remove(int startIndex, int length);
      public StringBuilder Replace(char oldChar, char newChar);
      public StringBuilder Replace(char oldChar, char newChar, int startIndex, int count);
      public StringBuilder Replace(string oldValue, string? newValue);
      public StringBuilder Replace(string oldValue, string? newValue, int startIndex, int count);
      void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
      public override string ToString();
      public string ToString(int startIndex, int length);
      public struct ChunkEnumerator {
        public ReadOnlyMemory<char> Current { get; }
        public StringBuilder.ChunkEnumerator GetEnumerator();
        public bool MoveNext();
      }
    }
    public struct StringRuneEnumerator : IDisposable, IEnumerable, IEnumerable<Rune>, IEnumerator, IEnumerator<Rune> {
      public Rune Current { get; }
      object? System.Collections.IEnumerator.Current { get; }
      public StringRuneEnumerator GetEnumerator();
      public bool MoveNext();
      IEnumerator<Rune> System.Collections.Generic.IEnumerable<System.Text.Rune>.GetEnumerator();
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      void System.Collections.IEnumerator.Reset();
      void System.IDisposable.Dispose();
    }
  }
  namespace System.Text.Unicode {
    public static class Utf8 {
      public static OperationStatus FromUtf16(ReadOnlySpan<char> source, Span<byte> destination, out int charsRead, out int bytesWritten, bool replaceInvalidSequences = true, bool isFinalBlock = true);
      public static OperationStatus ToUtf16(ReadOnlySpan<byte> source, Span<char> destination, out int bytesRead, out int charsWritten, bool replaceInvalidSequences = true, bool isFinalBlock = true);
    }
  }
  namespace System.Threading {
    public readonly struct CancellationToken {
      public CancellationToken(bool canceled);
      public bool CanBeCanceled { get; }
      public bool IsCancellationRequested { get; }
      public static CancellationToken None { get; }
      public WaitHandle WaitHandle { get; }
      public override bool Equals(object? other);
      public bool Equals(CancellationToken other);
      public override int GetHashCode();
      public static bool operator ==(CancellationToken left, CancellationToken right);
      public static bool operator !=(CancellationToken left, CancellationToken right);
      public CancellationTokenRegistration Register(Action callback);
      public CancellationTokenRegistration Register(Action callback, bool useSynchronizationContext);
      public CancellationTokenRegistration Register(Action<object?> callback, object? state);
      public CancellationTokenRegistration Register(Action<object?> callback, object? state, bool useSynchronizationContext);
      public void ThrowIfCancellationRequested();
      public CancellationTokenRegistration UnsafeRegister(Action<object?> callback, object? state);
    }
    public readonly struct CancellationTokenRegistration : IAsyncDisposable, IDisposable, IEquatable<CancellationTokenRegistration> {
      public CancellationToken Token { get; }
      public void Dispose();
      public ValueTask DisposeAsync();
      public override bool Equals(object? obj);
      public bool Equals(CancellationTokenRegistration other);
      public override int GetHashCode();
      public static bool operator ==(CancellationTokenRegistration left, CancellationTokenRegistration right);
      public static bool operator !=(CancellationTokenRegistration left, CancellationTokenRegistration right);
      public bool Unregister();
    }
    public enum LazyThreadSafetyMode {
      ExecutionAndPublication = 2,
      None = 0,
      PublicationOnly = 1,
    }
    public static class Timeout {
      public const int Infinite = -1;
      public static readonly TimeSpan InfiniteTimeSpan;
    }
    public abstract class WaitHandle : MarshalByRefObject, IDisposable {
      public const int WaitTimeout = 258;
      protected static readonly IntPtr InvalidHandle;
      protected WaitHandle();
      public virtual IntPtr Handle { get; set; }
      public SafeWaitHandle? SafeWaitHandle { get; set; }
      public virtual void Close();
      public void Dispose();
      protected virtual void Dispose(bool explicitDisposing);
      public static bool SignalAndWait(WaitHandle toSignal, WaitHandle toWaitOn);
      public static bool SignalAndWait(WaitHandle toSignal, WaitHandle toWaitOn, int millisecondsTimeout, bool exitContext);
      public static bool SignalAndWait(WaitHandle toSignal, WaitHandle toWaitOn, TimeSpan timeout, bool exitContext);
      public static bool WaitAll(WaitHandle[] waitHandles);
      public static bool WaitAll(WaitHandle[] waitHandles, int millisecondsTimeout);
      public static bool WaitAll(WaitHandle[] waitHandles, int millisecondsTimeout, bool exitContext);
      public static bool WaitAll(WaitHandle[] waitHandles, TimeSpan timeout);
      public static bool WaitAll(WaitHandle[] waitHandles, TimeSpan timeout, bool exitContext);
      public static int WaitAny(WaitHandle[] waitHandles);
      public static int WaitAny(WaitHandle[] waitHandles, int millisecondsTimeout);
      public static int WaitAny(WaitHandle[] waitHandles, int millisecondsTimeout, bool exitContext);
      public static int WaitAny(WaitHandle[] waitHandles, TimeSpan timeout);
      public static int WaitAny(WaitHandle[] waitHandles, TimeSpan timeout, bool exitContext);
      public virtual bool WaitOne();
      public virtual bool WaitOne(int millisecondsTimeout);
      public virtual bool WaitOne(int millisecondsTimeout, bool exitContext);
      public virtual bool WaitOne(TimeSpan timeout);
      public virtual bool WaitOne(TimeSpan timeout, bool exitContext);
    }
    public static class WaitHandleExtensions {
      public static SafeWaitHandle? GetSafeWaitHandle(this WaitHandle waitHandle);
      public static void SetSafeWaitHandle(this WaitHandle waitHandle, SafeWaitHandle? value);
    }
  }
  namespace System.Threading.Tasks {
    public class Task : IAsyncResult, IDisposable {
      public Task(Action action);
      public Task(Action action, CancellationToken cancellationToken);
      public Task(Action action, CancellationToken cancellationToken, TaskCreationOptions creationOptions);
      public Task(Action action, TaskCreationOptions creationOptions);
      public Task(Action<object?> action, object? state);
      public Task(Action<object?> action, object? state, CancellationToken cancellationToken);
      public Task(Action<object?> action, object? state, CancellationToken cancellationToken, TaskCreationOptions creationOptions);
      public Task(Action<object?> action, object? state, TaskCreationOptions creationOptions);
      public object? AsyncState { get; }
      public static Task CompletedTask { get; }
      public TaskCreationOptions CreationOptions { get; }
      public static int? CurrentId { get; }
      public AggregateException? Exception { get; }
      public static TaskFactory Factory { get; }
      public int Id { get; }
      public bool IsCanceled { get; }
      public bool IsCompleted { get; }
      public bool IsCompletedSuccessfully { get; }
      public bool IsFaulted { get; }
      public TaskStatus Status { get; }
      WaitHandle System.IAsyncResult.AsyncWaitHandle { get; }
      bool System.IAsyncResult.CompletedSynchronously { get; }
      public ConfiguredTaskAwaitable ConfigureAwait(bool continueOnCapturedContext);
      public Task ContinueWith(Action<Task, object?> continuationAction, object? state);
      public Task ContinueWith(Action<Task, object?> continuationAction, object? state, CancellationToken cancellationToken);
      public Task ContinueWith(Action<Task, object?> continuationAction, object? state, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task ContinueWith(Action<Task, object?> continuationAction, object? state, TaskContinuationOptions continuationOptions);
      public Task ContinueWith(Action<Task, object?> continuationAction, object? state, TaskScheduler scheduler);
      public Task ContinueWith(Action<Task> continuationAction);
      public Task ContinueWith(Action<Task> continuationAction, CancellationToken cancellationToken);
      public Task ContinueWith(Action<Task> continuationAction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task ContinueWith(Action<Task> continuationAction, TaskContinuationOptions continuationOptions);
      public Task ContinueWith(Action<Task> continuationAction, TaskScheduler scheduler);
      public Task<TResult> ContinueWith<TResult>(Func<Task, object?, TResult> continuationFunction, object? state);
      public Task<TResult> ContinueWith<TResult>(Func<Task, object?, TResult> continuationFunction, object? state, CancellationToken cancellationToken);
      public Task<TResult> ContinueWith<TResult>(Func<Task, object?, TResult> continuationFunction, object? state, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task<TResult> ContinueWith<TResult>(Func<Task, object?, TResult> continuationFunction, object? state, TaskContinuationOptions continuationOptions);
      public Task<TResult> ContinueWith<TResult>(Func<Task, object?, TResult> continuationFunction, object? state, TaskScheduler scheduler);
      public Task<TResult> ContinueWith<TResult>(Func<Task, TResult> continuationFunction);
      public Task<TResult> ContinueWith<TResult>(Func<Task, TResult> continuationFunction, CancellationToken cancellationToken);
      public Task<TResult> ContinueWith<TResult>(Func<Task, TResult> continuationFunction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task<TResult> ContinueWith<TResult>(Func<Task, TResult> continuationFunction, TaskContinuationOptions continuationOptions);
      public Task<TResult> ContinueWith<TResult>(Func<Task, TResult> continuationFunction, TaskScheduler scheduler);
      public static Task Delay(int millisecondsDelay);
      public static Task Delay(int millisecondsDelay, CancellationToken cancellationToken);
      public static Task Delay(TimeSpan delay);
      public static Task Delay(TimeSpan delay, CancellationToken cancellationToken);
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      public static Task FromCanceled(CancellationToken cancellationToken);
      public static Task<TResult> FromCanceled<TResult>(CancellationToken cancellationToken);
      public static Task FromException(Exception exception);
      public static Task<TResult> FromException<TResult>(Exception exception);
      public static Task<TResult> FromResult<TResult>(TResult result);
      public TaskAwaiter GetAwaiter();
      public static Task Run(Action action);
      public static Task Run(Action action, CancellationToken cancellationToken);
      public static Task Run(Func<Task?> function);
      public static Task Run(Func<Task?> function, CancellationToken cancellationToken);
      public static Task<TResult> Run<TResult>(Func<Task<TResult>?> function);
      public static Task<TResult> Run<TResult>(Func<Task<TResult>?> function, CancellationToken cancellationToken);
      public static Task<TResult> Run<TResult>(Func<TResult> function);
      public static Task<TResult> Run<TResult>(Func<TResult> function, CancellationToken cancellationToken);
      public void RunSynchronously();
      public void RunSynchronously(TaskScheduler scheduler);
      public void Start();
      public void Start(TaskScheduler scheduler);
      public void Wait();
      public bool Wait(int millisecondsTimeout);
      public bool Wait(int millisecondsTimeout, CancellationToken cancellationToken);
      public void Wait(CancellationToken cancellationToken);
      public bool Wait(TimeSpan timeout);
      public static void WaitAll(params Task[] tasks);
      public static bool WaitAll(Task[] tasks, int millisecondsTimeout);
      public static bool WaitAll(Task[] tasks, int millisecondsTimeout, CancellationToken cancellationToken);
      public static void WaitAll(Task[] tasks, CancellationToken cancellationToken);
      public static bool WaitAll(Task[] tasks, TimeSpan timeout);
      public static int WaitAny(params Task[] tasks);
      public static int WaitAny(Task[] tasks, int millisecondsTimeout);
      public static int WaitAny(Task[] tasks, int millisecondsTimeout, CancellationToken cancellationToken);
      public static int WaitAny(Task[] tasks, CancellationToken cancellationToken);
      public static int WaitAny(Task[] tasks, TimeSpan timeout);
      public static Task WhenAll(IEnumerable<Task> tasks);
      public static Task WhenAll(params Task[] tasks);
      public static Task<TResult[]> WhenAll<TResult>(IEnumerable<Task<TResult>> tasks);
      public static Task<TResult[]> WhenAll<TResult>(params Task<TResult>[] tasks);
      public static Task<Task> WhenAny(IEnumerable<Task> tasks);
      public static Task<Task> WhenAny(params Task[] tasks);
      public static Task<Task<TResult>> WhenAny<TResult>(IEnumerable<Task<TResult>> tasks);
      public static Task<Task<TResult>> WhenAny<TResult>(params Task<TResult>[] tasks);
      public static YieldAwaitable Yield();
    }
    public class Task<TResult> : Task {
      public Task(Func<object?, TResult> function, object? state);
      public Task(Func<object?, TResult> function, object? state, CancellationToken cancellationToken);
      public Task(Func<object?, TResult> function, object? state, CancellationToken cancellationToken, TaskCreationOptions creationOptions);
      public Task(Func<object?, TResult> function, object? state, TaskCreationOptions creationOptions);
      public Task(Func<TResult> function);
      public Task(Func<TResult> function, CancellationToken cancellationToken);
      public Task(Func<TResult> function, CancellationToken cancellationToken, TaskCreationOptions creationOptions);
      public Task(Func<TResult> function, TaskCreationOptions creationOptions);
      public static new TaskFactory<TResult> Factory { get; }
      public TResult Result { get; }
      public new ConfiguredTaskAwaitable<TResult> ConfigureAwait(bool continueOnCapturedContext);
      public Task ContinueWith(Action<Task<TResult>, object?> continuationAction, object? state);
      public Task ContinueWith(Action<Task<TResult>, object?> continuationAction, object? state, CancellationToken cancellationToken);
      public Task ContinueWith(Action<Task<TResult>, object?> continuationAction, object? state, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task ContinueWith(Action<Task<TResult>, object?> continuationAction, object? state, TaskContinuationOptions continuationOptions);
      public Task ContinueWith(Action<Task<TResult>, object?> continuationAction, object? state, TaskScheduler scheduler);
      public Task ContinueWith(Action<Task<TResult>> continuationAction);
      public Task ContinueWith(Action<Task<TResult>> continuationAction, CancellationToken cancellationToken);
      public Task ContinueWith(Action<Task<TResult>> continuationAction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task ContinueWith(Action<Task<TResult>> continuationAction, TaskContinuationOptions continuationOptions);
      public Task ContinueWith(Action<Task<TResult>> continuationAction, TaskScheduler scheduler);
      public Task<TNewResult> ContinueWith<TNewResult>(Func<Task<TResult>, object?, TNewResult> continuationFunction, object? state);
      public Task<TNewResult> ContinueWith<TNewResult>(Func<Task<TResult>, object?, TNewResult> continuationFunction, object? state, CancellationToken cancellationToken);
      public Task<TNewResult> ContinueWith<TNewResult>(Func<Task<TResult>, object?, TNewResult> continuationFunction, object? state, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task<TNewResult> ContinueWith<TNewResult>(Func<Task<TResult>, object?, TNewResult> continuationFunction, object? state, TaskContinuationOptions continuationOptions);
      public Task<TNewResult> ContinueWith<TNewResult>(Func<Task<TResult>, object?, TNewResult> continuationFunction, object? state, TaskScheduler scheduler);
      public Task<TNewResult> ContinueWith<TNewResult>(Func<Task<TResult>, TNewResult> continuationFunction);
      public Task<TNewResult> ContinueWith<TNewResult>(Func<Task<TResult>, TNewResult> continuationFunction, CancellationToken cancellationToken);
      public Task<TNewResult> ContinueWith<TNewResult>(Func<Task<TResult>, TNewResult> continuationFunction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task<TNewResult> ContinueWith<TNewResult>(Func<Task<TResult>, TNewResult> continuationFunction, TaskContinuationOptions continuationOptions);
      public Task<TNewResult> ContinueWith<TNewResult>(Func<Task<TResult>, TNewResult> continuationFunction, TaskScheduler scheduler);
      public new TaskAwaiter<TResult> GetAwaiter();
    }
    public enum TaskContinuationOptions {
      AttachedToParent = 4,
      DenyChildAttach = 8,
      ExecuteSynchronously = 524288,
      HideScheduler = 16,
      LazyCancellation = 32,
      LongRunning = 2,
      None = 0,
      NotOnCanceled = 262144,
      NotOnFaulted = 131072,
      NotOnRanToCompletion = 65536,
      OnlyOnCanceled = 196608,
      OnlyOnFaulted = 327680,
      OnlyOnRanToCompletion = 393216,
      PreferFairness = 1,
      RunContinuationsAsynchronously = 64,
    }
    public enum TaskCreationOptions {
      AttachedToParent = 4,
      DenyChildAttach = 8,
      HideScheduler = 16,
      LongRunning = 2,
      None = 0,
      PreferFairness = 1,
      RunContinuationsAsynchronously = 64,
    }
    public class TaskFactory {
      public TaskFactory();
      public TaskFactory(CancellationToken cancellationToken);
      public TaskFactory(CancellationToken cancellationToken, TaskCreationOptions creationOptions, TaskContinuationOptions continuationOptions, TaskScheduler? scheduler);
      public TaskFactory(TaskCreationOptions creationOptions, TaskContinuationOptions continuationOptions);
      public TaskFactory(TaskScheduler? scheduler);
      public CancellationToken CancellationToken { get; }
      public TaskContinuationOptions ContinuationOptions { get; }
      public TaskCreationOptions CreationOptions { get; }
      public TaskScheduler? Scheduler { get; }
      public Task ContinueWhenAll(Task[] tasks, Action<Task[]> continuationAction);
      public Task ContinueWhenAll(Task[] tasks, Action<Task[]> continuationAction, CancellationToken cancellationToken);
      public Task ContinueWhenAll(Task[] tasks, Action<Task[]> continuationAction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task ContinueWhenAll(Task[] tasks, Action<Task[]> continuationAction, TaskContinuationOptions continuationOptions);
      public Task<TResult> ContinueWhenAll<TAntecedentResult, TResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>[], TResult> continuationFunction);
      public Task<TResult> ContinueWhenAll<TAntecedentResult, TResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>[], TResult> continuationFunction, CancellationToken cancellationToken);
      public Task<TResult> ContinueWhenAll<TAntecedentResult, TResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>[], TResult> continuationFunction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task<TResult> ContinueWhenAll<TAntecedentResult, TResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>[], TResult> continuationFunction, TaskContinuationOptions continuationOptions);
      public Task ContinueWhenAll<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Action<Task<TAntecedentResult>[]> continuationAction);
      public Task ContinueWhenAll<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Action<Task<TAntecedentResult>[]> continuationAction, CancellationToken cancellationToken);
      public Task ContinueWhenAll<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Action<Task<TAntecedentResult>[]> continuationAction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task ContinueWhenAll<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Action<Task<TAntecedentResult>[]> continuationAction, TaskContinuationOptions continuationOptions);
      public Task<TResult> ContinueWhenAll<TResult>(Task[] tasks, Func<Task[], TResult> continuationFunction);
      public Task<TResult> ContinueWhenAll<TResult>(Task[] tasks, Func<Task[], TResult> continuationFunction, CancellationToken cancellationToken);
      public Task<TResult> ContinueWhenAll<TResult>(Task[] tasks, Func<Task[], TResult> continuationFunction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task<TResult> ContinueWhenAll<TResult>(Task[] tasks, Func<Task[], TResult> continuationFunction, TaskContinuationOptions continuationOptions);
      public Task ContinueWhenAny(Task[] tasks, Action<Task> continuationAction);
      public Task ContinueWhenAny(Task[] tasks, Action<Task> continuationAction, CancellationToken cancellationToken);
      public Task ContinueWhenAny(Task[] tasks, Action<Task> continuationAction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task ContinueWhenAny(Task[] tasks, Action<Task> continuationAction, TaskContinuationOptions continuationOptions);
      public Task<TResult> ContinueWhenAny<TAntecedentResult, TResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>, TResult> continuationFunction);
      public Task<TResult> ContinueWhenAny<TAntecedentResult, TResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>, TResult> continuationFunction, CancellationToken cancellationToken);
      public Task<TResult> ContinueWhenAny<TAntecedentResult, TResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>, TResult> continuationFunction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task<TResult> ContinueWhenAny<TAntecedentResult, TResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>, TResult> continuationFunction, TaskContinuationOptions continuationOptions);
      public Task ContinueWhenAny<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Action<Task<TAntecedentResult>> continuationAction);
      public Task ContinueWhenAny<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Action<Task<TAntecedentResult>> continuationAction, CancellationToken cancellationToken);
      public Task ContinueWhenAny<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Action<Task<TAntecedentResult>> continuationAction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task ContinueWhenAny<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Action<Task<TAntecedentResult>> continuationAction, TaskContinuationOptions continuationOptions);
      public Task<TResult> ContinueWhenAny<TResult>(Task[] tasks, Func<Task, TResult> continuationFunction);
      public Task<TResult> ContinueWhenAny<TResult>(Task[] tasks, Func<Task, TResult> continuationFunction, CancellationToken cancellationToken);
      public Task<TResult> ContinueWhenAny<TResult>(Task[] tasks, Func<Task, TResult> continuationFunction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task<TResult> ContinueWhenAny<TResult>(Task[] tasks, Func<Task, TResult> continuationFunction, TaskContinuationOptions continuationOptions);
      public Task FromAsync(Func<AsyncCallback, object?, IAsyncResult> beginMethod, Action<IAsyncResult> endMethod, object? state);
      public Task FromAsync(Func<AsyncCallback, object?, IAsyncResult> beginMethod, Action<IAsyncResult> endMethod, object? state, TaskCreationOptions creationOptions);
      public Task FromAsync(IAsyncResult asyncResult, Action<IAsyncResult> endMethod);
      public Task FromAsync(IAsyncResult asyncResult, Action<IAsyncResult> endMethod, TaskCreationOptions creationOptions);
      public Task FromAsync(IAsyncResult asyncResult, Action<IAsyncResult> endMethod, TaskCreationOptions creationOptions, TaskScheduler scheduler);
      public Task<TResult> FromAsync<TArg1, TArg2, TArg3, TResult>(Func<TArg1, TArg2, TArg3, AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, TArg1 arg1, TArg2 arg2, TArg3 arg3, object? state);
      public Task<TResult> FromAsync<TArg1, TArg2, TArg3, TResult>(Func<TArg1, TArg2, TArg3, AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, TArg1 arg1, TArg2 arg2, TArg3 arg3, object? state, TaskCreationOptions creationOptions);
      public Task FromAsync<TArg1, TArg2, TArg3>(Func<TArg1, TArg2, TArg3, AsyncCallback, object?, IAsyncResult> beginMethod, Action<IAsyncResult> endMethod, TArg1 arg1, TArg2 arg2, TArg3 arg3, object? state);
      public Task FromAsync<TArg1, TArg2, TArg3>(Func<TArg1, TArg2, TArg3, AsyncCallback, object?, IAsyncResult> beginMethod, Action<IAsyncResult> endMethod, TArg1 arg1, TArg2 arg2, TArg3 arg3, object? state, TaskCreationOptions creationOptions);
      public Task<TResult> FromAsync<TArg1, TArg2, TResult>(Func<TArg1, TArg2, AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, TArg1 arg1, TArg2 arg2, object? state);
      public Task<TResult> FromAsync<TArg1, TArg2, TResult>(Func<TArg1, TArg2, AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, TArg1 arg1, TArg2 arg2, object? state, TaskCreationOptions creationOptions);
      public Task FromAsync<TArg1, TArg2>(Func<TArg1, TArg2, AsyncCallback, object?, IAsyncResult> beginMethod, Action<IAsyncResult> endMethod, TArg1 arg1, TArg2 arg2, object? state);
      public Task FromAsync<TArg1, TArg2>(Func<TArg1, TArg2, AsyncCallback, object?, IAsyncResult> beginMethod, Action<IAsyncResult> endMethod, TArg1 arg1, TArg2 arg2, object? state, TaskCreationOptions creationOptions);
      public Task<TResult> FromAsync<TArg1, TResult>(Func<TArg1, AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, TArg1 arg1, object? state);
      public Task<TResult> FromAsync<TArg1, TResult>(Func<TArg1, AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, TArg1 arg1, object? state, TaskCreationOptions creationOptions);
      public Task FromAsync<TArg1>(Func<TArg1, AsyncCallback, object?, IAsyncResult> beginMethod, Action<IAsyncResult> endMethod, TArg1 arg1, object? state);
      public Task FromAsync<TArg1>(Func<TArg1, AsyncCallback, object?, IAsyncResult> beginMethod, Action<IAsyncResult> endMethod, TArg1 arg1, object? state, TaskCreationOptions creationOptions);
      public Task<TResult> FromAsync<TResult>(Func<AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, object? state);
      public Task<TResult> FromAsync<TResult>(Func<AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, object? state, TaskCreationOptions creationOptions);
      public Task<TResult> FromAsync<TResult>(IAsyncResult asyncResult, Func<IAsyncResult, TResult> endMethod);
      public Task<TResult> FromAsync<TResult>(IAsyncResult asyncResult, Func<IAsyncResult, TResult> endMethod, TaskCreationOptions creationOptions);
      public Task<TResult> FromAsync<TResult>(IAsyncResult asyncResult, Func<IAsyncResult, TResult> endMethod, TaskCreationOptions creationOptions, TaskScheduler scheduler);
      public Task StartNew(Action action);
      public Task StartNew(Action action, CancellationToken cancellationToken);
      public Task StartNew(Action action, CancellationToken cancellationToken, TaskCreationOptions creationOptions, TaskScheduler scheduler);
      public Task StartNew(Action action, TaskCreationOptions creationOptions);
      public Task StartNew(Action<object?> action, object? state);
      public Task StartNew(Action<object?> action, object? state, CancellationToken cancellationToken);
      public Task StartNew(Action<object?> action, object? state, CancellationToken cancellationToken, TaskCreationOptions creationOptions, TaskScheduler scheduler);
      public Task StartNew(Action<object?> action, object? state, TaskCreationOptions creationOptions);
      public Task<TResult> StartNew<TResult>(Func<object?, TResult> function, object? state);
      public Task<TResult> StartNew<TResult>(Func<object?, TResult> function, object? state, CancellationToken cancellationToken);
      public Task<TResult> StartNew<TResult>(Func<object?, TResult> function, object? state, CancellationToken cancellationToken, TaskCreationOptions creationOptions, TaskScheduler scheduler);
      public Task<TResult> StartNew<TResult>(Func<object?, TResult> function, object? state, TaskCreationOptions creationOptions);
      public Task<TResult> StartNew<TResult>(Func<TResult> function);
      public Task<TResult> StartNew<TResult>(Func<TResult> function, CancellationToken cancellationToken);
      public Task<TResult> StartNew<TResult>(Func<TResult> function, CancellationToken cancellationToken, TaskCreationOptions creationOptions, TaskScheduler scheduler);
      public Task<TResult> StartNew<TResult>(Func<TResult> function, TaskCreationOptions creationOptions);
    }
    public class TaskFactory<TResult> {
      public TaskFactory();
      public TaskFactory(CancellationToken cancellationToken);
      public TaskFactory(CancellationToken cancellationToken, TaskCreationOptions creationOptions, TaskContinuationOptions continuationOptions, TaskScheduler? scheduler);
      public TaskFactory(TaskCreationOptions creationOptions, TaskContinuationOptions continuationOptions);
      public TaskFactory(TaskScheduler? scheduler);
      public CancellationToken CancellationToken { get; }
      public TaskContinuationOptions ContinuationOptions { get; }
      public TaskCreationOptions CreationOptions { get; }
      public TaskScheduler? Scheduler { get; }
      public Task<TResult> ContinueWhenAll(Task[] tasks, Func<Task[], TResult> continuationFunction);
      public Task<TResult> ContinueWhenAll(Task[] tasks, Func<Task[], TResult> continuationFunction, CancellationToken cancellationToken);
      public Task<TResult> ContinueWhenAll(Task[] tasks, Func<Task[], TResult> continuationFunction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task<TResult> ContinueWhenAll(Task[] tasks, Func<Task[], TResult> continuationFunction, TaskContinuationOptions continuationOptions);
      public Task<TResult> ContinueWhenAll<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>[], TResult> continuationFunction);
      public Task<TResult> ContinueWhenAll<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>[], TResult> continuationFunction, CancellationToken cancellationToken);
      public Task<TResult> ContinueWhenAll<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>[], TResult> continuationFunction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task<TResult> ContinueWhenAll<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>[], TResult> continuationFunction, TaskContinuationOptions continuationOptions);
      public Task<TResult> ContinueWhenAny(Task[] tasks, Func<Task, TResult> continuationFunction);
      public Task<TResult> ContinueWhenAny(Task[] tasks, Func<Task, TResult> continuationFunction, CancellationToken cancellationToken);
      public Task<TResult> ContinueWhenAny(Task[] tasks, Func<Task, TResult> continuationFunction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task<TResult> ContinueWhenAny(Task[] tasks, Func<Task, TResult> continuationFunction, TaskContinuationOptions continuationOptions);
      public Task<TResult> ContinueWhenAny<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>, TResult> continuationFunction);
      public Task<TResult> ContinueWhenAny<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>, TResult> continuationFunction, CancellationToken cancellationToken);
      public Task<TResult> ContinueWhenAny<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>, TResult> continuationFunction, CancellationToken cancellationToken, TaskContinuationOptions continuationOptions, TaskScheduler scheduler);
      public Task<TResult> ContinueWhenAny<TAntecedentResult>(Task<TAntecedentResult>[] tasks, Func<Task<TAntecedentResult>, TResult> continuationFunction, TaskContinuationOptions continuationOptions);
      public Task<TResult> FromAsync(Func<AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, object? state);
      public Task<TResult> FromAsync(Func<AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, object? state, TaskCreationOptions creationOptions);
      public Task<TResult> FromAsync(IAsyncResult asyncResult, Func<IAsyncResult, TResult> endMethod);
      public Task<TResult> FromAsync(IAsyncResult asyncResult, Func<IAsyncResult, TResult> endMethod, TaskCreationOptions creationOptions);
      public Task<TResult> FromAsync(IAsyncResult asyncResult, Func<IAsyncResult, TResult> endMethod, TaskCreationOptions creationOptions, TaskScheduler scheduler);
      public Task<TResult> FromAsync<TArg1, TArg2, TArg3>(Func<TArg1, TArg2, TArg3, AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, TArg1 arg1, TArg2 arg2, TArg3 arg3, object? state);
      public Task<TResult> FromAsync<TArg1, TArg2, TArg3>(Func<TArg1, TArg2, TArg3, AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, TArg1 arg1, TArg2 arg2, TArg3 arg3, object? state, TaskCreationOptions creationOptions);
      public Task<TResult> FromAsync<TArg1, TArg2>(Func<TArg1, TArg2, AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, TArg1 arg1, TArg2 arg2, object? state);
      public Task<TResult> FromAsync<TArg1, TArg2>(Func<TArg1, TArg2, AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, TArg1 arg1, TArg2 arg2, object? state, TaskCreationOptions creationOptions);
      public Task<TResult> FromAsync<TArg1>(Func<TArg1, AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, TArg1 arg1, object? state);
      public Task<TResult> FromAsync<TArg1>(Func<TArg1, AsyncCallback, object?, IAsyncResult> beginMethod, Func<IAsyncResult, TResult> endMethod, TArg1 arg1, object? state, TaskCreationOptions creationOptions);
      public Task<TResult> StartNew(Func<object?, TResult> function, object? state);
      public Task<TResult> StartNew(Func<object?, TResult> function, object? state, CancellationToken cancellationToken);
      public Task<TResult> StartNew(Func<object?, TResult> function, object? state, CancellationToken cancellationToken, TaskCreationOptions creationOptions, TaskScheduler scheduler);
      public Task<TResult> StartNew(Func<object?, TResult> function, object? state, TaskCreationOptions creationOptions);
      public Task<TResult> StartNew(Func<TResult> function);
      public Task<TResult> StartNew(Func<TResult> function, CancellationToken cancellationToken);
      public Task<TResult> StartNew(Func<TResult> function, CancellationToken cancellationToken, TaskCreationOptions creationOptions, TaskScheduler scheduler);
      public Task<TResult> StartNew(Func<TResult> function, TaskCreationOptions creationOptions);
    }
    public abstract class TaskScheduler {
      protected TaskScheduler();
      public static TaskScheduler Current { get; }
      public static TaskScheduler Default { get; }
      public int Id { get; }
      public virtual int MaximumConcurrencyLevel { get; }
      public static event EventHandler<UnobservedTaskExceptionEventArgs> UnobservedTaskException;
      public static TaskScheduler FromCurrentSynchronizationContext();
      protected abstract IEnumerable<Task>? GetScheduledTasks();
      protected internal abstract void QueueTask(Task task);
      protected internal virtual bool TryDequeue(Task task);
      protected bool TryExecuteTask(Task task);
      protected abstract bool TryExecuteTaskInline(Task task, bool taskWasPreviouslyQueued);
    }
    public enum TaskStatus {
      Canceled = 6,
      Created = 0,
      Faulted = 7,
      RanToCompletion = 5,
      Running = 3,
      WaitingForActivation = 1,
      WaitingForChildrenToComplete = 4,
      WaitingToRun = 2,
    }
    public class UnobservedTaskExceptionEventArgs : EventArgs {
      public UnobservedTaskExceptionEventArgs(AggregateException? exception);
      public AggregateException? Exception { get; }
      public bool Observed { get; }
      public void SetObserved();
    }
    public readonly struct ValueTask : IEquatable<ValueTask> {
      public ValueTask(IValueTaskSource source, short token);
      public ValueTask(Task task);
      public bool IsCanceled { get; }
      public bool IsCompleted { get; }
      public bool IsCompletedSuccessfully { get; }
      public bool IsFaulted { get; }
      public Task AsTask();
      public ConfiguredValueTaskAwaitable ConfigureAwait(bool continueOnCapturedContext);
      public override bool Equals(object? obj);
      public bool Equals(ValueTask other);
      public ValueTaskAwaiter GetAwaiter();
      public override int GetHashCode();
      public static bool operator ==(ValueTask left, ValueTask right);
      public static bool operator !=(ValueTask left, ValueTask right);
      public ValueTask Preserve();
    }
    public readonly struct ValueTask<TResult> : IEquatable<ValueTask<TResult>> {
      public ValueTask(IValueTaskSource<TResult> source, short token);
      public ValueTask(Task<TResult> task);
      public ValueTask(TResult result);
      public bool IsCanceled { get; }
      public bool IsCompleted { get; }
      public bool IsCompletedSuccessfully { get; }
      public bool IsFaulted { get; }
      public TResult Result { get; }
      public Task<TResult> AsTask();
      public ConfiguredValueTaskAwaitable<TResult> ConfigureAwait(bool continueOnCapturedContext);
      public override bool Equals(object? obj);
      public bool Equals(ValueTask<TResult> other);
      public ValueTaskAwaiter<TResult> GetAwaiter();
      public override int GetHashCode();
      public static bool operator ==(ValueTask<TResult> left, ValueTask<TResult> right);
      public static bool operator !=(ValueTask<TResult> left, ValueTask<TResult> right);
      public ValueTask<TResult> Preserve();
      public override string? ToString();
    }
  }
  namespace System.Threading.Tasks.Sources {
    public interface IValueTaskSource {
      void GetResult(short token);
      ValueTaskSourceStatus GetStatus(short token);
      void OnCompleted(Action<object?> continuation, object? state, short token, ValueTaskSourceOnCompletedFlags flags);
    }
    public interface IValueTaskSource<out TResult> {
      TResult GetResult(short token);
      ValueTaskSourceStatus GetStatus(short token);
      void OnCompleted(Action<object?> continuation, object? state, short token, ValueTaskSourceOnCompletedFlags flags);
    }
    public struct ManualResetValueTaskSourceCore<TResult> {
      public bool RunContinuationsAsynchronously { get; set; }
      public short Version { get; }
      public TResult GetResult(short token);
      public ValueTaskSourceStatus GetStatus(short token);
      public void OnCompleted(Action<object?> continuation, object? state, short token, ValueTaskSourceOnCompletedFlags flags);
      public void Reset();
      public void SetException(Exception error);
      public void SetResult(TResult result);
    }
    public enum ValueTaskSourceOnCompletedFlags {
      FlowExecutionContext = 2,
      None = 0,
      UseSchedulingContext = 1,
    }
    public enum ValueTaskSourceStatus {
      Canceled = 3,
      Faulted = 2,
      Pending = 0,
      Succeeded = 1,
    }
  }
 }
```