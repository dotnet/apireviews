# API Review 10/28/2025

## Provide an async entry point helper to enable platforms that need an async return from `Main()`

**Approved** | [#runtime/121046](https://github.com/dotnet/runtime/issues/121046#issuecomment-3457693566)

FYI @333fred 
## Add missing overloads to "Flow System.Text.Rune through more APIs" proposal

**Approved** | [#runtime/120015](https://github.com/dotnet/runtime/issues/120015#issuecomment-3457800777)

We talked about the F#-breaks that happened as a result of this.  In the end, we decided to keep it as-is, and just accept that such breaks can occur in F# when writing functions with weakly typed parameters (or similar weak context calls).
## Add EventSource guid ctors for non-reflection creation

**Approved** | [#runtime/28290](https://github.com/dotnet/runtime/issues/28290#issuecomment-3457869922)

We flipped the string and Guid parameters (so the `base()` call starts with the name, not the GUID); otherwise looks good as proposed.

```csharp
namespace System.Diagnostics.Tracing;

public partial class EventSource
{
    protected EventSource(string eventSourceName, Guid eventSourceGuid);
    protected EventSource(string eventSourceName, Guid eventSourceGuid, EventSourceSettings settings, string[] traits = null);
}
```
