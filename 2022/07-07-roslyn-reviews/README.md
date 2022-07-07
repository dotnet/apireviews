# API Review 07/07/2022

## Public API for "file-local types"

**Approved** | [#roslyn/62254](https://github.com/dotnet/roslyn/issues/62254#issuecomment-1178238643)

## API Reviews

* IsFileLocal instead of IsFile
* Could we implement metadata support?
    * Technically, yes, it could
    * However, local to which file? That data isn't preserved
* Expand a bit on the doc comment: mention that it's only true for source, clarify the implementation details.
* No public use case at the moment, we can look at it again when we have a use case

### Conclusion

Approved, with `IsFileLocal` instead of `IsFile`. We will not expose a symbol display option for now.
## represent `ref` fields and `scoped` parameters and locals

**NeedsWork** | [#roslyn/61647](https://github.com/dotnet/roslyn/issues/61647#issuecomment-1178239504)

## API Review

* Need to update the docs for SymbolDisplayMemberOptions.IncludeRef
* For locals and parameters, could we hide IncludeRef and add a new IncludeRefAndScoped with the same value?
* Should we have a ScopedKind now, instead of IsScoped? That way we could adapt to `ref scoped` in the future
    * An enum `ScopedKind`, with `None`, `ScopedValue`, and `ScopedRef`
