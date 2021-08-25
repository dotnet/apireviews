# API Review 08/25/2021

## Obsolete Rfc2898DeriveBytes constructors with unsafe defaults

**Approved** | [#runtime/57046](https://github.com/dotnet/runtime/issues/57046#issuecomment-905106244)

__tl;dr:__ I'm good with these obsoletions and the proposed fixer which rewrites them using the newer overloads.

### Words, words, words

I see this API as a fundamental building block. Cryptographic specs are usually written in terms of families of algorithms which have various configuration knobs. Sometimes the knobs should be tweaked depending on the various user scenario (CBC vs CTS padding modes for symmetric algorithms) or as a security / performance tradeoff (key length or iteration count).

IMO, .NET should not be in the business of prescribing defaults for cryptographic primitives. By setting such defaults, we're either lifting one scenario to be the One True Scenario™ that should be preferred above all others, or we're locking in a security / performance tradeoff that might only make sense at a snapshot in time. (The defaults in this API fall under this latter bucket.)

Ideally our basic cryptographic primitive APIs (including this!) should require callers to specify all configuration knobs as appropriate for their scenario. Yes, it's verbose, but it's the right thing to do to ensure that the caller is fully aware of what they're doing and that they understand they take full responsibility for its applicability to their scenario. This gatekeeps these APIs to an extent, but I am ok with that given that only knowledgeable people should be using the primitives directly.

By forcing callers to set parameters explicitly, we also make it easier for code reviewers or automated tools to audit the call sites for appropriateness. This could include mandating a minimum iteration count or banning certain algorithm families. We have such rules within Microsoft, for instance.

The vast majority of our users - those who are scenario-driven rather than trying to adhere to a particular protocol - would be better served by an opinionated crypto stack. Opinionated stacks are somewhat opaque in that they hide the complexity of choosing appropriate defaults, but they're usually _much_ easier to use and expose only pit-of-success APIs. (The aspnet crypto stack is the best example of an opinionated crypto stack within .NET, but I'm also biased here, soooooo... 😃)

Given that this is a building block and not an opinionated API, I'm not terribly concerned with keeping it approachable. It should be as verbose as necessary to make the call site understandable, with appropriate documentation updates if needed.
## Provide an API to get the native module handle of the native process entry-point module

**Approved** | [#runtime/56331](https://github.com/dotnet/runtime/issues/56331#issuecomment-905849011)

* It was requested to rename the API from `GetEntryPointModuleHandle()` to `GetMainProgramHandle()`.
* Looks good as proposed

```C#
namespace System.Runtime.InteropServices
{
     public static class NativeLibrary
     {
          public static IntPtr GetMainProgramHandle();
     }
}
```
