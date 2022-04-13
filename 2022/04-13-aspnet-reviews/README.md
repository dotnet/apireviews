# API Review 04/13/2022

## Introduce Results.Typed factory methods for creating results whose types properly reflect the response type & shape

**NeedsWork** | [#aspnetcore/41009](https://github.com/dotnet/aspnetcore/issues/41009#issuecomment-1097399822)

API review notes:

- What about linker friendliness?
  - This will be addressed separately along with the existing `Results` methods.
- Do we prefer `Results.Typed` or `TypedResults`?
  - We prefer `TypedResults` in the `Microsoft.AspNetCore.Http` namespace alongside `Results`. This avoids a nested type or static property.
- We should use longer names for noun results.
  - `Ok` is okay
  - `Problem` is not. Should be `ProblemResult` or something.

