# Quick Reviews 02/11/2021

## Task Dialog: Improve hyperlink usage

**NeedsWork** | [#winforms/1199](https://github.com/dotnet/winforms/issues/1199#issuecomment-777151783) | [Video](https://www.youtube.com/watch?v=FiLPzjvfX2c&t=0h0m0s)

* `LinkId` is a bit weird because ID implies some sort of opaque identifier that the developer have to map to something whereas it's really the value of the `href` attribute. If you want to avoid the web naming conventions, I suggest something like `LinkTarget`.
* It feels a bit heavy handed in the unassisted scenario having to derive from the page. It seems easier if `TaskDialogPage` would offer an event. Creating types is a fairly heavy operation and can't be done inside a method whereas an event can be subscribed to with a local function or lambda expression.
* We should either consistently use "hyperlink" or "link". We'd prefer "link" because "hyperlink" doesn't feel like UI but more like HTML.
* It seems you'll always need the unassisted model (for cases where the number of links isn't statically known). It also seems the result with factory methods wouldn't be easily toolable in the designer. It feels easier to just say the page/dialog has a `LinkClicked` event that provides the link target in the event args and not have `TaskDialogLink` class at all.

```C#
namespace System.Windows.Forms
{
    public class TaskDialogLink
    {
        public event EventHandler Click;
        public string Text { get; }
        public override string ToString();
    }
    public class TaskDialogLinkClickedEventArgs : EventArgs
    {
        public string LinkTarget { get; }
    }
    public partial class TaskDialogPage
    {
       protected bool EnableLinks { get; set; }
       public TaskDialogLink CreateLink(string text);
       protected virtual void OnLinkClicked(TaskDialogLinkClickedEventArgs e);
    }
}
```

