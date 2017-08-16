# Design Review: Hardware Intrinsics for Intel

Status: **Approved with Feedback** | [Issue] | [Video] | [Design]

[Issue]: https://github.com/dotnet/corefx/issues/22940
[Video]: https://www.youtube.com/watch?v=52Fjrhx7pKU
[Design]: https://github.com/dotnet/designs/blob/master/accepted/platform-intrinsics.md

We recently approved a [design proposal][Design] to expose hardware intrinsics.
In this context, an *intrinsic* is a special API that doesn't actually have an
implementation in IL form but is specialized by the code generator (JIT, AOT).

This allows exposing specialized CPU instructions, such as SIMD or CRC32,
without having to drop to the assembly level. Developers can write regular code,
think in expressions and local variables, and the code generator can still apply
the usual optimizations.

This approach differs from our existing SIMD infrastructure, especially
`Vector<T>`, in that these API are hardware specific. In other words, code using
it is no longer portable between CPU architectures. Furthermore, we do not
provide a software fallback. Instead, the developer is expected to guard calls
using a provided capability API. Failing to do so will cause
`PlatformNotSupportedException` at runtime.

In order to remain portable, the developer must provide a software fallback.
While this sounds like we're putting the burden on the developer, we learned
that we simply can't provide an efficient enough software fallback at the
granularity level of intrinsics. Vectorized and non-vectorized approaches are
fundamentally different and running a software-implemented vectorized algorithm
is often several factors slower than a native non-vectorized version.

Today, we looked at a concrete proposal from our friends at Intel to expose SIMD
building blocks. We've also used this proposal to refine our general approach on
how these should be exposed.

## Capability Checks

Since developers are expected to perform capability checks, it's important to
have a granularity that is (1) easy to understand and (2) not too fragile.

Making every single API capability-based would be annoying to use and hard to
evolve as the capability checks are usually at a higher layer, thus far away
from where the intrinsics are actually consumed. This makes it fragile in the
sense that slight revisions to the code might require new capability checks.

We decided to tie capability checks to instruction set specifications. That's
because instruction set specifications are expected to be implemented whole sale
or not at all by a given CPU model. Also, developers using intrinsics have to be
familiar with the specification in order to put them to use. Hence, the
specifications can be expected to be a part of the developer's domain knowledge.

We'll provide one type per instruction set specification which includes a single
capability API to check for the entire set. For instance, the namespace `X86`
will contain the types `Sse`, `Sse2`, and `Avx`. In order to use SSE2 specific
functionality, the developer only has to check for `Sse2.IsSupported` and can
then assume that all intrinsics provided for `Sse2` will work. To use both SSE2
and AVX the developer has to check for `Sse2.IsSupported && Avx.IsSupported`.

This approach minimizes the number of capability checks and makes it easy for
the developer to reason about the granularity and associated guarantees.

## Namespaces

* `System.Runtime.Intrinsics`. Contains types we expect to be used across
  several architectures (e.g. `Vector128`)
* `System.Runtime.Intrinsics.x86`. Contains types for instruction sets that
  are specific for the Intel x86 architecture.

**Note:** We decided not to use the `CompilerServices` namespace as this
namespace is designed not to be used directly by developers but by compilers to
aid the code generation. For instance, it includes helper types for the async
state machine.

## Just-in-time (JIT) vs. ahead-of-time (AOT) compilation

In the JIT scenario the runtime will initialize internal data structures during
startup that effectively populate the `IsSupported` checks into compile time
constants. During JIT, code blocks guarded by unsupported capability checks are
simply thrown away. This results in smaller code sizes and even allows granular
checks inside of loops.

In the case of AOT, there are basically two options:

1. **No light-up**. This is logically equivalent to run the JIT for a specific
   environment and persist the output. This means all the capability checks are
   effectively evaluated at build time. Code will not light up on CPU models
   with more features.

2. **With light-up**. This would mean the build is produced for a specific CPU
   architecture with dynamic checks for specific instruction sets. This allows
   resulting code, for instance, to leverage AVX instructions while still
   running on CPUs that only support SSE2. This poses interesting questions to
   how this impacts the granularity of the checks because hot loops shouldn't
   have to evaluate capability checks on every iteration. Ideally, this would be
   solved with a smart AOT compiler.

## C# language feature: Requiring parameters to be literals

Certain intrinsics require the operand to become part of the encoded instruction
and thus have to be literals (i.e. these can't be dynamic values which are
stored in memory or a register). There is currently no way in C# to require a
parameter value to be a literal.

The idea is to have a way to mark a parameter (e.g. using `const` syntax or via
an attribute) and encode the requirement as a `modreq` in metadata. We need to
bring this feature request to the LDT (C# language design team) as it might
impact overload resolution.

Some addition comments:

* C++ doesn't honor `modreq`s it doesn't understand, i.e. it will simply ignore
  them and allow the code to call the API.
* F# is the same as C++, but they
  [seem to be open to change that](https://github.com/Microsoft/visualfsharp/issues/3105)
* C# and VB honor them correctly, i.e. they generate errors for calls to APIs
  where at least one of the applied `modreq`s isn't understood.
* We cannot reuse the existing C++ `IsConst` modifier because we need stronger
  guarantees (we need literals, C++ constants aren't the same as C#'s)
* For now, we don't care about enforcing well-formed bit patterns

## Notes

* `IsSupported` should be a property
* Naming
    - No wrappers for C++ style intrinsics, someone else could write it
    - We should follow .NET FX casing conventions
    - We believe the abbreviations are fine
* Load instructions
    - All require unsafe, which is unfortunate because it blocks VB users,
      same for F#
    - We should probably add overloads that take `Span<T>` or `ref T`. Let's
      go with just `Span<T>` for now
    - One problem with `Span<T>` is that it has overhead (bounds checking)
    - Some operations require alignment, but doing that is quite hard. While the
      runtime does provide some alignment on stack is does not guarantee to be
      aligned arbitrarily (e.g. 256 boundary)
    - `mem` -> `address`. Maybe we should call it `vector` instead, to make it
      clear that we'll read more values.
* Comparisons
    - `CompareVector128` seems like it should be an overload (i.e. just
      `Compare`)
* We should probably provide a software `Crc32` somewhere else in the BCL
    - Potentially we could optimize that to take advantage
