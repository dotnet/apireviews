# API Review 06/07/2022

## Add Output Caching middleware

**NeedsWork** | [#aspnetcore/41955](https://github.com/dotnet/aspnetcore/issues/41955#issuecomment-1148057739)

API Review Notes:

- Can we have an overload that takes just the policy name as a "string"
  - We like `builder.Policy("policyName")`. over `builder.Profile("profileName")`
  - Everywhere we reference **`Profile`** we should use **`Policy`** instead to align with auth.
- Wait. Is a policy different from a profile?
  - No. We're going with a single profile.
  - But couldn't this be infinitely recursive?

```csharp
options.Policies["NoCache"] = new OutputCachePolicyBuilder().Profile("NoCache").Build();
```

- What's `OutputCachePolicyBuilder().Enabled()` for?
  - We need a disabled state.
  - Review: **We should be enabled by default.** if endpoint has output caching metadata.
- Can we copy auth more?
  - Caching should be opt-in by default.
    - Auth is also opt-in by default. Auth also doesn't activate unless it has metadata defined on the given endpoint
  - The default policy should be the policy used by endpoints with `.CacheOutput()` with no parameters. It should not be applied to all endpoints.
- Should `CacheOutput(Action<OutputCachePolicyBuilder>)` use a default `OutputCachePolicyBuilder`?
  - Not any "defaults" configured by the application. Each `OutputCachePolicyBuilder` should be fresh instance.
- Can this support nested groups?
  - Not yet. It uses the most local metadata and then chains to the default policy.
  - We should be able to chain to arbitrary parent polices to support nested groups of arbitrary depths.
- What policy is used why I call `app.MapGet("/uncached", Gravatar.WriteGravatar).CacheOutput(myPolicy);` where `var myPolicy = new OutputCachePolicyBuilder().NoCache().Build();`?
  - It will use the `OutputCachingOptions.DefaultPolicy` with behaviors merged with `myPolicy`. Where there are conflicts, the more local `myPolicy` is preferred.
- How do I use caching from custom middleware?
  - Can we add a feature to the HttpContext that says "cache this response"? `HttpContext.CacheOutput()` This is somewhat similar to being able to call `HttpContext.ChallengeAsync()` for auth?
  - Keep the caching as static as possible. We can have things like `OutputCachePolicyBuilder.VaryByQuery`, but we'd only check it if the policy built by that `OutputCachePolicyBuilder` was applied to the given endpoint.
  - This probably means that you cannot share any caches across endpoints.
  - We should have a service that makes it easy to store and responses and replay them based on a user-specified key. Like a higher level `IMemoryCache`.
  - Let's not redo routing either even if it leverages common logic.
- Can we use `IDistributedCache` instead of `IOutputCache`?
  - No, because it doesn't support `EvictByTagAsync(string tag)`.
  - Can we define a more generic abstraction that could also be used by rate limiting at least? See https://github.com/dotnet/aspnetcore/issues/41861.
  - `List<byte[]> Segments` should be a `Stream` or `PipeReader` (or `ReadOnlySequence<byte>`).
- We should have zero extra allocations when an endpoint that doesn't have a policy defined is hit.
- Should Endpoint middleware warn if an endpoint has output caching metadata with not output caching middleware?
  - Yes.
- `IOutputCachingContext` should just be a public sealed class.
- Can we use `ActivatorUtilities` for adding a policy based on `Type` that injects singleton services.
- We shouldn't expose `options.Policies`. It should be `options.AddPolicy(string name, IOutputCachingPolicy)` and `optinos.AddPolicy<TPolicy>(string name) where TPolicy : IOutputCachingPolicy`.
- Make allocation pay per play
  - If there is no policy defined specifically on the endpoint even if there is a `DefaultPolicy`. Or otherwise needs to be opt-in.
- Should `WebApplicationBuilder` auto-add output caching middleware?
  - Didn't get to this.
