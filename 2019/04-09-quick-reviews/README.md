# Quick Reviews 4/9/2019

## Making the info GC.GetMemoryInfo provides public

**Approved** | [#corefx/34631](https://github.com/dotnet/corefx/issues/34631#issuecomment-481358549) | [Video](https://www.youtube.com/watch?v=BgF2UKtQjWA&t=0h0m0s)

Looks good! Comments from the review:

* `GCMemoryInfo` should be marked readonly
* We should suffix all members with `Bytes`
* We should normalzie all values into bytes
* `HighMemoryLoadThreshold` should return `long` and be named `HighMemoryLoadThresholdBytes`
* `MemoryLoad` should return `long` and be named `MemoryLoadBytes`
* `Fragmentation` should be `FragmentedBytes`
* `GetTotalAllocatedBytes()` should default the parameter to `false`
## Revisit Index/Range API requirements

**Approved** | [#corefx/35972](https://github.com/dotnet/corefx/issues/35972) | [Video](https://www.youtube.com/watch?v=BgF2UKtQjWA&t=0h37m15s)

## HWIntrinsics API Proposal: VPMOVZXBD and friends need pointer-based overloads

**Approved** | [#corefx/35768](https://github.com/dotnet/corefx/issues/35768) | [Video](https://www.youtube.com/watch?v=BgF2UKtQjWA&t=0h49m27s)

## Take another look at the `COMISS` and `UCOMISS` hardware intrinisics

**Approved** | [#corefx/34881](https://github.com/dotnet/corefx/issues/34881#issuecomment-481371966) | [Video](https://www.youtube.com/watch?v=BgF2UKtQjWA&t=1h9m5s)

Works for us.
## Clarity and control on a JsonDocument lifetime

**Approved** | [#corefx/36152](https://github.com/dotnet/corefx/issues/36152#issuecomment-481375975) | [Video](https://www.youtube.com/watch?v=BgF2UKtQjWA&t=1h13m43s)

Let's remove `IsPersistable` for now and make `Clone()` a no-op. This way, callers can always call `Clone()` before storing it.
