# Quick Reviews 07/30/2020

## System.Diagnostics.ActivitySource API Addition

**Approved** | [#runtime/40046](https://github.com/dotnet/runtime/issues/40046#issuecomment-666588567) | [Video](https://www.youtube.com/watch?v=7EBzsfHCQOM&t=0h0m0s)

* We should rename `w3cid` to `traceParent`
* We considered offering a span-based overload but right now the most common case for the use of the API starts with a header value which is currently only ever going to be a string. If the networking layer will give access in a span-based fashion we can add it.
* We should have a corresponding `Parse` method

```C#
namespace System.Diagnostics
{
    public readonly partial struct ActivityContext : IEquatable<ActivityContext>
    {
        public static ActivityContext Parse(string traceParent, string? traceState);
        public static bool TryParse(string traceParent, string? traceState, out ActivityContext context);
    }
}
```

## Mark existing Windows-specific APIs without a version number

**Approved** | [#runtime/40095](https://github.com/dotnet/runtime/issues/40095#issuecomment-666600842) | [Video](https://www.youtube.com/watch?v=7EBzsfHCQOM&t=0h32m1s)

* Looks good
* We should consider re-baselining when we bump the minimum Windows version, for similar benefits for net-new code.
* We should consider having an analyzer that flags checks for a higher version than what the guarded code actually needs.

```C#
[SupportedOSPlatform("windows")]
public void TheRegistry() { ... }

public void DoSomething()
{
    if (RuntimeInformation.IsOSPlatform(OSPlatform.Windows))
        TheRegistry();
}
```
## Mark System.Security.Cryptography.OpenSsl as unsupported on Windows

**Approved** | [#runtime/40101](https://github.com/dotnet/runtime/issues/40101#issuecomment-666606594) | [Video](https://www.youtube.com/watch?v=7EBzsfHCQOM&t=0h52m4s)

* Looks good.
* It seems the direction is that Windows will require a store package. That's not expressible in an OS check. If that becomes the case we can remove the attribute, build a one-off analyzer, or generalize this into a new attribute.

**Assembly**: System.Security.Cryptography.OpenSsl

```C#
[assembly: UnsupportedOSPlatform("windows")]
```

## Provide an overload of float/double.ToString that allow precision greater than 99

**Approved** | [#runtime/30657](https://github.com/dotnet/runtime/issues/30657#issuecomment-666609015) | [Video](https://www.youtube.com/watch?v=7EBzsfHCQOM&t=1h0m23s)

* It was suggested to add support for custom cultures. New shape:

```C#
namespace System
{
    public struct Single
    {
        public string ToString(char format, int precision);
        public string ToString(char format, int precision, IFormatProvider? provider);
    }
    public struct Double
    {
        public string ToString(char format, int precision);
        public string ToString(char format, int precision, IFormatProvider? provider);
    }
}
```
