# Quick Reviews 07/28/2020

## configurable HTTP/2 PING timeouts in HttpClient

**Approved** | [#runtime/31198](https://github.com/dotnet/runtime/issues/31198#issuecomment-665169571) | [Video](https://www.youtube.com/watch?v=Fe2BQCZ0Xdk&t=0h0m0s)

* It seems the setting `Always` is undesirable. Instead of adding this enum, we should just use `WithActiveRequests` as the default
* The API is reasonable an enum is preferred, but let's defer this until we know for we need a way to allow `Always` (even though all other clients have it).

```C#
namespace System.Net.Http
{
    public class SocketsHttpHandler
    {
        public TimeSpan KeepAlivePingDelay { get; set; }
        public TimeSpan KeepAlivePingTimeout { get; set; }
    }
}
```

## Add span overloads to `TextRenderer`

**Approved** | [#winforms/3651](https://github.com/dotnet/winforms/issues/3651#issuecomment-665176770) | [Video](https://www.youtube.com/watch?v=Fe2BQCZ0Xdk&t=0h16m47s)

* Looks good as proposed

```C#
namespace System.Windows.Forms
{
    public sealed class TextRenderer
    {
        public static void DrawText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Point pt, Color foreColor);
        public static void DrawText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Point pt, Color foreColor, Color backColor);
        public static void DrawText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Point pt, Color foreColor, Color backColor, TextFormatFlags flags);
        public static void DrawText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Point pt, Color foreColor, TextFormatFlags flags);
        public static void DrawText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Rectangle bounds, Color foreColor);
        public static void DrawText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Rectangle bounds, Color foreColor, Color backColor);
        public static void DrawText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Rectangle bounds, Color foreColor, Color backColor, TextFormatFlags flags);
        public static void DrawText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Rectangle bounds, Color foreColor, TextFormatFlags flags);
        public static Size MeasureText(IDeviceContext dc, ReadOnlySpan<char> text, Font font);
        public static Size MeasureText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Size proposedSize);
        public static Size MeasureText(IDeviceContext dc, ReadOnlySpan<char> text, Font font, Size proposedSize, TextFormatFlags flags);
        public static Size MeasureText(ReadOnlySpan<char> text, Font font);
        public static Size MeasureText(ReadOnlySpan<char> text, Font font, Size proposedSize);
        public static Size MeasureText(ReadOnlySpan<char> text, Font font, Size proposedSize, TextFormatFlags flags);
    }
}
```

## Unify MinimumOSPlatformAttribute and UnsupportedOSPlatformAttribute

**Approved** | [#runtime/39989](https://github.com/dotnet/runtime/issues/39989#issuecomment-665197220) | [Video](https://www.youtube.com/watch?v=Fe2BQCZ0Xdk&t=0h30m24s)

* We'll rename `MinimumOSPlatformAttribute` and `RemovedInOSPlatformAttribute` to `SupportedOSPlatformAttribute` and `UnsupportedOSPlatformAttribute` respectively
* We'll move them (plus `ObsoletedInOSPlatform`) from `System.Runtime.Versioning` to `System.Runtime.InteropServices` to co-locate them with them guarding API `RuntimeInformation`
* We don't like `InteropServices`. @terrajobst to follow up if w can have the guard methods on a type/namespace that isn't `RuntimeInformation`
* We'll leave both `OSPlatformAttribute` and `TargetPlatformAttribute` in `System.Runtime.Versioning`
* The difference between `net5.0-windows` and `[SupportedOSPlatform("windows)]` is that the TFM is expanded at build time to a concrete version, which for .NET 5 is `windows7.0` while the platform name in the attribute means `0.0`. The resulting behavior will be intuitive for the user, but it's an important distinction.
* We generally don't use `params` on attributes that can be applied multiple times

```C#
namespace System.Runtime.Versioning
{
    public abstract class OSPlatformAttribute : Attribute
    {
        private protected OSPlatformAttribute(string platformName);
        public string PlatformName { get; }
    }

    [AttributeUsage(AttributeTargets.Assembly,
                    AllowMultiple=false, Inherited=false)]
    public sealed class TargetPlatformAttribute : OSPlatformAttribute
    {
        public TargetPlatformAttribute(string platformName);
    }
}
namespace System.Runtime.InteropServices
{
    // Renamed from MinimumOSPlatformAttribute
    [AttributeUsage(AttributeTargets.Assembly |
                    AttributeTargets.Class |
                    AttributeTargets.Constructor |
                    AttributeTargets.Enum |
                    AttributeTargets.Event |
                    AttributeTargets.Field |
                    AttributeTargets.Method |
                    AttributeTargets.Module |
                    AttributeTargets.Property |
                    AttributeTargets.Struct,
                    AllowMultiple = true, Inherited = false)]
    public sealed class SupportedOSPlatformAttribute : OSPlatformAttribute
    {
        public SupportedOSPlatformAttribute(string platformName);
    }

    // RemovedInOSPlatformAttribute
    [AttributeUsage(AttributeTargets.Assembly |
                    AttributeTargets.Class |
                    AttributeTargets.Constructor |
                    AttributeTargets.Enum |
                    AttributeTargets.Event |
                    AttributeTargets.Field |
                    AttributeTargets.Method |
                    AttributeTargets.Module |
                    AttributeTargets.Property |
                    AttributeTargets.Struct,
                    AllowMultiple = true, Inherited = false)]
    public sealed class UnsupportedOSPlatformAttribute : OSPlatformAttribute
    {
        public UnsupportedOSPlatformAttribute(string platformName);
    }

    // Same shape, but different namespace
    [AttributeUsage(AttributeTargets.Assembly |
                    AttributeTargets.Class |
                    AttributeTargets.Constructor |
                    AttributeTargets.Event |
                    AttributeTargets.Method |
                    AttributeTargets.Module |
                    AttributeTargets.Property |
                    AttributeTargets.Struct,
                    AllowMultiple=true, Inherited=false)]
    public sealed class ObsoletedInOSPlatformAttribute : OSPlatformAttribute
    {
        public ObsoletedInPlatformAttribute(string platformName);
        public ObsoletedInPlatformAttribute(string platformName, string message);
        public string Message { get; }
        public string Url { get; set; }
    }
}
```

## SecureString obsoletions and shrouded buffer proposal

**NeedsWork** | [#designs/147](https://github.com/dotnet/designs/pull/147#issuecomment-665227008) | [Video](https://www.youtube.com/watch?v=Fe2BQCZ0Xdk&t=1h10m46s)

* We think we need to make decision whether or not the framework is interested in providing building blocks like `ShroudedBuffer<T>` in order for a customer to build hygienic solutions around handling privileged information, which also includes enables customers to build a PCI compliant solution. This requires and end-to-end how privileged is read into a process and how it leaves the process (file I/O, networking API, ASP.NET buffer pools, etc). This also includes GC behavior around copying memory, crash dumps, and logging.
* We should think through what it means to obsoleted `SecureString` for APIs that can only be used be used with it, such as many PowerShell Cmdlets and some of our networking/crypto APIs.
