# API Review 04/11/2023

## Ability to set the default icon for Forms.

**Approved** | [#winforms/8905](https://github.com/dotnet/winforms/issues/8905#issuecomment-1503796848) | [Video](https://www.youtube.com/watch?v=sPkVcPeFeis&t=0h0m0s)

* Looks good as proposed
* We should make sure that `Form.DefaultIcon.Dispose()` does something useful

```C#
namespace System.Windows.Forms;

public partial class Form
{
    public static System.Drawing.Icon DefaultIcon { get; set; }
}
```
## Allow ControlCollection.AddRange to use params keyword.

**Approved** | [#winforms/8938](https://github.com/dotnet/winforms/issues/8938#issuecomment-1503804011) | [Video](https://www.youtube.com/watch?v=sPkVcPeFeis&t=0h13m20s)

* Looks good as proposed

```C#
namespace System.Windows.Forms;

public partial class Control
{
    public partial class ControlCollection
    {
        // Existing signature:
        // public virtual void AddRange(Control[] controls);
        public virtual void AddRange(params Control[] controls);
    }
}
```
## Add Extract methods to Icon

**Approved** | [#winforms/8929](https://github.com/dotnet/winforms/issues/8929#issuecomment-1503837274) | [Video](https://www.youtube.com/watch?v=sPkVcPeFeis&t=0h19m13s)

* `size` should be of type `int`
* Let's not expose `GetIconCount()`, folks who want to enumerate can use a `while`-loop plus a `null` check
    - We'd like to add a better API that returns all icons later, having a count method encourages a less performant pattern because enumeration repeatedly opens and closes the provided file.

```C#
namespace System.Drawing;

public partial class Icon
{
    // Existing:
    // public static Icon ExtractAssociatedIcon(string filePath);
    public static Icon? ExtractIcon(string filePath, int id, bool smallIcon = false);
    public static Icon? ExtractIcon(string filePath, int id, int size);
}

public partial class SystemIcons
{
    // Existing:
    // public static Icon GetStockIcon(StockIconId stockIcon, StockIconOptions options = StockIconOptions.Default);
    public static Icon GetStockIcon(StockIconId stockIcon, int size);
}
```
## Add first class System.Threading.Lock type

**Approved** | [#runtime/34812](https://github.com/dotnet/runtime/issues/34812#issuecomment-1503911687) | [Video](https://www.youtube.com/watch?v=sPkVcPeFeis&t=0h46m15s)

* `Enter()` and `TryEnter()` shouldn't throw `OverflowException` because that is meant for arithmetic overflows. A more appropriate exception would be `LockRecursionException`.
* Will support OS threads and green threads
* This will require the `Enter()`/`Exit()` to be used from the same thread (OS thread or green thread), hence async APIs can't be used which is also why we wouldn't expose `EnterAsync()`. If we wanted that, we'd expose an `AsyncLock` type too.
* We wouldn't expose a wait handle, if you wanted to have one, use `Mutex` instead
* `EnterLockWithHolder` seems unfortunate, how about `EnterScope()`?
    - We should rename the type to `Scope`
    - Alternatively, we could make `Enter` return the holder rename all other methods that operate without a holder by having a `Raw` or `Slim` suffix

```C#
namespace System.Threading;

public sealed class Lock
{
    public Scope EnterScope();

    public void Enter();
    public void Exit();
    public bool TryEnter();
    public bool TryEnter(TimeSpan timeout);
    public bool TryEnter(int millisecondsTimeout);

    public bool IsHeldByCurrentThread { get; }

    public struct Scope : IDisposable
    {
        public void Dispose();
    }
}
```

## Support binary format specifier 'b' and 'B' (standard numeric format strings)

**Approved** | [#runtime/83619](https://github.com/dotnet/runtime/issues/83619#issuecomment-1503944035) | [Video](https://www.youtube.com/watch?v=sPkVcPeFeis&t=1h45m40s)

* Looks good as proposed
    - `b` and `B` are the formatting modifiers

```C#
namespace System.Globalization;

[Flags]
public partial enum NumberStyles
{
    AllowBinarySpecifier = 0x00000400,
    BinaryNumber = AllowLeadingWhite | AllowTrailingWhite | AllowBinarySpecifier,
}
```

