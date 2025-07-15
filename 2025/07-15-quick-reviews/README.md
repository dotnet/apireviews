# API Review 07/15/2025

## CompilerLoweringPreserveAttribute

**Approved** | [#runtime/103430](https://github.com/dotnet/runtime/issues/103430#issuecomment-3074486825) | [Video](https://www.youtube.com/watch?v=anGS-KNPYE4&t=0h0m0s)

Looks good as proposed.

```C#
namespace System.Runtime.CompilerServices;

[AttributeUsage(AttributeTargets.Class, Inherited = false)]
public sealed class CompilerLoweringPreserveAttribute : Attribute
{
    public CompilerLoweringPreserveAttribute() { }
}
```
## Fix the name of certain `Tensor.Create` APIs to remove ambiguity

**Approved** | [#runtime/117372](https://github.com/dotnet/runtime/issues/117372#issuecomment-3074509409) | [Video](https://www.youtube.com/watch?v=anGS-KNPYE4&t=0h7m41s)

Changes look good as proposed.

```diff
 namespace System.Numerics.Tensors;

 public interface ITensor<TSelf, T>
 {
-    static abstract TSelf Create(scoped ReadOnlySpan<nint> lengths, bool pinned = false);
-    static abstract TSelf Create(scoped ReadOnlySpan<nint> lengths, scoped ReadOnlySpan<nint> strides, bool pinned = false);
+    static abstract TSelf CreateFromShape(scoped ReadOnlySpan<nint> lengths, bool pinned = false);
+    static abstract TSelf CreateFromShape(scoped ReadOnlySpan<nint> lengths, scoped ReadOnlySpan<nint> strides, bool pinned = false);

-    static abstract TSelf CreateUninitialized(scoped ReadOnlySpan<nint> lengths, bool pinned = false);
-    static abstract TSelf CreateUninitialized(scoped ReadOnlySpan<nint> lengths, scoped ReadOnlySpan<nint> strides, bool pinned = false);
+    static abstract TSelf CreateFromShapeUninitialized(scoped ReadOnlySpan<nint> lengths, bool pinned = false);
+    static abstract TSelf CreateFromShapeUninitialized(scoped ReadOnlySpan<nint> lengths, scoped ReadOnlySpan<nint> strides, bool pinned = false);
}

 public static class Tensor
 {
     public static Tensor<T> Create<T>(T[] array);
     public static Tensor<T> Create<T>(T[] array, scoped ReadOnlySpan<nint> lengths);
     public static Tensor<T> Create<T>(T[] array, scoped ReadOnlySpan<nint> lengths, scoped ReadOnlySpan<nint> strides);
     public static Tensor<T> Create<T>(T[] array, int start, scoped ReadOnlySpan<nint> lengths, scoped ReadOnlySpan<nint> strides);

-    public static Tensor<T> Create<T>(IEnumerable<T> enumerable, bool pinned = false);
-    public static Tensor<T> Create<T>(IEnumerable<T> enumerable, scoped ReadOnlySpan<nint> lengths, bool pinned = false);
-    public static Tensor<T> Create<T>(IEnumerable<T> enumerable, scoped ReadOnlySpan<nint> lengths, scoped ReadOnlySpan<nint> strides, bool pinned = false);

-    public static Tensor<T> CreateAndFillGaussianNormalDistribution<T>(scoped ReadOnlySpan<nint> lengths);
-    public static Tensor<T> CreateAndFillGaussianNormalDistribution<T>(Random random, scoped ReadOnlySpan<nint> lengths);

-    public static Tensor<T> CreateAndFillUniformDistribution<T>(scoped ReadOnlySpan<nint> lengths);
-    public static Tensor<T> CreateAndFillUniformDistribution<T>(Random random, scoped ReadOnlySpan<nint> lengths);

-    public static Tensor<T> Create<T>(scoped ReadOnlySpan<nint> lengths, bool pinned = false);
-    public static Tensor<T> Create<T>(scoped ReadOnlySpan<nint> lengths, scoped ReadOnlySpan<nint> strides, bool pinned = false);
+    public static Tensor<T> CreateFromShape<T>(scoped ReadOnlySpan<nint> lengths, bool pinned = false);
+    public static Tensor<T> CreateFromShape<T>(scoped ReadOnlySpan<nint> lengths, scoped ReadOnlySpan<nint> strides, bool pinned = false);

-    public static Tensor<T> CreateUninitialized<T>(scoped ReadOnlySpan<nint> lengths, bool pinned = false);
-    public static Tensor<T> CreateUninitialized<T>(scoped ReadOnlySpan<nint> lengths, scoped ReadOnlySpan<nint> strides, bool pinned = false);
+    public static Tensor<T> CreateFromShapeUninitialized<T>(scoped ReadOnlySpan<nint> lengths, bool pinned = false);
+    public static Tensor<T> CreateFromShapeUninitialized<T>(scoped ReadOnlySpan<nint> lengths, scoped ReadOnlySpan<nint> strides, bool pinned = false);
 }
```
## Allow getting a contiguous span from the given tensor

**Approved** | [#runtime/117375](https://github.com/dotnet/runtime/issues/117375#issuecomment-3074579920) | [Video](https://www.youtube.com/watch?v=anGS-KNPYE4&t=0h15m17s)

* Looks good as proposed
* The room was split on whether to recommend `Tensor<T>` implicitly or explicitly implement IReadOnlyTensor.TryGetSpan.  So... whatever feels right.

```C#
namespace System.Numerics.Tensors;

public partial interface ITensor<TSelf, T>
{
    Span<T> GetSpan(scoped ReadOnlySpan<nint> startIndexes, int length);
    Span<T> GetSpan(scoped ReadOnlySpan<NIndex> startIndexes, int length);

    bool TryGetSpan(scoped ReadOnlySpan<nint> startIndexes, int length, out Span<T> span);
    bool TryGetSpan(scoped ReadOnlySpan<NIndex> startIndexes, int length, out Span<T> span);
}

public partial interface IReadOnlyTensor<TSelf, T>
{
    ReadOnlySpan<T> GetSpan(scoped ReadOnlySpan<nint> startIndexes, int length);
    ReadOnlySpan<T> GetSpan(scoped ReadOnlySpan<NIndex> startIndexes, int length);

    bool TryGetSpan(scoped ReadOnlySpan<nint> startIndexes, int length, out ReadOnlySpan<T> span);
    bool TryGetSpan(scoped ReadOnlySpan<NIndex> startIndexes, int length, out ReadOnlySpan<T> span);
}

// Publicly exposed on Tensor<T>, TensorSpan<T>, and ReadOnlyTensorSpan<T>
```
## Allow custom filtering logic for `FakeLogger`

**Approved** | [#extensions/5615](https://github.com/dotnet/extensions/issues/5615#issuecomment-3074621717) | [Video](https://www.youtube.com/watch?v=anGS-KNPYE4&t=0h28m16s)

The property shouldn't be marked as Experimental, otherwise looks good as proposed.

```C#
namespace Microsoft.Extensions.Logging.Testing;

public partial class FakeLogCollectorOptions
{
    public Func<FakeLogRecord, bool>? CustomFilter { get; set; }
}
```
## Pkcs12LoaderLimits.AllowDuplicateAttributes

**Approved** | [#runtime/117516](https://github.com/dotnet/runtime/issues/117516#issuecomment-3074654002) | [Video](https://www.youtube.com/watch?v=anGS-KNPYE4&t=0h35m52s)

Looks good as proposed

```C#
namespace System.Security.Cryptography.X509Certificates;

public sealed partial class Pkcs12LoaderLimits {
    public bool AllowDuplicateAttributes { get; set; }
}
```
## Add ExcludeStatics to RequiresUnreferencedCode and RequiresDynamicCode

**Approved** | [#runtime/117524](https://github.com/dotnet/runtime/issues/117524#issuecomment-3074713281) | [Video](https://www.youtube.com/watch?v=anGS-KNPYE4&t=0h42m25s)

* Attributes are generally get/set, not get/init, so updated for consistency.
* Otherwise, looks good as proposed

```C#
namespace System.Runtime.CompilerServices;

public sealed partial class RequiresUnreferencedCodeAttribute : Attribute
{
    public bool ExcludeStatics { get; set; }
}

public sealed partial class RequiresDynamicCodeAttribute : Attribute
{
    public bool ExcludeStatics { get; set; }
}
```
## Expose TensorMarshal for manipulating the underlying byref of a TensorSpan

**Approved** | [#runtime/117373](https://github.com/dotnet/runtime/issues/117373#issuecomment-3074835519) | [Video](https://www.youtube.com/watch?v=anGS-KNPYE4&t=0h50m36s)

Looks good as proposed

```C#
namespace System.Runtime.InteropServices;

public static class TensorMarshal
{
    public static TensorSpan<T> CreateTensorSpan<T>(ref T data, nint dataLength, scoped ReadOnlySpan<nint> lengths, scoped ReadOnlySpan<nint> strides, bool pinned);
    public static ReadOnlyTensorSpan<T> CreateReadOnlyTensorSpan<T>(ref readonly T data, nint dataLength, scoped ReadOnlySpan<nint> lengths, scoped ReadOnlySpan<nint> strides, bool pinned);

    public static ref T GetReference<T>(in TensorSpan<T> tensorSpan);
    public static ref readonly T GetReference<T>(in ReadOnlyTensorSpan<T> tensorSpan);
}
```
## Respect `ISupportErrorInfo` for `HRESULT` to `Exception` throwing conversion

**Approved** | [#runtime/117438](https://github.com/dotnet/runtime/issues/117438#issuecomment-3074934276) | [Video](https://www.youtube.com/watch?v=anGS-KNPYE4&t=1h10m13s)

* `targetIID` should be named `iid` for consistency
* Marshal.QueryInterface takes iid as `in`, here it should, too
* The parameter order here (iid, pUnk) is opposite of QueryInterface, but that is intentional and has value.

```C#
namespace System.Runtime.InteropServices;

public static partial class Marshal
{
     public static void ThrowExceptionForHR(int errorCode, in Guid iid, IntPtr pUnk);
     public static Exception? GetExceptionForHR(int errorCode, in Guid iid, IntPtr pUnk);
}
```
