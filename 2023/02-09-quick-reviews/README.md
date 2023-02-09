# API Review 02/09/2023

## COM source generator APIs

**Approved** | [#runtime/79121](https://github.com/dotnet/runtime/issues/79121#issuecomment-1424764604)

* The generic on GeneratedComInterfaceAttribute is being removed (per the feature team)
* Consider moving GeneratedComInterfaceAttribute to System.Runtime.InteropServices for visibility and parity/discovery with ComImportAttribute
* `GeneratedComWrappersBase` needs a new name because a) all feature users need to derive from it (so it shouldn't use the suffix "Base") and b) the type isn't tied to sourcegen, it could be successfully used independent of the generators.
  * Suggestions were `ComObjectComWrappers` and `StrategyBasedComWrappers` (`StrategyBasedComWrappers` was preferred by the feature team in the meeting)
* `GeneratedComWrappersBase.GetOrCreateUniqueObjectForComInstance` should be removed until there's a demonstrated need for it to be public.
* The `DefaultIUnknownInterfaceDetailsStrategy`, `FreeThreadedStrategy`, and `DefaultCaching` types don't need to be public.
  * `DefaultIUnknownInterfaceDetailsStrategy.Instance` => `(public static) GeneratedComWrappersBase.DefaultIUnknownInterfaceDetailsStrategy { get; }`
  * `FreeThreadedStrategy.Instance` => `(public static) GeneratedComWrappersBase.DefaultIUnknownStrategy { get; }`
  * `DefaultCaching..ctor()` => `(protected static) GeneratedComWrappersBase.CreateDefaultCacheStrategy()`
* `CreateInterfaceDetailsStrategy` => `GetOrCreateInterfaceDetailsStrategy` (to indicate reuse is permitted)
* `CreateIUnknownStrategy` => `GetOrCreateIUnknownStrategy` (to indicate reuse is permitted)
* (`CreateCacheStrategy` does not get "GetOr-" to indicate that reuse is NOT permitted)
* `IIUnknownStrategy.Release`'s return type should be `uint`, not `int`, and parameter should be `instance` instead of `instancePtr`
* `VirtualMethodTableInfo.ThisPointer` => `VirtualMethodTableInfo.Instance` (and updated in parameters accordingly)
* `IUnmanagedVirtualMethodTableProvider.GetVirtualMethodTableInfoForKey` => `IUnmanagedVirtualMethodTableProvider.GetVirtualMethodTableInfoForType`
* `VirtualMethodTableManagedImplementation`'s property type should be `void**` instead of `void*`, for consistency with other vtable exposure
* `VirtualMethodTableManagedImplementation` => `ManagedVirtualMethodTable`
* `IIUnknownStrategy.QueryInterface` should take `iid` by value, not by `in` (for all the reasons why `in` is hard)
* `IUnknownDerivedAttribute` should be sealed.
* `IUnknownDerivedDetails` => `IDerivedIUnknownDetails` or `IIUnknownDerivedDetails` ("II" seems to be preferred)
* `IIUnknownStrategy.QueryInterface`'s first parameter `instancePtr` => `instance`
* `IIUnknownCacheStrategy.TableInfo` property renames: `ThisPtr` => `Instance`, `Table` => `VirtualMethodTable`, `ManagedType` => `Implementation`

(Some updates from the previous session may not have been applied to the surface here, but it should be pretty close to correct.)

```C#
using System.Collections;

namespace System.Runtime.InteropServices.Marshalling;

/// <summary>
/// Information about a virtual method table and the unmanaged instance pointer.
/// </summary>
public readonly unsafe struct VirtualMethodTableInfo
{
    /// <summary>
    /// Construct a <see cref="VirtualMethodTableInfo"/> from a given instance pointer and table memory.
    /// </summary>
    /// <param name="instance">The pointer to the instance.</param>
    /// <param name="virtualMethodTable">The block of memory that represents the virtual method table.</param>
    public VirtualMethodTableInfo(void* instance, void** virtualMethodTable);

    /// <summary>
    /// The unmanaged instance pointer
    /// </summary>
    public void* Instance { get; }

    /// <summary>
    /// The virtual method table.
    /// </summary>
    public void** VirtualMethodTable { get; }

    /// <summary>
    /// Deconstruct this structure into its two fields.
    /// </summary>
    /// <param name="instance">The <see cref="Instance"/> result</param>
    /// <param name="virtualMethodTable">The <see cref="VirtualMethodTable"/> result</param>
    public void Deconstruct(out void* instance, out void** virtualMethodTable);
}

/// <summary>
/// This interface allows an object to provide information about a virtual method table for a managed interface to enable invoking methods in the virtual method table.
/// </summary>
public interface IUnmanagedVirtualMethodTableProvider
{
    /// <summary>
    /// Get the information about the virtual method table for a given unmanaged interface type represented by <paramref name="type"/>.
    /// </summary>
    /// <param name="type">The managed type for the unmanaged interface.</param>
    /// <returns>The virtual method table information for the unmanaged interface.</returns>
    VirtualMethodTableInfo GetVirtualMethodTableInfoForKey(Type type);
}

/// <summary>
///  Identify the Interface ID (IID) for an IUnknown based interface.
/// </summary>
public interface IIUnknownInterfaceType : IUnmanagedInterfaceType
{
    public abstract static Guid Iid { get; }

    /// <summary>
    /// Get a pointer to the virtual method table of managed implementations of the unmanaged interface type.
    /// </summary>
    /// <returns>A pointer to the virtual method table of managed implementations of the unmanaged interface type</returns>
    /// <remarks>
    /// Implementation will be provided by a source generator if not explicitly implemented.
    /// This property can return <c>null</c>. If it does, then the interface is not supported for passing managed implementations to unmanaged code.
    /// </remarks>
    public abstract static unsafe void** ManagedVirtualMethodTable { get; }
}

/// <summary>
/// Details for the IUnknown derived interface.
/// </summary>
public interface IIUnknownDerivedDetails
{
    /// <summary>
    /// Interface ID.
    /// </summary>
    Guid Iid { get; }

    /// <summary>
    /// Managed type used to project the IUnknown derived interface.
    /// </summary>
    Type Implementation { get; }

    /// <summary>
    /// A pointer to the virtual method table to enable unmanaged callers to call a managed implementation of the interface.
    /// </summary>
    unsafe void** ManagedVirtualMethodTable { get; }
}

[AttributeUsage(AttributeTargets.Interface)]
public sealed class IIUnknownDerivedAttribute<T, TImpl> : Attribute, IIUnknownDerivedDetails
    where T : IIUnknownInterfaceType
    where TImpl : T
{
    /// <inheritdoc />
    public Guid Iid { get; }

    /// <inheritdoc />
    public Type Implementation { get; }

    /// <inheritdoc />
    public unsafe void** ManagedVirtualMethodTable { get; }
}

/// <summary>
/// IUnknown interaction strategy.
/// </summary>
public unsafe interface IIUnknownStrategy
{
    /// <summary>
    /// Create an instance pointer that represents the provided IUnknown instance.
    /// </summary>
    /// <param name="unknown">The IUnknown instance.</param>
    /// <returns>A pointer representing the unmanaged instance.</returns>
    /// <remarks>
    /// This method is used to create an instance pointer that can be used to interact with the other members of this interface.
    /// For example, this method can return an IAgileReference instance for the provided IUnknown instance
    /// that can be used in the QueryInterface and Release methods to enable creating thread-local instance pointers to us
    /// through the IAgileReference APIs instead of directly calling QueryInterface on the IUnknown.
    /// </remarks>
    public void* CreateInstancePointer(void* unknown);

    /// <summary>
    /// Perform a QueryInterface() for an IID on the unmanaged instance.
    /// </summary>
    /// <param name="instance">A pointer representing the unmanaged instance.</param>
    /// <param name="interfaceId">The Interface ID (IID) to query for.</param>
    /// <param name="obj">The resulting interface</param>
    /// <returns>Returns an HRESULT represents the success of the operation</returns>
    /// <seealso cref="Marshal.QueryInterface(nint, ref Guid, out nint)"/>
    public int QueryInterface(void* instance, Guid interfaceId, out void* obj);

    /// <summary>
    /// Perform a Release() call on the supplied unmanaged instance.
    /// </summary>
    /// <param name="instance">A pointer representing the unmanaged instance.</param>
    /// <returns>The current reference count.</returns>
    /// <seealso cref="Marshal.Release(nint)"/>
    public uint Release(void* instance);
}

/// <summary>
/// Strategy for acquiring interface details.
/// </summary>
public interface IIUnknownInterfaceDetailsStrategy
{
    /// <summary>
    /// Given a <see cref="RuntimeTypeHandle"/> get the IUnknown details.
    /// </summary>
    /// <param name="type">RuntimeTypeHandle instance</param>
    /// <returns>Details if type is known.</returns>
    IUnknownDerivedDetails? GetIUnknownDerivedDetails(RuntimeTypeHandle type);
}

/// <summary>
/// Unmanaged virtual method table look up strategy.
/// </summary>
public unsafe interface IIUnknownCacheStrategy
{
    public readonly struct TableInfo
    {
        public void* Instance { get; init; }
        public void** VirtualMethodTable { get; init; }
        public RuntimeTypeHandle Implementation { get; init; }
    }

    /// <summary>
    /// Construct a <see cref="TableInfo"/> instance.
    /// </summary>
    /// <param name="handle">RuntimeTypeHandle instance</param>
    /// <param name="ptr">Pointer to the instance to query</param>
    /// <param name="info">A <see cref="TableInfo"/> instance</param>
    /// <returns>True if success, otherwise false.</returns>
    TableInfo ConstructTableInfo(RuntimeTypeHandle handle, IUnknownDerivedDetails interfaceDetails, void* ptr);

    /// <summary>
    /// Get associated <see cref="TableInfo"/>.
    /// </summary>
    /// <param name="handle">RuntimeTypeHandle instance</param>
    /// <param name="info">A <see cref="TableInfo"/> instance</param>
    /// <returns>True if found, otherwise false.</returns>
    bool TryGetTableInfo(RuntimeTypeHandle handle, out TableInfo info);

    /// <summary>
    /// Set associated <see cref="TableInfo"/>.
    /// </summary>
    /// <param name="handle">RuntimeTypeHandle instance</param>
    /// <param name="info">A <see cref="TableInfo"/> instance</param>
    /// <returns>True if set, otherwise false.</returns>
    bool TrySetTableInfo(RuntimeTypeHandle handle, TableInfo info);

    /// <summary>
    /// Clear the cache
    /// </summary>
    /// <param name="unknownStrategy">The <see cref="IIUnknownStrategy"/> to use for clearing</param>
    void Clear(IIUnknownStrategy unknownStrategy);
}

/// <summary>
/// Base class for all COM source generated Runtime Callable Wrapper (RCWs).
/// </summary>
public sealed class ComObject : IDynamicInterfaceCastable, IUnmanagedVirtualMethodTableProvider
{
    ~ComObject();

    /// <summary>
    /// Returns an IDisposable that can be used to perform a final release
    /// on this COM object wrapper.
    /// </summary>
    /// <remarks>
    /// This property will only be non-null if the ComObject was created using
    /// CreateObjectFlags.UniqueInstance.
    /// </remarks>
    public void FinalRelease();

    /// <inheritdoc />
    RuntimeTypeHandle IDynamicInterfaceCastable.GetInterfaceImplementation(RuntimeTypeHandle interfaceType);

    /// <inheritdoc />
    bool IDynamicInterfaceCastable.IsInterfaceImplemented(RuntimeTypeHandle interfaceType, bool throwIfNotImplemented);

    /// <inheritdoc />
    VirtualMethodTableInfo IUnmanagedVirtualMethodTableProvider.GetVirtualMethodTableInfoForKey(Type type);
}

[AttributeUsage(AttributeTargets.Interface)]
public sealed class GeneratedComInterfaceAttribute : Attribute
{
}

public abstract class StrategyBasedComWrappers : ComWrappers
{
    public static IIUnknownInterfaceDetailsStrategy DefaultIUnknownInterfaceDetailsStrategy { get; }

    public static IIUnknownStrategy DefaultIIUnknownStrategy { get; }

    protected static IIUnknownCacheStrategy CreateDefaultCacheStrategy();

    protected virtual IIUnknownInterfaceDetailsStrategy GetOrCreateInterfaceDetailsStrategy() => DefaultIUnknownInterfaceDetailsStrategy;

    protected virtual IIUnknownStrategy GetOrCreateIUnknownStrategy() => FreeThreadedStrategy;

    protected virtual IIUnknownCacheStrategy CreateCacheStrategy() => CreateDefaultCacheStrategy();

    protected override sealed unsafe object CreateObject(nint externalComObject, CreateObjectFlags flags);

    protected override sealed void ReleaseObjects(IEnumerable objects);
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
