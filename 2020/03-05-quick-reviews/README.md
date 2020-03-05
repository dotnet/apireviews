# Quick Reviews 3/5/2020

## to expose FileDialog.ClientGuid

**Approved** | [#winforms/2858](https://github.com/dotnet/winforms/issues/2858#issuecomment-595474869) | [Video](https://www.youtube.com/watch?v=gwXkFHKPKok&t=0h0m0s)

* Looks good as proposed

```C#
namespace System.Windows.Forms
{
    public partial class FileDialog
    {
        public System.Guid? ClientGuid { get; set; }
    }
}
```
## Add support for tasks to ListViewGroup

**Approved** | [#winforms/2656](https://github.com/dotnet/winforms/issues/2656#issuecomment-595478125) | [Video](https://www.youtube.com/watch?v=gwXkFHKPKok&t=0h10m1s)

* Let's stop creating new delegates for the basic event pattern and just use `EventHandler<T>`
* Is `GroupLink` the best name for the concept? Or is it some kind of index? If so, the name should reflect that.

```C#
namespace System.Windows.Forms
{
    public class GroupLinkClickEventArgs : EventArgs
    {
        public GroupLinkClickEventArgs(int groupLink);
        public int GroupLink { get; }
    }
    public partial class ListView
    {
        public event EventHandler<GroupLinkClickEventArgs> GroupLinkClick;
        protected void OnGroupLinkClick(GroupEventArgs e);
    }
    public partial class ListViewGroup
    {
        public string Task { get; set; }
    }
}
```
## Add support for subtitles in ListViewGroup

**Approved** | [#winforms/2655](https://github.com/dotnet/winforms/issues/2655#issuecomment-595479361) | [Video](https://www.youtube.com/watch?v=gwXkFHKPKok&t=0h19m13s)

* Looks good as proposed

```C#
namespace System.Windows.Forms
{
    public partial class ListViewGroup
    {
        public string Subtitle { get; set; }
    }
}
```
## Add support for footers to ListViewGroup

**Approved** | [#winforms/2653](https://github.com/dotnet/winforms/issues/2653#issuecomment-595480175) | [Video](https://www.youtube.com/watch?v=gwXkFHKPKok&t=0h22m40s)

* Looks good as proposed

```C#
namespace System.Windows.Forms
{
    public partial class ListViewGroup
    {
        public string Footer { get; set; }
        public HorizontalAlignment FooterAlignment { get; set; }
    }
}
```
## Discussion/proposal: add more LVCOLUMN ComCtl 6.0 features

**Approved** | [#winforms/2627](https://github.com/dotnet/winforms/issues/2627#issuecomment-595483352) | [Video](https://www.youtube.com/watch?v=gwXkFHKPKok&t=0h24m31s)

* Looks good as proposed
* We think the individual booleans seems easier than the flags, given that it's a single use

```C#
namespace System.Windows.Forms
{
    public partial class ColumnHeader
    {
        public int MinimumWidth { get; set; }
        public bool FixedWidth { get; set; }
        public bool SplitButton { get; set; }
    }
    public class ColumnDropDownClickEventArgs : ColumnClickEventArgs
    {
        public ColumnDropDownClickEventArgs(int column, Point screenLocation);
        public Point ScreenLocation { get; set; }
    }
    public partial class ListView
    {
        public event EventHandler<ColumnDropDownClickEventArgs> ColumnDropDownClicked;
        protected void OnColumnDropDownClicked(ColumnDropDownClickEventArgs e);
    }
}
```
## add SelectionStart and SelectionEnd to TrackBar

**Approved** | [#winforms/2642](https://github.com/dotnet/winforms/issues/2642#issuecomment-595486672) | [Video](https://www.youtube.com/watch?v=gwXkFHKPKok&t=0h34m12s)

* We should probably follow `TextBoxBase` and use `SelectionStart` and `SelectionLength`, because that avoids ordering issues and the need for sentinel values. @RussKie, it would be good to check what the throw behavior is for `TextBoxBase`.
* We considered making it method like `SetSelection(int start, int length)` but that would mean you can't set the selection using the designer, which feels odd

```C#
namespace System.Windows.Forms
{
    public partial class TrackBar
    {
        public bool ShowSelectionRange { get; set; }
        public int SelectionStart { get; set; }
        public int SelectionLength { get; set; }
    }
}
```

@hughbe What the other properties you show in your UI? Like `ShowThumb` and `ThumbWidth`?
## Add Bold Property to TreeNode

**Needs Work** | [#winforms/2788](https://github.com/dotnet/winforms/issues/2788#issuecomment-595496212) | [Video](https://www.youtube.com/watch?v=gwXkFHKPKok&t=0h45m45s)

* We prefer `IsBold` is nicer than `Bold`
* Assuming `IsBold` is independent of the `NodeFont` we like it.
* If there is a weird relationship between `IsBold` and `NodeFont`, e.g. when setting `IsBold` modifies the `NodeFont`, then we don't like it because it creates referential integrity issues where setting a boolean in one place suddenly causes the node to use a different font when the font was meant to be shared across multiple nodes. We should investigate that before we approve the API.

```C#
namespace System.Windows.Forms
{
    public partial class TreeNode
    {
        public bool IsBold { get; set; }
    }
}
```

