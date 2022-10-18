# API Review 10/18/2022

## Runtime should be updated to support the `__vectorcall` calling convention

**Approved** | [#runtime/8300](https://github.com/dotnet/runtime/issues/8300#issuecomment-1282751028) | [Video](https://www.youtube.com/watch?v=--yp28cKQ_E&t=0h0m0s)

* The existing CallConv types that end in "conv" use a lowercase C, so this should as well.
* Do we also need to update System.Reflection.SignatureCallingConvention and/or System.Runtime.InteropServices.CallingConvention?

```C#
namespace System.Runtime.CompilerServices
{
     public class CallConvVectorcall
     {
         // This type has no members and is identical in structure to other `CallConv*` types
     }
}
```

## Add CollectionsMarshal.SetCount API for List<T>

**Approved** | [#runtime/55217](https://github.com/dotnet/runtime/issues/55217#issuecomment-1282783078) | [Video](https://www.youtube.com/watch?v=--yp28cKQ_E&t=0h12m52s)

* We debated just making it a public set_Count on `List<T>`, but nullable reference types made that slightly weird.
  * Also because of the perf-driven non-clearing behaviors.
* We discussed Resize vs SetCount, and feel SetCount is the more appropriate name.
* Regarding the pseudo-implementation, EnsureCapacity may be better than set_Capacity in the growth path.
* There wasn't enough quorum to know if we think returning the Span (as if AsSpan was called) instead of void.

```C#
namespace System.Runtime.InteropServices
{
    public static partial class CollectionsMarshal {
         public static void SetCount<T>(List<T> list, int count);
    }
}
```
## Add a BitArray.Contains method

**Approved** | [#runtime/72999](https://github.com/dotnet/runtime/issues/72999#issuecomment-1282827961) | [Video](https://www.youtube.com/watch?v=--yp28cKQ_E&t=0h40m37s)

* The proposed API and the proposed reason are logical opposites... "has all bits set" is `!arr.Contains(false)`.  So we came up with more direct questions.
* Count == 0 almost certainly wants both of these methods to return `false`.

```C#
namespace System.Collections
{
    public partial class BitArray
    {
        public bool HasAllSet();
        public bool HasAnySet();
    }
}
```
## Consider adding unmanaged pointer overloads for `Marshal` APIs

**Approved** | [#runtime/75630](https://github.com/dotnet/runtime/issues/75630#issuecomment-1282842504) | [Video](https://www.youtube.com/watch?v=--yp28cKQ_E&t=1h13m32s)

* We feel that the Read* and Write* APIs won't ever get called, since someone already working with pointers is almost certainly going to write `*(byte*)ptr` instead of `Marshal.ReadByte(ptr)`.
* There's a question of if StructureToPtr does unique work (string marshalling?) or is just a fancy Write.  If it's literally the same as `*ptr = struct` then it should be removed.
* The rest look good as proposed.

```C#
namespace System.Runtime.InteropServices
{
    public static partial class Marshal
    {
         public static unsafe int AddRef(void* pUnk);
         public static unsafe void Copy(byte[] source, int startIndex, void* destination, int length);
         public static unsafe void Copy(char[] source, int startIndex, void* destination, int length);
         public static unsafe void Copy(double[] source, int startIndex, void* destination, int length);
         public static unsafe void Copy(short[] source, int startIndex, void* destination, int length);
         public static unsafe void Copy(int[] source, int startIndex, void* destination, int length);
         public static unsafe void Copy(long[] source, int startIndex, void* destination, int length);
         public static unsafe void Copy(void* source, byte[] destination, int startIndex, int length);
         public static unsafe void Copy(void* source, char[] destination, int startIndex, int length);
         public static unsafe void Copy(void* source, double[] destination, int startIndex, int length);
         public static unsafe void Copy(void* source, short[] destination, int startIndex, int length);
         public static unsafe void Copy(void* source, int[] destination, int startIndex, int length);
         public static unsafe void Copy(void* source, long[] destination, int startIndex, int length);
         public static unsafe void Copy(void* source, void*[] destination, int startIndex, int length);
         public static unsafe void Copy(void* source, float[] destination, int startIndex, int length);
         public static unsafe void Copy(void*[] source, int startIndex, void* destination, int length);
         public static unsafe void Copy(float[] source, int startIndex, void* destination, int length);
         [System.Runtime.Versioning.SupportedOSPlatformAttribute("windows")]
         public static unsafe void* CreateAggregatedObject<T>(void* pOuter, T o) where T : notnull;
         public static unsafe void DestroyStructure<T>(void* ptr);
         public static unsafe TDelegate GetDelegateForFunctionPointer<TDelegate>(void* ptr);
         public static unsafe System.Exception? GetExceptionForHR(int errorCode, void* errorInfo);
         [System.Runtime.Versioning.SupportedOSPlatformAttribute("windows")]
         public static unsafe object GetObjectForIUnknown(void* pUnk);
         [System.Runtime.Versioning.SupportedOSPlatformAttribute("windows")]
         public static unsafe object GetTypedObjectForIUnknown(void* pUnk, System.Type t);
         [System.Runtime.Versioning.SupportedOSPlatformAttribute("windows")]
         public static unsafe object GetUniqueObjectForIUnknown(void* unknown);
         public static unsafe void InitHandle(SafeHandle safeHandle, IntPtr handle);
         public static unsafe string? PtrToStringAnsi(void* ptr);
         public static unsafe string PtrToStringAnsi(void* ptr, int len);
         public static unsafe string? PtrToStringAuto(void* ptr);
         public static unsafe string? PtrToStringAuto(void* ptr, int len);
         public static unsafe string PtrToStringBSTR(void* ptr);
         public static unsafe string? PtrToStringUni(void* ptr);
         public static unsafe string PtrToStringUni(void* ptr, int len);
         public static unsafe string? PtrToStringUTF8(void* ptr);
         public static unsafe string PtrToStringUTF8(void* ptr, int byteLen);
         public static unsafe T? PtrToStructure<[System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.NonPublicConstructors | System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.PublicConstructors)]T>(void* ptr);
         public static unsafe void PtrToStructure<T>(void* ptr, [System.Diagnostics.CodeAnalysis.DisallowNullAttribute] T structure);
         public static unsafe int QueryInterface(void* pUnk, ref System.Guid iid, out void* ppv);
         public static unsafe int Release(void* pUnk);
         public static unsafe void StructureToPtr<T>([System.Diagnostics.CodeAnalysis.DisallowNullAttribute] T structure, void* ptr, bool fDeleteOld);
         public static unsafe void ThrowExceptionForHR(int errorCode, void* errorInfo);
    }
}
```
## Generic ObjectFactory and ActivatorUtilities.CreateFactory

**Approved** | [#runtime/34471](https://github.com/dotnet/runtime/issues/34471#issuecomment-1282871195) | [Video](https://www.youtube.com/watch?v=--yp28cKQ_E&t=1h26m41s)

* Consider, in the future, adding more generic overloads that avoid the Type[] and object?[]? elements for pre-known signatures.  These would probably use Func instead of new generic arities of ObjectFactory

```C#
namespace Microsoft.Extensions.DependencyInjection
{
    public delegate T ObjectFactory<T>(IServiceProvider serviceProvider, object?[]? arguments);

    public static partial class ActivatorUtilities
    {
        public static ObjectFactory<T> CreateFactory<T>(Type[] argumentTypes) { throw null; }
    }
}
```
