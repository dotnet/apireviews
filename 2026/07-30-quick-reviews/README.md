# API Review 07/30/2026

## Mark Vector-related APIs as caller-unsafe

**Approved** | [#runtime/128075](https://github.com/dotnet/runtime/issues/128075#issuecomment-5134112396) | [Video](https://www.youtube.com/watch?v=2DLMO_4Ms3k&t=0h0m0s)

Looks good as proposed

```diff
namespace System.Runtime.Intrinsics

public static partial class Vector64
{
+   /// <safety>Initializes only the lowest element and leaves the remaining elements uninitialized; reading them observes undefined data.</safety>
+   unsafe
    public static Vector64<byte> CreateScalarUnsafe(byte value)
+   unsafe
    public static Vector64<double> CreateScalarUnsafe(double value)
+   unsafe
    public static Vector64<short> CreateScalarUnsafe(short value)
+   unsafe
    public static Vector64<int> CreateScalarUnsafe(int value)
+   unsafe
    public static Vector64<long> CreateScalarUnsafe(long value)
+   unsafe
    public static Vector64<nint> CreateScalarUnsafe(nint value)
+   unsafe
    public static Vector64<nuint> CreateScalarUnsafe(nuint value)
+   unsafe
    public static Vector64<sbyte> CreateScalarUnsafe(sbyte value)
+   unsafe
    public static Vector64<float> CreateScalarUnsafe(float value)
+   unsafe
    public static Vector64<ushort> CreateScalarUnsafe(ushort value)
+   unsafe
    public static Vector64<uint> CreateScalarUnsafe(uint value)
+   unsafe
    public static Vector64<ulong> CreateScalarUnsafe(ulong value)
+   unsafe
    public static Vector64<T> CreateScalarUnsafe<T>(T value)

+   /// <safety>Loads a full vector starting at the given reference with no bounds check; may read past the end of the source.</safety>
+   unsafe
    public static Vector64<T> LoadUnsafe<T>(ref readonly T source)
+   unsafe
    public static Vector64<T> LoadUnsafe<T>(ref readonly T source, nuint elementOffset)

+   /// <safety>Stores a full vector starting at the given reference with no bounds check; may write past the end of the destination.</safety>
+   unsafe
    public static void StoreUnsafe<T>(this Vector64<T> source, ref T destination)
+   unsafe
    public static void StoreUnsafe<T>(this Vector64<T> source, ref T destination, nuint elementOffset)

+   /// <safety>Widens to a larger vector, leaving the upper elements uninitialized.</safety>
+   unsafe
    public static Vector128<T> ToVector128Unsafe<T>(this Vector64<T> vector)
}

public static partial class Vector128
{
+   /// <safety>Reinterprets the value as a 128-bit vector, leaving the upper elements uninitialized.</safety>
+   unsafe
    public static Vector128<float> AsVector128Unsafe(this System.Numerics.Vector2 value)
+   unsafe
    public static Vector128<float> AsVector128Unsafe(this System.Numerics.Vector3 value)

+   /// <safety>Initializes only the lowest element and leaves the remaining elements uninitialized; reading them observes undefined data.</safety>
+   unsafe
    public static Vector128<byte> CreateScalarUnsafe(byte value)
+   unsafe
    public static Vector128<double> CreateScalarUnsafe(double value)
+   unsafe
    public static Vector128<short> CreateScalarUnsafe(short value)
+   unsafe
    public static Vector128<int> CreateScalarUnsafe(int value)
+   unsafe
    public static Vector128<long> CreateScalarUnsafe(long value)
+   unsafe
    public static Vector128<nint> CreateScalarUnsafe(nint value)
+   unsafe
    public static Vector128<nuint> CreateScalarUnsafe(nuint value)
+   unsafe
    public static Vector128<sbyte> CreateScalarUnsafe(sbyte value)
+   unsafe
    public static Vector128<float> CreateScalarUnsafe(float value)
+   unsafe
    public static Vector128<ushort> CreateScalarUnsafe(ushort value)
+   unsafe
    public static Vector128<uint> CreateScalarUnsafe(uint value)
+   unsafe
    public static Vector128<ulong> CreateScalarUnsafe(ulong value)
+   unsafe
    public static Vector128<T> CreateScalarUnsafe<T>(T value)

+   /// <safety>Loads a full vector starting at the given reference with no bounds check; may read past the end of the source.</safety>
+   unsafe
    public static Vector128<T> LoadUnsafe<T>(ref readonly T source)
+   unsafe
    public static Vector128<T> LoadUnsafe<T>(ref readonly T source, nuint elementOffset)

+   /// <safety>Stores a full vector starting at the given reference with no bounds check; may write past the end of the destination.</safety>
+   unsafe
    public static void StoreUnsafe<T>(this Vector128<T> source, ref T destination)
+   unsafe
    public static void StoreUnsafe<T>(this Vector128<T> source, ref T destination, nuint elementOffset)

+   /// <safety>Widens to a larger vector, leaving the upper elements uninitialized.</safety>
+   unsafe
    public static Vector256<T> ToVector256Unsafe<T>(this Vector128<T> vector)
}

public static partial class Vector256
{
+   /// <safety>Initializes only the lowest element and leaves the remaining elements uninitialized; reading them observes undefined data.</safety>
+   unsafe
    public static Vector256<byte> CreateScalarUnsafe(byte value)
+   unsafe
    public static Vector256<double> CreateScalarUnsafe(double value)
+   unsafe
    public static Vector256<short> CreateScalarUnsafe(short value)
+   unsafe
    public static Vector256<int> CreateScalarUnsafe(int value)
+   unsafe
    public static Vector256<long> CreateScalarUnsafe(long value)
+   unsafe
    public static Vector256<nint> CreateScalarUnsafe(nint value)
+   unsafe
    public static Vector256<nuint> CreateScalarUnsafe(nuint value)
+   unsafe
    public static Vector256<sbyte> CreateScalarUnsafe(sbyte value)
+   unsafe
    public static Vector256<float> CreateScalarUnsafe(float value)
+   unsafe
    public static Vector256<ushort> CreateScalarUnsafe(ushort value)
+   unsafe
    public static Vector256<uint> CreateScalarUnsafe(uint value)
+   unsafe
    public static Vector256<ulong> CreateScalarUnsafe(ulong value)
+   unsafe
    public static Vector256<T> CreateScalarUnsafe<T>(T value)

+   /// <safety>Loads a full vector starting at the given reference with no bounds check; may read past the end of the source.</safety>
+   unsafe
    public static Vector256<T> LoadUnsafe<T>(ref readonly T source)
+   unsafe
    public static Vector256<T> LoadUnsafe<T>(ref readonly T source, nuint elementOffset)

+   /// <safety>Stores a full vector starting at the given reference with no bounds check; may write past the end of the destination.</safety>
+   unsafe
    public static void StoreUnsafe<T>(this Vector256<T> source, ref T destination)
+   unsafe
    public static void StoreUnsafe<T>(this Vector256<T> source, ref T destination, nuint elementOffset)

+   /// <safety>Widens to a larger vector, leaving the upper elements uninitialized.</safety>
+   unsafe
    public static Vector512<T> ToVector512Unsafe<T>(this Vector256<T> vector)
}

public static partial class Vector512
{
+   /// <safety>Initializes only the lowest element and leaves the remaining elements uninitialized; reading them observes undefined data.</safety>
+   unsafe
    public static Vector512<byte> CreateScalarUnsafe(byte value)
+   unsafe
    public static Vector512<double> CreateScalarUnsafe(double value)
+   unsafe
    public static Vector512<short> CreateScalarUnsafe(short value)
+   unsafe
    public static Vector512<int> CreateScalarUnsafe(int value)
+   unsafe
    public static Vector512<long> CreateScalarUnsafe(long value)
+   unsafe
    public static Vector512<nint> CreateScalarUnsafe(nint value)
+   unsafe
    public static Vector512<nuint> CreateScalarUnsafe(nuint value)
+   unsafe
    public static Vector512<sbyte> CreateScalarUnsafe(sbyte value)
+   unsafe
    public static Vector512<float> CreateScalarUnsafe(float value)
+   unsafe
    public static Vector512<ushort> CreateScalarUnsafe(ushort value)
+   unsafe
    public static Vector512<uint> CreateScalarUnsafe(uint value)
+   unsafe
    public static Vector512<ulong> CreateScalarUnsafe(ulong value)
+   unsafe
    public static Vector512<T> CreateScalarUnsafe<T>(T value)

+   /// <safety>Loads a full vector starting at the given reference with no bounds check; may read past the end of the source.</safety>
+   unsafe
    public static Vector512<T> LoadUnsafe<T>(ref readonly T source)
+   unsafe
    public static Vector512<T> LoadUnsafe<T>(ref readonly T source, nuint elementOffset)

+   /// <safety>Stores a full vector starting at the given reference with no bounds check; may write past the end of the destination.</safety>
+   unsafe
    public static void StoreUnsafe<T>(this Vector512<T> source, ref T destination)
+   unsafe
    public static void StoreUnsafe<T>(this Vector512<T> source, ref T destination, nuint elementOffset)
}
```

```diff
namespace System.Numerics

public static partial class Vector
{
+   /// <safety>Widens to a larger vector, leaving the upper elements uninitialized.</safety>
+   unsafe
    public static Vector3 AsVector3Unsafe(this Vector2 value)
+   unsafe
    public static Vector4 AsVector4Unsafe(this Vector2 value)
+   unsafe
    public static Vector4 AsVector4Unsafe(this Vector3 value)

+   /// <safety>Initializes only the lowest element and leaves the remaining elements uninitialized; reading them observes undefined data.</safety>
+   unsafe
    public static Vector<T> CreateScalarUnsafe<T>(T value)

+   /// <safety>Loads a full vector starting at the given reference with no bounds check; may read past the end of the source.</safety>
+   unsafe
    public static Vector<T> LoadUnsafe<T>(ref readonly T source)
+   unsafe
    public static Vector<T> LoadUnsafe<T>(ref readonly T source, nuint elementOffset)

+   /// <safety>Stores a full vector starting at the given reference with no bounds check; may write past the end of the destination.</safety>
+   unsafe
    public static void StoreUnsafe<T>(this Vector<T> source, ref T destination)
+   unsafe
    public static void StoreUnsafe(this Vector2 source, ref float destination)
+   unsafe
    public static void StoreUnsafe(this Vector3 source, ref float destination)
+   unsafe
    public static void StoreUnsafe(this Vector4 source, ref float destination)
+   unsafe
    public static void StoreUnsafe<T>(this Vector<T> source, ref T destination, nuint elementOffset)
+   unsafe
    public static void StoreUnsafe(this Vector2 source, ref float destination, nuint elementOffset)
+   unsafe
    public static void StoreUnsafe(this Vector3 source, ref float destination, nuint elementOffset)
+   unsafe
    public static void StoreUnsafe(this Vector4 source, ref float destination, nuint elementOffset)
}

public partial struct Vector2
{
+   /// <safety>Initializes only the lowest element and leaves the remaining elements uninitialized; reading them observes undefined data.</safety>
+   unsafe
    public static Vector2 CreateScalarUnsafe(float x)

+   /// <safety>Loads a full vector starting at the given reference with no bounds check; may read past the end of the source.</safety>
+   unsafe
    public static Vector2 LoadUnsafe(ref readonly float source)
+   unsafe
    public static Vector2 LoadUnsafe(ref readonly float source, nuint elementOffset)
}

public partial struct Vector3
{
+   /// <safety>Initializes only the lowest element and leaves the remaining elements uninitialized; reading them observes undefined data.</safety>
+   unsafe
    public static Vector3 CreateScalarUnsafe(float x)

+   /// <safety>Loads a full vector starting at the given reference with no bounds check; may read past the end of the source.</safety>
+   unsafe
    public static Vector3 LoadUnsafe(ref readonly float source)
+   unsafe
    public static Vector3 LoadUnsafe(ref readonly float source, nuint elementOffset)
}

public partial struct Vector4
{
+   /// <safety>Initializes only the lowest element and leaves the remaining elements uninitialized; reading them observes undefined data.</safety>
+   unsafe
    public static Vector4 CreateScalarUnsafe(float x)

+   /// <safety>Loads a full vector starting at the given reference with no bounds check; may read past the end of the source.</safety>
+   unsafe
    public static Vector4 LoadUnsafe(ref readonly float source)
+   unsafe
    public static Vector4 LoadUnsafe(ref readonly float source, nuint elementOffset)
}
```
## Mark System.Runtime.InteropServices APIs as caller-unsafe

**Approved** | [#runtime/129750](https://github.com/dotnet/runtime/issues/129750#issuecomment-5134322013) | [Video](https://www.youtube.com/watch?v=2DLMO_4Ms3k&t=0h6m40s)

* For the IDisposable GCHandle types, consider explicitly implementing IDisposable.Dispose and making public void Dispose be unsafe, so the alloc (ctor) and free (Dispose) are both unsafe (assuming a strong typeref).
  * Especially because value-copied (or box-copied) versions of the GCHandle type can cause a double free.
* Otherwise, looks good as proposed.

```diff
namespace System.Runtime.InteropServices.Marshalling
{
    public partial class StrategyBasedComWrappers : System.Runtime.InteropServices.ComWrappers
    {
        protected sealed override object CreateObject(nint externalComObject, System.Runtime.InteropServices.CreateObjectFlags flags);

+       unsafe
        protected sealed override object? CreateObject(nint externalComObject, System.Runtime.InteropServices.CreateObjectFlags flags, object? userState, out System.Runtime.InteropServices.CreatedWrapperFlags wrapperFlags);
}

namespace System.IO
{
    public partial class UnmanagedMemoryAccessor : System.IDisposable
    {
+       unsafe
        public int ReadArray<T>(long position, T[] array, int offset, int count) where T : struct;

+       unsafe
        public void Read<T>(long position, out T structure) where T : struct;

+       unsafe
        public void WriteArray<T>(long position, T[] array, int offset, int count) where T : struct;

+       unsafe
        public void Write<T>(long position, ref T structure) where T : struct;
    }
}

namespace System.Runtime.InteropServices
{
    public static partial class Marshal
    {
+       unsafe
        public static int AddRef(System.IntPtr pUnk);

+       unsafe
        public static System.IntPtr CreateAggregatedObject(System.IntPtr pOuter, object o);

+       unsafe
        public static System.IntPtr CreateAggregatedObject<T>(System.IntPtr pOuter, T o) where T : notnull;

+       unsafe
        public static void Copy(byte[] source, int startIndex, System.IntPtr destination, int length);

+       unsafe
        public static void Copy(char[] source, int startIndex, System.IntPtr destination, int length);

+       unsafe
        public static void Copy(double[] source, int startIndex, System.IntPtr destination, int length);

+       unsafe
        public static void Copy(short[] source, int startIndex, System.IntPtr destination, int length);

+       unsafe
        public static void Copy(int[] source, int startIndex, System.IntPtr destination, int length);

+       unsafe
        public static void Copy(long[] source, int startIndex, System.IntPtr destination, int length);

+       unsafe
        public static void Copy(System.IntPtr source, byte[] destination, int startIndex, int length);

+       unsafe
        public static void Copy(System.IntPtr source, char[] destination, int startIndex, int length);

+       unsafe
        public static void Copy(System.IntPtr source, double[] destination, int startIndex, int length);

+       unsafe
        public static void Copy(System.IntPtr source, short[] destination, int startIndex, int length);

+       unsafe
        public static void Copy(System.IntPtr source, int[] destination, int startIndex, int length);

+       unsafe
        public static void Copy(System.IntPtr source, long[] destination, int startIndex, int length);

+       unsafe
        public static void Copy(System.IntPtr source, System.IntPtr[] destination, int startIndex, int length);

+       unsafe
        public static void Copy(System.IntPtr source, float[] destination, int startIndex, int length);

+       unsafe
        public static void Copy(System.IntPtr[] source, int startIndex, System.IntPtr destination, int length);

+       unsafe
        public static void Copy(float[] source, int startIndex, System.IntPtr destination, int length);

+       unsafe
        public static void DestroyStructure(System.IntPtr ptr, System.Type structuretype);

+       unsafe
        public static void DestroyStructure<T>(System.IntPtr ptr);

+       unsafe
        public static void FreeBSTR(System.IntPtr ptr);

+       unsafe
        public static void FreeCoTaskMem(System.IntPtr ptr);

+       unsafe
        public static void FreeHGlobal(System.IntPtr hglobal);

+       unsafe
        public static System.Delegate GetDelegateForFunctionPointer(System.IntPtr ptr, System.Type t);

+       unsafe
        public static TDelegate GetDelegateForFunctionPointer<TDelegate>(System.IntPtr ptr);

+       unsafe
        public static System.Exception? GetExceptionForHR(int errorCode, System.IntPtr errorInfo);

+       unsafe
        public static System.Exception? GetExceptionForHR(int errorCode, in System.Guid iid, System.IntPtr pUnk);

+       unsafe
        public static void GetNativeVariantForObject(object? obj, System.IntPtr pDstNativeVariant);

+       unsafe
        public static void GetNativeVariantForObject<T>(T? obj, System.IntPtr pDstNativeVariant);

+       unsafe
        public static object GetObjectForIUnknown(System.IntPtr pUnk);

+       unsafe
        public static object? GetObjectForNativeVariant(System.IntPtr pSrcNativeVariant);

+       unsafe
        public static T? GetObjectForNativeVariant<T>(System.IntPtr pSrcNativeVariant);

+       unsafe
        public static object?[] GetObjectsForNativeVariants(System.IntPtr aSrcNativeVariant, int cVars);

+       unsafe
        public static T[] GetObjectsForNativeVariants<T>(System.IntPtr aSrcNativeVariant, int cVars);

+       unsafe
        public static object GetTypedObjectForIUnknown(System.IntPtr pUnk, System.Type t);

+       unsafe
        public static object GetUniqueObjectForIUnknown(System.IntPtr unknown);

+       unsafe
        public static string? PtrToStringAnsi(System.IntPtr ptr);

+       unsafe
        public static string PtrToStringAnsi(System.IntPtr ptr, int len);

+       unsafe
        public static string? PtrToStringAuto(System.IntPtr ptr);

+       unsafe
        public static string? PtrToStringAuto(System.IntPtr ptr, int len);

+       unsafe
        public static string PtrToStringBSTR(System.IntPtr ptr);

+       unsafe
        public static string? PtrToStringUni(System.IntPtr ptr);

+       unsafe
        public static string PtrToStringUni(System.IntPtr ptr, int len);

+       unsafe
        public static string? PtrToStringUTF8(System.IntPtr ptr);

+       unsafe
        public static string PtrToStringUTF8(System.IntPtr ptr, int byteLen);

+       unsafe
        public static void PtrToStructure(System.IntPtr ptr, object structure);

+       unsafe
        public static object? PtrToStructure(System.IntPtr ptr, System.Type structureType);

+       unsafe
        public static T? PtrToStructure<T>(System.IntPtr ptr);

+       unsafe
        public static void PtrToStructure<T>(System.IntPtr ptr, T structure);

+       unsafe
        public static int QueryInterface(System.IntPtr pUnk, in System.Guid iid, out System.IntPtr ppv);

+       unsafe
        public static byte ReadByte(System.IntPtr ptr);

+       unsafe
        public static byte ReadByte(System.IntPtr ptr, int ofs);

+       unsafe
        public static byte ReadByte(object ptr, int ofs);

+       unsafe
        public static short ReadInt16(System.IntPtr ptr);

+       unsafe
        public static short ReadInt16(System.IntPtr ptr, int ofs);

+       unsafe
        public static short ReadInt16(object ptr, int ofs);

+       unsafe
        public static int ReadInt32(System.IntPtr ptr);

+       unsafe
        public static int ReadInt32(System.IntPtr ptr, int ofs);

+       unsafe
        public static int ReadInt32(object ptr, int ofs);

+       unsafe
        public static long ReadInt64(System.IntPtr ptr);

+       unsafe
        public static long ReadInt64(System.IntPtr ptr, int ofs);

+       unsafe
        public static long ReadInt64(object ptr, int ofs);

+       unsafe
        public static System.IntPtr ReadIntPtr(System.IntPtr ptr);

+       unsafe
        public static System.IntPtr ReadIntPtr(System.IntPtr ptr, int ofs);

+       unsafe
        public static System.IntPtr ReadIntPtr(object ptr, int ofs);

+       unsafe
        public static System.IntPtr ReAllocCoTaskMem(System.IntPtr pv, int cb);

+       unsafe
        public static System.IntPtr ReAllocHGlobal(System.IntPtr pv, System.IntPtr cb);

+       unsafe
        public static int Release(System.IntPtr pUnk);

+       unsafe
        public static void StructureToPtr(object structure, System.IntPtr ptr, bool fDeleteOld);

+       unsafe
        public static void StructureToPtr<T>(T structure, System.IntPtr ptr, bool fDeleteOld);

+       unsafe
        public static void ThrowExceptionForHR(int errorCode, System.IntPtr errorInfo);

+       unsafe
        public static void ThrowExceptionForHR(int errorCode, in System.Guid iid, System.IntPtr pUnk);

+       unsafe
        public static void WriteByte(System.IntPtr ptr, byte val);

+       unsafe
        public static void WriteByte(System.IntPtr ptr, int ofs, byte val);

+       unsafe
        public static void WriteByte(object ptr, int ofs, byte val);

+       unsafe
        public static void WriteInt16(System.IntPtr ptr, char val);

+       unsafe
        public static void WriteInt16(System.IntPtr ptr, short val);

+       unsafe
        public static void WriteInt16(System.IntPtr ptr, int ofs, char val);

+       unsafe
        public static void WriteInt16(System.IntPtr ptr, int ofs, short val);

+       unsafe
        public static void WriteInt16(object ptr, int ofs, char val);

+       unsafe
        public static void WriteInt16(object ptr, int ofs, short val);

+       unsafe
        public static void WriteInt32(System.IntPtr ptr, int val);

+       unsafe
        public static void WriteInt32(System.IntPtr ptr, int ofs, int val);

+       unsafe
        public static void WriteInt32(object ptr, int ofs, int val);

+       unsafe
        public static void WriteInt64(System.IntPtr ptr, int ofs, long val);

+       unsafe
        public static void WriteInt64(System.IntPtr ptr, long val);

+       unsafe
        public static void WriteInt64(object ptr, int ofs, long val);

+       unsafe
        public static void WriteIntPtr(System.IntPtr ptr, int ofs, System.IntPtr val);

+       unsafe
        public static void WriteIntPtr(System.IntPtr ptr, System.IntPtr val);

+       unsafe
        public static void WriteIntPtr(object ptr, int ofs, System.IntPtr val);

+       unsafe
        public static void ZeroFreeBSTR(System.IntPtr s);

+       unsafe
        public static void ZeroFreeCoTaskMemAnsi(System.IntPtr s);

+       unsafe
        public static void ZeroFreeCoTaskMemUnicode(System.IntPtr s);

+       unsafe
        public static void ZeroFreeCoTaskMemUTF8(System.IntPtr s);

+       unsafe
        public static void ZeroFreeGlobalAllocAnsi(System.IntPtr s);

+       unsafe
        public static void ZeroFreeGlobalAllocUnicode(System.IntPtr s);
    }

    public abstract class SafeBuffer
    {
+       unsafe
        public T Read<T>(ulong byteOffset) where T : struct;

+       unsafe
        public void ReadArray<T>(ulong byteOffset, T[] array, int index, int count) where T : struct;

+       unsafe
        public void ReadSpan<T>(ulong byteOffset, System.Span<T> buffer) where T : struct;

+       unsafe
        public void Write<T>(ulong byteOffset, T value) where T : struct;

+       unsafe
        public void WriteArray<T>(ulong byteOffset, T[] array, int index, int count) where T : struct;

+       unsafe
        public void WriteSpan<T>(ulong byteOffset, System.ReadOnlySpan<T> data) where T : struct;
    }

    public static partial class SequenceMarshal
    {
+       unsafe
        public static bool TryRead<T>(ref System.Buffers.SequenceReader<byte> reader, out T value) where T : unmanaged;
    }

    public struct GCHandle
    {
+       unsafe
        public static GCHandle FromIntPtr(System.IntPtr value);

+       unsafe
        public static explicit operator GCHandle(System.IntPtr value);

+       unsafe
        public void Free();
    }

    public struct GCHandle<T> where T : class?
    {
+       unsafe
        public GCHandle(T target);

+       unsafe
        public static GCHandle<T> FromIntPtr(System.IntPtr value);
    }

    public struct PinnedGCHandle<T> where T : class?
    {
+       unsafe
        public PinnedGCHandle(T target);

+       unsafe
        public static PinnedGCHandle<T> FromIntPtr(System.IntPtr value);

+       unsafe
        public T Target { readonly get; set; }
    }

    public struct WeakGCHandle<T> where T : class?
    {
+       unsafe
        public WeakGCHandle(T target, bool trackResurrection = false);

+       unsafe
        public static WeakGCHandle<T> FromIntPtr(System.IntPtr value);

+       unsafe
        public readonly void SetTarget(T target);
    }

    public abstract class ComWrappers
    {
+       unsafe
        public object GetOrCreateObjectForComInstance(System.IntPtr externalComObject, System.Runtime.InteropServices.CreateObjectFlags flags);

+       unsafe
        public object GetOrCreateObjectForComInstance(System.IntPtr externalComObject, System.Runtime.InteropServices.CreateObjectFlags flags, object? userState);

+       unsafe
        protected abstract object? CreateObject(System.IntPtr externalComObject, System.Runtime.InteropServices.CreateObjectFlags flags);

+       unsafe
        protected virtual object? CreateObject(System.IntPtr externalComObject, System.Runtime.InteropServices.CreateObjectFlags flags, object? userState, out System.Runtime.InteropServices.CreatedWrapperFlags wrapperFlags);

+       unsafe
        public object GetOrRegisterObjectForComInstance(System.IntPtr externalComObject, System.Runtime.InteropServices.CreateObjectFlags flags, object wrapper);

+       unsafe
        public object GetOrRegisterObjectForComInstance(System.IntPtr externalComObject, System.Runtime.InteropServices.CreateObjectFlags flags, object wrapper, System.IntPtr inner);
    }

    public partial interface ICustomMarshaler
    {
+       unsafe
        void CleanUpNativeData(System.IntPtr pNativeData);

+       unsafe
        object MarshalNativeToManaged(System.IntPtr pNativeData);
    }

    public static class ObjectiveCMarshal
    {
+       unsafe
        public static void SetMessageSendCallback(System.Runtime.InteropServices.ObjectiveCMarshal.MessageSendFunction msgSendFunction, System.IntPtr func);
    }
}

namespace System.Runtime.InteropServices.Marshalling
{
    public struct ComVariant : System.IDisposable
    {
+       unsafe
        public static System.Runtime.InteropServices.Marshalling.ComVariant CreateRaw<T>(System.Runtime.InteropServices.VarEnum vt, T rawValue) where T : unmanaged;

+       unsafe
        public ref T GetRawDataRef<T>() where T : unmanaged;
    }
}

namespace System.Runtime.InteropServices.ComTypes
{
    public partial interface IEnumConnectionPoints
    {
+       unsafe
        int Next(int celt, System.Runtime.InteropServices.ComTypes.IConnectionPoint[] rgelt, System.IntPtr pceltFetched);
    }

    public partial interface IEnumConnections
    {
+       unsafe
        int Next(int celt, System.Runtime.InteropServices.ComTypes.CONNECTDATA[] rgelt, System.IntPtr pceltFetched);
    }

    public partial interface IEnumMoniker
    {
+       unsafe
        int Next(int celt, System.Runtime.InteropServices.ComTypes.IMoniker[] rgelt, System.IntPtr pceltFetched);
    }

    public partial interface IEnumString
    {
+       unsafe
        int Next(int celt, string[] rgelt, System.IntPtr pceltFetched);
    }

    public partial interface IEnumVARIANT
    {
+       unsafe
        int Next(int celt, object?[] rgVar, System.IntPtr pceltFetched);
    }

    public partial interface IStream
    {
+       unsafe
        void CopyTo(System.Runtime.InteropServices.ComTypes.IStream pstm, long cb, System.IntPtr pcbRead, System.IntPtr pcbWritten);

+       unsafe
        void Read(byte[] pv, int cb, System.IntPtr pcbRead);

+       unsafe
        void Seek(long dlibMove, int dwOrigin, System.IntPtr plibNewPosition);

+       unsafe
        void Write(byte[] pv, int cb, System.IntPtr pcbWritten);
    }

    public partial interface ITypeInfo
    {
+       unsafe
        void GetDllEntry(int memid, System.Runtime.InteropServices.ComTypes.INVOKEKIND invKind, System.IntPtr pBstrDllName, System.IntPtr pBstrName, System.IntPtr pwOrdinal);

+       unsafe
        void Invoke(object pvInstance, int memid, short wFlags, ref System.Runtime.InteropServices.ComTypes.DISPPARAMS pDispParams, System.IntPtr pVarResult, System.IntPtr pExcepInfo, out int puArgErr);

+       unsafe
        void ReleaseFuncDesc(System.IntPtr pFuncDesc);

+       unsafe
        void ReleaseTypeAttr(System.IntPtr pTypeAttr);

+       unsafe
        void ReleaseVarDesc(System.IntPtr pVarDesc);
    }

    public partial interface ITypeInfo2
    {
+       unsafe
        void GetAllCustData(System.IntPtr pCustData);

+       unsafe
        void GetAllFuncCustData(int index, System.IntPtr pCustData);

+       unsafe
        void GetAllImplTypeCustData(int index, System.IntPtr pCustData);

+       unsafe
        void GetAllParamCustData(int indexFunc, int indexParam, System.IntPtr pCustData);

+       unsafe
        void GetAllVarCustData(int index, System.IntPtr pCustData);
    }

    public partial interface ITypeLib
    {
+       unsafe
        void ReleaseTLibAttr(System.IntPtr pTLibAttr);
    }

    public partial interface ITypeLib2
    {
+       unsafe
        void GetAllCustData(System.IntPtr pCustData);

+       unsafe
        void GetLibStatistics(System.IntPtr pcUniqueNames, out int pcchUniqueNames);

+       unsafe
        void ReleaseTLibAttr(System.IntPtr pTLibAttr);
    }
}
```
## Batch #1 of caller-unsafe APIs

**Approved** | [#runtime/129751](https://github.com/dotnet/runtime/issues/129751#issuecomment-5134589466) | [Video](https://www.youtube.com/watch?v=2DLMO_4Ms3k&t=0h28m22s)

* ArrayPool and MemoryPool were removed to make sure we have the proper audience before taking the change.
* SafeHandle itself seems to be missing from the proposal
* The rest look good as proposed

```diff
namespace System;

public static class GC
{
+   /// <safety>Returns a T[] whose elements are not zero-initialized, so reads observe leftover, uninitialized heap bytes.</safety>
+   unsafe
    public static T[] AllocateUninitializedArray<T>(int length, bool pinned = false);
}
```

```diff
namespace System.Runtime.CompilerServices;

public static class RuntimeHelpers
{
+   /// <safety>Reads SizeOf(type) bytes from a caller-supplied reference and boxes them as that type; the bytes may be out of range, uninitialized, or contain a fabricated reference field.</safety>
+   unsafe
    public static object? Box(ref byte target, System.RuntimeTypeHandle type);
}
```

```diff
namespace System.Security.Cryptography;

public sealed partial class DSAOpenSsl : System.Security.Cryptography.DSA
{
+   /// <safety>Wraps a raw, caller-supplied EVP_PKEY* pointer that the crypto stack later dereferences.</safety>
+   unsafe
    public DSAOpenSsl(System.IntPtr handle);
}
```

```diff
namespace System.Security.Cryptography;

public sealed partial class ECDiffieHellmanOpenSsl : System.Security.Cryptography.ECDiffieHellman
{
+   /// <safety>Wraps a raw, caller-supplied EVP_PKEY* pointer that the crypto stack later dereferences.</safety>
+   unsafe
    public ECDiffieHellmanOpenSsl(System.IntPtr handle);
}
```

```diff
namespace System.Security.Cryptography;

public sealed partial class ECDsaOpenSsl : System.Security.Cryptography.ECDsa
{
+   /// <safety>Wraps a raw, caller-supplied EVP_PKEY* pointer that the crypto stack later dereferences.</safety>
+   unsafe
    public ECDsaOpenSsl(System.IntPtr handle);
}
```

```diff
namespace System.Security.Cryptography;

public sealed partial class RSAOpenSsl : System.Security.Cryptography.RSA
{
+   /// <safety>Wraps a raw, caller-supplied EVP_PKEY* pointer that the crypto stack later dereferences.</safety>
+   unsafe
    public RSAOpenSsl(System.IntPtr handle);
}
```

```diff
namespace System.Security.Cryptography.X509Certificates;

public partial class X509Certificate : System.IDisposable, System.Runtime.Serialization.IDeserializationCallback, System.Runtime.Serialization.ISerializable
{
+   /// <safety>Wraps a raw, caller-supplied PCCERT_CONTEXT pointer that the certificate stack later dereferences.</safety>
+   unsafe
    public X509Certificate(System.IntPtr handle);
}
```

```diff
namespace System.Security.Cryptography.X509Certificates;

public partial class X509Certificate2 : System.Security.Cryptography.X509Certificates.X509Certificate
{
+   /// <safety>Wraps a raw, caller-supplied PCCERT_CONTEXT pointer that the certificate stack later dereferences.</safety>
+   unsafe
    public X509Certificate2(System.IntPtr handle);
}
```

```diff
namespace System.Security.Cryptography.X509Certificates;

public partial class X509Chain : System.IDisposable
{
+   /// <safety>Wraps a raw, caller-supplied PCCERT_CHAIN_CONTEXT pointer that the chain engine later dereferences.</safety>
+   unsafe
    public X509Chain(System.IntPtr chainContext);
}
```

```diff
namespace System.Security.Cryptography.X509Certificates;

public sealed partial class X509Store : System.IDisposable
{
+   /// <safety>Wraps a raw, caller-supplied HCERTSTORE handle (a pointer to a native store) that the store stack later dereferences.</safety>
+   unsafe
    public X509Store(System.IntPtr storeHandle);
}
```

```diff
namespace System.Diagnostics.SymbolStore;

public partial interface ISymbolBinder1
{
+   /// <safety>Dereferences a raw, caller-supplied native/COM importer interface pointer.</safety>
+   unsafe
    System.Diagnostics.SymbolStore.ISymbolReader? GetReader(System.IntPtr importer, string filename, string searchPath);
}
```

```diff
namespace System.Diagnostics.SymbolStore;

public partial interface ISymbolWriter
{
+   /// <safety>Dereferences a raw, caller-supplied native/COM emitter interface pointer.</safety>
+   unsafe
    void Initialize(System.IntPtr emitter, string filename, bool fFullBuild);

+   /// <safety>Dereferences a raw, caller-supplied native/COM writer interface pointer.</safety>
+   unsafe
    void SetUnderlyingWriter(System.IntPtr underlyingWriter);
}
```

```diff
namespace System.Transactions;

public partial interface IDtcTransaction
{
+   /// <safety>Reads the abort-reason structure through a raw, caller-supplied IntPtr.</safety>
+   unsafe
    void Abort(System.IntPtr reason, int retaining, int async);

+   /// <safety>Writes XACTTRANSINFO through a raw, caller-supplied IntPtr.</safety>
+   unsafe
    void GetTransactionInfo(System.IntPtr transactionInformation);
}
```

```diff
namespace System.Security.Principal;

public sealed partial class SecurityIdentifier : System.Security.Principal.IdentityReference, System.IComparable<System.Security.Principal.SecurityIdentifier>
{
+   /// <safety>Reads the binary SID through a raw, caller-supplied address.</safety>
+   unsafe
    public SecurityIdentifier(System.IntPtr binaryForm);
}
```

```diff
namespace System;

public struct RuntimeTypeHandle
{
+   /// <safety>Builds a handle from a raw, unvalidated MethodTable* pointer that the runtime later dereferences.</safety>
+   unsafe
    public static RuntimeTypeHandle FromIntPtr(System.IntPtr value);
}
```

```diff
namespace System;

public struct RuntimeMethodHandle
{
+   /// <safety>Builds a handle from a raw, unvalidated MethodDesc* pointer that the runtime later dereferences.</safety>
+   unsafe
    public static RuntimeMethodHandle FromIntPtr(System.IntPtr value);
}
```

```diff
namespace System;

public struct RuntimeFieldHandle
{
+   /// <safety>Builds a handle from a raw, unvalidated FieldDesc* pointer that the runtime later dereferences.</safety>
+   unsafe
    public static RuntimeFieldHandle FromIntPtr(System.IntPtr value);
}
```

```diff
namespace System.Security.Cryptography;

public sealed partial class SafeEvpPKeyHandle : System.Runtime.InteropServices.SafeHandle
{
+   /// <safety>Wraps a raw, caller-supplied EVP_PKEY* pointer that the crypto stack later dereferences.</safety>
+   unsafe
    public SafeEvpPKeyHandle(System.IntPtr handle, bool ownsHandle);
}
```

```diff
namespace Microsoft.Win32.SafeHandles;

public sealed partial class SafeAccessTokenHandle : System.Runtime.InteropServices.SafeHandle
{
+   /// <safety>Wraps a raw, caller-supplied OS handle; the safe Dispose/finalizer later closes it, so a bogus or non-owned handle can close an unrelated handle and corrupt process state.</safety>
+   unsafe
    public SafeAccessTokenHandle(System.IntPtr handle);
}
```

```diff
namespace Microsoft.Win32.SafeHandles;

public sealed partial class SafeFileHandle : Microsoft.Win32.SafeHandles.SafeHandleZeroOrMinusOneIsInvalid
{
+   /// <safety>Wraps a raw, caller-supplied OS handle; the safe Dispose/finalizer later closes it, so a bogus or non-owned handle can close an unrelated handle and corrupt process state.</safety>
+   unsafe
    public SafeFileHandle(System.IntPtr preexistingHandle, bool ownsHandle);
}
```

```diff
namespace Microsoft.Win32.SafeHandles;

public sealed partial class SafeNCryptKeyHandle : Microsoft.Win32.SafeHandles.SafeNCryptHandle
{
+   /// <safety>Wraps a raw, caller-supplied OS handle; the safe Dispose/finalizer later closes it, so a bogus or non-owned handle can close an unrelated handle and corrupt process state.</safety>
+   unsafe
    public SafeNCryptKeyHandle(System.IntPtr handle, System.Runtime.InteropServices.SafeHandle parentHandle);
}
```

```diff
namespace Microsoft.Win32.SafeHandles;

public sealed partial class SafePipeHandle : Microsoft.Win32.SafeHandles.SafeHandleZeroOrMinusOneIsInvalid
{
+   /// <safety>Wraps a raw, caller-supplied OS handle; the safe Dispose/finalizer later closes it, so a bogus or non-owned handle can close an unrelated handle and corrupt process state.</safety>
+   unsafe
    public SafePipeHandle(System.IntPtr preexistingHandle, bool ownsHandle);
}
```

```diff
namespace Microsoft.Win32.SafeHandles;

public sealed partial class SafeProcessHandle : Microsoft.Win32.SafeHandles.SafeHandleZeroOrMinusOneIsInvalid
{
+   /// <safety>Wraps a raw, caller-supplied OS handle; the safe Dispose/finalizer later closes it, so a bogus or non-owned handle can close an unrelated handle and corrupt process state.</safety>
+   unsafe
    public SafeProcessHandle(System.IntPtr existingHandle, bool ownsHandle);
}
```

```diff
namespace Microsoft.Win32.SafeHandles;

public sealed partial class SafeRegistryHandle : Microsoft.Win32.SafeHandles.SafeHandleZeroOrMinusOneIsInvalid
{
+   /// <safety>Wraps a raw, caller-supplied OS handle; the safe Dispose/finalizer later closes it, so a bogus or non-owned handle can close an unrelated handle and corrupt process state.</safety>
+   unsafe
    public SafeRegistryHandle(System.IntPtr preexistingHandle, bool ownsHandle);
}
```

```diff
namespace Microsoft.Win32.SafeHandles;

public sealed partial class SafeWaitHandle : Microsoft.Win32.SafeHandles.SafeHandleZeroOrMinusOneIsInvalid
{
+   /// <safety>Wraps a raw, caller-supplied OS handle; the safe Dispose/finalizer later closes it, so a bogus or non-owned handle can close an unrelated handle and corrupt process state.</safety>
+   unsafe
    public SafeWaitHandle(System.IntPtr existingHandle, bool ownsHandle);
}
```

```diff
namespace System.Net.Sockets;

public sealed partial class SafeSocketHandle : Microsoft.Win32.SafeHandles.SafeHandleMinusOneIsInvalid
{
+   /// <safety>Wraps a raw, caller-supplied OS handle; the safe Dispose/finalizer later closes it, so a bogus or non-owned handle can close an unrelated handle and corrupt process state.</safety>
+   unsafe
    public SafeSocketHandle(nint preexistingHandle, bool ownsHandle);
}
```
