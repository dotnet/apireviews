# API Review 02/16/2023

## Move `UseValueTuple` from `SymbolDisplayCompilerInternalOptions` to `SymbolDisplayMiscellaneousOptions`

**Approved** | [#roslyn/66757](https://github.com/dotnet/roslyn/issues/66757#issuecomment-1433781770)

### API Review

* `ExpandValueTuple` in Misc options
* Approved

## API proposal for "Using Alias to any Type"

**Approved** | [#roslyn/66913](https://github.com/dotnet/roslyn/issues/66913#issuecomment-1433782355)

### API Review

* Have we thought about what we'd need to do if we end up doing generics in alias names?
    * The part that would change is independent of this.
* Do we want an "expanded using alias" thing to avoid API breaks?
    * Would result in more pain for users than making `Name` nullable, as everywhere that is currently `UsingDirectiveSyntax` is now `BaseUsingDirectiveSyntax`
* Rename `Type` to be `NamespaceOrType` to be more clear
* Approved
## New APIs for "Primary Constructors"

**Approved** | [#roslyn/66914](https://github.com/dotnet/roslyn/issues/66914#issuecomment-1433783095)

### API Review

* We may want to move ParameterList up to BaseTypeDeclarationSyntax for error recovery purposes at some point
    * We will confirm that that won't a binary break. Otherwise, this is approved.
