# API Review 02/22/2023

## EnC API for method instrumentation

**Approved** | [#roslyn/66770](https://github.com/dotnet/roslyn/issues/66770#issuecomment-1439329717)

### API Review

* The removal needs to be just removing the default parameters and adding `EditorBrowsable.Never`, like we do for the rest of our compat shims
* Otherwise, looks good.

Approved.
## Adjust SymbolDisplay member options behavior for interface members

**Approved** | [#roslyn/66753](https://github.com/dotnet/roslyn/issues/66753#issuecomment-1439331789)

### API Review

* Also need to verify `static` non-abstract members
* Who is going to be hit by the `ToDisplayString()` change?
    * Error messages
    * People misusing the API for generating code (only `ITypeSymbol` has any sort of contract on producing valid C#)
* Classic interface behavior has been around for a decade
    * Putting `abstract` on classic interface members might give some areas bad behavior, and `public` has always been the default for interface members.

We'll go with "don't print the defaults, otherwise always print."
IncludeAccessibility ON | before | after
----|---|---
classic interface members | `void M()` | `void M()`
default interface implementations | `void M()` | `void M()`
static interface members | `void M()` | `void M()`
static abstract interface members | `void M()` | `void M()`

IncludeModifiers ON | before | after
----|---|---
classic interface members | `void M()` | `void M()`
default interface implementations | `void M()` | `virtual void M()`
static interface members | `void M()` | `static void M()`
static abstract interface members | `void M()` | `static abstract void M()`
## New APIs for "Primary Constructors"

**Approved** | [#roslyn/66914](https://github.com/dotnet/roslyn/issues/66914#issuecomment-1439332467)

### API Review

Updated definitions are approved.
