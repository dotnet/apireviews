# API Review 11/09/2023

## Collection expressions: IOperation support

**NeedsWork** | [#roslyn/70631](https://github.com/dotnet/roslyn/issues/70631#issuecomment-1804759092)

### API Review

* What level do we want these to exist on? How much abstraction?
    * Do we even want to show the `Add` calls in this API?
    * It's in a weird middle ground: Not quite the specification representation, not quite the lowered form.
        * Span is a good example: we may not wrap an array, and the spec doesn't say we wrap an array.
    * Do we even want to have the `Add` calls?
        * We do for regular collection initializers
* Should ISpreadOperation's element be named `Element`?
    * Or `Operand`
* For the CFG, how far do we lower?
    * `await` doesn't lower at all
    * `foreach` lowers to gotos, collection initializers lower to the series of add calls
    * Perhaps this should just resurface the collection expression back?
        * We have looser specifications on when enumerations occur, for example, and it's a bit of a disservice if the CFG conveys an ordering

**Conclusion**: We'll simplify the proposal to remove these implementation details and bring it back.
## Make CompilationOptions.DebugPlus public

**Approved** | [#roslyn/70670](https://github.com/dotnet/roslyn/issues/70670#issuecomment-1804759506)

### API Review

* API was originally internal because we weren't sure if we wanted it... 6 years ago.
* Should it be a special value on OptimizationLevel?
    * `DebuggableRelease`?
    * `ReleaseWithDebugInfo`
    * Are people using `Optimization.Release` in a comparison that would be broken?
        * Not from what we can tell

**Conclusion**: We'll go with an addition to `OptimizationLevel` instead of a separate flag for this, called `ReleaseWithDebugInfo`.
