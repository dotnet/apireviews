# API Review 11/17/2023

## Nullability analysis perf investigation during razor semantic classification

**NeedsWork** | [#roslyn/70609](https://github.com/dotnet/roslyn/issues/70609#issuecomment-1815536958)

### API Review

* How complex will it be to implement with any amount of sharing?
    * Could it just be a wrapper around an internal version of the proposed APIs?
* Today, we have an invariant that semantic models are independent. Should this be that relationship, or is closer to the speculative semantic model relationship?
    * It would mean that the nullable-disabled semantic model could affect the caches of the nullable enabled one. This could be undesirable from an API perspective?
* Consumption side, having different results based on order of operations; ie, don't _maybe_ give nullable info from the disabled version
    * Likely to result in IDE bugs that will be hard to track down.
* Compiler team needs to sit down and work through the feasibility of sharing, and whether we're ok with such a relationship, before we move forward with the API.
## Collection expressions: IOperation support

**Approved** | [#roslyn/70631](https://github.com/dotnet/roslyn/issues/70631#issuecomment-1815538227)

### API Review

* Is there always a conversion, even if it's Identity?
    * If it is true, perhaps we should doc it? Not sure if it is true.
    * We should probably remove the conversion phrasing, we don't doc it anywhere else and as it written, it could imply that there always is.
        * Or add "if necessary"?
* Naming
    * `GetItemConversion`? Spread isn't really needed in the name.
    * Is `Item` the right name?
        * Use `Element` instead

**Conclusion**: Approved. Remove `Spread` from `GetSpreadItemConversion`, and change `Item`->`Element` across the board for consistency with `ForEachStatementInfo`.
