# API Review 11/03/2026

## Generator-facing JSExport registration and function-pointer binding

**Approved** | [#runtime/136464](https://github.com/dotnet/runtime/issues/136464#issuecomment-6612273705) | [Video](https://www.youtube.com/watch?v=wtlDhc3YrhU&t=0h0m0s)


* Even though it's only accessible via infrastructure code, changing `string[]` to `ReadOnlySpan<string>` is more aligned with our current API designs (preventing caller modification of a reused array)

```csharp
namespace System.Runtime.InteropServices.JavaScript;

[SupportedOSPlatform("browser")]
[EditorBrowsable(EditorBrowsableState.Never)]
public abstract class JSExportProvider
{
    protected JSExportProvider();

    [DebuggerNonUserCode]
    public abstract ReadOnlySpan<string> GetExportNames();

    [DebuggerNonUserCode]
    public abstract void Bind(string methodId);
}

// Existing infrastructure type; only the proposed overload is shown.
[SupportedOSPlatform("browser")]
[CLSCompliant(false)]
[EditorBrowsable(EditorBrowsableState.Never)]
public sealed partial class JSFunctionBinding
{
    [EditorBrowsable(EditorBrowsableState.Never)]
    [DebuggerNonUserCode]
    public static unsafe JSFunctionBinding BindManagedFunction(
        string fullyQualifiedName,
        int signatureHash,
        ReadOnlySpan<JSMarshalerType> signatures,
        delegate*<JSMarshalerArgument*, void> callback);
}
```
## [DatabaseLoader] Create higher level convenient methods for DatabaseLoader

**Approved** | [#machinelearning/4182](https://github.com/dotnet/machinelearning/issues/4182#issuecomment-6612555955) | [Video](https://www.youtube.com/watch?v=wtlDhc3YrhU&t=0h27m28s)

Looks good as proposed

```csharp
namespace Microsoft.ML
{
    public static class DatabaseLoaderCatalog
    {
        public static IDataView LoadFromDatabase(
            this DataOperationsCatalog catalog,
            DbProviderFactory providerFactory,
            string connectionString,
            string commandText,
            params DatabaseLoader.Column[] columns);

        public static IDataView LoadFromDatabase(
            this DataOperationsCatalog catalog,
            DbProviderFactory providerFactory,
            string connectionString,
            string commandText,
            DatabaseLoader.Options options);

        public static IDataView LoadFromDatabase<TInput>(
            this DataOperationsCatalog catalog,
            DbProviderFactory providerFactory,
            string connectionString,
            string commandText);

        public static IDataView LoadFromDatabase(
            this DataOperationsCatalog catalog,
            DatabaseSource source,
            params DatabaseLoader.Column[] columns);

        public static IDataView LoadFromDatabase(
            this DataOperationsCatalog catalog,
            DatabaseSource source,
            DatabaseLoader.Options options);

        public static IDataView LoadFromDatabase<TInput>(
            this DataOperationsCatalog catalog,
            DatabaseSource source);
    }
}
```
## Implement `ISpanFormattable` on `StringBuilder`

**Approved** | [#runtime/105870](https://github.com/dotnet/runtime/issues/105870#issuecomment-6613206774) | [Video](https://www.youtube.com/watch?v=wtlDhc3YrhU&t=0h41m0s)


* Let's preemptively add IUtf8SpanFormattable
* We've mirrored the partial-copy capability of ToString(startIndex, length) with TryFormat(destination, sourceRange)

```csharp
namespace System.Text;

public partial class StringBuilder : IFormattable, ISpanFormattable, IUtf8SpanFormattable
{
    public bool TryFormat(Span<char> destination, out int charsWritten);
    public bool TryFormat(Span<char> destination, Range sourceRange, out int charsWritten);

    public bool TryFormat(Span<byte> utf8Destination, out int bytesWritten);
    public bool TryFormat(Span<byte> utf8Destination, Range sourceRange, out int bytesWritten);

    string IFormattable.ToString(string? format, IFormatProvider? formatProvider);
    bool ISpanFormattable.TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format, IFormatProvider? provider);
    bool IUtf8SpanFormattable.TryFormat(Span<byte> utf8Destination, out int bytesWritten, ReadOnlySpan<char> format, IFormatProvider? provider);
}
```


## Trim-safe Type.GetInterfaces() filtering output based on type definition

**Approved** | [#runtime/136546](https://github.com/dotnet/runtime/issues/136546#issuecomment-6614036360) | [Video](https://www.youtube.com/watch?v=wtlDhc3YrhU&t=1h14m25s)


* The normal name for a Type parameter representing an interface in the Type class is `interfaceType`, so let's match.
* GetInterfacesWithSameMetadataDefinitionAs feels wordy, but matches GetMemberWithSameMetadataDefinitionAs, so, it is what it is.

```csharp
namespace System;

public partial abstract class Type
{
    public virtual Type[] GetInterfacesWithSameMetadataDefinitionAs(Type interfaceType);
}
```


