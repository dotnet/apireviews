# API Review 11/01/2021

## RouteConstraints aren't linker friendly

**Approved** | [#aspnetcore/24723](https://github.com/dotnet/aspnetcore/issues/24723#issuecomment-956444763)

API review:

Approved API:

```diff
public class RouteOptions
{
+    public void SetParameterPolicy<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)]T>(string text) where T : IParameterPolicy;
+    public void SetParameterPolicy(string text, [DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)]Type constraintType);
}
```

The API is approved. We we want to prevent users from mutating `Routeoptions.ConstraintMap` since that could again introduce trimmer unfriendly behavior. We think the best way to do this is to annotate the property with `RequiresUnreferencedCodeAttribute` and make a note that says the warning can be suppressed if all they are doing is reading or deleting from the dictionary.

The API is named `Setxxx` since it will always replace an existing key.
