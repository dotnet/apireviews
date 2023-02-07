# API Review 02/07/2023

## COM source generator APIs

**NeedsWork** | [#runtime/79121](https://github.com/dotnet/runtime/issues/79121#issuecomment-1421555838)

* `VirtualMethodTableInfo`
    - Do we need the `Deconstruct`? Yes, as it simplifies the generated code.
* `IUnmanagedVirtualMethodTableProvider`
    - Should we have a generic method `GetVirtualMethodTableInfoForKey()`? We concluded that we don't want to pay for the additional complexity for a usability gain.
* `ComObject.FinalRelease`
    - Should be a void returning method
    - Make it a no-op if the COM object can't be released
    - Make the second call a no-op
    - Throw `InvalidOperationException` rather than `ObjectDisposedException` when the COM object is used after calling FinalRelease

```C#
using System.Collections;

namespace System.Runtime.InteropServices.Marshalling;

public readonly unsafe struct VirtualMethodTableInfo
{
    public VirtualMethodTableInfo(void* thisPointer, void** virtualMethodTable);
    public void* ThisPointer { get; }
    public void** VirtualMethodTable { get; }
    public void Deconstruct(out void* thisPointer, out void** virtualMethodTable);
}

public interface IUnmanagedVirtualMethodTableProvider
{
    VirtualMethodTableInfo GetVirtualMethodTableInfoForKey(Type type);
}

public interface IUnmanagedInterfaceType
{
    public abstract static unsafe void* VirtualMethodTableManagedImplementation { get; }
}

public interface IIUnknownInterfaceType : IUnmanagedInterfaceType
{
    public abstract static Guid Iid { get; }
}

public interface IUnknownDerivedDetails
{
    Guid Iid { get; }
    Type Implementation { get; }
    unsafe void* VirtualMethodTableManagedImplementation { get; }
}

[AttributeUsage(AttributeTargets.Interface)]
public class IUnknownDerivedAttribute<T, TImpl> : Attribute, IUnknownDerivedDetails
    where T : IIUnknownInterfaceType
    where TImpl : T
{
    public Guid Iid { get; }
    public Type Implementation { get; }
    public unsafe void* VirtualMethodTableManagedImplementation { get; }
}

public unsafe interface IIUnknownStrategy
{
    void* CreateInstancePointer(void* unknown);
    int QueryInterface(void* instancePtr, in Guid iid, out void* ppObj);
    int Release(void* instancePtr);
}

public interface IIUnknownInterfaceDetailsStrategy
{
    IUnknownDerivedDetails? GetIUnknownDerivedDetails(RuntimeTypeHandle type);
}

public unsafe interface IIUnknownCacheStrategy
{
    public readonly struct TableInfo
    {
        public void* ThisPtr { get; init; }
        public void** Table { get; init; }
        public RuntimeTypeHandle ManagedType { get; init; }
    }

    TableInfo ConstructTableInfo(RuntimeTypeHandle handle, IUnknownDerivedDetails interfaceDetails, void* ptr);
    bool TryGetTableInfo(RuntimeTypeHandle handle, out TableInfo info);
    bool TrySetTableInfo(RuntimeTypeHandle handle, TableInfo info);
    void Clear(IIUnknownStrategy unknownStrategy);
}

public sealed class ComObject : IDynamicInterfaceCastable, IUnmanagedVirtualMethodTableProvider
{
    ~ComObject();
    public IDisposable? FinalRelease { get; }
    RuntimeTypeHandle IDynamicInterfaceCastable.GetInterfaceImplementation(RuntimeTypeHandle interfaceType);
    bool IDynamicInterfaceCastable.IsInterfaceImplemented(RuntimeTypeHandle interfaceType, bool throwIfNotImplemented);
    VirtualMethodTableInfo IUnmanagedVirtualMethodTableProvider.GetVirtualMethodTableInfoForKey(Type type);
}

[AttributeUsage(AttributeTargets.Interface)]
public sealed class GeneratedComInterfaceAttribute<TComWrappers> : Attribute
    where TComWrappers : GeneratedComWrappersBase
{
}

public abstract class GeneratedComWrappersBase : ComWrappers
{
    protected virtual IIUnknownInterfaceDetailsStrategy CreateInterfaceDetailsStrategy() => DefaultIUnknownInterfaceDetailsStrategy.Instance;
    protected virtual IIUnknownStrategy CreateIUnknownStrategy() => FreeThreadedStrategy.Instance;
    protected virtual IIUnknownCacheStrategy CreateCacheStrategy() => new DefaultCaching();
    protected override sealed unsafe object CreateObject(nint externalComObject, CreateObjectFlags flags);
    protected override sealed void ReleaseObjects(IEnumerable objects);
    public ComObject GetOrCreateUniqueObjectForComInstance(nint comInstance, CreateObjectFlags flags);
}

public sealed class DefaultIUnknownInterfaceDetailsStrategy : IIUnknownInterfaceDetailsStrategy
{
    public static readonly IIUnknownInterfaceDetailsStrategy Instance;
    public IUnknownDerivedDetails? GetIUnknownDerivedDetails(RuntimeTypeHandle type);
}

public sealed unsafe class FreeThreadedStrategy : IIUnknownStrategy
{
    public static readonly IIUnknownStrategy Instance;
    void* IIUnknownStrategy.CreateInstancePointer(void* unknown);
    unsafe int IIUnknownStrategy.QueryInterface(void* thisPtr, in Guid handle, out void* ppObj);
    unsafe int IIUnknownStrategy.Release(void* thisPtr);
}

public sealed unsafe class DefaultCaching : IIUnknownCacheStrategy
{
    private readonly Dictionary<RuntimeTypeHandle, IIUnknownCacheStrategy.TableInfo> _cache = new();
    IIUnknownCacheStrategy.TableInfo IIUnknownCacheStrategy.ConstructTableInfo(RuntimeTypeHandle handle, IUnknownDerivedDetails details, void* ptr);
    bool IIUnknownCacheStrategy.TryGetTableInfo(RuntimeTypeHandle handle, out IIUnknownCacheStrategy.TableInfo info);
    bool IIUnknownCacheStrategy.TrySetTableInfo(RuntimeTypeHandle handle, IIUnknownCacheStrategy.TableInfo info);
    void IIUnknownCacheStrategy.Clear(IIUnknownStrategy unknownStrategy);
}
```
