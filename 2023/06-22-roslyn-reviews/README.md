# API Review 06/22/2023

## Collection literals

**NeedsWork** | [#roslyn/68687](https://github.com/dotnet/roslyn/issues/68687#issuecomment-1603314314)

### API Review

* Should it be named CollectionLiteralExpressionSyntax?
* Philosophically, do these things appear out of nowhere, or are they thought of as a series of operations?
* Language feature is called collection literals, language grammar says collection literals
* Are analyzer authors going to want to handle these in the same family as other literalexpression syntaxes?
    * What is the use case?
    * Can be added later if we find a use case, no for now.
* What about just `CollectionExpression`s? Parallels with `TupleExpression`, which is a similar-feeling feature
* We like CollectionExpressionSyntax. Separately, LDM may want to consider renaming the feature to get away from calling these literals, as that can evoke mutability concerns in users we've talked to.

**Conclusion** Syntax nodes will be named `CollectionExpressionSyntax`, we will not ship dictionary elements for now. We will come back to conversions and ioperation in the next meeting.
## Public API changes for InlineArrays feature

**Approved** | [#roslyn/68719](https://github.com/dotnet/roslyn/issues/68719#issuecomment-1603314949)

### API Review

* Why not use `IArrayElementReferenceOperation`?
    * Different semantics, likely that users would have to filter them out because of how different they are.

**Conclusion** API approved as proposed
