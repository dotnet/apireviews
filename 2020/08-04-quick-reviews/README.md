# Quick Reviews 8/4/2020

## Exception types for System.Net.Connections

**Approved** | [#runtime/39941](https://github.com/dotnet/runtime/issues/39941#issuecomment-668737191) | [Video](https://www.youtube.com/watch?v=O6zAv29ak1g&t=0h0m0s)

* We generally recommend against either extreme (one exception with lots of error codes or as many exceptions as error states) because it creates a mess.
* Instead, we recommend to have one exception type per mainline scenario that the user is likely to write error handling for. It's OK if more advanced cases involve inspecting the exception's error code or inner exception.
* It seems Option B is closest to that.
* We can even introduce dedicated exception types for specific error codes later, so long they extend the generic type and have the proper error code. We should exposing a factory so that people can construct an exception given an error code. This means the constructor should be protected.
* It seems *abort* and *reset* would be candidates for dedicated types.

```C#
namespace System.IO
{
    public class NetworkException : IOException
    {
        public static NetworkException Create(NetworkError error, Exception innerException = null, string message = null);
        protected NetworkException(NetworkError error, Exception innerException = null, string message = null);
        // serialization constructor as usual

        public NetworkError NetworkError { get; }
    }

    public enum NetworkError
    {
        Unknown,

        AddressInUse,       // SocketError.AddressAlreadyInUse
        InvalidAddress,     // SocketError.AddressFamilyNotSupported, SocketError.AddressNotAvailable, etc
        ConnectionRefused,  // SocketError.ConnectionRefused
        HostNotFound,       // SocketError.HostNotFound, SocketError.HostUnreachable, etc

        ConnectionAborted,
        ConnectionReset,
    }
}
```

## Moving platform guards out of System.Runtime.InteropServices

**Approved** | [#runtime/40111](https://github.com/dotnet/runtime/issues/40111#issuecomment-668771421) | [Video](https://www.youtube.com/watch?v=O6zAv29ak1g&t=0h47m13s)

* We'll capitalize the first letter in Apple platforms when they are part of a longer name, such as `IsTvOS` (instead of `IstvOS`).
* We've decided to move all members to `OperatingSystem`, which doesn't expose any statics today. This avoids creating a dumping ground in `Environment`.
* We'll try to pull the members from `OSPlatform` we added in the earlier .NET 5 preview
* `FreeBSD` has versions, so it should have `IsFreeBSDVersionAtLeast`
* We agreed to move all attributes to `System.Runtime.Versioning`. Most developers will apply these attributes through code completion or code fixers, so namespace discoverability won't be a big issue, but polluting the `System` namespace is a big concern.

```C#
namespace System
{
    public partial class OperatingSystem
    {
        // Primary
        public static bool IsOSPlatform(string platform);
        public static bool IsOSPlatformVersionAtLeast(string platform, int major, int minor = 0, int build = 0, int revision = 0);

        // Accelerators for platforms where versions don't make sense
        public static bool IsBrowser();
        public static bool IsLinux();

        // Accelerators with version checks

        public static bool IsFreeBSD();
        public static bool IsFreeBSDVersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0);

        public static bool IsAndroid();
        public static bool IsAndroidVersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0);

        public static bool IsIOS();
        public static bool IsIOSVersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0);

        public static bool IsMacOS();
        public static bool IsMacOSVersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0);

        public static bool IsTvOS();
        public static bool IsTvOSVersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0);

        public static bool IsWatchOS();
        public static bool IsWatchOSVersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0);

        public static bool IsWindows();
        public static bool IsWindowsVersionAtLeast(int major, int minor = 0, int build = 0, int revision = 0);
    }
}
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

```diff
namespace System.Runtime.InteropServices
{
    public readonly partial struct OSPlatform
    {
-       public static System.Runtime.InteropServices.OSPlatform Android { get; }
-       public static System.Runtime.InteropServices.OSPlatform Browser { get; }
        public static System.Runtime.InteropServices.OSPlatform FreeBSD { get; }
-       public static System.Runtime.InteropServices.OSPlatform iOS { get; }
        public static System.Runtime.InteropServices.OSPlatform Linux { get; }
-       public static System.Runtime.InteropServices.OSPlatform macOS { get; }
-       [System.ComponentModel.EditorBrowsable(System.ComponentModel.EditorBrowsableState.Never)]
        public static System.Runtime.InteropServices.OSPlatform OSX { get; }
-       public static System.Runtime.InteropServices.OSPlatform tvOS { get; }
-       public static System.Runtime.InteropServices.OSPlatform watchOS { get; }
        public static System.Runtime.InteropServices.OSPlatform Windows { get; }
    }
}
```

