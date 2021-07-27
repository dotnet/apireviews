# API Review 07/27/2021

## Finalize logging analyzers

**Approved** | [#runtime/36064](https://github.com/dotnet/runtime/issues/36064#issuecomment-887699727) | [Video](https://www.youtube.com/watch?v=fnqHXFKSpaw&t=0h0m0s)

API Review notes


* Diagnostic 1
  * A fixer seems doable here.
  * A level higher than Disabled seems warranted.  (Hidden?)
  * Category: Naming
* Diagnostic 2
  * A fixer seems possible here, but the resolution should create a static field for the defined message, not a local variable.
  * Should be Hidden, not Disabled.
  * Category: Performance
* Diagnostic 3
  * The example should be updated.  `logger.LogTrace("{0}", value)` => `logger.LogTrace("{Value}", value)`
  * Information / Usage
* Diagnostic 4
  * This should not apply to LoggerMessage.Define, just LogTrace (and friends).
  * Information / Usage
* Diagnostic 5
  * Warning does seems appropriate when/since you can prove it's a guaranteed exception.
  * Since it's about an exception, let's use Reliability instead of Usage.
  * Warning / Reliability
## Prefer Nullable<T>.GetValueOrDefault() over manual checking

**Rejected** | [#runtime/33792](https://github.com/dotnet/runtime/issues/33792#issuecomment-887702637) | [Video](https://www.youtube.com/watch?v=fnqHXFKSpaw&t=0h28m51s)

The feeling in the room during API review is we should just fix the JIT here.  The resulting code this suggests looks rather unnatural.
## Avoid implicitly allocating params arrays in loops

**Approved** | [#runtime/33793](https://github.com/dotnet/runtime/issues/33793#issuecomment-887713862) | [Video](https://www.youtube.com/watch?v=fnqHXFKSpaw&t=0h33m19s)

The proposed fixes, and situations where they won't fix things, seem a bit strange.  There's value in highlighting that the array allocation is happening in a loop, but the manner in which it gets fixed (or ignored) is too variable.  So we shouldn't have a fixer at this time.  (Ideally we'll get params-span and that'll make this go away)

* Category: Performance
* Severity: Hidden
## Prefer Dictionary.Remove(key, out value) over Dictionary.this[key], followed by Dictionary.Remove(key)

**Approved** | [#runtime/33800](https://github.com/dotnet/runtime/issues/33800#issuecomment-887733349) | [Video](https://www.youtube.com/watch?v=fnqHXFKSpaw&t=0h50m51s)

We came up with a lot of cases where the fixer can cause surprising functional changes, so this really only applies when there's an indexer followed by a call to Remove with no other method calls in between (because any method call could have modified the dictionary).  As a specific example, in https://github.com/dotnet/runtime/issues/33800#issuecomment-853477405 if `Foo` throws then in the before case the dictionary is not modified, but in the after case it is.

The case of `x = dict[key]; dict.Remove(key);` isn't safe to replace, because the exception is lost.

Conclusion:

If we find a dictionary call to TryGetValue that is immediately followed by a call to Remove (and the dictionary and the key are both simple variable or parameter expressions) then suggest collapsing them to Remove(key, out T).  ("immediate" definitely means that there are no method calls or delegate invocations between those two expressions)

* Category: Performance
* Severity: Information

## Proposal: reconsider CLSCompliant and CA1014

**NeedsWork** | [#runtime/44194](https://github.com/dotnet/runtime/issues/44194#issuecomment-887742046) | [Video](https://www.youtube.com/watch?v=fnqHXFKSpaw&t=1h21m23s)

Removing the rule from AllEnabledByDefault, if possible, is reasonable.  But otherwise this is tightly coupled to the still in progress question of what we're doing with CLS compliance and we can't really take action at this time.
## Add analyzer: Prevent wrong usage of string Split

**Approved** | [#runtime/47927](https://github.com/dotnet/runtime/issues/47927#issuecomment-887754040) | [Video](https://www.youtube.com/watch?v=fnqHXFKSpaw&t=1h32m56s)

Using a messaging along the lines of "suspicious cast", having the rule identify calls to string.Split or StringBuilder..ctor that involves an implicit conversion from char to int seems like goodness (and we may add more specific API over time).  All of the methods/constructors that we group together in this rule will use the same diagnostic ID.

There are some generalizations (any implicit char->int in an argument; or that but only when there's an overload that takes char/string) which would be good to follow up on, but they're out of scope for this issue.

* Category: Correctness
* Severity: Warning
## Recommend use of concrete types to maximize devirtualization potential

**Approved** | [#runtime/51193](https://github.com/dotnet/runtime/issues/51193#issuecomment-887770999) | [Video](https://www.youtube.com/watch?v=fnqHXFKSpaw&t=1h48m56s)

As far as performance is concerned, the best results happen when this goes from an interface to a sealed type, but there's still goodness in moving up to a more-derived type (and less so for a more-derived interface).

The main identified complication is when a field, auto-property or variable is declared of an interface and changing it to a concrete type causes a compile failure (or runtime behavior) because of explicit interface implementations.

* Look at all assignments into the field or variable
* Find the most specific common type for all of those assignments
* If that type is not the current declaration type, trigger the diagnostic.  (And the fixer does that substitution)

Do not run on public or protected fields (or auto-properties) -- or `internal` if the assembly has any InternalsVisibleTo.

This may, in the case of explicit interface implementations, cause code to change or fail to compile, but we think we're OK with that.

* Category: Performance
* Severity: Information
