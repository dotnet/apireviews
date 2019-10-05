# Quick Reviews 1/2/2018

## Add StringBuilder.Equals(string) to efficiently compare a StringBuilder with a string.

**Approved** | [#corefx/25846](https://github.com/dotnet/corefx/issues/25846#issuecomment-354837835) | [Video](https://www.youtube.com/watch?v=dOpH6bUNbcw&t=0h-4m-40s)

It's a bit odd that the existing `Equals(StringBuilder)` does an ordinal comparison -- that's inconsistent with how string comparisons work (defaults to current culture). Hence, it seems acceptable to have an `Equals(string)` that is ordinal only too. Adding `Equals(string, StringComparison)` would be doable too, but the only efficient one would be ordinal (all others likely have to enumerate char by char or even allocate), which might make the API a performance trap.

Conclusion: approved as proposed.
## Add MemberInfo.IsCollectible & Assembly.IsCollectible

**Approved** | [#corefx/25671](https://github.com/dotnet/corefx/issues/25671#issuecomment-354839520) | [Video](https://www.youtube.com/watch?v=dOpH6bUNbcw&t=0h15m54s)

Looks good as proposed; my only question is whether we need/should expose this on `TypeInfo` as well?

**Edit** Looks like `TypeInfo` now inherits from `Type` so we should be fine.
## XPathNodeIterator should implement IEnumerable<XPathNavigator>

**Needs Work** | [#corefx/1593](https://github.com/dotnet/corefx/issues/1593#issuecomment-354841164) | [Video](https://www.youtube.com/watch?v=dOpH6bUNbcw&t=0h23m2s)

Implementing `IE<X>` additionally would be fine, but this doesn't buy you anything for avoiding the cast in the `foreach` scenario as the compiler will still call through the existing non-generic `GetEnumerator()` method.

Am I missing anything?
## X509Certificate GetCertHash and GetCertHashString with SHA256

**Approved** | [#corefx/16493](https://github.com/dotnet/corefx/issues/16493#issuecomment-354843685) | [Video](https://www.youtube.com/watch?v=dOpH6bUNbcw&t=0h32m14s)

Looks good as proposed.
