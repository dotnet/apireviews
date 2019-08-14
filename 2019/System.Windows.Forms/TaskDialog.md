# Task Dialog APIs

```C#
namespace System.Windows.Forms {
    public class TaskDialog : IWin32Window {
        public TaskDialog();
        public TaskDialog(TaskDialogPage page);
        public IntPtr Handle { get; }
        public TaskDialogPage Page { get; set; }
        public bool SetToForeground { get; set; }
        public TaskDialogStartupLocation StartupLocation { get; set; }
        public event EventHandler Closed;
        public event EventHandler<TaskDialogClosingEventArgs> Closing;
        public event EventHandler Opened;
        public event EventHandler Shown;
        public void Close();
        protected void OnClosed(EventArgs e);
        protected void OnClosing(TaskDialogClosingEventArgs e);
        protected void OnOpened(EventArgs e);
        protected void OnShown(EventArgs e);
        public TaskDialogButton Show();
        public TaskDialogButton Show(IntPtr hwndOwner);
        public static TaskDialogResult Show(IntPtr hwndOwner, string text, string instruction = null, string title = null, TaskDialogButtons buttons = TaskDialogButtons.OK, TaskDialogStandardIcon icon = TaskDialogStandardIcon.None);
        public static TaskDialogResult Show(string text, string instruction = null, string title = null, TaskDialogButtons buttons = TaskDialogButtons.OK, TaskDialogStandardIcon icon = TaskDialogStandardIcon.None);
        public TaskDialogButton Show(IWin32Window owner);
        public static TaskDialogResult Show(IWin32Window owner, string text, string instruction = null, string title = null, TaskDialogButtons buttons = TaskDialogButtons.OK, TaskDialogStandardIcon icon = TaskDialogStandardIcon.None);
    }
    public abstract class TaskDialogButton : TaskDialogControl {
        public bool DefaultButton { get; set; }
        public bool ElevationRequired { get; set; }
        public bool Enabled { get; set; }
        public event EventHandler<TaskDialogButtonClickedEventArgs> Click;
        public void PerformClick();
    }
    public class TaskDialogButtonClickedEventArgs : EventArgs {
        public bool CancelClose { get; set; }
    }
    public enum TaskDialogButtons {
        Abort = 65536,
        Cancel = 8,
        Close = 32,
        Continue = 524288,
        Help = 1048576,
        Ignore = 131072,
        No = 4,
        None = 0,
        OK = 1,
        Retry = 16,
        TryAgain = 262144,
        Yes = 2,
    }
    public sealed class TaskDialogCheckBox : TaskDialogControl {
        public TaskDialogCheckBox();
        public TaskDialogCheckBox(string text);
        public bool Checked { get; set; }
        public string Text { get; set; }
        public event EventHandler CheckedChanged;
        public void Focus();
        public override string ToString();
    }
    public class TaskDialogClosingEventArgs : CancelEventArgs {
        public TaskDialogButton CloseButton { get; }
    }
    public abstract class TaskDialogControl {
        public object Tag { get; set; }
    }
    public sealed class TaskDialogCustomButton : TaskDialogButton {
        public TaskDialogCustomButton();
        public TaskDialogCustomButton(string text, string descriptionText = null);
        public string DescriptionText { get; set; }
        public string Text { get; set; }
        public override string ToString();
    }
    public class TaskDialogCustomButtonCollection : Collection<TaskDialogCustomButton> {
        public TaskDialogCustomButtonCollection();
        public TaskDialogCustomButton Add(string text, string descriptionText = null);
        protected override void ClearItems();
        protected override void InsertItem(int index, TaskDialogCustomButton item);
        protected override void RemoveItem(int index);
        protected override void SetItem(int index, TaskDialogCustomButton item);
    }
    public enum TaskDialogCustomButtonStyle {
        CommandLinks = 1,
        CommandLinksNoIcon = 2,
        Default = 0,
    }
    public sealed class TaskDialogExpander : TaskDialogControl {
        public TaskDialogExpander();
        public TaskDialogExpander(string text);
        public string CollapsedButtonText { get; set; }
        public bool Expanded { get; set; }
        public string ExpandedButtonText { get; set; }
        public bool ExpandFooterArea { get; set; }
        public string Text { get; set; }
        public event EventHandler ExpandedChanged;
        public override string ToString();
    }
    public sealed class TaskDialogFooter : TaskDialogControl {
        public TaskDialogFooter();
        public TaskDialogFooter(string text);
        public TaskDialogIcon Icon { get; set; }
        public string Text { get; set; }
        public override string ToString();
    }
    public class TaskDialogHyperlinkClickedEventArgs : EventArgs {
        public string Hyperlink { get; }
    }
    public abstract class TaskDialogIcon {
        public static TaskDialogIcon Get(TaskDialogStandardIcon icon);
        public static implicit operator TaskDialogIcon (Icon icon);
        public static implicit operator TaskDialogIcon (TaskDialogStandardIcon icon);
    }
    public class TaskDialogIconHandle : TaskDialogIcon {
        public TaskDialogIconHandle(Icon icon);
        public TaskDialogIconHandle(IntPtr iconHandle);
        public IntPtr IconHandle { get; }
    }
    public class TaskDialogPage {
        public TaskDialogPage();
        public bool AllowCancel { get; set; }
        public bool CanBeMinimized { get; set; }
        public TaskDialogCheckBox CheckBox { get; set; }
        public TaskDialogCustomButtonCollection CustomButtons { get; set; }
        public TaskDialogCustomButtonStyle CustomButtonStyle { get; set; }
        public bool EnableHyperlinks { get; set; }
        public TaskDialogExpander Expander { get; set; }
        public TaskDialogFooter Footer { get; set; }
        public TaskDialogIcon Icon { get; set; }
        public string Instruction { get; set; }
        public TaskDialogProgressBar ProgressBar { get; set; }
        public TaskDialogRadioButtonCollection RadioButtons { get; set; }
        public bool RightToLeftLayout { get; set; }
        public bool SizeToContent { get; set; }
        public TaskDialogStandardButtonCollection StandardButtons { get; set; }
        public string Text { get; set; }
        public string Title { get; set; }
        public int Width { get; set; }
        public event EventHandler Created;
        public event EventHandler Destroyed;
        public event EventHandler HelpRequest;
        public event EventHandler<TaskDialogHyperlinkClickedEventArgs> HyperlinkClicked;
        protected internal void OnCreated(EventArgs e);
        protected internal void OnDestroyed(EventArgs e);
        protected internal void OnHelpRequest(EventArgs e);
        protected internal void OnHyperlinkClicked(TaskDialogHyperlinkClickedEventArgs e);
    }
    public sealed class TaskDialogProgressBar : TaskDialogControl {
        public TaskDialogProgressBar();
        public TaskDialogProgressBar(TaskDialogProgressBarState state);
        public int MarqueeSpeed { get; set; }
        public int Maximum { get; set; }
        public int Minimum { get; set; }
        public TaskDialogProgressBarState State { get; set; }
        public int Value { get; set; }
    }
    public enum TaskDialogProgressBarState {
        Error = 2,
        Marquee = 3,
        MarqueePaused = 4,
        None = 5,
        Normal = 0,
        Paused = 1,
    }
    public sealed class TaskDialogRadioButton : TaskDialogControl {
        public TaskDialogRadioButton();
        public TaskDialogRadioButton(string text);
        public bool Checked { get; set; }
        public bool Enabled { get; set; }
        public string Text { get; set; }
        public event EventHandler CheckedChanged;
        public override string ToString();
    }
    public class TaskDialogRadioButtonCollection : Collection<TaskDialogRadioButton> {
        public TaskDialogRadioButtonCollection();
        public TaskDialogRadioButton Add(string text);
        protected override void ClearItems();
        protected override void InsertItem(int index, TaskDialogRadioButton item);
        protected override void RemoveItem(int index);
        protected override void SetItem(int index, TaskDialogRadioButton item);
    }
    public enum TaskDialogResult {
        Abort = 3,
        Cancel = 2,
        Close = 8,
        Continue = 11,
        Help = 9,
        Ignore = 5,
        No = 7,
        None = 0,
        OK = 1,
        Retry = 4,
        TryAgain = 10,
        Yes = 6,
    }
    public sealed class TaskDialogStandardButton : TaskDialogButton {
        public TaskDialogStandardButton();
        public TaskDialogStandardButton(TaskDialogResult result);
        public TaskDialogResult Result { get; set; }
        public bool Visible { get; set; }
        public override string ToString();
    }
    public class TaskDialogStandardButtonCollection : KeyedCollection<TaskDialogResult, TaskDialogStandardButton> {
        public TaskDialogStandardButtonCollection();
        public TaskDialogStandardButton Add(TaskDialogResult result);
        protected override void ClearItems();
        protected override TaskDialogResult GetKeyForItem(TaskDialogStandardButton item);
        protected override void InsertItem(int index, TaskDialogStandardButton item);
        public static implicit operator TaskDialogStandardButtonCollection (TaskDialogButtons buttons);
        protected override void RemoveItem(int index);
        protected override void SetItem(int index, TaskDialogStandardButton item);
    }
    public enum TaskDialogStandardIcon {
        Error = 65534,
        Information = 65533,
        None = 0,
        SecurityErrorRedBar = 65529,
        SecurityShield = 65532,
        SecurityShieldBlueBar = 65531,
        SecurityShieldGrayBar = 65527,
        SecuritySuccessGreenBar = 65528,
        SecurityWarningYellowBar = 65530,
        Warning = 65535,
    }
    public enum TaskDialogStartupLocation {
        CenterParent = 1,
        CenterScreen = 0,
    }
}
```