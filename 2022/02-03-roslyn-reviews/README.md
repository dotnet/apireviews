# API Review 02/03/2022

## Provide a way to efficiently get the list of global aliases in a compilation.

**NeedsWork** | [#roslyn/59088](https://github.com/dotnet/roslyn/issues/59088#issuecomment-1029434575)

## API Review

After some discussion, we decided we want to investigate a more symbol-based approach to see if it can answer all the questions the IDE has, and to not separate out global using as special. Instead, @CyrusNajmabadi will investigate whether a `SemanticModel` API that returns some type of hierarchical list of "here are all the using scopes at a given location", with each scope containing all the usings and alias symbols for the current location. If that will be sufficient for the IDE's purposes, we like it much better as an API than singling out global usings specially.
