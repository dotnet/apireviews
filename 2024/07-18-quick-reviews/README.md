# API Review 07/18/2024

## FluentWindows Theme Switch in WPF

**Approved** | [#wpf/8932](https://github.com/dotnet/wpf/issues/8932#issuecomment-2237161073) | [Video](https://www.youtube.com/watch?v=5Dqp1704JVA&t=0h0m0s)


* Instead of loose strings, it should use strongly-typed strings.
* After discussion of what the property means, and what the OS calls it, we think "ThemeName" is the best name.

```C#
namespace System.Windows
{
    public class Application
    {
        [Experimental]
        public ThemeMode ThemeMode { get; set; }
    }

    public class Window
    {
        [Experimental]
        public ThemeMode ThemeMode { get; set; }
    }

    [Experimental]
    public readonly struct ThemeMode : IEquatable<ThemeMode>
    {
        public static ThemeMode None => new ThemeMode();
        public static ThemeMode System => new ThemeMode("System");
        public static ThemeMode Light => new ThemeMode("Light");
        public static ThemeMode Dark => new ThemeMode("Dark");

        public string Value => _value ?? "None";

        public ThemeMode(string value) => _value = value;

        // + IEquatable members
    }
}
```

## Introduce Dark Mode and A11Y compatible Visual Styles for Apps targeting .NET 9 and Win 11

**NeedsWork** | [#winforms/7641](https://github.com/dotnet/winforms/issues/7641#issuecomment-2237384450) | [Video](https://www.youtube.com/watch?v=5Dqp1704JVA&t=0h44m1s)


* DarkMode.Inherits => DarkMode.Inherited (to match the tense of the other members)
* "Why is DefaultDarkMode a get-only property with a separate named setter?", answer: consistency with existing Winforms patterns.
* We removed DarkMode.NotSupported, and renumbered Disabled to be zero.
* EnvironmentDarkMode changed to a boolean, as Inherited doesn't make sense there; then renamed to IsSystemDarkModeEnabled
* SetDefaultDarkMode changed from bool-returning to void.
* Control.IsDarkModeEnabled is no longer virtual
* Control.SetDarkModeCore was replaced with the event pattern, for consistency.
* Control.DarkModeSupported was removed.
* ApplicationColors is removed.  SystemColors needs to be enhanced to understand dark vs light mode for the app.
  * Adding System.Drawing.SystemColors.DarkMode(get/set) to empower that.
* Application.IsVisualStylesSupported is removed
* We ran out of time while discussing VisualStylesMode

```diff
namespace System.Windows.Forms;

    public class TextBoxBase : Control
    {
-        public new Padding Padding {get; set; }
    }
```

```C#

// ***************************************
// Dark Mode
// ***************************************

namespace System.Drawing
{
    partial class SystemColors
    {
        public static bool DarkMode { get; set; }
    }
}

namespace System.Windows.Forms
{
    [Experimental("WFO9000")]
    public enum DarkMode
    {
        Disabled = 0,
        Enabled = 1,
        Inherited = 2,
    }

    public static partial class Application
    {
        [Experimental("WFO9000")]
        public static DarkMode DefaultDarkMode { get; }

        [Experimental("WFO9000")]
        public static void SetDefaultDarkMode(DarkMode darkMode);

        [Experimental("WFO9000")]
        public static bool IsSystemDarkModeEnabled { get; }

        [Experimental("WFO9000")]
        public static bool IsDarkModeEnabled { get; }
    }

    public unsafe partial class Control
    {
        [Experimental("WFO9000")]
        public DarkMode DarkMode { get; set; }

        [Experimental("WFO9000")]
        public event EventHandler? DarkModeChanged;

        [Experimental("WFO9000")]
        protected virtual void OnDarkModeChanged(EventArgs e);

        [Experimental("WFO9000")]
        public bool IsDarkModeEnabled { get; }
    }

   // ***************************************
   // Windows Title Bar Control (based on new official public Win32 APIs)
   // ***************************************

    public unsafe partial class Form
    {
        [Experimental("WFO9000")]
        public Color FormBorderColor { get; set; }

        [Experimental("WFO9000")]
        public Color FormCaptionColor { get; set; }

        [Experimental("WFO9000")]
        public Color FormCaptionTextColor { get; set; }

        [Experimental("WFO9000")]
        public FormCornerPreference FormCornerPreference { get; set; }

        // add events appropriate to these properties
    }

    [Experimental("WFO9000")]
    public enum FormCornerPreference
    {
        Default = 0,
        DoNotRound = 1,
        Round = 2,
        RoundSmall = 3
    }
}
```


Discussed but not approved:

```C#

    // ***************************************
    // Visual Styles Versioning
    // ***************************************

    [Experimental("WFO9000")]
    public enum VisualStylesMode
    {
        Disabled = 0,
        Classic = 1,
        Latest = 2
    }

    public static partial class Application
    {
        [Experimental("WFO9000")]
        public static VisualStylesMode DefaultVisualStylesMode { get; }

        public static void EnableVisualStyles() => EnableVisualStyles(VisualStylesMode.Classic);

        [Experimental("WFO9000")]
        public static void EnableVisualStyles(VisualStylesMode visualStylesMode);
    }

    public unsafe partial class Control 
    {
        [Experimental("WFO9000")]
        public VisualStylesMode VisualStylesMode { get; set; }

        [Experimental("WFO9000")]
        protected virtual VisualStylesMode DefaultVisualStylesMode { get; }

        [Experimental("WFO9000")]
        protected virtual bool ValidateVisualStylesMode(VisualStylesMode visualStylesMode)
    }

    /// To address checkbox A11Y min size requirements.
    public enum Appearance
    {
        Normal = 0,
        Button = 1,

        /// <summary>
        ///  The appearance of a Modern UI toggle switch.
        ///  This setting is not taken into account, when <see cref="VisualStylesMode"/> is set
        ///  to <see cref="VisualStylesMode.Disabled"/> or <see cref="VisualStylesMode.Classic"/>.
        /// </summary>
        [Experimental("WFO9000")]
        ToggleSwitch = 2,
    }
```
