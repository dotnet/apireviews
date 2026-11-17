# API Review 11/17/2026

## Let Process.Run* kill the entire process tree on timeout or cancellation

**Approved** | [#runtime/136466](https://github.com/dotnet/runtime/issues/136466#issuecomment-6957704184) | [Video](https://www.youtube.com/watch?v=qY4UuIvAxHY&t=0h0m0s)

* We prefer putting the option on ProcessStartInfo.
* To mitigate the confusion of the value when it is returned from querying running processes, we made it nullable.
* After discussing names for a long time, we landed on KillTreeOnRunCancellation

```csharp
namespace System.Diagnostics
{
    public class ProcessStartInfo
    {
        public bool? KillTreeOnRunCancellation { get; set; }
    } 
}
```
## Check for correct event ID passed to `EventSource.WriteEvent` based on the `System.Diagnostics.Tracing.EventAttribute`

**Approved** | [#runtime/137091](https://github.com/dotnet/runtime/issues/137091#issuecomment-6958127347) | [Video](https://www.youtube.com/watch?v=qY4UuIvAxHY&t=0h48m12s)

* As a test case, decide if the two need to evaluate to the same constant expression or be literally the same expression.
  * E.g. can the attribute reference a `const` and the WriteEvent call be a literal?
* Before removing support for runtime manifest matching, decide if the loss of fidelity from F# or COBOL.NET is acceptable.

Category: Usage
Level: Warning
## Expose the type and value stored in a System.Reflection.Pointer

**Approved** | [#runtime/137512](https://github.com/dotnet/runtime/issues/137512#issuecomment-6958202952) | [Video](https://www.youtube.com/watch?v=qY4UuIvAxHY&t=1h6m12s)

Looks good as proposed.

```csharp
namespace System.Reflection;

public sealed partial class Pointer
{
    // The pointer type of this instance, for example typeof(int*).
    public Type PointerType { get; }

    // The pointer stored in this instance.
    public void* Value { get; }
}
```
## Warn on attributes with no effect

**Approved** | [#runtime/131094](https://github.com/dotnet/runtime/issues/131094#issuecomment-6958388410) | [Video](https://www.youtube.com/watch?v=qY4UuIvAxHY&t=1h9m8s)

We feel that grouping all of these into the existing `CA2259` is the right answer.  The generalized form is something like `"The '[{AttributeName}]' attribute has no effect on member '{MemberName}'"`, but fully customized messages per attribute is fine.

Category: [maintain current]
Level: [maintain current]
