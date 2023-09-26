# API Review 09/26/2023

## Marshaller for OLE `VARIANT`

**Approved** | [#runtime/89543](https://github.com/dotnet/runtime/issues/89543#issuecomment-1736089650) | [Video](https://www.youtube.com/watch?v=9z2P-pZ2Id4&t=0h0m0s)

* Generally looks good as proposed
* The instance members on OleVariant that do not modify the instance should be declared as `readonly`. (That was edited in here, but might be wrong, or incomplete)

```C#
using System.Diagnostics.CodeAnalysis;
using System.Runtime.CompilerServices;
namespace System.Runtime.InteropServices.Marshalling;

[StructLayout(LayoutKind.Explicit)]
public struct OleVariant : IDisposable
{
    // private fields to match the layout of VARIANT

    public OleVariant();

    public void Dispose();

    // Determine the VarEnum for the variant based on T.
    // Respects the System.Runtime.InteropServices.*Wrapper types for specifying the variant type.
    // We will not allow System.Object as T (we will throw an InvalidOperationException).
    // If a System.Runtime.InteropServices.*Wrapper type that corresponds to a COM interface is specified,
    // the following behavior occurs:
    // - If Marshal.IsComObject() returns true, call Marshal.GetIUnknown/IDispatchForObject
    // - Otherwise, use ComInterfaceMarshaller<T> to get the interface pointer.
    // Types without a corresponding VarEnum type are not allowed.
    public static OleVariant Create<T>([DisallowNull] T value);

    // Explicitly specify the VarEnum for the variant and the raw value.
    // Throws if T is larger than the VARIANT's data union.
    // Throws for VT_DECIMAL.
    // Throws if T does not match the size of the data union member that vt corresponds to.
    public static OleVariant CreateRaw<T>(VarEnum vt, T rawValue) where T : unmanaged;

    // Create VT_NULL variant
    public static OleVariant Null { get; };

    // Get a .NET object of the specified type that represents the underlying VARIANT value.
    // For COM object-based VARIANTs, uses ComInterfaceMarshaller to create the managed object.
    // If passed one of the System.Runtime.InteropServices.*Wrapper types, the wrapper type will be used
    // if it matches the variant type. Using a wrapper type is not required.
    // If the requested type does not match the underlying type, throws an exception.
    public readonly T As<T>();

    // For cases where the built-in Create/As methods are not sufficient:
    // Expose accessors for the underlying fields of the VARIANT struct
    // but in a limited fashion.
    // This will allow consumers such as WinForms to support VARIANT types
    // that we do not plan to support immediately (like SAFEARRAY).
    public readonly VarEnum VarType { get; }

    // Returns a reference to the data union within the variant.
    // Throws if T is larger than the VARIANT's data union.
    [UnscopedRef]
    public ref T GetRawDataRef<T>() where T : unmanaged;
}

// Let's start with a marshaller for object<->OleVariant as this will cover the majority of cases.
// We can add a generic marshaller for T<->OleVariant later if needed (e.g. for an IDispatch-based generator)
[CustomMarshaller(typeof(object?), MarshalMode.Default, typeof(OleVariantMarshaller))]
// To ensure correct behavior, we will need to update the generator to use the "ref" shape for both of the following cases.
// Is this behavior we want to generalize in an opt-in way? Or should we just special-case this case for now?
[CustomMarshaller(typeof(object?), MarshalMode.UnmanagedToManagedIn, typeof(OleVariantMarshaller.RefPropogate))]
[CustomMarshaller(typeof(object?), MarshalMode.UnmanagedToManagedRef, typeof(OleVariantMarshaller.RefPropogate))]
public static class OleVariantMarshaller
{
    public static OleVariant ConvertToUnmanaged(object? managed);
    public static object? ConvertToManaged(OleVariant unmanaged);
    public static void Free(OleVariant unmanaged);

    public struct RefPropogate
    {
        public void FromUnmanaged(OleVariant unmanaged);
        public void FromManaged(object? managed);
        public OleVariant ToUnmanaged();
        public object? ToManaged();
        public void Free();
    }
}
```
## Consider support for IParsable<Uri> on System.Uri

**Approved** | [#runtime/92285](https://github.com/dotnet/runtime/issues/92285#issuecomment-1736132429) | [Video](https://www.youtube.com/watch?v=9z2P-pZ2Id4&t=0h39m21s)

Looks good as proposed.

When discussing if the TryParse and Parse should be implicit or explicit, we feel that having both TryCreate and TryParse is sub-optimal.

```C#
namespace System;

public class Uri : IParsable<Uri> // New interface with an explicit implementation
{
    static bool IParsable<Uri>.TryParse(string? s, IFormatProvider? provider, out Uri result) =>
        TryCreate(uriString, RelativeOrAbsolute, NoImplicitFilePaths, out result);
    static Uri IParsable<Uri>.Parse(string s, IFormatProvider? provider) =>
        new Uri(s, RelativeOrAbsolute, NoImplicitFilePaths);
}
```
