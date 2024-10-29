# API Review 10/29/2024

## New Clipboard and DataObject APIs

**Approved** | [#winforms/12362](https://github.com/dotnet/winforms/issues/12362#issuecomment-2445132495)

* There was discussion around a lot of the JSON-ness, in the end it was felt that the proposed API is sufficient for the initial version.
* There was discussion around whether WinForms' Clipboard.GetData and WPF's Clipboard.GetData should use the same diagnostic ID, or two different diagnostic IDs.  It was decided that using one common ID was better, even if it uses a WinForms prefix in a WPF app.
* The GetData methods should be marked Obsolete, but not EditorBrowsable.Never
* This space feels like it has to handle downlevel interop, but that feels lacking.
  * DIMs on IDataObject can't be utilized on .NET Framework (no DIMs, and can't replace the framework type)
  * .NET Framework extracting data written via SetData
  * So rather than adding DIMs on IDataObject, we invented a new interface, ITypedDataObject
* The GetData methods on IDataObject should not be marked as Obsolete in this version
* One AppContext switch seems good, the name we came to by committee is `Windows.ClipboardDragDrop.EnableUnsafeBinaryFormatterSerialization`

```diff
namespace System.Windows.Forms;
and 
namespace System.Windows;

public static partial class Clipboard
{
+   public static void SetDataAsJson<T>(string format, T data) { }

+   [Obsolete("`Clipboard.GetData(string)` method is obsolete. Use `Clipboard.TryGetData<T>` instead.", false, DiagnosticId = "WFDEV005", UrlFormat = "https://aka.ms/winforms-warnings/{0}")
    public static object? GetData(string format) { }
    
+   public static bool TryGetData<T>(string format, out T data) { }
+   public static bool TryGetData<T>(string format, Func<Reflection.Metadata.TypeName, Type> resolver, out T data) { }
}

public partial class DataObject : IDataObject, Runtime.InteropServices.ComTypes.IDataObject
+                                 ITypedDataObject
{
+   public void SetDataAsJson<T>(T data) { }
+   public void SetDataAsJson<T>(string format, T data) { }
+   public void SetDataAsJson<T>(string format, bool autoConvert, T data) { }

+   [Obsolete("`DataObject.GetData` methods are obsolete. Use the corresponding `DataObject.TryGetData<T>` instead.", false, DiagnosticId = "WFDEV005", UrlFormat = "https://aka.ms/winforms-warnings/{0}")]
    public virtual object? GetData(string format, bool autoConvert) { }

+   [Obsolete("`DataObject.GetData` methods are obsolete. Use the corresponding `DataObject.TryGetData<T>` instead.", false, DiagnosticId = "WFDEV005", UrlFormat = "https://aka.ms/winforms-warnings/{0}")]
    public virtual object? GetData(string format) { }

+   [Obsolete("`DataObject.GetData` methods are obsolete. Use the corresponding `DataObject.TryGetData<T>` instead.", false, DiagnosticId = "WFDEV005", UrlFormat = "https://aka.ms/winforms-warnings/{0}")]
    public virtual object? GetData(Type format) { }

+   public bool TryGetData<T>(out T data) { }
+   public bool TryGetData<T>(string format, out T data) { }
+   public bool TryGetData<T>(string format, bool autoConvert, out T data) { }
+   public bool TryGetData<T>(string format, Func<Reflection.Metadata.TypeName, Type> resolver, bool autoConvert, out T data) { }
+   protected virtual bool TryGetDataCore<T>(string format, Func<Reflection.Metadata.TypeName, Type> resolver, bool autoConvert, out T data) { }
}

+public interface ITypedDataObject
+{
+   bool TryGetData<T>(out T data);
+   bool TryGetData<T>(string format, out T data);
+   bool TryGetData<T>(string format, bool autoConvert, out T data);
+   bool TryGetData<T>(string format, Func<Reflection.Metadata.TypeName, Type> resolver, bool autoConvert, out T data);
+}

+public sealed class DataObjectExtensions
{
+   public static bool TryGetData<T>(this IDataObject dataObject, out T data);
+   public static bool TryGetData<T>(this IDataObject dataObject, string format, out T data);
+   public static bool TryGetData<T>(this IDataObject dataObject, string format, bool autoConvert, out T data);
+   public static bool TryGetData<T>(this IDataObject dataObject, string format, Func<Reflection.Metadata.TypeName, Type> resolver, bool autoConvert, out T data);
}
```

```diff
namespace System.Windows.Forms;
public class partial class Control
{
+   public DragDropEffects DoDragDropAsJson<T>(T data, DragDropEffects allowedEffects, Bitmap? dragImage, Point cursorOffset, bool useDefaultDragImage)
+   public DragDropEffects DoDragDropAsJson<T>(T data, DragDropEffects allowedEffects)
}
```

```diff
namespace System.Windows;
public static class DragDrop
{
+    public static DragDropEffects DoDragDropAsJson<T>(DependencyObject dragSource, T data, DragDropEffects allowedEffects)
}
```

```diff
// VisualBasic wrapper for WinForms Clipboard.
namespace Microsoft.VisualBasic.MyServices
public partial class ClipboardProxy
{
+   public void SetDataAsJson<T>(T data) { }
+   public void SetDataAsJson<T>(string format, T data) { }

+   [Obsolete("`ClipboardProxy.GetData(As String)` method is obsolete. Use `ClipboardProxy.TryGetData(Of T)` instead.", false, DiagnosticId = "WFDEV005", UrlFormat = "https://aka.ms/winforms-warnings/{0}")
    public object GetData(string format) { } 

+   public bool TryGetData<T>(string format, out T data) { }
+   public bool TryGetData<T>(string format, System.Func<System.Reflection.Metadata.TypeName, System.Type> resolver, out T data) { }
}
```
