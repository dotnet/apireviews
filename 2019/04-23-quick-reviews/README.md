# Quick Reviews 4/23/2019

## System.Resources API additions for non-primitive objects

**Approved** | [#corefx/37041](https://github.com/dotnet/corefx/issues/37041#issuecomment-485934735) | [Video](https://www.youtube.com/watch?v=5heMq4U2ek8&t=0h-12m-42s)

Looks good:

* Seal the types and make the protected members private or remove them
* `closeAfterWrite` should be defaulted
* Remove the `TypeNameConverter`, assuming MSBuild doens't need it

