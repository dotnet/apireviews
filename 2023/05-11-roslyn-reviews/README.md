# API Review 05/11/2023

## [Lightbulb Perf] Expose FilterSpan/FilterTree public API on remaining analysis contexts

**Approved** | [#roslyn/68033](https://github.com/dotnet/roslyn/issues/68033#issuecomment-1544649533)

### API Review

* What if we did `SyntaxTree` everywhere? 
    * Prefer `FilterTree` naming
* We don't have enough info to make a decision on AdditionalTexts here, so that will need to be a followup.

**Conclusion**: APIs 1 and 2 are approved (adding `FilterSpan` and `FilterTree`). We will add `FilterTree` even where it might be duplicative, for ease of API usage.
## Optimized symbol location collections

**NeedsWork** | [#roslyn/68020](https://github.com/dotnet/roslyn/issues/68020#issuecomment-1544651494)

### API Review

* Similar to OperationList in goal
* Is it still necessary now that the compiler had better with the new representations?
    * Who is the consumer of this API. Compiler has already moved to a new internal pattern.
    * We're unsure at this point. Need examples.
* What about other APIs that our other lists have? Reversed, LastOrDefault, etc?
* API goal seems overall fine.

**Conclusion**: We're generally ok with the idea of the API, but need to see examples of where this would help outside the compiler.
