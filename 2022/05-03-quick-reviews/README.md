# API Review 05/03/2022

## Move APIs for generated marshalling to different namespace - System.Runtime.Marshalling

**Approved** | [#runtime/68248](https://github.com/dotnet/runtime/issues/68248#issuecomment-1116409109) | [Video](https://www.youtube.com/watch?v=ggdC8BVTZRQ&t=0h0m0s)

* We'd prefer putting these under `InteropServices`

```C#
namespace System.Runtime.InteropServices
{
    public sealed class LibraryImportAttribute : Attribute { /*...*/ }
    public enum StringMarshalling { /*...*/ }
}

// Was previously also approved for System.Runtime.InteropServices

namespace System.Runtime.InteropServices.Marshalling
{
    public sealed class NativeMarshallingAttribute : Attribute { /*...*/ }
    public sealed class MarshalUsingAttribute : Attribute { /*...*/ }
    public sealed class CustomTypeMarshallerAttribute : Attribute { /*...*/ }

    public enum CustomTypeMarshallerDirection { /*...*/ }
    public enum CustomTypeMarshallerFeatures { /*...*/ }
    public enum CustomTypeMarshallerKind { /*...*/ }

    public unsafe ref struct AnsiStringMarshaller { /*...*/ }
    public unsafe ref struct ArrayMarshaller { /*...*/ }
    public unsafe ref struct PointerArrayMarshaller { /*...*/ }
    public unsafe ref struct Utf16StringMarshaller { /*...*/ }
    public unsafe ref struct Utf8StringMarshaller { /*...*/ }
}
```
## support drag and drop with icon and text label

**Approved** | [#winforms/5884](https://github.com/dotnet/winforms/issues/5884#issuecomment-1116453228) | [Video](https://www.youtube.com/watch?v=ggdC8BVTZRQ&t=0h9m31s)

* `DropImageType`
    - We should align the names of `DropImageType` with Windows terminology, which includes using `Image` over `Icon`
        + `Default` -> `Invalid`
        + `NoDrop` -> `NoImage`
* `DragEventArgs`
    - `DropIcon` to `DropImageType`
    - The `Message*` properties should probably be nullable
* `GiveFeedbackEventArgs`
    - The constructor should match `DoDragDrop` with respect to nullability of bitmap, so we should probably make it non-nullable

```C#
namespace System.Windows.Forms;

public enum DropImageType
{
    Invalid = -1,
    None = 0,
    Copy = DragDropEffects.Copy,
    Move = DragDropEffects.Move,
    Link = DragDropEffects.Link,
    Label = 6,
    Warning = 7,
    NoImage = 8
}

public class DragEventArgs : EventArgs
{
    // Existing:
    // public DragEventArgs(IDataObject? data, int keyState, int x, int y, DragDropEffects allowedEffect, DragDropEffects effect);

    public DragEventArgs(IDataObject? data, int keyState, int x, int y, DragDropEffects allowedEffect, DragDropEffects effect, DropImageType dropIcon, string message, string insert);

    // Existing:
    // public IDataObject? Data { get; }
    // public int KeyState { get; }
    // public int X { get; }
    // public int Y { get; }
    // public DragDropEffects AllowedEffect { get; }
    // public DragDropEffects Effect { get; set; }

    public DropImageType DropImageType { get; set; }
    public string? Message { get; set; }
    public string? MessageReplacementToken { get; set; }
}

public class GiveFeedbackEventArgs : EventArgs
{
    // Existing:
    // public GiveFeedbackEventArgs(DragDropEffects effect, bool useDefaultCursors);

    public GiveFeedbackEventArgs(DragDropEffects effect, bool useDefaultCursors, Bitmap? dragImage, Point cursorOffset, bool useDefaultDragImage);

    // Existing:
    // public DragDropEffects Effect { get; }
    // public bool UseDefaultCursors { get; set; }

    public Bitmap? DragImage { get; set; }
    public Point CursorOffset { get; set; }
    public bool UseDefaultDragImage { get; set; }
}

public partial class Control
{
    // Existing:
    // public DragDropEffects DoDragDrop(object data, DragDropEffects allowedEffects);

    public DragDropEffects DoDragDrop(object data, DragDropEffects allowedEffects, Bitmap dragImage, Point cursorOffset, bool useDefaultDragImage);
}

public partial class ToolStripItem
{
    // Existing:
    // public DragDropEffects DoDragDrop(object data, DragDropEffects allowedEffects);

    public DragDropEffects DoDragDrop(object data, DragDropEffects allowedEffects, Bitmap dragImage, Point cursorOffset, bool useDefaultDragImage);
}
```
## Analyzer/fixer for converting Regex use to source generator

**Approved** | [#runtime/68147](https://github.com/dotnet/runtime/issues/68147#issuecomment-1116464572) | [Video](https://www.youtube.com/watch?v=ggdC8BVTZRQ&t=0h53m49s)

* Looks good as proposed
