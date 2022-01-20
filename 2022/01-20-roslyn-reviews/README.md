# API Review 01/20/2022

## Parameter null-checking public API

**Approved** | [#roslyn/58307](https://github.com/dotnet/roslyn/issues/58307#issuecomment-1017966518)

### API Review

1. IParameterSymbol.IsNullChecked will be removed. If issues such as poor performance are discovered in practice when implementing IDE features for parameter null-checking, we can revisit.
    * Whether the parameter symbol is coming from source or metadata would affect the result of this property, and we feel that, currently, any IDE scenario that needs the information is in the method with already-hydrated syntax trees.
2. Syntax API is fine as written in issue description.
3. CFG will not produce additional nodes to represent parameter null checks.
    * No real benefit to adding this info to the tree: it just adds some null checks to the start of the method. Anything that actually cares about that can be relatively easily updated to calculate that info.
