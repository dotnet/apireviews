# API Review 10/26/2023

## Add Compilation parameter to EmitBaseline.CreateInitialBaseline (breaking change)

**Approved** | [#roslyn/70413](https://github.com/dotnet/roslyn/issues/70413#issuecomment-1781863582)

### API Review

* Alternative could be to have the old method work, just throw when we need it?
    * Probably not worth the effort
* Runtime at least is going to move to the new API
    * Should be able to at their convenience, it's not an in-VS scenario.
* Why Obsolete with a warning if it's going to throw?
    * Obsolete with an error, mention the new overload

Approved. Make the `Obsolete` an error instead of a warning.
## Obsolete PreserveLocalVariables, add RuntimeRudeEdits

**NeedsWork** | [#roslyn/70450](https://github.com/dotnet/roslyn/issues/70450#issuecomment-1781864707)

### API Review

* Should we put all the optional state into a struct so we don't have to keep obsoleting old things?
* `preserveLocalVariables` wasn't being used
* What happens when `runtimeRudeEdit` is `null`?
    * Can't work in a fallback with lambdas
    * Don't have a clear answer at the moment, need that to understand what we should do with both old constructors.
* Should `runtimeRudeEdit` have more info in the callback?
    * It's already a struct, we can add more info in the future as needed

Needs work to understand what the fallback behavior (if any) is and how that should affect the existing backcompat overloaded constructor. We'll try to do this over email.
