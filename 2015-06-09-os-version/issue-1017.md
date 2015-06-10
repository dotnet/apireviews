```diff
---E:\ProjectK\binaries\x86ret\Contracts\System.Runtime.Environment\4.0.0.0\System.Runtime.Environment.dll
+++E:\ProjectK\binaries\x86ret\Contracts\System.Runtime.Environment\4.0.10.0\System.Runtime.Environment.dll
 assembly System.Runtime.Environment {
  namespace System.Runtime.InteropServices {
-   [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct OSName : IEquatable<OSName> {
      ^ Immo Landwerth: OSName is not a good name. Make it OSPlatform.
      ^ Immo Landwerth: We assume that using the default ctor doesn't cause null refs down the road
      ^ Immo Landwerth: The general consensus seems to be that either approach would work, but we like the struct based solution a bit more.
-     public static readonly OSName Linux;
      ^ Immo Landwerth: These should all bit static read-only properties, mostly for consistency
-     public static readonly OSName OSX;
-     public static readonly OSName Windows;
-     public OSName(string name);
      ^ Immo Landwerth: Replace with Create(). This forces folks to scratch their head when the write `new OSName(` and nothing shows up.
-     public override bool Equals(object obj);
-     public bool Equals(OSName other);
-     public override int GetHashCode();
-     public static bool operator ==(OSName left, OSName right);
-     public static bool operator !=(OSName left, OSName right);
-     public override string ToString();
    }
+   public enum OSPlatforms {
      ^ Immo Landwerth: Remove
+     Linux = 2,
+     OSX = 4,
+     Windows = 1,
    }
    public static class RuntimeInformation {
      ^ Immo Landwerth: We're fine with the name.
-     public static bool IsOperatingSystem(OSName osName);
      ^ Immo Landwerth: Bring back and name IsOSPlatform
+     public static bool IsOSPlatform(OSPlatforms osPlatforms);
      ^ Immo Landwerth: Remove
      ^ Immo Landwerth: The nice thing abut this API pattern is that we can return true for multiple values.
    }
  }
 }
```