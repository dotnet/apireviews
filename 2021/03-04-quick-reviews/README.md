# Quick Reviews 03/04/2021

## Support InitialDirectory and ClientGuid on FolderBrowserDialog

**Approved** | [#winforms/3512](https://github.com/dotnet/winforms/issues/3512#issuecomment-790176927) | [Video](https://www.youtube.com/watch?v=f4L59H56o9I&t=0h0m0s)

* Looks good as proposed.

```C#
namespace System.Windows.Forms
{
    public partial class FolderBrowserDialog 
    {
        [DefaultValue("")]
        public string InitialDirectory { get; set; }

        [Browsable(false)]
        [DesignerSerializationVisibility(DesignerSerializationVisibility.Hidden)]
        [Localizable(false)]
        public Guid? ClientGuid { get; set; }
    }
}
```

## Provide source span in LinkClicked handler of RichTextBox

**Approved** | [#winforms/4431](https://github.com/dotnet/winforms/issues/4431#issuecomment-790180629) | [Video](https://www.youtube.com/watch?v=f4L59H56o9I&t=0h10m34s)

* Looks good as proposed.
    - It's a bit odd to default `LinkStart` to zero, but that would be consistent without, for example, `SelectionStart` and `SelectionLength` work in WinForms

```C#
namespace System.Windows.Forms
{
    public partial class LinkClickedEventArgs : EventArgs
    {
        // Existing
        // public LinkClickedEventArgs(string linkText);
        // public string LinkText { get; }
        public LinkClickedEventArgs(string linkText, int linkStart, int linkLength);
        public int LinkStart { get; }
        public int LinkLength { get; }
    }
}
```

## Add IsAncestorSiteInDesignMode for Control

**Approved** | [#winforms/4146](https://github.com/dotnet/winforms/issues/4146#issuecomment-790189643) | [Video](https://www.youtube.com/watch?v=f4L59H56o9I&t=0h18m11s)

* Makes sense, but it might be better to introduce a property that combines `Component.DesignMode` and `Control.IsAncestorSiteInDesignMode` so that you have a single property that answers "am I being designed or is one of my parents being designed"

```C#
namespace System.Windows.Forms
{
    public partial class Control
    {
        protected bool IsAncestorSiteInDesignMode { get; }
    }
}
```

## Simplify `Invoke` and `BeginInvoke` signature and accept `Action`

**Approved** | [#winforms/4608](https://github.com/dotnet/winforms/issues/4608#issuecomment-790192221) | [Video](https://www.youtube.com/watch?v=f4L59H56o9I&t=0h35m13s)

* Looks good as proposed
  - ~~Is this a source breaking change for VB? @KathleenDollard @jaredpar?~~ Jared already said looks good. But it looks like @KathleenDollard still has come concerns. This should be verified.

```C#
namespace System.Windows.Forms
{
    public partial class Control
    {
        public IAsyncResult BeginInvoke(Action method);
        // public IAsyncResult BeginInvoke(Delegate method);
        // public IAsyncResult BeginInvoke(Delegate method, params object[] args);

        public T Invoke<T>(Func<T> method);
        public void Invoke(Action method);
        // public object Invoke(Delegate method);
        // public object Invoke(Delegate method, params object[] args);
    }
}
```

