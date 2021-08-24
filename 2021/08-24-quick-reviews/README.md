# API Review 08/24/2021

## Allow for atomic appends to end of file

**NeedsWork** | [#runtime/53432](https://github.com/dotnet/runtime/issues/53432#issuecomment-904876479) | [Video](https://www.youtube.com/watch?v=6l_654hvx0g&t=0h0m0s)

* We should measure the performance overhead and establish clarity on what we believe the "right behavior" is for new code. One option, that we shouldn't dismiss, is doing a behavior breaking change in .NET 7 and provide compat switch. However, that's only appropriate if we believe being broken is a fringe case and code should generally use the atomic append behavior. There are conceivable scenarios where someone might open multiple handles within the same process and the new behavior would regress performance.
* Another middle ground option is to expose the new setting and change the behavior of higher level APIs, such as `File.AppendLines`. This way, we don't make the average person's code more complex while also giving the power users the control they need.
* @adamsitnik will do some benchmarking and come back with findings.
## Obsolete Rfc2898DeriveBytes constructors with unsafe defaults

**NeedsWork** | [#runtime/57046](https://github.com/dotnet/runtime/issues/57046#issuecomment-904885558) | [Video](https://www.youtube.com/watch?v=6l_654hvx0g&t=0h24m46s)

* Looks good.
* We should change the wording to what Jeremy suggested:
   > The default hash algorithm and iteration counts in Rfc2898DeriveBytes constructors are outdated and insecure. Use a constructor that accepts the hash algorithm and the number of iterations.
* We might want to consider calling out the defaults in the obsolete message so that people can apply the current defaults (having a fixer would be even better though)
* We should assign a custom diagnostic ID
## Add UnicodeRanges.ArabicExtendedB to System.Text.Encodings.Web

**Approved** | [#runtime/57609](https://github.com/dotnet/runtime/issues/57609#issuecomment-904887016) | [Video](https://www.youtube.com/watch?v=6l_654hvx0g&t=0h39m30s)

Looks good as proposed

```C#
namespace System.Text.Unicode
{
    // existing type
    public static class UnicodeRanges
    {
        // new API
        public static UnicodeRange ArabicExtendedB { get; }
    }
}
```
## RegexOptions.Constrained

**NeedsWork** | [#runtime/57891](https://github.com/dotnet/runtime/issues/57891#issuecomment-904900270) | [Video](https://www.youtube.com/watch?v=6l_654hvx0g&t=0h41m33s)

* It seems we need to find a name that makes it clear what it does; also it seems we want most people to pick this for new regexes, assuming they can live with the caveats.
* We should discuss this with @stephentoub and others

```C#
namespace System.Text.RegularExpressions
{
    public enum RegexOptions
    {
        Constrained = 0x0400
    }
}
```
