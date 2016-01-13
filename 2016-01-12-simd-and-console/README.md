# API Review 2016-01-12

This API review was also recorded and is available on [Google+](https://plus.google.com/events/cthpiilbq2hhe0etgeg3mgue72g).

## Overview

In this API review we reviewed the following PRs and proposals:

* [Notes](#5202-api-proposal-support-for-constructing-and-copying-vectort-from-pointers) | [#5202](https://github.com/dotnet/corefx/issues/5202) [API Proposal] Support for constructing and copying Vector\<T> from pointers
* [Notes](#4636-missing-several-valuable-members-of-systemconsole-in-contract) | [#4636](https://github.com/dotnet/corefx/issues/4636) Missing several valuable members of System.Console in contract

There are also some [follow-ups for the API review board](#follow-ups-for-api-review-board).

## Notes

### #5202 [API Proposal] Support for constructing and copying Vector\<T> from pointers

Status: **Approved with feedback** |
[APIs](VectorOfT.md) |
[Issue](https://github.com/dotnet/corefx/issues/5202) |

* We should probably get rid of `IntPtr` overloads because the code is inherently unsafe
* We should get rid of the `offset` parameter, because it's unclear whether it's a byte offset or an index.
* Should we include a `length` for the constructors and/or the `CopyTo` method?
    - @terrajobst will follow u
* Should we include a ctor that allows padding?
    - Is a separate feature, filed [#5360](https://github.com/dotnet/corefx/issues/5360)

For agreed upon shape, see [APIs](VectorOfT.md).

#### Q & A

> Question with IntPtr would that be required for use by VB.NET?
> --Ben Adams

That's correct. However, in order to use the API meaningfully you already have to deal with native memory and thus are not likely using VB because it doesn't support `stackalloc` or pointers in general.

Also, it's worth pointing out that this API doesn't allow you to do something that can't do already. It's simply initializing a vector from a pointer. You can obviously initialize a vector via the other ways (e.g. from an array).

> Pass in remaining buffer length? Error if `Vector.Count > remaining` in buffer. If you take length to copy, people will just pass in `Vector<T>.Count`; would be better to be an amount remaining in buffer check (like a bounds check)
> --Ben Adams

Good point. See comments above; @terrajobst is following up.

### #4636 Missing several valuable members of System.Console in contract

Status: **Approved with feedback** |
[APIs](Console.md) |
[Issue](https://github.com/dotnet/corefx/issues/4636)

* We should throw from `CursurSize.set`
* We're fine with exposing `Encoding`, `TextReader`, and `TextWriter` from `Console`, i.e. no further factoring is expected to happen
* We shouldn't expose `__arglist` overloads as we don't really want to support them on .NET Core (can only be used from managed C++ which isn't supported on .NET Core either)
* We generally want to expose APIs that we can't support in a cross-platform fashion

For agreed upon shape, see [APIs](Console.md).

#### Q & A

> I would rather that an API that is not supported would throw an exception. But like mentioned, there needs to be enough information in the exception to know what wrong - and if possible how to fix it
> --Mordechai Zuber

Agreed, we came to the same conclusion.

### Follow-ups for API review board

* Should we always port APIs that we can't support everywhere?
