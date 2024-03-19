# API Review 03/19/2024

## TypeNameParser API

**Approved** | [#runtime/97566](https://github.com/dotnet/runtime/issues/97566#issuecomment-2007850686) | [Video](https://www.youtube.com/watch?v=kKONKtQBr2U&t=0h0m0s)


* All properties that have the same values from something on `Type` should use the same name as Type.
  * If any are missed in the approval notes here, they should match what it says in Type, not what it says in these notes.
* IsElementalType has been renamed to IsSimple; and we will also add IsSimple to System.Type.
* UnderlyingType was split into GetElementType() and GetGenericTypeDefinition()
* Added AssemblySimpleName as part of answering the question of whether a type is assembly-qualified.
* There was a lively debate on whether or not the complexity property should exist.
  * It morphed into a `GetNodeCount()` method.
* GetGenericArguments now returns ImmutableArray<TypeName>
* I(Span)Parsable/I(Span)Formattable were brought up, and decided that they're not worth the complexity (since this type will be available for netstandard)
* We don't like the word "Strict" in StrictValidation, but no new name was provided yet
  * Maybe even breaking it into separate flags
  * It is removed for now, may come back in the future
* TypeNameParseOptions.AllowFullyQualifiedName has been removed
* TypeNameParseOptions.MaxTotalComplexity is now MaxNodes
* MaxNodes will have a default value, less than int.MaxValue, but the actual number is TBD.

```c#
namespace System.Reflection.Metadata
{
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

        public bool IsSimple { get; }
        
        public TypeName GetElementType();
        public TypeName GetGenericTypeDefinition();

        public static TypeName Parse(ReadOnlySpan<char> typeName, TypeNameParseOptions? options = null);

        public static bool TryParse(ReadOnlySpan<char> typeName, [NotNullWhenAttribute(true)] out TypeName? result, TypeNameParseOptions? options = null);
        
        public int GetArrayRank();
        
        public Reflection.AssemblyName? GetAssemblyName();
        
        public ImmutableArray<TypeName> GetGenericArguments();

        public int GetNodeCount();
    }

    public sealed class TypeNameParseOptions
    {
        public TypeNameParseOptions() { }

        public int MaxNodes { get; set; } = 10;
    }
}

namespace System
{
    partial class Type
    {
        public bool IsSimple { get; }
    }
}
```
## Expose ElementType and KeyType on JsonTypeInfo metadata.

**Approved** | [#runtime/77306](https://github.com/dotnet/runtime/issues/77306#issuecomment-2007887560) | [Video](https://www.youtube.com/watch?v=kKONKtQBr2U&t=1h13m48s)

Looks good as proposed.

```C#
public partial class JsonTypeInfo
{
    public Type? KeyType { get; } // relevant for JsonTypeInfoKind.Dictionary
    public Type? ElementType { get; } // relevant for JsonTypeInfoKind.Enumerable/Dictionary
}
```
## params ReadOnlySpan<T> overloads for existing params T[] overloads

**Approved** | [#runtime/77873](https://github.com/dotnet/runtime/issues/77873) | [Video](https://www.youtube.com/watch?v=kKONKtQBr2U&t=1h36m36s)

