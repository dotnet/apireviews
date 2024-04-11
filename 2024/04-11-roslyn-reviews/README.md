# API Review 04/11/2024

## Add RegisterSyntaxNodeStartAction to allow analyzers to immediately bail out from analyzing a file prior to processing nodes.

**NeedsWork** | [#roslyn/72053](https://github.com/dotnet/roslyn/issues/72053#issuecomment-2050575193)

### API Review

- Manish looked into the details and we need this regardless for the IDE.
- Should you be able to register operations? Seems like it
- Should we allow it from `SymbolStartAction` as well?
    - That seems weird. What does it mean for a symbol action to be disabled for part of a symbol?
    - No current requests for this.
- Because of the `IOperation` requests, `RegisterSemanticModelAction` seems like the right name.

**Conclusion**: Needs work. We'll review the final proposed API with the rename and IOperation APIs offline on email.
## Make MetadataDisplayFormat public

**NeedsWork** | [#roslyn/72844](https://github.com/dotnet/roslyn/issues/72844#issuecomment-2050575682)

### API Review

- Rather than exposing a new `MetadataDisplayFormat`, can we just expose `UsePlusForNestedTypes` option?
- Let users then create the format they need
- What part of metadata format does this cover? Is it class declaration? Is it call syntax with substituted generic types?
    - If we want to have a premade format for this scenario, we should be very precise as to what is supported and what is not.

**Conclusion**: Needs work. Exposing a `UsePlusForNestedTypes` option seems reasonable by itself. A format would then need to be very precise as to what it supports and where.
## Return tokens to emitted symbols on EmitResult

**NeedsWork** | [#roslyn/72869](https://github.com/dotnet/roslyn/issues/72869#issuecomment-2050576559)

### API Review

- Could we just not use local functions here to make it easier?
   - Would require more wrapping of C# that we don't want to introduce.
- Tokens aren't really understood by the compiler, and it seems odd that we'd return them.
- This seems like it would be a significant chunk of work
- There are other cases that would need to be considered, such as anonymous function symbols.

**Conclusion**: Needs work. We'll work offline to determine the future of the proposal.
