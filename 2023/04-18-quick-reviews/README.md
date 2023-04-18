# API Review 04/18/2023

## OpenFolderDialog

**Approved** | [#wpf/7689](https://github.com/dotnet/wpf/issues/7689#issuecomment-1513581597) | [Video](https://www.youtube.com/watch?v=Ba_j0Z-Xbh8&t=0h0m0s)

* Corrected the proposal to show that `FileDialog` should extend the new `CommonItemDialog`
* CommonItemDialog.ForceShowHidden was renamed to ShowHiddenFiles to match WinForms
* Changed CommonItemDialog..ctor from `protected` to `private protected` to indicate the type cannot be derived outside of WPF (due to internal abstract members not shown)
* We discussed the removal of the protected members on FileDialog, and decided it's OK because the type cannot be derived outside the assembly due to (not listed) `internal abstract` members.
* FileDialog..ctor should change in the implementation to be `private protected` or `internal` to keep the hierarchy effectively sealed (until/unless there's a decision to change that in the future).

```diff
  namespace Microsoft.Win32;
  
  public abstract partial class CommonDialog
  {
      protected CommonDialog();
      public object Tag { get; set; }
      protected virtual void CheckPermissionsToShowDialog();
      protected virtual IntPtr HookProc(IntPtr hwnd, int msg, IntPtr wParam, IntPtr lParam);
      public abstract void Reset();
      protected abstract bool RunDialog(IntPtr hwndOwner);
      public virtual bool? ShowDialog();
      public bool? ShowDialog(Window owner);
  }

+ public abstract partial class CommonItemDialog : CommonDialog
+ {
+     private protected CommonItemDialog();
+
+     // lifted up from FileDialog
+     public IList<FileDialogCustomPlace> CustomPlaces { get; set; }
+     public bool DereferenceLinks { get; set; }
+     public string InitialDirectory { get; set; }
+     public string FileName { get; set; }
+     public string[] FileNames { get; }
+     public bool RestoreDirectory { get; set; }
+     public string SafeFileName { get; }
+     public string[] SafeFileNames { get; }
+     public string Title { get; set; }
+     public bool ValidateNames { get; set; }
+     public event CancelEventHandler FileOk { add; remove; }
+     protected void OnFileOk(CancelEventArgs e);
+     protected override bool RunDialog(IntPtr hwndOwner};
+     public override void Reset();
+     public override string ToString();
+
+     // new members
+     public bool AddToRecent { get; set; }        // = true;  (FOS_DONTADDTORECENT)
+     public bool AllowReadOnlyItems { get; set; } // = false; (FOS_NOREADONLYRETURN)
+     public Guid? ClientGuid { get; set; }        //          (IFileDialog::SetClientGuid)
+     public string DefaultDirectory { get; set; } //          (IFileDialog::SetDefaultFolder)
+     public bool ShowHiddenFiles { get; set; }    // = false; (FOS_FORCESHOWHIDDEN )
+     public string RootDirectory { get; set; }    //          (IFileDialog2::SetNavigationRoot)
+ }
  
- // unless noted otherwise, all removed members lifted up to CommonItemDialog
-
- public abstract partial class FileDialog : CommonDialog
+ public abstract partial class FileDialog : CommonItemDialog
  {
-     protected FileDialog();
+     private protected FileDialog();
      public bool AddExtension { get; set; }
      public virtual bool CheckFileExists { get; set; }
      public bool CheckPathExists { get; set; } // WAS VIRTUAL
-     public IList<FileDialogCustomPlace> CustomPlaces { get; set; }
      public string DefaultExt { get; set; }
-     public bool DereferenceLinks { get; set; }
-     public string InitialDirectory { get; set; }
-     public string FileName { get; set; }
-     public string[] FileNames { get; }
      public string Filter { get; set; }
      public int FilterIndex { get; set; }
-     protected int Options { get; } // REMOVED
-     public bool RestoreDirectory { get; set; }
-     public string SafeFileName { get; }
-     public string[] SafeFileNames { get; }
-     public string Title { get; set; }
-     public bool ValidateNames { get; set; }
-     public event CancelEventHandler FileOk { add; remove; }
-     protected override IntPtr HookProc(IntPtr hwnd, int msg, IntPtr wParam, IntPtr lParam); // OVERRIDE REMOVED
-     protected void OnFileOk(CancelEventArgs e);
      public override void Reset();
      protected override bool RunDialog(IntPtr hwndOwner);
      public override string ToString();
  }

  public sealed partial class OpenFileDialog : FileDialog
  {
      public OpenFileDialog();
+     public bool ForcePreviewPane { get; set; } // = false; (FOS_FORCEPREVIEWPANEON)
      public bool Multiselect { get; set; }
      public bool ReadOnlyChecked { get; set; }
      public bool ShowReadOnly { get; set; }
      protected override void CheckPermissionsToShowDialog();
      public System.IO.Stream OpenFile();
      public System.IO.Stream[] OpenFiles();
      public override void Reset();
  }

+ public sealed partial class OpenFolderDialog : CommonItemDialog
+ {
+     public OpenFolderDialog();
+     public bool Multiselect { get; set; }
+     public override void Reset();
+ }

  public sealed partial class SaveFileDialog : FileDialog
  {
      public SaveFileDialog();
+     public bool CreateTestFile { get; set; } // = true; (FOS_NOTESTFILECREATE)
      public bool CreatePrompt { get; set; }
      public bool OverwritePrompt { get; set; }
      public System.IO.Stream OpenFile();
      public override void Reset();
  }
```
## Add GetElapsedTime(long)

**Approved** | [#runtime/84945](https://github.com/dotnet/runtime/issues/84945#issuecomment-1513584177) | [Video](https://www.youtube.com/watch?v=Ba_j0Z-Xbh8&t=0h45m36s)

Looks good as proposed

```C#
namespace System
{
    public partial class TimeProvider
    {
	    /// <summary>
	    /// Gets the elapsed time since the <paramref name="startingTimestamp"/> value retrieved using <see cref="GetTimestamp"/>.
	    /// </summary>
        public TimeSpan GetElapsedTime(long startingTimestamp)
    }
}
```
## Maybe CA1043 should be deleted

**Approved** | [#runtime/79841](https://github.com/dotnet/runtime/issues/79841#issuecomment-1513602441) | [Video](https://www.youtube.com/watch?v=Ba_j0Z-Xbh8&t=0h47m47s)

This analyzer seems to be trying to codify

> AVOID indexers with parameter types other than System.Int32,
System.Int64, System.String, System.Range, System.Index, or an
enum, except on dictionary-like types.

however, in discussion we feel that the guideline is good ("don't abuse syntax"), but that actually making the decision of whether something is an abuse of syntax (or a type is "dictionary-like") is hard to codify... so perhaps the right answer is to delete the analyzer.
