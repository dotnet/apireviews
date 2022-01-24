# API Review 01/24/2022

## `CreatedResult` should accept null location

**Approved** | [#aspnetcore/39454](https://github.com/dotnet/aspnetcore/issues/39454#issuecomment-1020448781)

We could consider making only one of the overloads (the `string?` one) and make it nullable. If the compiler supports using nullablility as part of overload resolution, users could get away with writing `Created(null, "My-cool-value")` without having to cast. Outside of that API approved.
## Can't inject service in ValidationAttribute

**Approved** | [#aspnetcore/39382](https://github.com/dotnet/aspnetcore/issues/39382#issuecomment-1020441400)

API review:

Approved with a change to the obsolete warning message:
```diff
- [Obsolete("This API is obsolete and will be removed in a future version")]
+ [Obsolete("This API is obsolete and will be removed in a future version. Consider using the overload that accepts an IServiceProvider.")]
```
