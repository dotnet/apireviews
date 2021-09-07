# API Review 09/07/2021

## Port ObjectSerializer abstraction from the Azure SDK

**NeedsWork** | [#runtime/43669](https://github.com/dotnet/runtime/issues/43669#issuecomment-914555568) | [Video](https://www.youtube.com/watch?v=AGFnSBdnJ6o&t=0h0m0s)

* We should have a list of places where we'd **implement** the abstraction:
    - `System.Text.Json`
    - XML serializer?
    - Data contract serialization?
* We should have a list of placed where we'd **accept** the abstraction
    - Azure SDK (such as Cosmos)
    - ASP.NET Core's model binder?
    - `HttpClientJsonExtensions`?
* We should validate that the abstraction works well with the feature set of the serializers we have. There shouldn't be a bias towards just JSON. Also, we've added a ton of features to `System.Text.Json`. Would these all work?
* Can we make `ObjectSerializer` friendly for the linker?
    - At the minimum, we'd need generic overloads
* Is `Stream` the correct type? Should we also support span?
* If we were to ship this, which binaries would this ship in?
    - Azure SDK would need a .NET Standard 2.0 package.
    - Could we just make it part of the same package that provided `BinaryData`?
