# API Review 2015-04-28

This API review was also recorded and is available on [Channel 9](http://channel9.msdn.com/Blogs/dotnet/NET-Core-API-Review-2015-04-28).

## Overview

In this API review we reviewed the following PRs and proposals:

### PRs

* [Notes](#1029-propose-to-fix-a-bug-that-the-events-can-only-be-used-by-casting) | [#1029](https://github.com/dotnet/corefx/pull/1029) Propose to fix a bug that the events can only be used by casting

### Issues

* [Notes](#271-regex-collections-should-implement-generic-collection-interfaces) | [#271](https://github.com/dotnet/corefx/issues/271) Regex collections should implement generic collection interfaces
* [Notes](#394-add-getoradd-and-addorupdate-overloads-to-concurrentdictionarytkey-tvalue-that-take-a-targ-factoryargument) | [#394](https://github.com/dotnet/corefx/issues/394) Add GetOrAdd and AddOrUpdate overloads to ConcurrentDictionary\<TKey, TValue> that take a TArg factoryArgument
* [Notes](#1182-systemlinq-performance-improvement-suggestions) | [#1182](https://github.com/dotnet/corefx/issues/1182) System.Linq performance improvement suggestions
* [Notes](#1244-add-overloads-to-string-trimming) | [#1244](https://github.com/dotnet/corefx/issues/1244) Add overloads to string trimming
* [Notes](#1271-add-overload-for-case-insensitive-string-replace) | [#1271](https://github.com/dotnet/corefx/issues/1271) Add overload for case insensitive string Replace
* [Notes](#1378-add-ienumerablekeyvaluepairtkey-tvalue-variants-to-dictionarys-ctors) | [#1378](https://github.com/dotnet/corefx/issues/1378) Add IEnumerable\<KeyValuePair\<TKey, TValue>> variants to Dictionary's .ctors
* [Notes](#1450-make-timezoneinfoadjustmentrulebaseutcoffsetdelta-public) | [#1450](https://github.com/dotnet/corefx/issues/1450) Make TimeZoneInfo.AdjustmentRule.BaseUtcOffsetDelta public
* [Notes](#1513-add-stringsplit-overloads-that-take-a-single-char-and-string-separator) | [#1513](https://github.com/dotnet/corefx/issues/1513) Add String.Split overloads that take a single char and string separator

## Notes

### #1029 Propose to fix a bug that the events can only be used by casting

Status: **Not ready yet** |
[Issue](https://github.com/dotnet/corefx/pull/1029) |
[API Reference](issue-#1029.md) |
[Video](http://channel9.msdn.com/Blogs/dotnet/NET-Core-API-Review-2015-04-28#time=0h0m0s)

* The event exposes the items as non-generic, but other collection types already expose this event publicly
* It's a compile time breaking change, but we believe not many people override this event
    - Folks can only intercept adding/removing, not the event being raised
* We should look at data and we should check with our compatibility council (DDCC)

### #271 Regex collections should implement generic collection interfaces

Status: **Approved** |
[Issue](https://github.com/dotnet/corefx/issues/271) |
[Video](http://channel9.msdn.com/Blogs/dotnet/NET-Core-API-Review-2015-04-28#time=0h0m0s)

* Last point of contention was whether we should make methods like `Contains` public
* We're fine with leaving them non-public

### #394 Add GetOrAdd and AddOrUpdate overloads to ConcurrentDictionary\<TKey, TValue> that take a TArg factoryArgument

Status: **Approved** |
[Issue](https://github.com/dotnet/corefx/issues/394) |
[Video](http://channel9.msdn.com/Blogs/dotnet/NET-Core-API-Review-2015-04-28#time=0h0m0s)

* Last time, three concerns were raised:
    1. **Can those overloads be implemented efficiently on top?** Sort of, but doing it internally can gain a 10% speed-up.
    2. **Are these methods important enough to be provided by the core?** We want to make it easy for people to get to these overloads.
    3. **Is this a slippery slope, i.e. does this mean we'd have to add many overloads like this to the framework?**, It's a valid concern but those overloads are often used in perf critical scenarios. For this scenarios, we're fine adding that overload. But we'll not do it across the board.

### #1182 System.Linq performance improvement suggestions

Status: **Not ready yet** |
[Issue](https://github.com/dotnet/corefx/issues/1182) |
[Video](http://channel9.msdn.com/Blogs/dotnet/NET-Core-API-Review-2015-04-28#time=0h0m0s)

* Concerns:
    1. **It's an observable change**. The concern is that Linq may now return instances of well-known interfaces that consumers could cast to. This may make it harder for us in the future to change our chaining. We could eliminate this concern by using a private interface in Linq.
    2. **We make assumptions that `Count` produces the same number**. The concern is that some collections, such as concurrent collections, will change while being used by Linq. Other cases where this happens are eliminated by using enumerators whose either snapshot or use a version to enforce the underlying collection doesn't change. `Count` is often used in conjunction with `CopyTo` which makes the operation not atomic and thus prone to data corruption.
* Stephen will follow up with how to deal with concurrent collection
    - [#1561](https://github.com/dotnet/corefx/issues/1561) LINQ's Buffer ctor requires Count/CopyTo results be consistent
    - Specifically, here is [an example](https://github.com/dotnet/corefx/blob/4ba474b3ce646ac8c3ccd406defd79ecd56c35c1/src/System.Linq/src/System/Linq/Enumerable.cs#L3086-L3093) of problematic code.

### #1244 Add overloads to string trimming

Status: **Not ready yet** |
[Issue](https://github.com/dotnet/corefx/issues/1244) |
[Video](http://channel9.msdn.com/Blogs/dotnet/NET-Core-API-Review-2015-04-28#time=0h0m0s)

* It seems we should provide functionality that removes suffixes and prefixes
* In particular, looking that linked StackOverflow questions it seems folks want the non-repeating behavior, i.e. "SIdId".RemoveSuffix("Id") should return "SId", not "S"
* We don't want to introduce overloads for `TrimEnd` and `TrimStart` that have a non-repeating behavior because it's inconsistent with the existing ones.
* At the same time, we feel the `bool` option is overkill and not very readable from the call side.
* We think it's best to provide a separate set of methods but the naming is not clear. We should check what other platforms call them.
    - `RemoveSuffix()`, `RemoveEnd()`
    - `RemovePrefix()`, `RemoveStart()`
* Should be instance methods on `String` (as opposed to extension methods)
* Should we add corresponding methods to `TextInfo` (or whatever they care called in globalization)

### #1271 Add overload for case insensitive string Replace

Status: **Accepting PRs** |
[Issue](https://github.com/dotnet/corefx/issues/1271) |
[Video](http://channel9.msdn.com/Blogs/dotnet/NET-Core-API-Review-2015-04-28#time=0h0m0s)

* We should do a holistic pass over `String` to make sure that all methods that compare strings take a `StringComparison`. `Replace` is one, so is `Contains`, maybe there are more.
* This is a separate issue ([#1565](https://github.com/dotnet/corefx/issues/1565) System.String should be consistent with taking StringComparison arguments) and thus shouldn't block this issue.
* We shouldn't add an overload that takes a `CultureInfo` (due to layering). Instead, we should add it in the right place in glob
* We shouldn't add the `char` overload as it's not very useful

### #1378 Add IEnumerable\<KeyValuePair\<TKey, TValue>> variants to Dictionary's .ctors

Status: **Accepting PRs** |
[Issue](https://github.com/dotnet/corefx/issues/1378) |
[Video](http://channel9.msdn.com/Blogs/dotnet/NET-Core-API-Review-2015-04-28#time=0h0m0s)

* The implementation should throw if duplicate keys are detected (similar to what `Dictionary<K,V>` does when you pass in a collection with a different comparer or when you pass an enumerable of key value pairs to `ConcurrentDictionary<K,V>`).
* We should include the one that takes a comparer

### #1450 Make TimeZoneInfo.AdjustmentRule.BaseUtcOffsetDelta public

Status: **Accepting PRs** |
[Issue](https://github.com/dotnet/corefx/issues/1450) |
[Video](http://channel9.msdn.com/Blogs/dotnet/NET-Core-API-Review-2015-04-28#time=0h0m0s)

* It seems we shouldn't expose the delta because that's not actually what people want, instead, they want the UTC offset

However, the issue also asks about being able to construct the adjustment rules. That should be a separate issue because it's being a much bigger item:

* They need to be able construct an `AdjustmentRule` and pass in the values that affects `BaseUtcOffsetDelta`
* Currently, .NET Core doesn't support constructing `AdjustmentRule` at all

### #1513 Add String.Split overloads that take a single char and string separator

Status: **Accepting PRs** |
[Issue](https://github.com/dotnet/corefx/issues/1513) |
[Video](http://channel9.msdn.com/Blogs/dotnet/NET-Core-API-Review-2015-04-28#time=0h0m0s)

* Take as proposed
* The semantics of the overloads that take a string should be identical to the one that uses a `string[]` (when being called with a single element)