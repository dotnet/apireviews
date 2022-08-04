# API Review 08/04/2022

## Update `SymbolDisplay` to include `scoped` for parameters and locals

**NeedsWork** | [#roslyn/63208](https://github.com/dotnet/roslyn/issues/63208)

## represent `ref` fields and `scoped` parameters and locals

**Approved** | [#roslyn/61647](https://github.com/dotnet/roslyn/issues/61647#issuecomment-1205810012)

### API Review

The shape is generally approved, swapping the `IsScoped` for the `ScopedKind` previously discussed. We will also follow up with Cyrus when he's back from vacation on whether we should use the `ScopedTypeSyntax` for parameters, or whether we should be consistent with existing parameter parsing (putting the keyword as a modifier on the parameter).

```cs
namespace Microsoft.CodeAnalysis
{
    public interface IFieldSymbol : ISymbol
    {
       /// <summary>
       /// Returns the RefKind of the field.
       /// </summary>
       RefKind RefKind { get; }

       /// <summary>
       /// Custom modifiers associated with the ref modifier, or an empty array if there are none.
       /// </summary>
       ImmutableArray<CustomModifier> RefCustomModifiers { get; }
    }

    public interface ILocalSymbol : ISymbol
    {
       /// <summary>
       /// Returns the scope kind of the local.
       /// </summary>
       ScopedKind ScopedKind { get; }
    }

    public interface IParameterSymbol : ISymbol
    {
       /// <summary>
       /// Returns the scope kind of the parameter.
       /// </summary>
       ScopedKind ScopedKind { get; }
    }
    public enum ScopedKind
    {
        None,
        ScopedValue,
        ScopedRef,
    }
}
namespace Microsoft.CodeAnalysis.CSharp
{
    public enum SyntaxKind
    {
       /// <summary>Represents <see langword="scoped"/>.</summary>
       ScopedKeyword = 8447,
    }

    public class SyntaxFactory
    {
       public static ScopedTypeSyntax ScopedType(SyntaxToken scopedKeyword, TypeSyntax type);
    }
}

namespace Microsoft.CodeAnalysis.CSharp.Syntax
{
   public sealed class ScopedTypeSyntax : TypeSyntax
   {
       public SyntaxToken ScopedKeyword { get; }
       public TypeSyntax Type { get; }

       public ScopedTypeSyntax Update(SyntaxToken scopedKeyword, TypeSyntax type);

       public ScopedTypeSyntax WithScopedKeyword(SyntaxToken scopedKeyword);
       public ScopedTypeSyntax WithType(TypeSyntax type);
   }
}
```
## Expose snapshot of IWorkspaceServices off of Solution, instead of just off of Workspace.

**Approved** | [#roslyn/62914](https://github.com/dotnet/roslyn/issues/62914#issuecomment-1205811318)

### API Review

API shape is generally approved, but we will investigate renaming `HostSolutionServices` to just `SolutionServices`, and `HostProjectServices` to just `LanguageServices`, or `HostProjectLanguageServices` if we cannot do `LanguageServices`.

```cs
namespace Microsoft.CodeAnalysis
{
     public class Solution
     {
         public HostSolutionServices Services { get; }
     }

     public class Project
     {
         public HostProjectServices Services { get; }
     }

     public sealed class HostSolutionServices
     {
         // has internal constructor.  not publicly instantiable

         /// <inheritdoc cref="HostWorkspaceServices.GetService"/>
         public TWorkspaceService? GetService<TWorkspaceService>() where TWorkspaceService : IWorkspaceService;

         /// <inheritdoc cref="HostWorkspaceServices.GetRequiredService"/>
         public TWorkspaceService GetRequiredService<TWorkspaceService>() where TWorkspaceService : IWorkspaceService;

         /// <inheritdoc cref="HostWorkspaceServices.SupportedLanguages"/>
         public IEnumerable<string> SupportedLanguages { get; }
 
         /// <inheritdoc cref="HostWorkspaceServices.IsSupported"/>
         public bool IsSupported(string languageName);

         /// <summary>
         /// Gets the <see cref="HostProjectServices"/> for the language name.
         /// </summary>
         /// <exception cref="NotSupportedException">Thrown if the language isn't supported.</exception>
         public HostProjectServices GetProjectServices(string languageName);
     }
 
     public sealed class HostProjectServices
     {
         // has internal constructor.  not publicly instantiable

         public HostSolutionServices SolutionServices { get; }

         /// <inheritdoc cref="HostLanguageServices.Language"/>
         public string Language { get; }

         /// <inheritdoc cref="HostLanguageServices.GetService"/>
         public TLanguageService? GetService<TLanguageService>() where TLanguageService : ILanguageService;

         /// <inheritdoc cref="HostLanguageServices.GetRequiredService"/>
         public TLanguageService GetRequiredService<TLanguageService>() where TLanguageService : ILanguageService;
     }
}
```
## Compiler lacks ways to get to the built-in operator methods it synthesizes from the semantic model

**Approved** | [#roslyn/63186](https://github.com/dotnet/roslyn/issues/63186#issuecomment-1205812603)

### API Review

The first variant is inline with existing compiler APIs, and is approved. We will investigate throwing for invalid inputs (ie, attempting to create an `IMethodSymbol` for a built-in that does not exist).

```cs
namespace Microsoft.CodeAnalysis;
public class Compilation
{
    // for binary operators
     public IMethodSymbol CreateBuiltInOperator(BinaryOperatorKind kind, ITypeSymbol left, ITypeSymbol right, ITypeSymbol returnType, bool isChecked);
    // for unary operators
     public IMethodSymbol CreateBuiltInOperator(UnaryOperatorKind kind, ITypeSymbol value, ITypeSymbol returnType, bool isChecked);
}
```
