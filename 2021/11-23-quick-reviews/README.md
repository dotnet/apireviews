# API Review 11/23/2021

## WinForm class OpenFileDialog and SaveFileDialog don't support OFN_DONTADDTORECENT flag

**Approved** | [#winforms/5405](https://github.com/dotnet/winforms/issues/5405#issuecomment-976969463) | [Video](https://www.youtube.com/watch?v=D9g8gYdViPY&t=0h0m0s)

* Makes sense
* Do we/should we replicate some of these in WPF's dialog (`OpenFileDialog` and `SaveFileDialog`)?

```C#
namespace System.Windows.Forms
{
    public partial class FileDialog
    {
        [DefaultValue(true)]
        public bool AddToRecent { get; set; } = true;

        [DefaultValue(false)]
        public bool ShowHiddenFiles { get; set; }

        [DefaultValue(true)]
        public bool ShowPinnedPlaces { get; set; } = true;

        [DefaultValue(false)]
        public bool OkRequiresInteraction { get; set; }
    }
    public partial class OpenFileDialog
    {
        [DefaultValue(false)]
        public bool ShowPreview { get; set; }

        [DefaultValue(false)]
        public bool SelectReadOnly { get; set; }
    }
    public partial class SaveFileDialog
    {
        [DefaultValue(true)]
        public bool ExpandedMode { get; set; } = true;

        [DefaultValue(true)]
        public bool CheckWriteAccess { get; set; } = true;
    }
    public partial class FolderBrowserDialog
    {
        [DefaultValue(true)]
        public bool AddToRecent { get; set; } = true;

        [DefaultValue(false)]
        public bool ShowHiddenFiles { get; set; }

        [DefaultValue(true)]
        public bool ShowPinnedPlaces { get; set; } = true;

        [DefaultValue(false)]
        public bool OkRequiresInteraction { get; set; }
    }
}
```
## System.DirectoryServices.Protocols.LdapFilter

**NeedsWork** | [#runtime/59900](https://github.com/dotnet/runtime/issues/59900#issuecomment-976996837) | [Video](https://www.youtube.com/watch?v=D9g8gYdViPY&t=0h13m16s)

* Do we need a new type? It seems more sensible to put it on an existing type.
    - `SearchRequest`
    - `LdapConnection` / `DirectoryConnection`
* Is this specific to filters? If not, should we remove the term "Filter" from the method?
* Do we need more a complex concept, such as builder-based approach?

```C#
namespace System.DirectoryServices.Protocols
{
    public static class LdapFilter
    {
        public static string EscapeFilterArgument(string filterArgument);
    }
}
```

## InlineArrayAttribute

**Approved** | [#runtime/61135](https://github.com/dotnet/runtime/issues/61135#issuecomment-977018394) | [Video](https://www.youtube.com/watch?v=D9g8gYdViPY&t=0h33m45s)

* Makes sense as proposed
* The assumptions are:
    - Runtimes that don't support this layout must not expose this attribute, which is why we don't need a `RuntimeFeature` field.
    - The developer won't apply the attribute manually, but uses language syntax and the compiler will emit the attribute, hence `System.Runtime.CompilerServices` is the appropriate home.

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Struct, AllowMultiple = false)]
    public sealed class InlineArrayAttribute : Attribute
    {
        public InlineArrayAttribute (int length);
        public int Length { get; }
    }
}
```

## Update the runtime to have deterministic floating-point to integer conversions

**Approved** | [#runtime/61885](https://github.com/dotnet/runtime/issues/61885#issuecomment-977069589) | [Video](https://www.youtube.com/watch?v=D9g8gYdViPY&t=0h50m53s)

* Without generic math/statics in interfaces, we'd prefer a design where those methods live on a dedicated type
* If we plan to expose them in generic math (and it seems we do), then it seems those need to exist on the primitive types; the only design choice we'd have is to hide them there. This would mean that people using generic math will have one way to call these "unsafe" methods while the people doing regular math/conversions have a different way of calling those.
* We could also decide that we don't expose these "unsafe" variants not on the generic interfaces and instead tell those developers to use regular type checks and invoke the corresponding methods on the dedicated type. However, for folks doing generic math this might stick out like a sore thumb.
* It seems we'd prefer a generic patten where we cut down the number of methods by passing in the output type:
    ```C#
    namespace System
    {
        public struct Double
        {
            public static TInteger ConvertToInteger<TInteger>(double value)
                where TInteger: IBinaryInteger;
            public static TInteger ConvertToIntegerNative<TInteger>(double value)
                where TInteger: IBinaryInteger;
        }
    }
    ```

