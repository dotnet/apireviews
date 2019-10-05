# Quick Reviews 5/29/2018

## Regex Match, Split and Matches should support RegexOptions.AnyNewLine as (?=[\r\n]|\z)

**Approved** | [#corefx/28410](https://github.com/dotnet/corefx/issues/28410#issuecomment-392863409) | [Video](https://www.youtube.com/watch?v=ZHKLi8qWTCs&t=0h0m0s)

Looks good. A few comments:

* We cannot use the proposed value of 128 because it's already taken (see `#if DBG` in code)
* The table looks wrong (Windows on Windows on the Current should work IMHO)
* May be `AcceptAllLineEndings`?
## GroupCollection should implement IReadOnlyDictionary interface to align with its Dictionary-Type Usage

**Approved** | [#corefx/23262](https://github.com/dotnet/corefx/issues/23262#issuecomment-392866863) | [Video](https://www.youtube.com/watch?v=ZHKLi8qWTCs&t=0h10m20s)

Looks good.

* We should also implement IDictionary<K,V> as the BCL exposes the notion of read-onlyness as a mode (`IsReadOnly` returns true)
* Given that we implemen `IList<>`, should we also implement `IReadOnlyList<>`?
## Socket support for TCP_QUICKACK

**Needs Work** | [#corefx/29917](https://github.com/dotnet/corefx/issues/29917) | [Video](https://www.youtube.com/watch?v=ZHKLi8qWTCs&t=0h21m40s)

## TypeInfo doesn't expose a parameterless constructor

**Approved** | [#corefx/14334](https://github.com/dotnet/corefx/issues/14334) | [Video](https://www.youtube.com/watch?v=ZHKLi8qWTCs&t=0h27m53s)

