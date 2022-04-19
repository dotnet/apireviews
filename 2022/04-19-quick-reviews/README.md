# API Review 04/19/2022

## Developers can customize the JSON serialization contracts of their types

**NeedsWork** | [#runtime/63686](https://github.com/dotnet/runtime/issues/63686#issuecomment-1102979549)

* `CanSerialize` should be named `ShouldSerialize`
* We should design the combinators pattern more; this would validate that the shape of the types is correct; also, this feature might be mainstream enough that we want to ship a way to combine them along with a set of best practices in V1 of this feature.
* We should consider removing the generic set/get as it's a pit of failure when structs are involved (requires unsafe unboxing in order to mutate the original value rather than a copy)
* Would be good to have a sample how source generation for libraries and custom type for the app would compose, or if this out of scope.

