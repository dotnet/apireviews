# API Review 02/13/2025

## Better support for unmanaged-like types

**Approved** | [#roslyn/76928](https://github.com/dotnet/roslyn/issues/76928#issuecomment-2657799308)

### API Review

* Maybe we should also expose Size and PackSize?
    * Yes, we think we should do this now, as one struct, rather than waiting for the next request
* Naming:
    * Internal structure currently uses Alignment, we'll rename to follow 335's term
    * StructLayoutAttribute can technically be applied to reference types, so we'll just call the
      struct TypeLayout

Finalized shape:

```cs
namespace Microsoft.CodeAnalysis
{
    public interface INamedTypeSymbol
    {
        public TypeLayout TypeLayout { get; }
    }

    /// <summary>
    /// Type layout information.
    /// </summary>
    public readonly struct TypeLayout : IEquatable<TypeLayout>
    {
        /// <summary>
        /// Layout kind (Layout flags in metadata).
        /// </summary>
        public LayoutKind Kind { get; }
 
        /// <summary>
        /// Packing size (PackingSize field in metadata).
        /// </summary>
        public ushort PackingSize { get; }
 
        /// <summary>
        /// Size of the type (The size field in metadata).
        /// </summary>
        public int Size { get; }

        public bool Equals(TypeLayout other);
 
        public override bool Equals(object? obj);
 
        public override int GetHashCode();
    }
}
```

**Approved**
## Public API for partial events

**Approved** | [#roslyn/77203](https://github.com/dotnet/roslyn/issues/77203#issuecomment-2657800442)

### API Review

* Seems consistent

**Approved as proposed**
## `SymbolDisplay.ToDisplayString(IntPtrTypeSymbol, FullnameFormat)` returns `nint` instead of `System.IntPtr` with .NET 8.0 SDK

**Approved** | [#roslyn/76895](https://github.com/dotnet/roslyn/issues/76895#issuecomment-2657814053)

### API Review

* On < .NET 7 (without `System.Runtime.CompilerServices.RuntimeFeature.NumericIntPtr`), when C# 9 or later is used, we use a separate wrapper type for `nint` vs `System.IntPtr`.
  * In these scenarios, the types are truly different (have different APIs and semantics), and we do not think `SymbolDisplayMiscellaneousOptions.UseSpecialTypes` should control whether `nint` is used. We feel that it is more likely for generators and refactorings to be successful by default if they use the type they were given.
* On > .NET 9 (or any platform with `System.Runtime.CompilerServices.RuntimeFeature.NumericIntPtr`), we do not use a wrapper type.
  * On these platforms, `nint` and `System.IntPtr` are truly equivalent, we think that `SymbolDisplayMiscellaneousOptions.UseSpecialTypes` can control whether `nint` is used without any pit of failures.

Given this, we came up with the following table:

| `S.R.CS.RuntimeFeature.NumericIntPtr` Present  | Uses Wrapper Type | SymbolDisplay uses keyword (behavior prior to #76895) | Proposed new behavior |
|-------------------------------------------------------------------------|-------------------|------------------------------------------------|----------|
| No | Yes | Yes for the wrapper, no for System.IntPtr | Keep behavior as is |
| Yes | No | Yes (always, no wrapper is used) | `SymbolDisplayMiscellaneousOptions.UseSpecialTypes` will control whether `nint` or `System.IntPtr` is used |

This is a minor breaking change for users of `ToDisplayString`, but we think that, for the most part, behavior will be better than it was before.

**Approved**
