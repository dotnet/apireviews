# API Review 06/01/2023

## Prefer ValueTuple over Tuple

**Approved** | [#runtime/33791](https://github.com/dotnet/runtime/issues/33791#issuecomment-1572486116) | [Video](https://www.youtube.com/watch?v=cAbUh4CD0Qg&t=0h0m0s)

Generally seems like a good idea.

A concern was raised about potentially limiting this to 5-tuples or smaller (or any other arbitrary number), but we don't feel that's important.

One diagnostic ID should be used for member bodies and for member signatures.  By default the analyzer should not report on "exposed" API (e.g. public, protected), but it would be appropriate to have a configuration parameter to allow opting in to exposed API.

For the fixer, we identified a concern around type coercion (e.g. `new Tuple<long, string>(42, "hi")` involves a cast of the `int` 42 to the `long` 42).  It may make sense to start the fixer with only `Tuple.Create` callsites, since type coercion is less of a concern with that pattern.

Category: Performance
Severity: Info
## Analyzer: Introduce an analyzer to recommend the more efficient string splitting features in .NET 8

**Approved** | [#runtime/85487](https://github.com/dotnet/runtime/issues/85487#issuecomment-1572516140) | [Video](https://www.youtube.com/watch?v=cAbUh4CD0Qg&t=0h17m22s)

Out of a concern of false positives, it seems like the initial version of this analyzer should be limited to those callsites where a count was passed into `string.Split`, as that pattern has a perfect analogue in the span-based split.

Using uncounted string.Split in a foreach would be better handled by an iterator-based split (which we don't have yet).  Other uses of uncounted split might be analyzable, but they're harder to describe, so they should probably be a different diagnostic ID (once the pattern can be analyzed) just to keep the docs/explanation sane.

The suggestion regarding Trim/TrimEnd/TrimStart may have merit, but seems like it might need some more thought (as it depends heavily on what happens after the call to Trim)... and should be addressed in a separate issue, with more details/examples.

Category: Performance
Severity: Info
## Analyzer: Recommend upgrading to CompositeFormat for enhanced performance

**Approved** | [#runtime/85525](https://github.com/dotnet/runtime/issues/85525#issuecomment-1572548360) | [Video](https://www.youtube.com/watch?v=cAbUh4CD0Qg&t=0h35m3s)

In discussion we identified three different cases, that should have three different diagnostic IDs:

* The format string is a literal.
  * The suggested fix for this is to use an interpolated string.
  * We believe we've already approved (and perhaps implemented) this one, but we can't find it.  If it doesn't exist, it's now approved.
  * Default visibility: Info
* The format string comes from a static reference (a static property, method, or field).
  * The suggested fix is to introduce a static CompositeFormat field and change the string.Format calls to use that field.
  * Default visibility: Hidden
* The format string comes from a `const`
  * Both fixers should be suggested.
  * There may be nuance... member-local consts, consts from a foreign assembly, ...
  * Default visibility: Hidden

Category: Performance
## Support await'ing a Task without throwing

**Approved** | [#runtime/22144](https://github.com/dotnet/runtime/issues/22144#issuecomment-1572617119) | [Video](https://www.youtube.com/watch?v=cAbUh4CD0Qg&t=0h59m53s)

In discussion we feel that it's valid to add this to `Task<T>` as well as `Task`, but that the ConfigureAwait method on `Task<T>` will throw an ArgumentException (or type derived therefrom) if SuppressException is asserted.

We may consider the ValueTask family in later releases.

We should also provide an analyzer that detects the call to `Task<T>.ConfigureAwait` asserting the SuppressException flag, and giving an explicit a priori Warning (category: Usage)

```C#
namespace System.Threading.Tasks;

public partial class Task
{
    public ConfiguredTaskAwaitable ConfigureAwait(ConfigureAwaitOptions options);
}

public partial class Task<TResult>
{
    // throws if ConfigureAwaitOptions.SuppressExceptions is asserted (or any unknown value, of course)
    public new ConfiguredTaskAwaitable<TResult> ConfigureAwait(ConfigureAwaitOptions options);
}

[Flags]
public enum ConfigureAwaitOptions
{
    None = 0,
    ContinueOnCapturedContext = 0x1,
    SuppressExceptions = 0x2,
    ForceYielding = 0x4,
    ForceAsynchronousContinuation = 0x8,
}
```
