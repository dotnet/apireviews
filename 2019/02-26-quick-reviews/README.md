# Quick Reviews 2/26/2019

## Obsolete string.Copy(string) method

**Approved** | [#corefx/33602](https://github.com/dotnet/corefx/issues/33602#issuecomment-467551787) | [Video](https://www.youtube.com/watch?v=qlj29o0R1rs&t=0h0m0s)

We should mark this obsolete in .NET Standard as well. I'll submit the PR for that.

@GrabYourPitchforks are you on the hook for cleaning up any call sites in CoreFX?
## Expose existing BitOps methods

**Approved** | [#corefx/35419](https://github.com/dotnet/corefx/issues/35419#issuecomment-467557134) | [Video](https://www.youtube.com/watch?v=qlj29o0R1rs&t=0h11m51s)

Having a separate type makes sense as the existing types don't make sense (`Math`, `BitConverter`). We shouldn't put it in the `System` namespace though?

```C#
namespace System.Numerics
{
    public static partial class BitOperations
    {
    }
}
```
## Consider removing Range.OffsetAndLength

**Approved** | [#corefx/35508](https://github.com/dotnet/corefx/issues/35508) | [Video](https://www.youtube.com/watch?v=qlj29o0R1rs&t=0h26m31s)

## Remove some unnecessary HWIntrinsic API overloads

**Approved** | [#corefx/35560](https://github.com/dotnet/corefx/issues/35560) | [Video](https://www.youtube.com/watch?v=qlj29o0R1rs&t=0h31m46s)

## Add Rune.TryEncodeToUtf8Bytes

**Approved** | [#corefx/35530](https://github.com/dotnet/corefx/issues/35530#issuecomment-467570023) | [Video](https://www.youtube.com/watch?v=qlj29o0R1rs&t=0h55m42s)

Looks good, minor suggestion from review:

```C#
bool TryEncodeAsUtf8(...);
```

@GrabYourPitchforks should we also add a non-`Try` version that throws?
## EnvelopedCms encryption with RSA padding modes

**Approved** | [#corefx/34366](https://github.com/dotnet/corefx/issues/34366) | [Video](https://www.youtube.com/watch?v=qlj29o0R1rs&t=1h1m14s)

## UTF-8 web encoders

**Needs Work** | [#corefx/34830](https://github.com/dotnet/corefx/issues/34830#issuecomment-467582155) | [Video](https://www.youtube.com/watch?v=qlj29o0R1rs&t=1h9m17s)

After giving it more thought, we'd prefer the alternative proposal, meaning having a single class hierarchy for UTF16 and UTF8.

There are two ways to do the alternative:

1. Completely additive, i.e. non-breaking.
2. Redo the abstract members to make it sane (avoid `char*` and encoding agnostic API pattern).

@GrabYourPitchforks, please send me a mail with a write-up on the justification for the breaking change and I'll help you getting this approved by whoever is in charge for that :-)
## Unsafe.Unbox should return "ref readonly T", not "ref T"

**Rejected** | [#corefx/34258](https://github.com/dotnet/corefx/issues/34258#issuecomment-467591284) | [Video](https://www.youtube.com/watch?v=qlj29o0R1rs&t=1h33m11s)

It seems `ref` is a closer match. There is nuance, and that is sufficiently addressed by the `Unsafe` name plus proper documentation.

@GrabYourPitchforks, could you file a doc bug / submit a PR?
