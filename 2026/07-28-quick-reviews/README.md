# API Review 07/28/2026

## Built-in System.Text.Json converters for BFloat16 and Decimal32/64/128

**Approved** | [#runtime/131097](https://github.com/dotnet/runtime/issues/131097#issuecomment-5107324738) | [Video](https://www.youtube.com/watch?v=D_Eoay_ZM10&t=0h0m0s)

Looks good as proposed.

```csharp
namespace System.Text.Json.Serialization.Metadata;

public static partial class JsonMetadataServices
{
    public static JsonConverter<System.Numerics.BFloat16> BFloat16Converter { get; }
    public static JsonConverter<System.Numerics.Decimal32> Decimal32Converter { get; }
    public static JsonConverter<System.Numerics.Decimal64> Decimal64Converter { get; }
    public static JsonConverter<System.Numerics.Decimal128> Decimal128Converter { get; }
}
```
## `TreeView.NodeLeading` - opt-in honest, text-scale-aware node-row height

**NeedsWork** | [#winforms/14584](https://github.com/dotnet/winforms/issues/14584#issuecomment-5107633016) | [Video](https://www.youtube.com/watch?v=D_Eoay_ZM10&t=0h5m3s)

We're not sold on "Leading", since it gets misread as... "leading", and people might think it only applies as top-padding.  And the word "Scale" or "Factor" should be in somewhere.

It's also not clear how this interacts with the ItemHeight property.

More data is required to proceed.

```csharp
namespace System.Windows.Forms;

public partial class TreeView
{
    /// <summary>
    ///  Gets or sets the per-node vertical leading factor applied on top of
    ///  the live <see cref="Control.Font"/> height. Acts as a sentinel
    ///  switch as well as a scaling factor &#x2014; see <c>remarks</c>.
    /// </summary>
    public float NodeLeading { get; set; }

    /// <summary>
    ///  Occurs when the value of <see cref="NodeLeading"/> changes.
    /// </summary>
    public event EventHandler? NodeLeadingChanged;

    /// <summary>
    ///  Raises the <see cref="NodeLeadingChanged"/> event.
    /// </summary>
    protected virtual void OnNodeLeadingChanged(EventArgs e);
}
```
## Introduce KioskModeManager component

**Approved** | [#winforms/14586](https://github.com/dotnet/winforms/issues/14586#issuecomment-5108482863) | [Video](https://www.youtube.com/watch?v=D_Eoay_ZM10&t=0h35m19s)

* We discussed MousePointerAutoHideDelay (name the units, or use TimeSpan).  Since it's WinForms and everything else already uses implicit milliseconds (because Win32), we left it as-is.
* We removed HideTaskbar to simplify the window models.
* We're not in favor of the full screen key being a global keyhook, it should be normal focus-driven keys.
* ToggleFullScreenKey => ToggleFullScreenKeys (consistency with the plurality of the type)
* SuppressPowerSaving => AlwaysOn
* The "Wakeup" elements felt more general than "KioskMode", so we removed them from here (they can reappear for a more broad scope, just not part of KiosModeManager)

```csharp
namespace System.Windows.Forms;

using System.Windows.Input;

public class KioskModeManager : Component, ISupportInitialize
{
    public KioskModeManager();

    // Designer Infrastructure
    public KioskModeManager(IContainer container);

    // Designer Infrastructure
    public override ISite? Site { get; set; }

    // Designer Infrastructure
    void ISupportInitialize.BeginInit();
    void ISupportInitialize.EndInit();

    // Infra: On placing it in the Component Tray, it gets assigned by the Designer.
    // The Site/Form at Design time, the Form to control at runtime.
    public ContainerControl? ContainerControl { get; set; }

    public bool EscapeExitsFullScreen { get; set; }

    [Bindable(true)]
    public bool FullScreen { get; set; }

    public int MousePointerAutoHideDelay { get; set; }
    public bool AlwaysOn { get; set; }
    public Keys ToggleFullScreenKeys { get; set; }
    public bool TopMostInFullScreen { get; set; }

    public event EventHandler? ContainerControlChanged;
    public event EventHandler? FullScreenChanged;

    public void ToggleFullScreen();

    protected virtual void OnContainerControlChanged(EventArgs e);
    protected virtual void OnFullScreenChanged(EventArgs e);
}
```
## Add Reset to the Zlib-based Encoders/Decoders

**Approved** | [#runtime/130465](https://github.com/dotnet/runtime/issues/130465#issuecomment-5107368569) | [Video](https://www.youtube.com/watch?v=D_Eoay_ZM10&t=1h54m44s)

Looks good as proposed

```csharp
namespace System.IO.Compression
{
    public sealed partial class DeflateDecoder : System.IDisposable
    {
        public void Reset();
    }

    public sealed partial class ZLibDecoder : System.IDisposable
    {
        public void Reset();
    }

    public sealed partial class GZipDecoder : System.IDisposable
    {
        public void Reset();
    }

    public sealed partial class DeflateEncoder : System.IDisposable
    {
        public void Reset();
    }

    public sealed partial class ZLibEncoder : System.IDisposable
    {
        public void Reset();
    }

    public sealed partial class GZipEncoder : System.IDisposable
    {
        public void Reset();
    }
}
```
