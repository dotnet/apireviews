# Quick Reviews 1/7/2020

## Expose Math.MaxNumber and Math.MinNumber functions that don't propagate NaN

**Needs work** | [#corefx/36925](https://github.com/dotnet/corefx/issues/36925#issuecomment-571721383) | [Video](https://www.youtube.com/watch?v=lSB-ACeetRo&t=0h0m0s)
 
* Should we have a wider discussion about putting future APIs on the primitive
  types (`double`, `half`, etc.) instead of the `Math` / `MathF` / `MathH`
  classes? Otherwise there's likely to be an explosion of `Math*` types and APIs
  on those types.
* If we decide that putting these APIs on the primitive types isn't worthwhile
  then the proposal as stated here seems viable. There was some discussion on
  whether we should keep the `Number` name and whether those APIs would be
  discoverable enough, but: (a) these names match what IEEE uses; and (b) it's
  ok if users keep using the old APIs instead of the new APIs for the majority
  of use cases.
 
## Add address family specific name resolution to Dns

**Approved** | [#runtime/939](https://github.com/dotnet/runtime/issues/939#issuecomment-571705400) | [Video](https://www.youtube.com/watch?v=lSB-ACeetRo&t=0h0m0s)
 
* Do we have some kind of guarantee that the supported OSes have a "return
  synchronously if you have the relevant data already cached" behavior? If so,
  then it would make sense for these APIs to return `ValueTask<T>`. Without that
  underlying OS support then we should keep these as `Task<T>`. It also helps
  usability if callers want to maintain their own cache of returned values so
  that they don't have to rely on the OS's underlying cache.
* Aside from this, marking _approved_.

## Happy Eyeballs support in Socket.ConnectAsync

**Approved** | [#runtime/861](https://github.com/dotnet/runtime/issues/861#issuecomment-571712410) | [Video](https://www.youtube.com/watch?v=lSB-ACeetRo&t=0h0m0s)
 
* Take the new parameter and move it to the end of the parameter list of the new
  overload.
* Remove `[Flags]` from the enum. We can add it later if necessary as long as we
  only have 1 legal value on the enum right now.
* Rename the zero value to _Default_.
* Consider renaming the enum to `ConnectionAlgorithm`.
 
## Add HashSet.AsReadOnly

**Needs work** | [#corefx/37220](https://github.com/dotnet/corefx/issues/37220#issuecomment-571732291) | [Video](https://www.youtube.com/watch?v=lSB-ACeetRo&t=0h0m0s)

* What are you actually trying to do here?
* `HashSet<T>` already implements `IReadOnlyCollection<T>`, so you can cast it
  directly: `IReadOnlyCollection<T> collection = myHashSet;`.
* If you want to create a `ReadOnlyCollection<T>`, it's one line:
  `ReadOnlyCollection<T> collection = new
  ReadOnlyCollection<T>(myHashSet.ToArray());`.

## Proposal: Extension method to get value or default with creation function for Dictionary

**Rejected** | [#corefx/37149](https://github.com/dotnet/corefx/issues/37149#issuecomment-571729598) | [Video](https://www.youtube.com/watch?v=lSB-ACeetRo&t=0h0m0s)
 
* This would be a compile-time breaking change because there would be an
  overload resolution conflict if the `null` literal is provided as the last
  parameter to the extension method.
* Please modify the proposal and reopen if you still have a strong desire +
  justification for this and there's a solution to working around the breaking
  change.
 
## One-shot PEM reader

**Needs work** | [#corefx/37748](https://github.com/dotnet/corefx/issues/37748#issuecomment-571743250) | [Video](https://www.youtube.com/watch?v=lSB-ACeetRo&t=0h0m0s)
 
 * Change _dataSize_ and _labelSize_ to have _*Length_ suffixes instead.
 * In `TryWrite`, move the _destination_ parameter toward the end of the
   parameter list.
 * Rename struct's _Base64_ property to _Base64Data_. Rename _EncodedByteSize_
   to _DecodedByteSize_.
 * Remove the base64-decoding helper method from the ref struct. Callers can use
   the `Convert` class instead.
 * Consider using a normal struct instead of a ref struct and use _Range_
   properties instead of _ROS_ properties. If we do this, then we can get away
   with not _out_ing a `Range` from the `Find` method, and instead just put
   _Range location_ on the struct itself.

## MemoryMarshal.GetRawArrayData

**Approved** | [#corefx/36133](https://github.com/dotnet/corefx/issues/36133#issuecomment-571725298) | [Video](https://www.youtube.com/watch?v=lSB-ACeetRo&t=0h0m0s)

* Rename to `GetArrayDataRef`.

## `ExcludeFromCodeCoverageAttribute(string reason)`

**Approved** | [#corefx/36652](https://github.com/dotnet/corefx/issues/36652#issuecomment-571726362) | [Video](https://www.youtube.com/watch?v=lSB-ACeetRo&t=0h0m0s)

* Don't add the new ctor. Instead add a get+set property `string? Justification`. This mirrors the `SuppressMessageAttribute` design.

## Expose CancellationTokenSource.CreateLinkedTokenSource(CancellationToken)

**Approved** | [#corefx/39555](https://github.com/dotnet/corefx/issues/39555#issuecomment-571746847) | [Video](https://www.youtube.com/watch?v=lSB-ACeetRo&t=0h0m0s)

* We discussed whether this would be necessary if we had a `params ROS<T>`
  overload. But there's not even 100% consensus that we want to add that
  overload, and the proposal in this issue has merit on its own regardless.