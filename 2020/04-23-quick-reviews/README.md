# Quick Reviews 4/23/2020

## Add DynamicallyAccessedMembersAttribute

**Approved** | [#runtime/33861](https://github.com/dotnet/runtime/issues/33861#issuecomment-618548274) | [Video](https://www.youtube.com/watch?v=1GkjoDSR7dE&t=0h0m0s)

* What about `MetadataLoadContext`?
	- Will need to make use of supressions, there is a separate attribute for that
* It feels the defaults are flipped
	- We should have `PublicMethods` and `NonPublicMethods` to make it clear what's being returned
	- The semantics should match `BindingFlags`
* Are attributes kept?
	- Depends, but there is a discussion on trimming those too
* Should the enum have an "All" member?
* We should have a `None` member with value `0`
* We should make sure the attribute can only be applied once is not inherited
* The linker tooling should warn if the attribute is applied to things that aren't `System.Type` or `System.String`, the linker should warn

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(
        AttributeTargets.Field |
        AttributeTargets.ReturnValue |
        AttributeTargets.GenericParameter |
        AttributeTargets.Parameter |
        AttributeTargets.Property)]
    public sealed class DynamicallyAccessedMembersAttribute : Attribute
    {
        public DynamicallyAccessedMembersAttribute(DynamicallyAccessedMemberKinds memberKinds)
        {
            MemberKinds = memberKinds;
        }

        public DynamicallyAccessedMemberKinds MemberKinds { get; }
    }

    [Flags]
    public enum DynamicallyAccessedMemberKinds
    {
        DefaultConstructor  = 0b00000000_00000001,
        PublicConstructors  = 0b00000000_00000011,
        Constructors        = 0b00000000_00000111,
        PublicMethods       = 0b00000000_00001000,
        Methods             = 0b00000000_00011000,
        PublicFields        = 0b00000000_00100000,
        Fields              = 0b00000000_01100000,
        PublicNestedTypes   = 0b00000000_10000000,
        NestedTypes         = 0b00000001_10000000,
        PublicProperties    = 0b00000010_00000000,
        Properties          = 0b00000110_00000000,
        PublicEvents        = 0b00001000_00000000,
        Events              = 0b00011000_00000000,
    }
}
```
## Add SuppressLinkerWarningAttribute

**Approved** | [#runtime/35339](https://github.com/dotnet/runtime/issues/35339#issuecomment-618570506) | [Video](https://www.youtube.com/watch?v=1GkjoDSR7dE&t=0h48m15s)

* We should position it as a peer to `SuppressMessageAttribute`, `UnconditionalSuppressMessageAttribute`
* It should be `System.Diagnostics.CodeAnalysis`
* The behavior is the same, except it's not marked `[Conditonal]` and thus will always remain in metadata 
* We considered establishing an inheritance relationshop with `SuppressMessageAttribute` but concluded we don't need it (neither for docs nor for consumption)

```C#
namespace System.Diagnostics.CodeAnalysis
{
    [AttributeUsage(AttributeTargets.All, Inherited = false, AllowMultiple = true)]
    public sealed class UnconditionalSuppressMessageAttribute : Attribute
    {
    	// Same API as SuppressMessageAttribute
    }
}
```
## Consider PreserveDependencyAttribute to help linker

**Approved** | [#runtime/30902](https://github.com/dotnet/runtime/issues/30902#issuecomment-618601805) | [Video](https://www.youtube.com/watch?v=1GkjoDSR7dE&t=1h24m22s)

* It seems wild cards are also supported, which raises a few questions:
	- Does it include private and public APIs or just public?
	- How hard is it for upstream dependencies to implement (e.g. ILSpy or VS Find All Usages)
	- Do we need them? We'd like because it seems [there are places](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Linq.Expressions/src/System/Runtime/CompilerServices/CallSite.cs#L278-L319) where we have a lot.
* The linker should at least warn when the strings can't be matched
* `typeName` and `assemblyName` are optional because they default to same and same assembly
* Looks like `memberSignature` should be non-nullable
* Condition is some specific string that is interpreted by the linker, today that's only `DEBUG` (which unfortunately doesn't map to the C# notion of compiling a debug build)

```C#
namespace System.Diagnostics.CodeAnalysis
{
    [AttributeUsage(AttributeTargets.Method |
                    AttributeTargets.Constructor |
                    AttributeTargets.Field,
                    AllowMultiple = true,
                    Inherited = false)]
    public sealed class DynamicDependencyAttribute : Attribute
    {
        public DynamicDependencyAttribute(string memberSignature);
        public DynamicDependencyAttribute(string memberSignature, string typeName, string assemblyName);
        public DynamicDependencyAttribute(DynamicallyAccessedMemberKinds memberKinds, string typeName, string assemblyName);

        public DynamicDependencyAttribute(string memberSignature, Type type);
        public DynamicDependencyAttribute(DynamicallyAccessedMemberKinds memberKinds, Type type);

        public DynamicallyAccessedMemberKinds? MemberKinds { get; }
        public string? MemberSignature { get; }
        public string? TypeName { get; }
        public string? AssemblyName { get; }
        public string? Condition { get; set; }
    }
}
```

