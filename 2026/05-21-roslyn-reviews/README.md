# API Review 05/21/2026

## Use params collections and ORPA for APIs with both `params T[]` and `ImmutableArray<T>` overloads.

**Approved** | [#roslyn/83155](https://github.com/dotnet/roslyn/issues/83155#issuecomment-4510890093)

### API Review

We validated the following scenarios:
* When compiling with a new compiler, but older language version, the array overload is still preferred for `params`
* When compiling with an old compiler and language version, the array overload is still preferred for `params`
* Everything behaves as expected with current compiler and language version (uses `ImmutableArray`).

**Conclusion**: Approved.
