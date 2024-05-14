# API Review 05/14/2024

## Assembly.SetEntryAssembly()

**Approved** | [#runtime/101616](https://github.com/dotnet/runtime/issues/101616#issuecomment-2110742551) | [Video](https://www.youtube.com/watch?v=Is8C-WJHtIM&t=0h0m0s)

Looks good as proposed

```C#
namespace System.Reflection;

public partial class Assembly
{
    public static void SetEntryAssembly(Assembly assembly);
}
```
## Introduce `SwiftSelf<T>` and `SwiftIndirectResult` to represent Swift structs and enums in C#

**Approved** | [#runtime/100543](https://github.com/dotnet/runtime/issues/100543#issuecomment-2110766984) | [Video](https://www.youtube.com/watch?v=Is8C-WJHtIM&t=0h7m4s)

* Looks good as proposed
* The original (non-generic) SwiftSelf might no longer be used and may be deleted, but if it's still used then there will just be both `SwiftSelf`-on-`void*` and `SwiftSelf<T>`, which is fine.

```c#
namespace System.Runtime.InteropServices.Swift
{
    public readonly struct SwiftSelf<T> where T: unmanaged
    {
        public SwiftSelf<T>(T value)
        {
            Value = value;
        }

        public T Value { get; }
    }
 
    public readonly unsafe struct SwiftIndirectResult
    {
        public SwiftIndirectResult(void* value)
        {
            Value = value;
        }

        public void* Value { get; }
    }
}
```
## New attribute for interop-specific struct concerns

**Approved** | [#runtime/100896](https://github.com/dotnet/runtime/issues/100896#issuecomment-2110876749) | [Video](https://www.youtube.com/watch?v=Is8C-WJHtIM&t=0h20m48s)


* `Custom` implies customization, so let's rename it to `Extended`
* `Sequential` and `Union` => `CStruct`, `CUnion`
* We discussed separate attributes for things like RequiredDiscriminatorBits and decided that one big grab bag is the better approach (for now)

```c#
namespace System.Runtime.InteropServices
{
    public enum LayoutKind
    {
        Extended = 1,
    }

    [AttributeUsage(AttributeTargets.Struct)]
    public sealed class ExtendedLayoutAttribute : Attribute
    {
        public ExtendedLayoutAttribute(ExtendedLayoutKind kind) {}
        
        // Only valid for ExtendedLayoutKind.SwiftEnum
        public int RequiredDiscriminatorBits { get; set; }
    }

    public enum ExtendedLayoutKind
    {
        CStruct, // C-style struct
        CUnion, // C-style union
        SwiftStruct, // Swift struct
        SwiftEnum, // Swift enumeration
    }
}

namespace System.Runtime.InteropServices.Swift
{
    // Represents a pointer to a Swift object (for the purposes of calculating spare bits)
    public readonly unsafe struct SwiftObject
    {
        // Would be implemented to mask out the spare bits
        public void* Value { get; }
    }

    // Represents a bool value in Swift (for the purposes of calculating spare bits)
    public readonly unsafe struct SwiftBool
    {
        // Would be implemented to mask out the spare bits
        // or assign while preserving spare bits
        public bool Value { get; }
    }
}
```
## Update GeneratedRegex to support partial properties

**Approved** | [#runtime/77797](https://github.com/dotnet/runtime/issues/77797#issuecomment-2110899545) | [Video](https://www.youtube.com/watch?v=Is8C-WJHtIM&t=1h29m46s)

Looks good as proposed

```diff
-[AttributeUsage(AttributeTargets.Method, AllowMultiple = false, Inherited = false)]
+[AttributeUsage(AttributeTargets.Method | AttributeTargets.Property, AllowMultiple = false, Inherited = false)]
 public sealed class GeneratedRegexAttribute : Attribute
```
## Add collection builders for frozen collection types

**Approved** | [#runtime/92190](https://github.com/dotnet/runtime/issues/92190#issuecomment-2110915604) | [Video](https://www.youtube.com/watch?v=Is8C-WJHtIM&t=1h35m39s)

Looks good as proposed

```diff
namespace System.Collections.Frozen;

+ [System.Runtime.CompilerServices.CollectionBuilder(typeof(FrozenSet)), nameof(FrozenSet.Create)]
public abstract class FrozenSet<T> : ISet<T>, IReadOnlySet<T>, IReadOnlyCollection<T>, ICollection
{
    // ...
}

public static class FrozenSet
{
+    public static FrozenSet<T> Create<T>(params ReadOnlySpan<T> source);
+    public static FrozenSet<T> Create<T>(IEqualityComparer<T>? equalityComparer, params ReadOnlySpan<T> source);
    // ...
}
```
## `OverloadResolutionPriorityAttribute`

**Approved** | [#runtime/102173](https://github.com/dotnet/runtime/issues/102173#issuecomment-2110959421) | [Video](https://www.youtube.com/watch?v=Is8C-WJHtIM&t=1h40m54s)

Looks good as proposed.

```C#
namespace System.Runtime.CompilerServices;

[AttributeUsage(
    AttributeTargets.Method | AttributeTargets.Constructor | AttributeTargets.Property,
    AllowMultiple = false,
    Inherited = false)]
public sealed class OverloadResolutionPriorityAttribute(int priority)
{
    public int Priority => priority;
}
```
