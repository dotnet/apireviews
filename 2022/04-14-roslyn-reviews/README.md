# API Review 04/14/2022

## Initial sketch of exposing imports from the semantic model

**Approved** | [#roslyn/59533](https://github.com/dotnet/roslyn/pull/59533#issuecomment-1099591449)

### API Review:

The API shape is approved. We should check to see the existing behavior of `DeclaringSyntaxReferences` in VB for global aliases, and mirror that for `DeclaringSyntaxReference` for other global imports in VB.
