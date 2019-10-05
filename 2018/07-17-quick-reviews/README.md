# Quick Reviews 7/17/2018

## Expand Process.Kill to Optionally Kill a Process Tree

**Approved** | [#corefx/26234](https://github.com/dotnet/corefx/issues/26234#issuecomment-405661428) | [Video](https://www.youtube.com/watch?v=dCwHCg9TUvA&t=0h0m0s)

We don't see the value in an enum as it seems unlikely that we'd do more than the two options. All examples we could come up with would likely result in more parameters. The usability of booleans on call sides have been solved in C# with named parameters.

We shouldn't make the parameter optional as there is no way this will ever be used by the compiler as we'd still have the parameterless version (which cannot be removed as that would be a binary breaking change).
## CSPRNG integers with ranges

**Approved** | [#corefx/30873](https://github.com/dotnet/corefx/issues/30873#issuecomment-405664452) | [Video](https://www.youtube.com/watch?v=dCwHCg9TUvA&t=0h10m49s)

We discussed if we should add a `float` or `double` version because that would be consistent with `Random` but that's because the underlying primitive there is a `double` while `RandomNumberGenerator`'s primitive is a random bits. We could add floating point version if someone ever cares.

We considered a parameterless overload `GetInt32()` as this would allow `int.MaxValue` to be part of the range but in practice there really isn't a scenario for it (and it can be down allocation free with the `GetBytes()` method if ever needed).

We also considered `GetInt64()` versions but given that these values are mostly used for indexing this seems equally over the top.

```csharp
namespace System.Security.Cryptography
{
  public abstract class RandomNumberGenerator
  {
    public static int GetInt32(int fromInclusive, int toExclusive);
    public static int GetInt32(int toExclusive);
  }
}
```
## Add Public API Marshal.GetExceptionPointers 

**Approved** | [#corefx/30677](https://github.com/dotnet/corefx/issues/30677#issuecomment-405668842) | [Video](https://www.youtube.com/watch?v=dCwHCg9TUvA&t=0h20m20s)

Agree with @jkotas  -- we should just return `null` as that's a valid answer even on Windows. Normally, if we cannot implement an API we'd throw `PlatformNotSupportedException` rather than `NotSupportedException`.

Looks good as proposed.
## Add MemoryExtensions.Trim methods for ReadOnlyMemory<char>

**Approved** | [#corefx/30592](https://github.com/dotnet/corefx/issues/30592) | [Video](https://www.youtube.com/watch?v=dCwHCg9TUvA&t=0h34m27s)

## Add better ZipFile extraction APIs

**Needs Work** | [#corefx/30424](https://github.com/dotnet/corefx/issues/30424#issuecomment-405676075) | [Video](https://www.youtube.com/watch?v=dCwHCg9TUvA&t=0h36m30s)

The scenario makes sense.

The idea with an option type looks good in principles, but we'd probably want one for creation and one for extraction because not all options for both sides -- which seems overkill.

@weshaggard had the idea of introducing the new method using default parameters so that we get away with fewer overloads.

@ianhays, could you give that a shot?
## Add ReadOnlySpan<char> overloads to CompareInfo

**Approved** | [#corefx/28001](https://github.com/dotnet/corefx/issues/28001#issuecomment-405683985) | [Video](https://www.youtube.com/watch?v=dCwHCg9TUvA&t=0h57m43s)

The scenario is reasonable.

@GrabYourPitchforks brought up that we should also add an overload for `GetHashCode()`.

It's worth pointing out `CompareInfo` can be derived from and the existing overloads are virtual. Presumably, these new APIs wouldn't normally delegate to them (because we want to avoid allocating a string in the first place), which now means that if a custom derivative exists and overrides the other methods, these methods would do the wrong thing.

However, the type doesn't expose a public constructor so in practice nobody can derive from it. And it doesn't seem our implementation does either.

Options:

1. **Ignore**. We make these methods non-virtual and don't delegate to the existing methods.

2. **Handle**. We make these methods virtual and have the implementation do a type check. If the instance is `CompareInfo` exactly, do the cheap thing. Otherwise, create strings and dispatch to the existing virtual methods. Implementers would have to override them to not pay the allocation cost.

We're leaning towards option (1) as we don't believe deriving from `CompareInfo` to be a thing.
## Add Badíʿ Calendar to Globalization

**Rejected** | [#corefx/30207](https://github.com/dotnet/corefx/issues/30207#issuecomment-405690297) | [Video](https://www.youtube.com/watch?v=dCwHCg9TUvA&t=1h18m58s)

Thanks for the detailed proposal!

The cost for us to support a calendar can be high because we have to react if definitions or rules change. It seems neither Windows, nor macOS, nor ICU support this calendar natively.

Thankfully, the .NET globalization APIs are extensible so you'll be able to ship this calendar as a NuGet package. If it gets very popular, adding it to the platform seems reasonable. But considering that major vendors haven't added it, makes us believe it might not be popular enough to warrant us carrying the calendar -- and committing on keeping it up-to-date.

For now, I'll be closing this issue. Please feel free to reopen if you believe your package has become so popular that adding it the platform is sensible.
