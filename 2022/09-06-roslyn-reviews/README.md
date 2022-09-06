# API Review 09/06/2022

## Add an overload of `CSharpSyntaxTree.Create` that allows to specify checksum algorithm

**NeedsWork** | [#roslyn/62923](https://github.com/dotnet/roslyn/issues/62923#issuecomment-1238739811)

### API Review

- Common case is the SourceText is what is calculating the checksum
- Default implementation forwards to SourceText's property
- However, IDE creates trees without the SourceText in some occasions
  - Don't want to rehydrate the SourceText for this scenario
- We'll make sure doc comments on the property explain the usage
- Can't calculate from the nodes: BOM can't be round-tripped, for example, as it's not represented in in-memory string
- Might consider changing the return of `Encoding` to be none for trees from binary data or other non-file sources, but that would be a breaking change.
- Do we regret having `Encoding` on the tree as well?
  - Neither `Encoding` or `Checksum` are really components of the tree itself. The tree isn't influenced by them, and just turns the tree into a property bag
  - Could the implementation PR keep this info in a different location?
- Maybe we could create `SourceTextProvider` component that gets a callback when we need to make a tree?
  - But `GetText` is already virtual, derived types can override as appropriate
- Next steps will be attempting to make `Encoding` obsolete in the IDE and see what progress we can make with plumbing extra info in that way.
