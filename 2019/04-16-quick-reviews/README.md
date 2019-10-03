# Quick Reviews 4/16/2019

## Json serializer support for a property name policy

**Approved** | [#corefx/36351](https://github.com/dotnet/corefx/issues/36351#issuecomment-483773918) | [Video](https://www.youtube.com/watch?v=M1Hc0qrL_Oo&t=-15h-41m-49s)

* `JsonNamePolicy` should have a protected constructor
* For `PropertyNameCaseInsensitive` we need to think about how we match names (comparisons and normalization aren't yielding the same results). @GrabYourPitchforks and @steveharter should chat.
* The `ConvertName()` method produces strings but that seems fine because they are going to be cached.
* The code calling `ConvertName()` should disallow `null` values, but must accept empty strings. Likewise, `ConvertName()` should be documented as  disallowing `null` values but as accepting empty strings.


## Evolving EventCounter API

**Approved** | [#corefx/36129](https://github.com/dotnet/corefx/issues/36129#issuecomment-483778953) | [Video](https://www.youtube.com/watch?v=M1Hc0qrL_Oo&t=0h38m51s)

The APIs were exposed via https://github.com/dotnet/coreclr/pull/23808 and all the changes discussed during the API review were implemented. 
## Add missing async methods in System.Data.Common and implement IAsyncDisposable

**Approved** | [#corefx/35012](https://github.com/dotnet/corefx/issues/35012#issuecomment-483779404) | [Video](https://www.youtube.com/watch?v=M1Hc0qrL_Oo&t=0h48m31s)

@roji @divega

* What does it mean to cancel `CancelAsync()`? Also, how is calling `CancelAsync()` different from cancelling the cancellation token that was passed to the operation that `CancelAsync()` would cancel?
* Is there a reason why `CloseAsync()` returns `ValueTask`? It doesn't seem to on the perf critical path?
* Should we really add `DbDataReader.CloseAsync()` seems odd to add an API that is known to be not useful.
