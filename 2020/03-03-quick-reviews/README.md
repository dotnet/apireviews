# Quick Reviews 3/3/2020

## Arm Shift and Permute intrinsics

**Approved** | [#runtime/31324](https://github.com/dotnet/runtime/issues/31324#issuecomment-594124186) | [Video](https://www.youtube.com/watch?v=3qXvVF9n0kQ&t=0h0m0s)

* Confirm that `ShiftLeftLogicalAndInsert` under `ArmBase` should move under `AdvSimd`, but is it 32 bit or 64 bit?
* Some of instructions are destructive on ARM32 and non-destructive on ARM64. Should we split these up into a 32-bit version with a different shape?

```C#
namespace System.Runtime.Intrinsics.Arm
{
    public partial class ArmBase
    {
        /// <summary>
        /// vslid_n_[su]64
        ///
        /// A64: SLI
        /// A32: VSLI
        /// </summary>
        public static Vector64<long>  ShiftLeftLogicalAndInsertScalar(Vector64<long>  left, Vector64<long>  right, byte shift);
        public static Vector64<ulong> ShiftLeftLogicalAndInsertScalar(Vector64<ulong> left, Vector64<ulong> right, byte shift);

        /// <summary>
        /// vsrid_n_[su]64
        ///
        /// A64: SRI
        /// A32: VSRI
        /// </summary>
        public static Vector64<long>  ShiftRightLogicalAndInsertScalar(Vector64<long>  left, Vector64<long>  right, byte shift);
        public static Vector64<ulong> ShiftRightLogicalAndInsertScalar(Vector64<ulong> left, Vector64<ulong> right, byte shift);
    }
    public partial class AdvSimd
    {
        /// <summary>
        /// vsli[q]_n_[su][8,16,32,64]
        //
        /// A64: SLI
        /// A32: VSLI
        /// </summary>
        public static Vector64<byte>   ShiftLeftLogicalAndInsert(Vector64<byte>   left, Vector64<byte>   right, byte shift);
        public static Vector64<ushort> ShiftLeftLogicalAndInsert(Vector64<ushort> left, Vector64<ushort> right, byte shift);
        public static Vector64<uint>   ShiftLeftLogicalAndInsert(Vector64<uint>   left, Vector64<uint>   right, byte shift);
        public static Vector64<sbyte>  ShiftLeftLogicalAndInsert(Vector64<sbyte>  left, Vector64<sbyte>  right, byte shift);
        public static Vector64<short>  ShiftLeftLogicalAndInsert(Vector64<short>  left, Vector64<short>  right, byte shift);
        public static Vector64<int>    ShiftLeftLogicalAndInsert(Vector64<int>    left, Vector64<int>    right, byte shift);

        public static Vector128<byte>   ShiftLeftLogicalAndInsert(Vector128<byte>   left, Vector128<byte>   right, byte shift);
        public static Vector128<ushort> ShiftLeftLogicalAndInsert(Vector128<ushort> left, Vector128<ushort> right, byte shift);
        public static Vector128<uint>   ShiftLeftLogicalAndInsert(Vector128<uint>   left, Vector128<uint>   right, byte shift);
        public static Vector128<ulong>  ShiftLeftLogicalAndInsert(Vector128<ulong>  left, Vector128<ulong>  right, byte shift);
        public static Vector128<sbyte>  ShiftLeftLogicalAndInsert(Vector128<sbyte>  left, Vector128<sbyte>  right, byte shift);
        public static Vector128<short>  ShiftLeftLogicalAndInsert(Vector128<short>  left, Vector128<short>  right, byte shift);
        public static Vector128<int>    ShiftLeftLogicalAndInsert(Vector128<int>    left, Vector128<int>    right, byte shift);
        public static Vector128<long>   ShiftLeftLogicalAndInsert(Vector128<long>   left, Vector128<long>   right, byte shift);

        /// <summary>
        /// vsri[q]_n_[su][8,16,32,64]
        ///
        /// A64: SRI
        /// A32: VSRI
        /// </summary>
        public static Vector64<byte>   ShiftRightAndInsert(Vector64<byte>   left, Vector64<byte>   right, byte shift);
        public static Vector64<ushort> ShiftRightAndInsert(Vector64<ushort> left, Vector64<ushort> right, byte shift);
        public static Vector64<uint>   ShiftRightAndInsert(Vector64<uint>   left, Vector64<uint>   right, byte shift);
        public static Vector64<sbyte>  ShiftRightAndInsert(Vector64<sbyte>  left, Vector64<sbyte>  right, byte shift);
        public static Vector64<short>  ShiftRightAndInsert(Vector64<short>  left, Vector64<short>  right, byte shift);
        public static Vector64<int>    ShiftRightAndInsert(Vector64<int>    left, Vector64<int>    right, byte shift);

        public static Vector128<byte>   ShiftRightAndInsert(Vector128<byte>   left, Vector128<byte>   right, byte shift);
        public static Vector128<ushort> ShiftRightAndInsert(Vector128<ushort> left, Vector128<ushort> right, byte shift);
        public static Vector128<uint>   ShiftRightAndInsert(Vector128<uint>   left, Vector128<uint>   right, byte shift);
        public static Vector128<ulong>  ShiftRightAndInsert(Vector128<ulong>  left, Vector128<ulong>  right, byte shift);
        public static Vector128<sbyte>  ShiftRightAndInsert(Vector128<sbyte>  left, Vector128<sbyte>  right, byte shift);
        public static Vector128<short>  ShiftRightAndInsert(Vector128<short>  left, Vector128<short>  right, byte shift);
        public static Vector128<int>    ShiftRightAndInsert(Vector128<int>    left, Vector128<int>    right, byte shift);
        public static Vector128<long>   ShiftRightAndInsert(Vector128<long>   left, Vector128<long>   right, byte shift);

        /// <summary>
        /// vmovn_[su][16,32,64]
        ///
        /// A64: XTN
        /// A32: VMOVN
        /// </summary>
        public static Vector64<sbyte>  ExtractAndNarrowLow(Vector128<short>  value);
        public static Vector64<short>  ExtractAndNarrowLow(Vector128<int>    value);
        public static Vector64<int>    ExtractAndNarrowLow(Vector128<long>   value);
        public static Vector64<byte>   ExtractAndNarrowLow(Vector128<ushort> value);
        public static Vector64<ushort> ExtractAndNarrowLow(Vector128<uint>   value);
        public static Vector64<uint>   ExtractAndNarrowLow(Vector128<ulong>  value);

        /// <summary>
        /// vmovn_high_[su][16,32,64]
        //
        /// A64: XTN2
        /// A32: VMOVN
        /// </summary>
        public static Vector128<sbyte>  ExtractAndNarrowHigh(Vector64<sbyte>  lower, Vector128<short>  value);
        public static Vector128<short>  ExtractAndNarrowHigh(Vector64<short>  lower, Vector128<int>    value);
        public static Vector128<int>    ExtractAndNarrowHigh(Vector64<int>    lower, Vector128<long>   value);
        public static Vector128<byte>   ExtractAndNarrowHigh(Vector64<byte>   lower, Vector128<ushort> value);
        public static Vector128<ushort> ExtractAndNarrowHigh(Vector64<ushort> lower, Vector128<uint>   value);
        public static Vector128<uint>   ExtractAndNarrowHigh(Vector64<uint>   lower, Vector128<ulong>  value);

        public partial class Arm64
        {
            /// <summary>
            /// vtrn1[q]_[suf][8,16,32,64]
            ///
            /// A64: UZP1
            /// </summary>
            public static Vector64<sbyte>  UnzipEven(Vector64<sbyte>  lower, Vector64<sbyte>  upper);
            public static Vector64<short>  UnzipEven(Vector64<short>  lower, Vector64<short>  upper);
            public static Vector64<int>    UnzipEven(Vector64<int>    lower, Vector64<int>    upper);
            public static Vector64<byte>   UnzipEven(Vector64<byte>   lower, Vector64<byte>   upper);
            public static Vector64<ushort> UnzipEven(Vector64<ushort> lower, Vector64<ushort> upper);
            public static Vector64<uint>   UnzipEven(Vector64<uint>   lower, Vector64<uint>   upper);
            public static Vector64<float>  UnzipEven(Vector64<float>  lower, Vector64<float>  upper);

            public static Vector128<sbyte>  UnzipEven(Vector128<sbyte>  lower, Vector128<sbyte>  upper);
            public static Vector128<short>  UnzipEven(Vector128<short>  lower, Vector128<short>  upper);
            public static Vector128<int>    UnzipEven(Vector128<int>    lower, Vector128<int>    upper);
            public static Vector128<long>   UnzipEven(Vector128<long>   lower, Vector128<long>   upper);
            public static Vector128<byte>   UnzipEven(Vector128<byte>   lower, Vector128<byte>   upper);
            public static Vector128<ushort> UnzipEven(Vector128<ushort> lower, Vector128<ushort> upper);
            public static Vector128<uint>   UnzipEven(Vector128<uint>   lower, Vector128<uint>   upper);
            public static Vector128<ulong>  UnzipEven(Vector128<ulong>  lower, Vector128<ulong>  upper);
            public static Vector128<float>  UnzipEven(Vector128<float>  lower, Vector128<float>  upper);
            public static Vector128<double> UnzipEven(Vector128<double> lower, Vector128<double> upper);

            /// <summary>
            /// vtrn2[q]_[suf][8,16,32,64]
            ///
            /// A64: UZP2
            /// </summary>
            public static Vector64<sbyte>  UnzipOdd(Vector64<sbyte>  lower, Vector64<sbyte>  upper);
            public static Vector64<short>  UnzipOdd(Vector64<short>  lower, Vector64<short>  upper);
            public static Vector64<int>    UnzipOdd(Vector64<int>    lower, Vector64<int>    upper);
            public static Vector64<byte>   UnzipOdd(Vector64<byte>   lower, Vector64<byte>   upper);
            public static Vector64<ushort> UnzipOdd(Vector64<ushort> lower, Vector64<ushort> upper);
            public static Vector64<uint>   UnzipOdd(Vector64<uint>   lower, Vector64<uint>   upper);
            public static Vector64<float>  UnzipOdd(Vector64<float>  lower, Vector64<float>  upper);

            public static Vector128<sbyte>  UnzipOdd(Vector128<sbyte>  lower, Vector128<sbyte>  upper);
            public static Vector128<short>  UnzipOdd(Vector128<short>  lower, Vector128<short>  upper);
            public static Vector128<int>    UnzipOdd(Vector128<int>    lower, Vector128<int>    upper);
            public static Vector128<long>   UnzipOdd(Vector128<long>   lower, Vector128<long>   upper);
            public static Vector128<byte>   UnzipOdd(Vector128<byte>   lower, Vector128<byte>   upper);
            public static Vector128<ushort> UnzipOdd(Vector128<ushort> lower, Vector128<ushort> upper);
            public static Vector128<uint>   UnzipOdd(Vector128<uint>   lower, Vector128<uint>   upper);
            public static Vector128<ulong>  UnzipOdd(Vector128<ulong>  lower, Vector128<ulong>  upper);
            public static Vector128<float>  UnzipOdd(Vector128<float>  lower, Vector128<float>  upper);
            public static Vector128<double> UnzipOdd(Vector128<double> lower, Vector128<double> upper);

            /// <summary>
            /// vzip1[q]_[suf][8,16,32,64]
            ///
            /// A64: ZIP1
            /// </summary>
            public static Vector64<sbyte>  ZipLow(Vector64<sbyte>  left, Vector64<sbyte>  right);
            public static Vector64<short>  ZipLow(Vector64<short>  left, Vector64<short>  right);
            public static Vector64<int>    ZipLow(Vector64<int>    left, Vector64<int>    right);
            public static Vector64<byte>   ZipLow(Vector64<byte>   left, Vector64<byte>   right);
            public static Vector64<ushort> ZipLow(Vector64<ushort> left, Vector64<ushort> right);
            public static Vector64<uint>   ZipLow(Vector64<uint>   left, Vector64<uint>   right);
            public static Vector64<float>  ZipLow(Vector64<float>  left, Vector64<float>  right);

            public static Vector128<sbyte>  ZipLow(Vector128<sbyte>  left, Vector128<sbyte>  right);
            public static Vector128<short>  ZipLow(Vector128<short>  left, Vector128<short>  right);
            public static Vector128<int>    ZipLow(Vector128<int>    left, Vector128<int>    right);
            public static Vector128<long>   ZipLow(Vector128<long>   left, Vector128<long>   right);
            public static Vector128<byte>   ZipLow(Vector128<byte>   left, Vector128<byte>   right);
            public static Vector128<ushort> ZipLow(Vector128<ushort> left, Vector128<ushort> right);
            public static Vector128<uint>   ZipLow(Vector128<uint>   left, Vector128<uint>   right);
            public static Vector128<ulong>  ZipLow(Vector128<ulong>  left, Vector128<ulong>  right);
            public static Vector128<float>  ZipLow(Vector128<float>  left, Vector128<float>  right);
            public static Vector128<double> ZipLow(Vector128<double> left, Vector128<double> right);

            /// <summary>
            /// vzip2[q]_[suf][8,16,32,64]
            ///
            /// A64: ZIP2
            /// </summary>
            public static Vector64<sbyte>  ZipHigh(Vector64<sbyte>  left, Vector64<sbyte>  right);
            public static Vector64<short>  ZipHigh(Vector64<short>  left, Vector64<short>  right);
            public static Vector64<int>    ZipHigh(Vector64<int>    left, Vector64<int>    right);
            public static Vector64<byte>   ZipHigh(Vector64<byte>   left, Vector64<byte>   right);
            public static Vector64<ushort> ZipHigh(Vector64<ushort> left, Vector64<ushort> right);
            public static Vector64<uint>   ZipHigh(Vector64<uint>   left, Vector64<uint>   right);
            public static Vector64<float>  ZipHigh(Vector64<float>  left, Vector64<float>  right);

            public static Vector128<sbyte>  ZipHigh(Vector128<sbyte>  left, Vector128<sbyte>  right);
            public static Vector128<short>  ZipHigh(Vector128<short>  left, Vector128<short>  right);
            public static Vector128<int>    ZipHigh(Vector128<int>    left, Vector128<int>    right);
            public static Vector128<long>   ZipHigh(Vector128<long>   left, Vector128<long>   right);
            public static Vector128<byte>   ZipHigh(Vector128<byte>   left, Vector128<byte>   right);
            public static Vector128<ushort> ZipHigh(Vector128<ushort> left, Vector128<ushort> right);
            public static Vector128<uint>   ZipHigh(Vector128<uint>   left, Vector128<uint>   right);
            public static Vector128<ulong>  ZipHigh(Vector128<ulong>  left, Vector128<ulong>  right);
            public static Vector128<float>  ZipHigh(Vector128<float>  left, Vector128<float>  right);
            public static Vector128<double> ZipHigh(Vector128<double> left, Vector128<double> right);

            /// <summary>
            /// vtrn1[q]_[suf][8,16,32,64]
            ///
            /// A64: TRN1
            /// </summary>
            public static Vector64<sbyte>  TransposeEven(Vector64<sbyte>  left, Vector64<sbyte>  right);
            public static Vector64<short>  TransposeEven(Vector64<short>  left, Vector64<short>  right);
            public static Vector64<int>    TransposeEven(Vector64<int>    left, Vector64<int>    right);
            public static Vector64<byte>   TransposeEven(Vector64<byte>   left, Vector64<byte>   right);
            public static Vector64<ushort> TransposeEven(Vector64<ushort> left, Vector64<ushort> right);
            public static Vector64<uint>   TransposeEven(Vector64<uint>   left, Vector64<uint>   right);
            public static Vector64<float>  TransposeEven(Vector64<float>  left, Vector64<float>  right);

            public static Vector128<sbyte>  TransposeEven(Vector128<sbyte>  left, Vector128<sbyte>  right);
            public static Vector128<short>  TransposeEven(Vector128<short>  left, Vector128<short>  right);
            public static Vector128<int>    TransposeEven(Vector128<int>    left, Vector128<int>    right);
            public static Vector128<long>   TransposeEven(Vector128<long>   left, Vector128<long>   right);
            public static Vector128<byte>   TransposeEven(Vector128<byte>   left, Vector128<byte>   right);
            public static Vector128<ushort> TransposeEven(Vector128<ushort> left, Vector128<ushort> right);
            public static Vector128<uint>   TransposeEven(Vector128<uint>   left, Vector128<uint>   right);
            public static Vector128<ulong>  TransposeEven(Vector128<ulong>  left, Vector128<ulong>  right);
            public static Vector128<float>  TransposeEven(Vector128<float>  left, Vector128<float>  right);
            public static Vector128<double> TransposeEven(Vector128<double> left, Vector128<double> right);

            /// <summary>
            /// vtrn2[q]_[suf][8,16,32,64]
            ///
            /// A64: TRN2
            /// </summary>
            public static Vector64<sbyte>  TransposeOdd(Vector64<sbyte>  left, Vector64<sbyte>  right);
            public static Vector64<short>  TransposeOdd(Vector64<short>  left, Vector64<short>  right);
            public static Vector64<int>    TransposeOdd(Vector64<int>    left, Vector64<int>    right);
            public static Vector64<byte>   TransposeOdd(Vector64<byte>   left, Vector64<byte>   right);
            public static Vector64<ushort> TransposeOdd(Vector64<ushort> left, Vector64<ushort> right);
            public static Vector64<uint>   TransposeOdd(Vector64<uint>   left, Vector64<uint>   right);
            public static Vector64<float>  TransposeOdd(Vector64<float>  left, Vector64<float>  right);

            public static Vector128<sbyte>  TransposeOdd(Vector128<sbyte>  left, Vector128<sbyte>  right);
            public static Vector128<short>  TransposeOdd(Vector128<short>  left, Vector128<short>  right);
            public static Vector128<int>    TransposeOdd(Vector128<int>    left, Vector128<int>    right);
            public static Vector128<long>   TransposeOdd(Vector128<long>   left, Vector128<long>   right);
            public static Vector128<byte>   TransposeOdd(Vector128<byte>   left, Vector128<byte>   right);
            public static Vector128<ushort> TransposeOdd(Vector128<ushort> left, Vector128<ushort> right);
            public static Vector128<uint>   TransposeOdd(Vector128<uint>   left, Vector128<uint>   right);
            public static Vector128<ulong>  TransposeOdd(Vector128<ulong>  left, Vector128<ulong>  right);
            public static Vector128<float>  TransposeOdd(Vector128<float>  left, Vector128<float>  right);
            public static Vector128<double> TransposeOdd(Vector128<double> left, Vector128<double> right);
        }
    }
}
```
## Extend ObsoleteAttribute

**Approved** | [#runtime/33089](https://github.com/dotnet/runtime/issues/33089) | [Video](https://www.youtube.com/watch?v=3qXvVF9n0kQ&t=1h21m19s)

## NativeCallableAttribute should be a public API

**Approved** | [#runtime/32462](https://github.com/dotnet/runtime/issues/32462#issuecomment-594132248) | [Video](https://www.youtube.com/watch?v=3qXvVF9n0kQ&t=1h27m41s)

* Looks good as proposed.
* We modelled it off of `DllImportAttribute`, which is why it's named `EntryPoint` and those are fields.

```C#
namespace System.Runtime.InteropServices
{
    [AttributeUsage(AttributeTargets.Method)]
    public sealed class NativeCallableAttribute : Attribute
    {
        public NativeCallableAttribute();

        public CallingConvention CallingConvention;
        public string? EntryPoint;
    }
}
```
## Low level API support for RCW and CCW management

**Approved** | [#runtime/1845](https://github.com/dotnet/runtime/issues/1845#issuecomment-594151682) | [Video](https://www.youtube.com/watch?v=3qXvVF9n0kQ&t=1h38m17s)

* Looks good as proposed
* We should align the naming `Vtable` vs. `vftbl`
* Some comments are out sync
* The design should be validated by people who are wrapping COM objects

```C#
namespace System.Runtime
{
    public static partial class RuntimeHelpers
    {
        /// <summary>
        /// Allocate memory that is associated with the <paramref name="type"/> and
        /// will be freed if and when the <see cref="System.Type"/> is unloaded.
        /// </summary>
        /// <param name="type">Type associated with the allocated memory.</param>
        /// <param name="size">Amount of memory in bytes to allocate.</param>
        /// <returns>The allocated memory</returns>
        public static IntPtr AllocateTypeAssociatedMemory(Type type, int size);
    }
}

namespace System.Runtime.InteropServices
{
    /// <summary>
    /// Enumeration of flags for <see cref="ComWrappers.GetOrCreateComInterfaceForObject(object, CreateComInterfaceFlags)"/>.
    /// </summary>
    [Flags]
    public enum CreateComInterfaceFlags
    {
        None = 0,

        /// <summary>
        /// The caller will provide an IUnknown Vtable.
        /// </summary>
        /// <remarks>
        /// This is useful in scenarios when the caller has no need to rely on an IUnknown instance
        /// that is used when running managed code is not possible (i.e. during a GC). In traditional
        /// COM scenarios this is common, but scenarios involving <see href="https://docs.microsoft.com/windows/win32/api/windows.ui.xaml.hosting.referencetracker/nn-windows-ui-xaml-hosting-referencetracker-ireferencetrackertarget">Reference Tracker hosting</see>
        /// calling of the IUnknown API during a GC is possible.
        /// </remarks>
        CallerDefinedIUnknown = 1,

        /// <summary>
        /// Flag used to indicate the COM interface should implement <see href="https://docs.microsoft.com/windows/win32/api/windows.ui.xaml.hosting.referencetracker/nn-windows-ui-xaml-hosting-referencetracker-ireferencetrackertarget">IReferenceTrackerTarget</see>.
        /// When this flag is passed, the resulting COM interface will have an internal implementation of IUnknown
        /// and as such none should be supplied by the caller.
        /// </summary>
        TrackerSupport = 2,
    }

    /// <summary>
    /// Enumeration of flags for <see cref="ComWrappers.GetOrCreateObjectForComInstance(IntPtr, CreateObjectFlags, object?)"/>.
    /// </summary>
    [Flags]
    public enum CreateObjectFlags
    {
        None = 0,

        /// <summary>
        /// Indicate if the supplied external COM object implements the <see href="https://docs.microsoft.com/windows/win32/api/windows.ui.xaml.hosting.referencetracker/nn-windows-ui-xaml-hosting-referencetracker-ireferencetracker">IReferenceTracker</see>.
        /// </summary>
        TrackerObject = 1,

        /// <summary>
        /// Ignore any internal caching and always create a unique instance.
        /// </summary>
        UniqueInstance = 2,
    }

    /// <summary>
    /// Class for managing wrappers of COM IUnknown types.
    /// </summary>
    [CLSCompliant(false)]
    public abstract partial class ComWrappers
    {
        /// <summary>
        /// Interface type and pointer to targeted VTable.
        /// </summary>
        public struct ComInterfaceEntry
        {
            /// <summary>
            /// Interface IID.
            /// </summary>
            public Guid IID;

            /// <summary>
            /// Memory must have the same lifetime as the memory returned from the call to <see cref="ComputeVtables(object, CreateComInterfaceFlags, out int)"/>.
            /// </summary>
            public IntPtr Vtable;
        }

        /// <summary>
        /// ABI for function dispatch of a COM interface.
        /// </summary>
        public struct ComInterfaceDispatch
        {
            public IntPtr vftbl;

            /// <summary>
            /// Given a <see cref="System.IntPtr"/> from a generated VTable, convert to the target type.
            /// </summary>
            /// <typeparam name="T">Desired type.</typeparam>
            /// <param name="dispatchPtr">Pointer supplied to VTable function entry.</param>
            /// <returns>Instance of type associated with dispatched function call.</returns>
            public static unsafe T GetInstance<T>(ComInterfaceDispatch* dispatchPtr) where T : class;
        }

        /// <summary>
        /// Create an COM representation of the supplied object that can be passed to an non-managed environment.
        /// </summary>
        /// <param name="instance">A GC Handle to the managed object to expose outside the .NET runtime.</param>
        /// <param name="flags">Flags used to configure the generated interface.</param>
        /// <returns>The generated COM interface that can be passed outside the .NET runtime.</returns>
        public IntPtr GetOrCreateComInterfaceForObject(object instance, CreateComInterfaceFlags flags);

        /// <summary>
        /// Compute the desired VTables for <paramref name="obj"/> respecting the values of <paramref name="flags"/>.
        /// </summary>
        /// <param name="obj">Target of the returned VTables.</param>
        /// <param name="flags">Flags used to compute VTables.</param>
        /// <param name="count">The number of elements contained in the returned memory.</param>
        /// <returns><see cref="ComInterfaceEntry" /> pointer containing memory for all COM interface entries.</returns>
        /// <remarks>
        /// All memory returned from this function must either be unmanaged memory, pinned managed memory, or have been
        /// allocated with the <see cref="System.Runtime.CompilerServices.RuntimeHelpers.AllocateTypeAssociatedMemory(Type, int)"/> API.
        ///
        /// If the interface entries cannot be created and <code>null</code> is returned, the call to <see cref="ComWrappers.GetOrCreateComInterfaceForObject(object, CreateComInterfaceFlags)"/> will throw a <see cref="System.ArgumentNullException"/>.
        /// </remarks>
        protected unsafe abstract ComInterfaceEntry* ComputeVtables(object obj, CreateComInterfaceFlags flags, out int count);

        /// <summary>
        /// Get the currently registered managed object or creates a new managed object and registers it.
        /// </summary>
        /// <param name="externalComObject">Object to import for usage into the .NET runtime.</param>
        /// <param name="flags">Flags used to describe the external object.</param>
        /// <param name="wrapper">An optional <see cref="object"/> to be used as the wrapper for the external object</param>
        /// <returns>Returns a managed object associated with the supplied external COM object.</returns>
        /// <remarks>
        /// Providing a <paramref name="wrapper"/> instance means <see cref="ComWrappers.GetOrCreateObjectForComInstance(IntPtr, CreateObjectFlags, object?)"/>
        /// will not be called.
        ///
        /// If the <paramref name="wrapper"/> instance already has an associated external object a <see cref="System.NotSupportedException"/> will be thrown.
        /// </remarks>
        public object GetOrCreateObjectForComInstance(IntPtr externalComObject, CreateObjectFlags flags, object? wrapper = null);

        /// <summary>
        /// Create a managed object for the object pointed at by <paramref name="externalComObject"/> respecting the values of <paramref name="flags"/>.
        /// </summary>
        /// <param name="externalComObject">Object to import for usage into the .NET runtime.</param>
        /// <param name="flags">Flags used to describe the external object.</param>
        /// <returns>Returns a managed object associated with the supplied external COM object.</returns>
        /// <remarks>
        /// If the object cannot be created and <code>null</code> is returned, the call to <see cref="ComWrappers.GetOrCreateObjectForComInstance(IntPtr, CreateObjectFlags, object?)"/> will throw a <see cref="System.ArgumentNullException"/>.
        /// </remarks>
        protected abstract object? CreateObject(IntPtr externalComObject, CreateObjectFlags flags);

        /// <summary>
        /// Called when a request is made for a collection of objects to be released.
        /// </summary>
        /// <param name="objects">Collection of objects to release.</param>
        /// <remarks>
        /// The default implementation of this function throws <see cref="System.NotImplementedException"/>.
        /// </remarks>
        protected virtual void ReleaseObjects(IEnumerable objects);

        /// <summary>
        /// Register this class's implementation to be used as the single global instance.
        /// </summary>
        /// <remarks>
        /// This function can only be called a single time. Subsequent calls to this function will result
        /// in a <see cref="System.InvalidOperationException"/> being thrown.
        ///
        /// Scenarios where the global instance may be used are:
        ///  * Object tracking via the <see cref="CreateComInterfaceFlags.TrackerSupport" /> and <see cref="CreateObjectFlags.TrackerObject" /> flags.
        ///  * Usage of COM related Marshal APIs.
        /// </remarks>
        public void RegisterAsGlobalInstance();

        /// <summary>
        /// Get the runtime provided IUnknown implementation.
        /// </summary>
        /// <param name="fpQueryInterface">Function pointer to QueryInterface.</param>
        /// <param name="fpAddRef">Function pointer to AddRef.</param>
        /// <param name="fpRelease">Function pointer to Release.</param>
        protected static void GetIUnknownImpl(out IntPtr fpQueryInterface, out IntPtr fpAddRef, out IntPtr fpRelease);
    }
}
```
