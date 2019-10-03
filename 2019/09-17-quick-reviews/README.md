# Quick Reviews 9/17/2019

## Add UnderScoreCase support for System.Text.Json

**Approved** | [#corefx/39564](https://github.com/dotnet/corefx/issues/39564#issuecomment-532341171) | [Video](https://www.youtube.com/watch?v=6Kd92ScY3gM&t=0h0m0s)

Makes sense. We also considered others (such as kebap casing) but it seems we should defer that until someone actually needs that.
## Add mechanism to handle circular references when serializing

**Needs Work** | [#corefx/41002](https://github.com/dotnet/corefx/issues/41002#issuecomment-532349735) | [Video](https://www.youtube.com/watch?v=6Kd92ScY3gM&t=0h9m39s)

We seem to lean towards not having `Ignore` -- it results in payloads that nobody can reason about. The right fix for those scenarios to modify the C# types to exclude back pointers from serialization.

We'd like to see more detailed spec for the behavior, specifically error cases.
## HashAlgorithmName.FromOid

**Approved** | [#corefx/40558](https://github.com/dotnet/corefx/issues/40558) | [Video](https://www.youtube.com/watch?v=6Kd92ScY3gM&t=0h32m28s)

## Add RemoveIfValue to ConcurrentDictionary

**Approved** | [#corefx/24770](https://github.com/dotnet/corefx/issues/24770#issuecomment-532361229) | [Video](https://www.youtube.com/watch?v=6Kd92ScY3gM&t=0h45m49s)

If we're adding it, it should be `TryRemove(KV<K, V>)`.
