# API Review 08/19/2021

## IOperation support for interpolated string handler conversions

**NeedsWork** | [#roslyn/54718](https://github.com/dotnet/roslyn/issues/54718#issuecomment-902250198)

## API Review

We've generally agreed that the interface should expose the Append calls as children directly, rather than trying to add the information to `IInterpolatedTextOperation` or `IInterpolatedContentOperation`. We don't want to expose the data on `IBinaryOperation` directly: we feel it's surprising and a violation of layering, and that the `IInterpolatedStringOperation` should be the top node. We have a few options we're considering:

* We collapse all the parts into a single `IInterpolatedStringOperation`. While this is the most faithful to the semantics of the language, we're concerned about the ergonomics of not having `IOperation` nodes that correspond to nested `BinaryExpressionSyntax`es or `InterpolatedStringExpressionSyntax`es, which this approach would necessitate.
* We have dedicated `IBinaryContentOperation` or similar nodes that represent the binary operations themselves, and are part of the `Parts` array on the root `IInterpolatedStringOperation` node. While this is more complex and more of a "syntactic" view, it will alleviate the concerns about missing explicit syntax nodes.

A smaller team will explore this space and update the design.

One other note is that we should have a flag on `IInterpolatedStringAppendOperation` that indicates whether the content is a literal or an interpolation placeholder.
