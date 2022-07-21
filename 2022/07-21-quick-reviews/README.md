# API Review 07/21/2022

## System.Diagnostics.CodeAnalysis.UnscopedRefAttribute

**Approved** | [#runtime/72074](https://github.com/dotnet/runtime/issues/72074#issuecomment-1191764092) | [Video](https://www.youtube.com/watch?v=E5djVCbZxo4&t=0h0m0s)

* We'd like to align the names between the synthesized attribute, the language keyword, and this new attribute
    - Could we go for `[ScopedRef]` and `[UnscopedRef]`?
* We don't believe we should have `AttributeTargets.Struct`
    - This should be rare, so consumers should be selective anyway

```C#
namespace System.Diagnostics.CodeAnalysis;

[AttributeUsage(AttributeTargets.Method |
                AttributeTargets.Property |
                AttributeTargets.Parameter,
                AllowMultiple = false,
                Inherited = false)]
public sealed class UnscopedRefAttribute : Attribute
{
    public UnscopedRefAttribute();
}
```


## Reconcile naming: RegexGeneratorAttribute to RegexAttribute

**Approved** | [#runtime/62123](https://github.com/dotnet/runtime/issues/62123#issuecomment-1191789133) | [Video](https://www.youtube.com/watch?v=E5djVCbZxo4&t=0h38m29s)

* We'd prefer `GeneratedRegexAttribute`

```diff
 namespace System.Text.RegularExpressions;

 [AttributeUsage(AttributeTargets.Method, AllowMultiple = false, Inherited = false)]
-public partial class RegexGeneratorAttribute : Attribute
+public partial class GeneratedRegexAttribute : Attribute
 {
     // ....        
 }
```

## Introduce Commands and DataContext to WinForms to adapt modern Binding scenarios

**Approved** | [#winforms/4895](https://github.com/dotnet/winforms/issues/4895#issuecomment-1191833180) | [Video](https://www.youtube.com/watch?v=E5djVCbZxo4&t=1h6m58s)

* We'd prefer not having the classes `CommandComponent` and `CommandControl`
    - Instead just follow the WinForms model of having a naming convention
* We still need `BindableComponent` because `Component` is lower than WinForms (where `IBindableComponent` lives).
* We'd like the designer to mark the generated `InitializeComponent` with `[RequiresPreviewFeatures(Url = "<placeholder>")]` when these are used

```C#
namespace System.Windows.Forms;

[RequiresPreviewFeatures(Url = "<placeholder>")]
public abstract class BindableComponent : Component, IBindableComponent
{
    public event EventHandler? BindingContextChanged;
    public ControlBindingsCollection? DataBindings { get; }
    public BindingContext? BindingContext { get; set; }
    protected virtual void OnBindingContextChanged(EventArgs e);
}

// Suppress warning
public abstract class Control : BindableComponent // Used to be Component
{
}

public partial class ButtonBase
{
    [RequiresPreviewFeatures(Url = "<placeholder>")]
    public event EventHandler? CommandCanExecuteChanged;
    [RequiresPreviewFeatures(Url = "<placeholder>")]
    public event EventHandler? CommandChanged;
    [RequiresPreviewFeatures(Url = "<placeholder>")]
    public event EventHandler? CommandParameterChanged;

    [RequiresPreviewFeatures(Url = "<placeholder>")]
    public ICommand? Command { get; set; }
    [RequiresPreviewFeatures(Url = "<placeholder>")]
    public object? CommandParameter { get; set; }

    [RequiresPreviewFeatures(Url = "<placeholder>")]
    protected virtual void OnCommandChanged(EventArgs e);
    [RequiresPreviewFeatures(Url = "<placeholder>")]
    protected virtual void OnCommandCanExecuteChanged(EventArgs e);
    [RequiresPreviewFeatures(Url = "<placeholder>")]
    protected virtual void OnCommandParameterChanged(EventArgs e);
    [RequiresPreviewFeatures(Url = "<placeholder>")]
    protected virtual void OnRequestCommandExecute(EventArgs e);
}

public abstract partial class ToolStripItem : BindableComponent // Used to be Component
{
    [RequiresPreviewFeatures(Url = "<placeholder>")]
    public event EventHandler? CommandCanExecuteChanged;
    [RequiresPreviewFeatures(Url = "<placeholder>")]
    public event EventHandler? CommandChanged;
    [RequiresPreviewFeatures(Url = "<placeholder>")]
    public event EventHandler? CommandParameterChanged;

    [RequiresPreviewFeatures(Url = "<placeholder>")]
    public ICommand? Command { get; set; }
    [RequiresPreviewFeatures(Url = "<placeholder>")]
    public object? CommandParameter { get; set; }

    [RequiresPreviewFeatures(Url = "<placeholder>")]
    protected virtual void OnCommandChanged(EventArgs e);
    [RequiresPreviewFeatures(Url = "<placeholder>")]
    protected virtual void OnCommandCanExecuteChanged(EventArgs e);
    [RequiresPreviewFeatures(Url = "<placeholder>")]
    protected virtual void OnCommandParameterChanged(EventArgs e);
    [RequiresPreviewFeatures(Url = "<placeholder>")]
    protected virtual void OnRequestCommandExecute(EventArgs e);
}
```

