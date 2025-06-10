# API Review 06/10/2025

## Add InitializeControl(int deviceDpi) to Control for Early Initialization

**Rejected** | [#winforms/13550](https://github.com/dotnet/winforms/issues/13550#issuecomment-2960189335) | [Video](https://www.youtube.com/watch?v=TZ-WbijLyKM&t=0h0m0s)

* The deviceDpi parameter doesn't seem to be addressing a real problem, so it should be removed.
* It seems like this initialization is already possible in overrides of get_CreateParams, and doing work either before or after the call to base.get_CreateParams.

```C#
namespace System.Windows.Forms;

public partial class Control
{
    protected virtual void InitializeControl(int deviceDpi);
}
```
## Generic Enumerable.Sequence method

**Approved** | [#runtime/97689](https://github.com/dotnet/runtime/issues/97689#issuecomment-2960335829) | [Video](https://www.youtube.com/watch?v=TZ-WbijLyKM&t=0h59m6s)

* We decided the best answer was to avoid complicating Range, and instead shift to the requested API of Sequence.
* Everyone felt that `(start, step, end)` was an unnatural ordering, so the terminating and non-terminating have different names.

```C#
namespace System.Linq;

public partial class Enumerable
{
    public static IEnumerable<T> InfiniteSequence<T>(T start, T step) where T : IAdditionOperators<T, T, T>;
    public static IEnumerable<T> Sequence<T>(T start, T endInclusive, T step) where T : INumber<T>;
}
```

