# API Review 11/18/2021

## Allow AnalyzeDataFlow for ConstructorInitializerSyntax

**Approved** | [#roslyn/57577](https://github.com/dotnet/roslyn/issues/57577#issuecomment-973331672)

## API Review

* We need to make sure that we add tests for Extract Method to make sure it doesn't have negative behavior introduced with this method.
* API shape is approved with the optional C#-specific overloads. The primary constructor overload needs to have its comments adjust to not imply that it's giving info about the base constructor itself, only the base call syntax.
## GetAllTypesByMetadataName

**NeedsWork** | [#roslyn/57802](https://github.com/dotnet/roslyn/issues/57802#issuecomment-973341909)

## API Review

We had a lengthy discussion about our philosophies in this area today. We generally think an API shaped similar to this is a good primitive to have for building heuristics, and there is some concern about adding heuristics to the compiler layer itself. We do, however, need to consider how we want to help users with common heuristics, like the equivalent of the compiler's GetWellKnownType internal API. Now that we've agreed on this API concept, we can look at specifics of the API itself in the next design meeting. Some initial comments:

* `GetTypesByMetadataName` seems to be the preferred naming
* `MetadataNameEnumerable` implies that we are enumerating metadata names. Perhaps `TypesByMetadataNameEnumerable` would be a better name.
* We might consider adding a filter delegate to the enumerable, but we will have to carefully consider how that works with arguments and potentially avoiding performance issues. It might be best to simply let perf scenarios filter with a foreach, and non-perf scenarios use LINQ.
