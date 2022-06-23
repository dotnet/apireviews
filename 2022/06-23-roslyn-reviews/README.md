# API Review 06/23/2022

## Public APIs for opened and closed event handling for non-source documents

**Approved** | [#roslyn/61523](https://github.com/dotnet/roslyn/issues/61523#issuecomment-1164883478)

# API Review Notes

- Proposal 3 is accepted as-is.
- We won't deprecate the existing APIs (yet) as it seems useful for consumers who aren't interested in non source texts.
## Add EditAndContinueMethodDebugInformation.Create overload that takes compressedStateMachineStateMap

**Approved** | [#roslyn/61636](https://github.com/dotnet/roslyn/issues/61636#issuecomment-1164883957)

# API Review Notes
- Approved
## Promote CompletionList.ItemsList to public API

**Approved** | [#roslyn/61971](https://github.com/dotnet/roslyn/issues/61971#issuecomment-1164885454)

# API Review Notes

- It would be nice if we had some sort of common array type.
- Should we just make this a breaking change instead of having two different collection types?
  - That could cause issues for in-proc extensions as a binary break
  - We would have to wait for a major version change
  - Too many people use this API
- We should make Items obsolete? and member visible hidden
- If we use IReadOnlyList<T> will then want to remove it for the allocation
	- Should we have a custom enumerator type instead?
	- Think IReadOnlyList<T> is sufficient 

**Conclusion**: Approved. Deprecate and Hide existing `Items`

## Merge CompletionServiceWithProviders into CompletionService

**Approved** | [#roslyn/61977](https://github.com/dotnet/roslyn/issues/61977#issuecomment-1164886646)

# API Review

- Nobody is using this (via github search), seems fine to remove.
- These constructors are internal so we know there can't be any more derived types
- We already made a breaking change for 4.1 so any consumers are already broken
- Its safe to use a virtcall on a non-virt so won't break binary compat

Approved.
## Add `SourceText.From(StringBuilder)` API

**NeedsWork** | [#roslyn/61326](https://github.com/dotnet/roslyn/issues/61326#issuecomment-1164898267)

# API Review

- This is useful as its avoids a potentially single large allocation
- Still has copies however
  - Would it be more useful to just directly build up the source text 
  - Feels like a BCL hole, but we can plug it somewhat
    - If we expose a builder type ourselves then if/when the BCL hole is filled we still have to maintain a parallel impl
    - If we only expose `StringBuilder` and keep the reader internal then we could just replace the internal call with the BCL impl
  - What is the urgency for this? 
    - If we don't ship it as part of 17.3 (unlikely at this point) then consumers won't be able to rely on it for another few versions anyway
    - That gives us time to evaluate the `SourceTextWriter` like approach and potentially make an even better experience for authors than just `StringBuilder`


Consensus: Let's wait on this, explore what the `SourceTextWriter` alternative looks like and revisit once we have an API shape and perf data for it.
