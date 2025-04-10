# API Review 04/10/2025

## Need an API to get the 'iteration type' of a type.

**Approved** | [#roslyn/77926](https://github.com/dotnet/roslyn/issues/77926#issuecomment-2795109736)

### API Review

* Haven't done a VB check
    * We don't really need it in VB
    * It is iteration type, which VB does have. Signature seems fine for VB, we can address further in code review.
* If there is no iteration type, the desire would be `null` return

**Conclusion**: Approved
