# Quick Reviews 12/4/2015

## Missing several valuable members of System.Console in contract

**Approved** | [#corefx/4636](https://github.com/dotnet/corefx/issues/4636#issuecomment-162039060) | [Video](https://www.youtube.com/watch?v=nG-Af_TgYbI&t=0h0m0s)

Looks good, except for `BufferWidth` and `BufferHeight` because they are not meaningful on Linux; they basically have to return `WindowWidth` and `WindowHeight`.

@stephentoub, can you create a list of all the missing APIs in `Console`? We should probably add all except for the ones we don't want/can't support.

## Add ValueTask to corefx

**Approved** | [#corefx/4708](https://github.com/dotnet/corefx/issues/4708#issuecomment-162042013) | [Video](https://www.youtube.com/watch?v=nG-Af_TgYbI&t=0h10m27s)

No major concerns. We should agree, though, how we name types that provide a struct-based alternative to a class. Should this be a prefix or a suffix?
- **Prefix**. Matches how must people read C# code, i.e. `struct Foo`, `ValueFoo`.
- **Suffix**. Sorts better in IntelliSense, documentation, or any other case where APIs are sorted.

The best location seems to be `System.Threading.Tasks`. We need to check whether we can make this a partial façade for .NET Framework.

