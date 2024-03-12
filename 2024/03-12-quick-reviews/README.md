# API Review 03/12/2024

## Add PersistedAssemblyBuilder type

**Approved** | [#runtime/97015](https://github.com/dotnet/runtime/issues/97015#issuecomment-1992201440) | [Video](https://www.youtube.com/watch?v=uaB7vBT-V7g&t=0h0m0s)

Breaking API compatibility from .NET Framework (by having Save somewhere other than on AssemblyBuilder) is unfortunate; but seems tolerable based on expected usage.

Given that, looks good as proposed.

```diff
namespace System.Reflection.Emit;

public abstract partial class AssemblyBuilder
{
    // Previously approved new APIs
-   public static AssemblyBuilder DefinePersistedAssembly(AssemblyName name, Assembly coreAssembly, IEnumerable<CustomAttributeBuilder>? assemblyAttributes = null);

-   public void Save(Stream stream);
-   public void Save(string assemblyFileName);
-   protected abstract void SaveCore(Stream stream);
}

+public sealed class PersistedAssemblyBuilder : AssemblyBuilder
{
+   public PersistedAssemblyBuilder(AssemblyName name, Assembly coreAssembly, IEnumerable<CustomAttributeBuilder>? assemblyAttributes = null);

+   public void Save(Stream stream);
+   public void Save(string assemblyFileName);
+   public MetadataBuilder GenerateMetadata(out BlobBuilder ilStream, out BlobBuilder mappedFieldData);
}
```
## TypeNameParser API

**NeedsWork** | [#runtime/97566](https://github.com/dotnet/runtime/issues/97566#issuecomment-1992369765) | [Video](https://www.youtube.com/watch?v=uaB7vBT-V7g&t=0h28m27s)

* All properties that have the same values from something on `Type` should use the same name as Type.
  * If any are missed in the approval notes here, they should match what it says in Type, not what it says in these notes.
* IsElementalType is just `TotalComplexity == 1`.  It's cut.  (Though maybe coming back)
* UnderlyingType was split into GetElementType() and GetGenericTypeDefinition()
* Added AssemblySimpleName as part of answering the question of whether a type is assembly-qualified.
* There was a lively debate on whether or not the complexity property should exist.  If it does exist, the group seemed to be favoring renaming it from `TotalComplexity` to just `Complexity`.
* GetGenericArguments should return ReadOnlyMemory, not ReadOnlySpan.
* We don't like the word "Strict" in StrictValidation, but no new name was provided yet
  * Maybe even breaking it into separate flags

```c#
namespace System.Reflection.Metadata;

public sealed class TypeName : IEquatable<TypeName>
{
    internal TypeName() { }
    
    public string? AssemblySimpleName { get; }
    public string AssemblyQualifiedName { get; }
    
    public TypeName? DeclaringType { get; }

    public string FullName { get; }
    
    public bool IsArray { get; }
    public bool IsConstructedGenericType { get; }
    public bool IsByRef { get; }
    public bool IsNested { get; }
    public bool IsSZArray { get; }
    public bool IsPointer { get; }
    public bool IsVariableBoundArray { get; }
  
    public string Name { get; }

    public int Complexity { get; }
    
    public TypeName GetElementType();
    public TypeName GetGenericTypeDefinition();

    public static TypeName Parse(ReadOnlySpan<char> typeName, TypeNameParseOptions? options = null);

    public static bool TryParse(ReadOnlySpan<char> typeName, [NotNullWhenAttribute(true)] out TypeName? result, TypeNameParseOptions? options = null);
    
    public int GetArrayRank();
    
    public Reflection.AssemblyName? GetAssemblyName();
    
    public ReadOnlyMemory<TypeName> GetGenericArguments();
}

public sealed class TypeNameParseOptions
{
    public TypeNameParseOptions() { }
    public bool AllowFullyQualifiedName { get; set; } = true;
    
    public int MaxTotalComplexity { get; set; } = 10;
    
    public bool StrictValidation { get; set; } = false;
}
```
