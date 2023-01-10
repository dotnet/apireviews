# API Review 01/05/2023

## Remove/replace virtual method in UserManager.cs constructor

**Rejected** | [#aspnetcore/45358](https://github.com/dotnet/aspnetcore/issues/45358#issuecomment-1371768982)

Yep. Sounds like CallBase is the right solution here. Was an interesting interlude into the mockability or lack thereof of `UserManager<TUser>` and did cause us to have a bit of a discussion on how we might do things differently in the future. I'll go ahead and close this issue for now.
## Adding Http(Request/Response) extension methods overloads accepting non-generic JsonTypeInfo

**Approved** | [#aspnetcore/45568](https://github.com/dotnet/aspnetcore/issues/45568#issuecomment-1372913890)

API Review Notes:

1. This matches the pattern used by JsonSerializer.DeserializeAsync/SerializeAsync
2. We're curious how JsonSerializer handles null, but we figure it just treats the type as `object`.

API Approved as proposed!
## Add tag trough OutputCacheAttribute

**Approved** | [#aspnetcore/45842](https://github.com/dotnet/aspnetcore/issues/45842#issuecomment-1372918369)

API Review Notes:

1. This follows a very similar pattern to other `string[]?` properties on the `OutputCacheAttribute`.
2. @sebastienros omitted this originally because tag data is often dynamic, but even in our samples, we sometimes use static sets of tags for a given endpoint.

API approved as proposed!

## [Analyzer] Warn on ambiguous route patterns

**Approved** | [#aspnetcore/45223](https://github.com/dotnet/aspnetcore/issues/45223#issuecomment-1372930590)

API Review Notes:

- How conservative is this check? Very. Any unknown extension method on the IEndpointConventionBuilder causes this not to fire. Same if the builder is stored in the variable.
- Why not error level? We're a little worried about false positives still. If we don't get feedback about false positives, we can consider increasing the severity. Also, other endpoints can still work when there are conflicting routes.
- We consider any false positives a bug. We want to error on the side of false negatives in all cases.
- Does this fire once for each conflict, or each endpoint that conflicts? It will fire for each endpoint that conflicts.
- What do we think about the titles and messages?
  - Should we show what patterns it conflicts with? No. It could get really noisy, and each conflict is already highlighted.
  - "route's path" vs "route's pattern"? "route's pattern"
- What about the Usage category? Makes sense.

Analyzer approved as proposed with s/path/pattern.


