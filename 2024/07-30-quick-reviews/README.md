# API Review 07/30/2024

## Introduce Dark Mode and A11Y compatible Visual Styles for Apps targeting .NET 9 and Win 11

**Approved** | [#winforms/7641](https://github.com/dotnet/winforms/issues/7641#issuecomment-2258896980) | [Video](https://www.youtube.com/watch?v=ftOBEI9IlEU&t=0h0m0s)

* Having two diagnostics (one for dark mode and one for visual styles works)
* `Microsoft.VisualBasic.ApplicationServices`
    - The props on `WindowsFormsApplicationBase` should be marked as `[EditorBrowsable(Never)]`
    - The props should be marked as experimental as well
    - While the feature is experimental there won't be any UI for changing the dark mode / visual styles, it can only be done in user code, which also ensures that the user gets a diagnostic unless they opt-in

```C#
namespace System.Windows.Forms;

[Experimental("WFO9000")]
public enum SystemColorMode
{
    Classic = 0,
    System = 1,	
    Dark = 2,	
}

public static partial class Application
{
    [Experimental("WFO9000")]
    public static SystemColorMode ColorMode { get; }  

    [Experimental("WFO9000")]
    public static bool SetColorMode(SystemColorMode colorMode);

    [Experimental("WFO9000")]
    public static SystemColorMode SystemColorMode { get; }
}

public class TextBoxBase : Control
{
    // Remove:
    // public new Padding Padding {get; set; }
}

public enum ControlStyles
{
    [Experimental("WFO9001")]
    ApplyThemingImplicitly = 0b00001000_00000000_00000000, 
}

public unsafe partial class Form
{
    [Experimental("WFO9000")]
    public void SetWindowBorderColor(Color color);

    [Experimental("WFO9000")]
    public void SetWindowCaptionColor(Color color);

    [Experimental("WFO9000")]
    public void SetWindowCaptionTextColor(Color color);

    [Experimental("WFO9000")]
    public void SetWindowCornerPreference(WindowCornerPreference cornerPreference);

    [Experimental("WFO9000")]
    public enum WindowCornerPreference
    {
        Default = 0,
        DoNotRound = 1,
        Round = 2,
        RoundSmall = 3
    }
}

[Experimental("WFO9001")]
public enum VisualStylesMode : Short
{
    Classic = 0,
    Disabled = 1,
    Net10 = 2,
    Latest = short.MaxValue
}

public static partial class Application
{
    [Experimental("WFO9001")]
    public static VisualStylesMode DefaultVisualStylesMode { get; }

    [Experimental("WFO9001")]
    public static void SetDefaultVisualStylesMode(VisualStylesMode styleSetting);
}

public unsafe partial class Control 
{
    [Experimental("WFO9001")]
    public VisualStylesMode VisualStylesMode { get; set; }

    [Experimental("WFO9001")]
    public event EventHandler? VisualStylesModeChanged;

    [Experimental("WFO9001")]
    protected virtual void OnVisualStylesModeChanged(EventArgs e);

    [Experimental("WFO9001")]
    protected virtual VisualStylesMode DefaultVisualStylesMode { get; }
}

public enum Appearance
{
    Normal = 0,
    Button = 1,
    [Experimental("WFO9001")]
    ToggleSwitch = 2
}
```

```C#
namespace Microsoft.VisualBasic.ApplicationServices;

public partial class ApplyApplicationDefaultsEventArgs : EventArgs
{
    [Experimental("WFO9000")]
    public SystemColorMode ColorMode { get; set; }

    [Experimental("WFO9001")]
    public VisualStylesMode VisualStylesMode { get; set; }
}

public partial class WindowsFormsApplicationBase
{
    [Experimental("WFO9000")]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public SystemColorMode ColorMode { get; set; }

    [Experimental("WFO9001")]
    [EditorBrowsable(EditorBrowsableState.Never)]
    public VisualStylesMode VisualStylesMode { get; set; }
}
```
## Introduce InvokeAsync, ShowAsync, ShowDialogAsync

**Approved** | [#winforms/4631](https://github.com/dotnet/winforms/issues/4631#issuecomment-2259003267) | [Video](https://www.youtube.com/watch?v=ftOBEI9IlEU&t=0h47m38s)

### `InvokeAsync`

* Should take a cancellation token in the callbacks
* The callbacks should return value tasks
* Cancellation token should be defaulted
* Is not experimental

```C#
namespace System.Windows.Forms;

public partial class Control
{
    public Task InvokeAsync(Action callback, CancellationToken cancellationToken = default);
    public Task InvokeAsync(Func<CancellationToken, ValueTask> callback, CancellationToken cancellationToken = default);
    public Task<T> InvokeAsync<T>(Func<CancellationToken, ValueTask<T>> callback, CancellationToken cancellationToken = default);
    public Task<T> InvokeAsync<T>(Func<TResult> callback, CancellationToken cancellationToken = default);
}
```

### `ShowAsync`/`ShowDialogAsync`

* What happens if the token is cancelled?
    - The UI is being closed and the equivalent of `DialogResult.Cancelled` is returned.
    - That seems unfortunate and it's not what cancellation token are normally used for.
    - It seems better to leave off cancellation token for now.
* `TaskDialog`
    - Aligned the overloads with the existing `ShowDialog` method, including defaults
* What about `MessageBox.ShowDialogAsync()`?
    - Maybe letter; `TaskDialog` is meant to be the replacement for `MessageBox`.
* What about `CommonFileDialog.ShowDialogAsync()`
    - That's missing and should probably be added.
* Should be marked experimental
    - `WFOXXXX` is a placeholder; use a single diagnostic for all methods

```C#
namespace System.Windows.Forms;

public partial class TaskDialog
{
    [Experimental("WFOXXXX")]
    public static Task<TaskDialogButton> ShowDialogAsync(nint hwndOwner, TaskDialogPage page, TaskDialogStartupLocation startupLocation = TaskDialogStartupLocation.CenterOwner);

    [Experimental("WFOXXXX")]
    public static Task<TaskDialogButton> ShowDialogAsync(IWin32Window win32Window, TaskDialogPage page, TaskDialogStartupLocation startupLocation = TaskDialogStartupLocation.CenterOwner);

    [Experimental("WFOXXXX")]
    public static Task<TaskDialogButton> ShowDialogAsync(TaskDialogPage page, TaskDialogStartupLocation startupLocation = TaskDialogStartupLocation.CenterOwner);
}

public partial class Form
{
    [Experimental("WFOXXXX")]
    public Task ShowAsync(IWin32Window? owner = null);

    [Experimental("WFOXXXX")]
    public Task<DialogResult> ShowDialogAsync();

    [Experimental("WFOXXXX")]
    public Task<DialogResult> ShowDialogAsync(IWin32Window owner);
}
```
