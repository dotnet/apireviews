# API Review 01/27/2022

## Raw string literal API review.

**Approved** | [#roslyn/59046](https://github.com/dotnet/roslyn/issues/59046#issuecomment-1023702874)

### API Review

API is approved, merging the separate end tokens for the interpolated string form.

* Do we want to have a 1:1 relation between tokens and expressions, or is it fine to have multiple syntaxes map to StringLiteralExpressionSyntax/InterpolatedStringLiteralExpressionSyntax?
    * We've found the single expression kind useful for the IDE, our consumers probably will too.
    * These things are still just strings, have constant values.

**Conclusion**: We will not have separate expression kinds for these strings.

* Should we reuse SyntaxKind.OpenBrace for all interpolated raw string literals, no matter the number of braces?
    * We also think that a single token would be nice for consumers, just like a single expression kind.
    * Already happens in strings with @$" and $@", and for LessThan/GreaterThan with XML doc comments and generic types in crefs.
    * SyntaxFacts.GetText will return the default, whatever the default is, just as it does with the other exception cases today.

**Conclusion**: We will reuse OpenBrace as the token type, no matter how many braces are in the token. Our future selves should try to think about whether similar cases should be less specifically named to avoid confusion like this.

* Should we use trivia for the multi-line whitespace/newline portions?
    * For all but the trailing whitespace/newline after the open token, the whitespace _is_ significant.
    * Language takes the parsed text and creates semantic meaning for it. Syntax should reflect the written text.
    * If we ever want to add a language specifier at the start in the future, we should add a new token for that, and then keep the interior content the same as it is now.

**Conclusion**: All the content between the open and close tokens will be part of the text, no trivia involved.

* Should we merge the end tokens types for single and multiline?
    * Having different start tokens was useful for the IDE, but different end token kinds weren't.

**Conclusion**: Merge `InterpolatedSingleLineRawStringEndToken` and `InterpolatedMultiLineRawStringEndToken`.
