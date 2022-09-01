# API Review 09/01/2022

## Add API to determine if type contains valid entry point

**Approved** | [#roslyn/34337](https://github.com/dotnet/roslyn/issues/34337#issuecomment-1234766090)

### API Review

- There's a few scattered implementations for "what is the entry point", we want to unify them.
- Drop `Valid` from the name? And rename to `Get`?
  - `GetEntryPointCandidates`
- `CancellationToken`, not `ICancellationToken`

**Conclusion**: Approved with the name `GetEntryPointCandidates`.
## Add a CanApplyChanges api to CodeFixContext and CodeRefactoringContext

**NeedsWork** | [#roslyn/63457](https://github.com/dotnet/roslyn/issues/63457#issuecomment-1234766506)

### API Review

- Continued work in discouraging Solution->Workspace directionality
- Do we need the other `CanApplyX` methods?
  - CanApplyParseOptionChange - Used for changing language version
  - CanApplyCompilationOptionChange - Protected today? Maybe we don't need this one?
- CodeFixes and refactorings can access all of VS and import MEF services. Are we going to want to have an analyzer to prevent this at some point in the future?
  - Could start with a warning in the code fix provider
- Should this be a service that can be queried, instead of a method on the context?
  - We forsee that we could add this in other locations. A service probably makes more sense.

**Conclusion**: Generally approved in the service form. We need a followup email chain to name the service, and see what `CanApplyCompilationOptionChange` is used for and whether it should be included.
## Expose "IsGeneratedCode" on analysis contexts

**Approved** | [#roslyn/7578](https://github.com/dotnet/roslyn/issues/7578#issuecomment-1234766960)

### API Review

- Some users want to do something special for generated code, but only specific types. This gives an up-front check before doing a parse options check for specific generated code kinds.
- Should we add this to source generator contexts as well?
  - No requests for it now, but we could do it in the future.

**Conclusion**: Approved.
