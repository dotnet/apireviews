# API Review 07/28/2026

## MSBuildWorkspace API for opening file-based app projects

**Approved** | [#roslyn/84588](https://github.com/dotnet/roslyn/issues/84588#issuecomment-5107209646)

### API Review

* Want to support file-based apps in msbuildworkspace, for things like `dotnet format` and general analysis
* The heuristic is well-known and used by `dotnet` cli scenarios as well
* Option A gives the pass-through (just take the argument and pass to msbuildworkspace) for "free". We like that benefit.
    * It does mean that some users may need to react, but we think that's the better reaction, over forcing users to do the api choosing themselves.
* As a follow up, we should consider exposing the API that performs the "is this a file-based app" check for users to call themselves, if they so desire.

**Conclusion**: Approved with option a; enhance `OpenProjectAsync` with knowledge of file based apps. The heuristic should be well-documented on the API.
