# Quick Reviews 04/06/2021

## Compile-time source generation for System.Text.Json

**NeedsWork** | [#runtime/45448](https://github.com/dotnet/runtime/issues/45448#issuecomment-814368729) | [Video](https://www.youtube.com/watch?v=MNWITQvvMYU)

* General notes
    - The current proposal is that building application that use linking by default (Blazor, iOS, Android)
    - This means that by default, starting, in .NET, those application models will warn on usage of JSON serialization/deserialization because those will be marked as linker unfriendly. Can we improve this, by, for example, having more of a progressive mode?
    - Since this proposal is generating the serializer on a per assembly boundary, and each assembly's context contains the serialization of the type closure, it means the closure of assemblies is a single versioning bubble. In other words, if a type in a dependency adds new property, the serializer won't know about until it's being re-generated. That's different from the reflection approach.

* `JsonContext`
    - Consider dropping `From` and instead expose the constructor taking options directly
    - It seems unfortunate that for users to take advantage of the source generated serializer all call sites need to change to pass in the context/type info. This might make it harder for frameworks like ASP.NET Core that serialize user provided types without having access to the generated `JsonContext`
    - The `SerializeXxx` funcs are currently hardcoded. We need to make sure that they work for options that the context is tied to. We need to make sure that the generated code is correct; for example, it needs to honor the casing policy. It's OK to have a slower path for non-standard configuration, but we can't ignore the supplied configuration.
    - Can we also get a generic `GetTypeInfo<T>()`

