# API Review 07/21/2026

## Change Process.Run() and related methods to accept IEnumerable<string> instead of IList<string>

**Approved** | [#runtime/130364](https://github.com/dotnet/runtime/issues/130364#issuecomment-5036942162)

This increases consistency of the Process class, looks good as proposed.

```diff
namespace System.Diagnostics
{
    public partial class Process
    {
        public static ProcessExitStatus Run(ProcessStartInfo startInfo, TimeSpan? timeout = default);
-       public static ProcessExitStatus Run(string fileName, IList<string>? arguments = null, bool silent = false, TimeSpan? timeout = default);
+       public static ProcessExitStatus Run(string fileName, IEnumerable<string>? arguments = null, bool silent = false, TimeSpan? timeout = default);
        public static ProcessTextOutput RunAndCaptureText(ProcessStartInfo startInfo, TimeSpan? timeout = default);
-       public static ProcessTextOutput RunAndCaptureText(string fileName, IList<string>? arguments = null, TimeSpan? timeout = default);
+       public static ProcessTextOutput RunAndCaptureText(string fileName, IEnumerable<string>? arguments = null, TimeSpan? timeout = default);
        public static Task<ProcessTextOutput> RunAndCaptureTextAsync(ProcessStartInfo startInfo, CancellationToken cancellationToken = default);
-       public static Task<ProcessTextOutput> RunAndCaptureTextAsync(string fileName, IList<string>? arguments = null, CancellationToken cancellationToken = default);
+       public static Task<ProcessTextOutput> RunAndCaptureTextAsync(string fileName, IEnumerable<string>? arguments = null, CancellationToken cancellationToken = default);
        public static Task<ProcessExitStatus> RunAsync(ProcessStartInfo startInfo, CancellationToken cancellationToken = default);
-       public static Task<ProcessExitStatus> RunAsync(string fileName, IList<string>? arguments = null, bool silent = false, CancellationToken cancellationToken = default);
+       public static Task<ProcessExitStatus> RunAsync(string fileName, IEnumerable<string>? arguments = null, bool silent = false, CancellationToken cancellationToken = default);
        public static int StartAndForget(ProcessStartInfo startInfo);
-       public static int StartAndForget(string fileName, IList<string>? arguments = null);
+       public static int StartAndForget(string fileName, IEnumerable<string>? arguments = null);
    }
}
```
## JsonStructuralUnionTypeClassifier for structural union discrimination

**Approved** | [#runtime/129805](https://github.com/dotnet/runtime/issues/129805#issuecomment-5037280968)

We felt that moving "Structural" later in the name added clarity.

There were some concerns voiced at the potential complexity of this feature given where we are in the release cycle, so the advice was to keep it simple in v1 (which happens to be v11).

```csharp
namespace System.Text.Json.Serialization;

public class JsonUnionTypeStructuralClassifier : JsonTypeClassifierFactory
{
    public JsonUnionTypeStructuralClassifier();
}
```
## Unify sync and async options validation contracts

**Approved** | [#runtime/130719](https://github.com/dotnet/runtime/issues/130719#issuecomment-5037334252)

Looks good as proposed.

```diff
 namespace Microsoft.Extensions.Options;

-public interface IAsyncValidateOptions<in TOptions>
+public interface IAsyncValidateOptions<TOptions> : IValidateOptions<TOptions>
     where TOptions : class
 {
     Task<ValidateOptionsResult> ValidateAsync(
         string? name,
         TOptions options,
         CancellationToken cancellationToken = default);
 }

-public interface IAsyncStartupValidator
+public interface IAsyncStartupValidator : IStartupValidator
 {
     Task ValidateAsync(CancellationToken cancellationToken = default);
 }
```
## JsonPolymorphicAttribute.InferClosedTypePolymorphism

**Approved** | [#runtime/130809](https://github.com/dotnet/runtime/issues/130809#issuecomment-5037401450)

Looks good as proposed.

```csharp
namespace System.Text.Json.Serialization;

public sealed partial class JsonPolymorphicAttribute : JsonAttribute
{
    public bool InferClosedTypePolymorphism { get; set; }
}
```
## Add Signal and ProcessExitStatus members to Process

**Approved** | [#runtime/128322](https://github.com/dotnet/runtime/issues/128322#issuecomment-5037476130)

The proposed additions to Process look good as proposed.  We don't feel it worth the hassle to rename the members on SafeProcessHandle.

```csharp
namespace System.Diagnostics
{
    public partial class Process
    {
        public bool Signal(PosixSignal signal);
        public ProcessExitStatus WaitForExitStatus();
        public bool TryWaitForExitStatus(TimeSpan timeout, [NotNullWhen(true)] out ProcessExitStatus? exitStatus);
        public Task<ProcessExitStatus> WaitForExitStatusAsync(CancellationToken cancellationToken = default);
    }
}
```
## Application.SystemVisualSettings — accent color, text scale, and accessibility visual settings

**Approved** | [#winforms/14583](https://github.com/dotnet/winforms/issues/14583#issuecomment-5037851739)

* After discussing what "client area" in ClientAreaAnimationsEnabled meant (it seems to mean "not the menu area"), we kept the prefix in SystemVisualSettings, and added it to SystemVisualSettingsCategories.Animations (now ClientAreaAnimations)
* Otherwise, looks good as proposed.

```csharp
namespace System.Windows.Forms;

public sealed class SystemVisualSettings
{
    public Color AccentColor { get; }
    public float TextScaleFactor { get; }
    public bool HighContrastEnabled { get; }
    public bool ClientAreaAnimationEnabled { get; }
    public bool KeyboardCuesVisible { get; }
    public Size FocusBorderMetrics { get; }
}

[Flags]
public enum SystemVisualSettingsCategories
{
    None = 0,
    AccentColor = 1 << 0,
    TextScale = 1 << 1,
    HighContrast = 1 << 2,
    ClientAreaAnimations = 1 << 3,
    KeyboardCues = 1 << 4,
    FocusMetrics = 1 << 5,
}

public class SystemVisualSettingsChangedEventArgs : EventArgs
{
    public SystemVisualSettings OldSettings { get; }
    public SystemVisualSettings NewSettings { get; }
    public SystemVisualSettingsCategories Changed { get; }
}

public delegate void SystemVisualSettingsChangedEventHandler(object? sender, SystemVisualSettingsChangedEventArgs e);

public sealed partial class Application
{
    public static SystemVisualSettings SystemVisualSettings { get; }
    public static event SystemVisualSettingsChangedEventHandler? SystemVisualSettingsChanged;
}

public partial class Control
{
    public event SystemVisualSettingsChangedEventHandler? SystemVisualSettingsChanged;
    protected virtual void OnSystemVisualSettingsChanged(SystemVisualSettingsChangedEventArgs e);
}
```

## Add APIs for performance improved, flicker-free WinForms UI mutation

**Approved** | [#winforms/14585](https://github.com/dotnet/winforms/issues/14585#issuecomment-5038170292)

* We renamed the LayoutSuspendTraversal members to improve clarity.
* SuspendPaintingScope should be an internal type.  Replaced usage with IDisposable.
* Control explicitly implements ISupportSuspendPainting so that callers only see the IDisposable version.

```csharp
namespace System.Windows.Forms;

public interface ISupportSuspendPainting
{
    void BeginSuspendPainting();
    void EndSuspendPainting();
}

public enum LayoutSuspendTraversal
{
    TargetOnly = 0,
    TargetAndChildren = 1,
    TargetAndDescendants = 2,
}

public static class ControlMutationExtensions
{
    public static IDisposable SuspendPainting(
        this ISupportSuspendPainting target);

    public static IDisposable SuspendPainting(
        this ISupportSuspendPainting target,
        LayoutSuspendTraversal layoutSuspendTraversal);

    public static IDisposable SuspendPainting(
        this ISupportSuspendPainting target,
        Func<Control, bool> suspendLayoutContainerFilter);
}

public partial class Control : ISupportSuspendPainting
{
    void ISupportSuspendPainting.BeginSuspendPainting()
        => BeginSuspendPaintingCore();

    void ISupportSuspendPainting.EndSuspendPainting()
        => EndSuspendPaintingCore();

    protected virtual void BeginSuspendPaintingCore();
    protected virtual void EndSuspendPaintingCore();
}

public enum FormRevealMode
{
    Inherit = -1,
    Classic = 0,
    Deferred = 1,
}

public partial class Form
{
    public virtual FormRevealMode FormRevealMode { get; set; }
}

public partial class Application
{
    public static FormRevealMode DefaultFormRevealMode { get; }
    public static void SetDefaultFormRevealMode(FormRevealMode mode);
    public static bool IsFormRevealDeferred { get; }
}
```

```vb
Namespace Microsoft.VisualBasic.ApplicationServices

    Public Class ApplyApplicationDefaultsEventArgs
        Inherits EventArgs

        Public Property FormRevealMode As FormRevealMode
    End Class
End Namespace
```
