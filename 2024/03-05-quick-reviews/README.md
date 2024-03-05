# API Review 03/05/2024

## Attribute model for feature APIs

**Approved** | [#runtime/96859](https://github.com/dotnet/runtime/issues/96859#issuecomment-1979450258) | [Video](https://www.youtube.com/watch?v=GhPvUdnY34I&t=0h0m0s)


* RequiresFeature was removed due to a clarified scope of need, and considering the complications with an analyzer (all the features using the same diagnostic ID)
* FeatureGuard ("the dependency flow") was removed for simplicity
* FeatureCheck ("I answer whether this thing is enabled") is renamed to FeatureGuard to be consistent with OSPlatformGuard.
* We made both remaining attributes apply only to AttributeTargets.Property.  There should ideally be an analyzer which restricts this to being applied to static get-only properties.
  * Potential enhancements, like methods, fields, and does-not-return-unless are deferred.

```c#
namespace System.Diagnostics.CodeAnalysis;

[AttributeUsage(AttributeTargets.Property, Inherited = false, AllowMultiple = true)]
public sealed class FeatureGuardAttribute : Attribute
{
    public Type FeatureType { get; }
    public FeatureGuardAttribute(Type featureType)
}

[AttributeUsage(AttributeTargets.Property, Inherited = false)]
public sealed class FeatureSwitchDefinitionAttribute : Attribute
{
    public string SwitchName { get; }
    public FeatureSwitchDefinitionAttribute(string switchName)
}
```
## Win11 Theming - Icon element support

**Approved** | [#wpf/8647](https://github.com/dotnet/wpf/issues/8647#issuecomment-1979506669) | [Video](https://www.youtube.com/watch?v=GhPvUdnY34I&t=0h57m27s)


* IconSourceElementConverter being an `internal` class is probably more consistent with the rest of WPF, but if it's needed as public then it's fine.
* Either approach seems fine, and are approved as alternatives of each other; no further discussion is necessary provided they are selected from, and not changed.

### Alternative 1

```c#
namespace System.Windows.Controls;

[TypeConverter(typeof(IconElementConverter))]
public abstract class IconElement : FrameworkElement
{
    protected abstract UIElement InitializeChildren();
    protected IconElement();

    public static readonly DependencyProperty ForegroundProperty;
    [Bindable(true)]
    [Category("Appearance")]
    public Brush Foreground { get; set; }
}

public class FontIcon : IconElement
{
    public FontIcon();

    public static readonly DependencyProperty FontFamilyProperty;
    [Bindable(true)]
    [Category("Appearance")]
    [Localizability(LocalizationCategory.Font)]
    public FontFamily FontFamily { get; set; }

    public static readonly DependencyProperty FontSizeProperty;
    [Bindable(true)]
    [Category("Appearance")]
    [TypeConverter(typeof(FontSizeConverter))]
    [Localizability(LocalizationCategory.None)]
    public double FontSize { get; set; }

    public static readonly DependencyProperty FontStyleProperty;
    [Bindable(true)]
    [Category("Appearance")]
    public FontStyle FontStyle { get; set; }

    public static readonly DependencyProperty FontWeightProperty;
    [Bindable(true)]
    [Category("Appearance")]
    public FontWeight FontWeight { get; set; }

    public static readonly DependencyProperty GlyphProperty;
    public string Glyph { get; set; }
}

[ContentProperty("IconSource")]
public class IconSourceElement : IconElement
{
    public IconSourceElement();

    public static readonly DependencyProperty IconSourceProperty;
    public IconSource IconSource { get; set; }
}

[ContentProperty("IconSource")]
public class ImageIcon : IconElement
{
    public ImageIcon();

    public static readonly DependencyProperty SourceProperty;
    public ImageSource Source { get; set; }
}

public abstract class IconSource : DependencyObject
{
    protected IconSource();
    public static readonly DependencyProperty ForegroundProperty;
    public Brush Foreground { get; set; }
    public abstract IconElement CreateIconElement();
}

public class FontIconSource : IconSource
{
    public FontIconSource();

    public static readonly DependencyProperty FontFamilyProperty;
    public FontFamily FontFamily { get; set; }

    public static readonly DependencyProperty FontSizeProperty;
    public double FontSize { get; set; }

    public static readonly DependencyProperty FontStyleProperty;
    public FontStyle FontStyle { get; set; }

    public static readonly DependencyProperty FontWeightProperty;
    public FontWeight FontWeight { get; set; }

    public static readonly DependencyProperty GlyphProperty;
    public string Glyph { get; set; }
}

public partial class IconSourceElementConverter : IValueConverter
{
    public IconSourceElementConverter();
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture);
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture);
}

[ContentProperty(nameof(Source))]
[MarkupExtensionReturnType(typeof(ImageIcon))]
public class ImageIconExtension : MarkupExtension
{
    public ImageIconExtension(ImageSource? source);
    [ConstructorArgument("source")]
    public ImageSource? Source { get; set; }
    public double Width { get; set; } = 16D;
    public double Height { get; set; } = 16D;
}

[ContentProperty(nameof(Glyph))]
[MarkupExtensionReturnType(typeof(FontIcon))]
public class FontIconExtension : MarkupExtension
{
    public FontIconExtension(string glyph);
    public FontIconExtension(string glyph, FontFamily fontFamily);

    [ConstructorArgument("glyph")]
    public string Glyph { get; set; }

    [ConstructorArgument("fontFamily")]
    public FontFamily FontFamily { get; set; }

    public double FontSize { get; set; }
}
```

### Alternative 2

```C#
    public partial class IconElement : System.Windows.FrameworkElement
    {
        public IconElement() { }
        
        public static readonly System.Windows.DependencyProperty IconSourceProperty;
        public System.Windows.Controls.IconSource IconSource { get { throw null; } set { } }
        
        protected virtual System.Windows.UIElement InitializeChildren() { throw null; }
        
        protected override int VisualChildrenCount { get { throw null; } }
        protected override System.Windows.Size ArrangeOverride(System.Windows.Size finalSize) { throw null; }
        protected override System.Windows.Media.Visual GetVisualChild(int index) { throw null; }
        protected override System.Windows.Size MeasureOverride(System.Windows.Size availableSize) { throw null; }
    }

        public abstract partial class IconSource : System.Windows.DependencyObject
    {
        protected IconSource() { }
        
        public static readonly System.Windows.DependencyProperty ForegroundProperty;
        public System.Windows.Media.Brush Foreground { get { throw null; } set { } }
        
        public abstract System.Windows.UIElement CreateIconContent();
    }

    public partial class FontIconSource : System.Windows.Controls.IconSource
    {
        public FontIconSource() { }
        
        public static readonly System.Windows.DependencyProperty FontFamilyProperty;
        public static readonly System.Windows.DependencyProperty FontSizeProperty;
        public static readonly System.Windows.DependencyProperty FontStyleProperty;
        public static readonly System.Windows.DependencyProperty FontWeightProperty;
        public static readonly System.Windows.DependencyProperty GlyphProperty;
        
        public System.Windows.Media.FontFamily FontFamily { get { throw null; } set { } }
        public double FontSize { get { throw null; } set { } }
        public System.Windows.FontStyle FontStyle { get { throw null; } set { } }
        public System.Windows.FontWeight FontWeight { get { throw null; } set { } }
        public string Glyph { get { throw null; } set { } }
        
        public override System.Windows.UIElement CreateIconContent() { throw null; }
    }

    [System.Windows.Markup.ContentPropertyAttribute("Glyph")]
    [System.Windows.Markup.MarkupExtensionReturnTypeAttribute(typeof(System.Windows.Controls.FontIconSource))]
    public partial class FontIconSourceExtension : System.Windows.Markup.MarkupExtension
    {
        public FontIconSourceExtension(string glyph) { }
        public FontIconSourceExtension(string glyph, System.Windows.Media.FontFamily fontFamily) { }
        
        [System.Windows.Markup.ConstructorArgumentAttribute("fontFamily")]
        public System.Windows.Media.FontFamily FontFamily { get { throw null; } set { } }
        
        public double FontSize { get { throw null; } set { } }
        
        [System.Windows.Markup.ConstructorArgumentAttribute("glyph")]
        public string Glyph { get { throw null; } set { } }
        
        public override object ProvideValue(System.IServiceProvider serviceProvider) { throw null; }
    }
```
## ParamCollectionAttribute

**Approved** | [#runtime/99285](https://github.com/dotnet/runtime/issues/99285#issuecomment-1979520298) | [Video](https://www.youtube.com/watch?v=GhPvUdnY34I&t=1h36m2s)


* It was asked whether there should be an "s" at the end of "Param", and the answer was "no".
* We also talked about the Inherited=true, and are sticking with "match what ParamArrayAttribute does", which is true.

```c#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Parameter, Inherited = true, AllowMultiple = false)]
    public sealed class ParamCollectionAttribute : Attribute
    {
        public ParamCollectionAttribute() { }
    }
}
```
## Add WebSocketOptions.Timeout to ClientWebSocket

**Approved** | [#runtime/48729](https://github.com/dotnet/runtime/issues/48729#issuecomment-1979542776) | [Video](https://www.youtube.com/watch?v=GhPvUdnY34I&t=1h47m56s)


Looks good as proposed.

```c#
namespace System.Net.WebSockets;

public partial class WebSocketCreationOptions
{
    public TimeSpan KeepAliveTimeout { get; set; } = Timeout.InfiniteTimeSpan;
}

public partial class ClientWebSocketOptions
{
    [UnsupportedOSPlatform("browser")]
    public TimeSpan KeepAliveTimeout { get; set; } = Timeout.InfiniteTimeSpan;
}
```
