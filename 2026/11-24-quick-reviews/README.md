# API Review 11/24/2026

## Add Architecture.Wasm64

**Approved** | [#runtime/137805](https://github.com/dotnet/runtime/issues/137805#issuecomment-7140292396) | [Video](https://www.youtube.com/watch?v=uV0L5ca43mU&t=0h0m0s)

Looks good as proposed.

```csharp
 namespace System.Runtime.InteropServices;

 public partial enum Architecture
 {
     Wasm64 = 10,
 }
```
## Lazy subcommand registration

**Approved** | [#runtime/138204](https://github.com/dotnet/runtime/issues/138204#issuecomment-7140793842) | [Video](https://www.youtube.com/watch?v=uV0L5ca43mU&t=0h5m22s)

Looks good as proposed.

The overload with `aliases` is being added in the middle, but we feel that's aesthetically OK so that the factory-lambda is the last argument, and there's no positional confusion as they're distinct types.

```csharp
namespace System.CommandLine;

public partial class Command
{
    public void Add(string name, Func<Command> commandFactory);
    public void Add(
        string name,
        ReadOnlySpan<string> aliases,
        Func<Command> commandFactory);
}
```
