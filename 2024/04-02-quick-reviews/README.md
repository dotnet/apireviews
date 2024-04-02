# API Review 04/02/2024

## Add APIs that needed for emitting symbols with reflection emit PersistedAssemblyBuilder

**Approved** | [#runtime/99935](https://github.com/dotnet/runtime/issues/99935#issuecomment-2032668480) | [Video](https://www.youtube.com/watch?v=D8yEKyCdTRA&t=0h0m0s)


* We added ModuleBuilder.DefineDocument taking 4 parameters for better signature/module compatibility with netfx
* Ref.Emit has already been using the Template Method Pattern, avoiding "public virtual", and should continue to do so.
* PersistedAssemblyBuilder.GenerateMetadata, we renamed the new parameter `pdbBuilder`

```c#
namespace System.Reflection.Emit;

public abstract partial class ModuleBuilder : System.Reflection.Module
{
    [EditorBrowsable(Never)]
    public ISymbolDocumentWriter DefineDocument (string url, Guid language, Guid languageVendor, Guid documentType) => DefineDocument(url, language);

    public ISymbolDocumentWriter DefineDocument(string url, Guid language = default);
    protected virtual ISymbolDocumentWriter DefineDocumentCore(string url, Guid language = default);
}

 public abstract partial class ILGenerator
 {
    public void MarkSequencePoint(ISymbolDocumentWriter document, int startLine, int startColumn, int endLine, int endColumn) { }
    protected virtual void MarkSequencePointCore(ISymbolDocumentWriter document, int startLine, int startColumn, int endLine, int endColumn) { }
 }
 
public abstract partial class LocalBuilder : LocalVariableInfo
{
    public void SetLocalSymInfo(string name);
    protected virtual void SetLocalSymInfoCore(string name) { }
}

public sealed class PersistedAssemblyBuilder : AssemblyBuilder
{
    public MetadataBuilder GenerateMetadata(out BlobBuilder ilStream, out BlobBuilder mappedFieldData, out MetadataBuilder pdbBuilder) { }
}
```
## RuntimeHelpers.Box to create a box around a dynamically-typed byref

**Approved** | [#runtime/97341](https://github.com/dotnet/runtime/issues/97341#issuecomment-2032677180) | [Video](https://www.youtube.com/watch?v=D8yEKyCdTRA&t=0h32m25s)


* Looks good as proposed

```c#
namespace System.Runtime.CompilerServices;

public static partial class RuntimeHelpers
{
     public object? Box(ref byte target, RuntimeTypeHandle type);
}
```
## Get the managed size of a System.Type instance

**Approved** | [#runtime/97344](https://github.com/dotnet/runtime/issues/97344#issuecomment-2032680084) | [Video](https://www.youtube.com/watch?v=D8yEKyCdTRA&t=0h37m28s)


* Looks good as proposed

```c#
namespace System.Runtime.CompilerServices;

public partial class RuntimeHelpers
{
     public static int SizeOf(RuntimeTypeHandle type);
}
```
## Provide vector functions covering common "mask" checks

**Approved** | [#runtime/98055](https://github.com/dotnet/runtime/issues/98055#issuecomment-2032720254) | [Video](https://www.youtube.com/watch?v=D8yEKyCdTRA&t=0h38m14s)


* We added a scalar parameter to all of these to allow expressing what the match should be on
* While silmutaneously renaming the existing unary ones to express "AllBitsSet"
* And changed things to look like existing concept names (IndexOfLast => LastIndexOf)

```c#
namespace System.Numerics
{
    public partial class Vector
    {
        public static bool Any<T>(Vector<T> vector, T value);
        public static bool AnyWhereAllBitsSet<T>(Vector<T> vector);

        public static int Count<T>(Vector<T> vector, T value);
        public static int CountWhereAllBitsSet<T>(Vector<T> vector);

        public static int IndexOf<T>(Vector<T> vector, T value);
        public static int IndexOfWhereAllBitsSet<T>(Vector<T> vector);
        
        public static int LastIndexOf<T>(Vector<T> vector, T value);
        public static int LastIndexOfWhereAllBitsSet<T>(Vector<T> vector);
    }
}

namespace System.Runtime.Intrinsics
{
    public partial class Vector64
    {
        // ditto.
    }

    public partial class Vector128
    {
        // ditto.
    }

    public partial class Vector256
    {
        // ditto.
    }

    public partial class Vector512
    {
        // ditto.
    }
}
```
## Channel.CreateUnboundedPrioritized

**Approved** | [#runtime/62761](https://github.com/dotnet/runtime/issues/62761#issuecomment-2032793065) | [Video](https://www.youtube.com/watch?v=D8yEKyCdTRA&t=1h2m34s)

Looks good as proposed.

```diff
namespace System.Threading.Channels;

public class Channel
{
+   public static Channel<T> CreateUnboundedPrioritized<T>();
+   public static Channel<T> CreateUnboundedPrioritized<T>(UnboundedPrioritizedChannelOptions<T> options);
}
+public sealed partial class UnboundedPrioritizedChannelOptions<T> : ChannelOptions
+{
+   public System.Collections.Generic.IComparer<T>? Comparer { get; set; }
+}
```
## `InvalidEnumArgumentException` should support `uint/long/ulong`-backed enumerations

**Rejected** | [#runtime/95696](https://github.com/dotnet/runtime/issues/95696#issuecomment-2032835697) | [Video](https://www.youtube.com/watch?v=D8yEKyCdTRA&t=1h33m31s)

In API Review we felt that this type isn't generally used (and most usages that "should" use it use ArgumentOutOfRangeException instead), and we feel that there's not sufficient value in modifying this legacy type.
