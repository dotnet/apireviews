# API Review 01/27/2026

## IL Emit support for FunctionPointer

**Approved** | [#runtime/75348](https://github.com/dotnet/runtime/issues/75348#issuecomment-3806797867) | [Video](https://www.youtube.com/watch?v=qkCNatTijWg&t=0h0m0s)

Looks good as proposed.

```c#
 namespace System
 {
    public abstract partial class Type
    {
        public static Type MakeFunctionPointerSignatureType(Type? returnType, Type[]? parameterTypes, bool isUnmanaged = false, Type[]? callingConventions = null);
        public static Type MakeModifiedSignatureType(Type type, Type[]? requiredCustomModifiers, Type[]? optionalCustomModifiers);
        public virtual Type MakeFunctionPointerType(Type[]? parameterTypes, bool isUnmanaged = false) => throw new NotSupportedException(SR.NotSupported_SubclassOverride);
    }
 }

 namespace System.Reflection.Emit
 {
    public abstract partial class ILGenerator
    {
        public virtual void EmitCalli(Type functionPointerType) => throw new NotImplementedException();
    }
 }
```
## Parameterized CRC computations

**Approved** | [#runtime/123164](https://github.com/dotnet/runtime/issues/123164#issuecomment-3807129824) | [Video](https://www.youtube.com/watch?v=qkCNatTijWg&t=0h21m22s)


* Let's leave the Residue property out.
* Move computation into the existing Crc32/Crc64 classes, taking a parameter set as a constructor/static-method parameter.
* Drop CRC-16 and CRC-8 for now.

```c#
namespace System.IO.Hashing;

public partial class Crc32
{
    public Crc32ParameterSet ParameterSet { get; }

    public Crc32(Crc32ParameterSet parameterSet);

    public static byte[] Hash(Crc32ParameterSet parameterSet, byte[] source);
    public static byte[] Hash(Crc32ParameterSet parameterSet, ReadOnlySpan<byte> source);
    public static int Hash(Crc32ParameterSet parameterSet, ReadOnlySpan<byte> source, Span<byte> destination);
    public static uint HashToUInt32(Crc32ParameterSet parameterSet, ReadOnlySpan<byte> source);
    public static bool TryHash(Crc32ParameterSet parameterSet, ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
}

[System.CLSCompliantAttribute(false)]
public partial class Crc32ParameterSet
{
    internal Crc32ParameterSet();

    public uint Polynomial { get; }
    public uint InitialValue { get; }
    public uint FinalXorValue { get; }
    public bool ReflectInput { get; }
    public bool ReflectOutput { get; }

    public static Crc32ParameterSet Create(uint polynomial, uint initialValue, uint finalXorValue, bool reflectInput, bool reflectOutput);
}

public partial class Crc32ParameterSet
{
    public static Crc32ParameterSet Crc32 { get; }
    public static Crc32ParameterSet Crc32C { get; }
}

public partial class Crc64
{
    public Crc64ParameterSet ParameterSet { get; }

    public Crc64(Crc64ParameterSet parameterSet);

    public static byte[] Hash(Crc64ParameterSet parameterSet, byte[] source);
    public static byte[] Hash(Crc64ParameterSet parameterSet, ReadOnlySpan<byte> source);
    public static int Hash(Crc64ParameterSet parameterSet, ReadOnlySpan<byte> source, Span<byte> destination);
    public static ulong HashToUInt64(Crc64ParameterSet parameterSet, ReadOnlySpan<byte> source);
    public static bool TryHash(Crc64ParameterSet parameterSet, ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
}

[System.CLSCompliantAttribute(false)]
public partial class Crc64ParameterSet
{
    internal Crc64ParameterSet();

    public ulong Polynomial { get; }
    public ulong InitialValue { get; }
    public ulong FinalXorValue { get; }
    public bool ReflectInput { get; }
    public bool ReflectOutput { get; }

    public static Crc64ParameterSet Create(ulong polynomial, ulong initialValue, ulong finalXorValue, bool reflectInput, bool reflectOutput);
}

public partial class Crc64ParameterSet
{
    public static Crc64ParameterSet Crc64 { get; }
    public static Crc64ParameterSet Nvme { get; }
}
```
## Add IReadOnlySet<T> support to System.Text.Json

**Approved** | [#runtime/121314](https://github.com/dotnet/runtime/issues/121314#issuecomment-3807175919) | [Video](https://www.youtube.com/watch?v=qkCNatTijWg&t=1h29m39s)

Looks good as proposed.

```C#
namespace System.Text.Json.Serialization.Metadata;

public partial class JsonMetadataServices
{
#if NET
    public static JsonTypeInfo<TCollection> CreateIReadOnlySetInfo<TCollection, TElement>(
        JsonSerializerOptions options,
        JsonCollectionInfoValues<TCollection> collectionInfo)
    where TCollection : IReadOnlySet<TElement>;
#endif
}
```
## ExcludeFromCodeCoverageAttribute can't be applied to interfaces

**Approved** | [#runtime/123405](https://github.com/dotnet/runtime/issues/123405#issuecomment-3807199738) | [Video](https://www.youtube.com/watch?v=qkCNatTijWg&t=1h38m59s)

Looks good as proposed.

Actually, we went ahead and pre-approved StackTraceHidden, too.

```diff
namespace System.Diagnostics.CodeAnalysis
{
-    [AttributeUsage(AttributeTargets.Assembly | AttributeTargets.Class | AttributeTargets.Struct | AttributeTargets.Constructor | AttributeTargets.Method | AttributeTargets.Property | AttributeTargets.Event, Inherited = false, AllowMultiple = false)]
+    [AttributeUsage(AttributeTargets.Assembly | AttributeTargets.Class | AttributeTargets.Struct | AttributeTargets.Constructor | AttributeTargets.Method | AttributeTargets.Property | AttributeTargets.Event | AttributeTargets.Interface, Inherited = false, AllowMultiple = false)]
    public sealed class ExcludeFromCodeCoverageAttribute : Attribute;
}

namespace System.Diagnostics
{
-    [AttributeUsage(AttributeTargets.Class | AttributeTargets.Struct | AttributeTargets.Constructor | AttributeTargets.Method, Inherited = false)]
+    [AttributeUsage(AttributeTargets.Class | AttributeTargets.Struct | AttributeTargets.Constructor | AttributeTargets.Method | AttributeTargets.Interface, Inherited = false)]
    public sealed class StackTraceHiddenAttribute : Attribute;
}
```
