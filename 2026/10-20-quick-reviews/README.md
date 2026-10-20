# API Review 10/20/2026

## `[LibraryExport]` analogous to `[JSExport]`

**Approved** | [#runtime/79786](https://github.com/dotnet/runtime/issues/79786#issuecomment-6299200273) | [Video](https://www.youtube.com/watch?v=OWQkgWF5W6Y&t=0h0m0s)

We heavily edited to separate the notion of "export" (which includes naming the function to an external caller) and "generate the native to managed stub doing the marshalling I said, and ended up with two attributes:

```csharp
namespace System.Runtime.InteropServices
{
    [AttributeUsage(AttributeTargets.Method, AllowMultiple=true, Inherited=false)]
    public sealed class LibraryExportAttribute : Attribute
    {
        public LibraryExportAttribute();

        public string? EntryPoint { get; set; }

        public StringMarshalling StringMarshalling { get; set; }
        public Type? StringMarshallingCustomType { get; set; }
    }

    [AttributeUsage(AttributeTargets.Method, AllowMultiple=true, Inherited=false)]
    public sealed class GenerateUnmanagedCallersOnlyAttribute : Attribute
    {
        public GenerateUnmanagedCallersOnlyAttribute(string stubName);

        public string StubName { get; }

        public StringMarshalling StringMarshalling { get; set; }
        public Type? StringMarshallingCustomType { get; set; }
    }
}
```
