# API Review 02/02/2023

## Introduce IResettable to streamline object pool usage

**Approved** | [#aspnetcore/44901](https://github.com/dotnet/aspnetcore/issues/44901#issuecomment-1414504236)

API Review Notes:

- We're a fan of the bool return value because it allows for objects to enter a non-resettable state.
- Can we get away with only updating the `DefaultPooledObjectPolicy<T>`? `StringBuilderPooledObjectPolicy` exists, but `StringBuilder` is sealed.
- Is there anything analogous for object creation? No.
- Do we expect custom policies to respect this? No. If you are unsure what the `PooledPolicy<T>` will be, don't rely on this.
- By removing the ResettablePooledObjectPolicy, we lose the generic type constraint. However, given a plain old `ObjectPool<T>` that you didn't construct, you can never know if it will call `IResettable.Reset()`.
- The alternative is always calling reset yourself before returning. The upside of this proposal is that if you construct the ObjectPool and define the type yourself, you know you won't accidentally not reset. Or have others forget. This is already possible with custom polices, but it's more verbose.

API Approved!

```csharp
namespace Microsoft.Extensions.ObjectPool;

public interface IResettable
{
    bool Reset();
}
```
## UserManager and SignInManager cancellation tokens

**NeedsWork** | [#aspnetcore/45869](https://github.com/dotnet/aspnetcore/issues/45869#issuecomment-1414513643)

API Review Notes:

1. @mitchdenny is right that it would have to be new overloads to be non-breaking.
2. [AspNetUserManager](https://github.com/dotnet/aspnetcore/blob/797642940abd147e37896151471a83b9f15c5419/src/Identity/Core/src/AspNetUserManager.cs#L42) passes in the RequestAborted token from the the `IHttpContextAccessor` (which is a singleton backed by an `AsyncLocal<HttpContext>`).
3. What about existing overrides that don't take the CancellationToken? We still have to call them, right? We could use reflection to detect this and cache the results, but wow!

Given the lengths we would have to go to to make this non-breaking, we cannot approve this now. So far no one has a plan that would make this work with decent performance without breaking. If you really need this functionality you can override `UserManager/SignInManager.CancellationToken` yourself in the meantime.
## Remove (Http)(Validation)ProblemDetails converters

**NeedsWork** | [#aspnetcore/44132](https://github.com/dotnet/aspnetcore/issues/44132#issuecomment-1414522085)

API Review notes:

1. Is removing the `[JsonPropertyName("type")]` etc... attributes safe? What if someone was manually serializing without web default previously. Is this an improvement or a regression?
2. Why do we need the setter? It's required to source gen the S.T.J stuff without the converter. This seems like a problem with S.T.J source generation. Why doesn't it support serializing read-only types?

@brunolins16 Can you double check if the setters are still absolutely necessary? If so, why?
