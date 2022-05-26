# API Review 05/26/2022

## Improve HostBuilder and WebApplicationBuilder Configuration integration

**Approved** | [#runtime/61634](https://github.com/dotnet/runtime/issues/61634#issuecomment-1138816628)

The rename looks good as proposed.

```diff
namespace Microsoft.Extensions.Hosting
{
    public static class SystemdServiceCollectionExtensions
    {
-        public static IServiceCollection UseSystemd(this IServiceCollection services);
+        public static IServiceCollection AddSystemd(this IServiceCollection services);
    }
    public static class WindowsServiceLifetimeServiceCollectionExtensions
    {
-        public static IServiceCollection UseWindowsService(this IServiceCollection services);
+        public static IServiceCollection AddWindowsService(this IServiceCollection services);
-        public static IServiceCollection UseWindowsService(this IServiceCollection services, Action<WindowsServiceLifetimeOptions> configure);
+        public static IServiceCollection AddWindowsService(this IServiceCollection services, Action<WindowsServiceLifetimeOptions> configure);
    }
}
```
## new System.ComponentModel.DateOnlyConverter and System.ComponentModel.TimeOnlyConverter classes

**Approved** | [#runtime/68743](https://github.com/dotnet/runtime/issues/68743#issuecomment-1138838418)

We added System.Half, Int128, and UInt128 to the list to get caught up on primitive-ish types in the TypeConverter space.  (We didn't think there was enough of a scenario for NFloat, NInt, or NUInt).

```C#
namespace System.ComponentModel
{
    public class DateOnlyConverter : System.ComponentModel.TypeConverter
    {
        public DateOnlyConverter() { }
    }

    public class TimeOnlyConverter : System.ComponentModel.TypeConverter
    {
        public TimeOnlyConverter() { }
    }

    public class HalfConverter : System.ComponentModel.BaseNumberConverter
    {
        public HalfConverter() { }
    }

    public class Int128Converter : System.ComponentModel.BaseNumberConverter
    {
        public Int128Converter() { }
    }

    public class UInt128Converter : System.ComponentModel.BaseNumberConverter
    {
        public UInt128Converter() { }
    }
}
```
## Reflection introspection support for FunctionPointer

**Approved** | [#runtime/69273](https://github.com/dotnet/runtime/issues/69273)

## add remaining APIs to complete NativeMemory set

**Approved** | [#runtime/69606](https://github.com/dotnet/runtime/issues/69606#issuecomment-1138911515)

Looks good as proposed.

```diff
 namespace System.Runtime.InteropServices;

 public static class NativeMemory
 {
-    public static void ZeroMemory(void* ptr, nuint byteCount); // Recently approved, to be replaced by Clear for consistency
+    public static void Clear(void* ptr, nuint byteCount);
+    public static void Fill(void* ptr, nuint byteCount, byte value);
+    public static void Copy(void* source, void* destination, nuint byteCount);
 }
 
 // Existing similar methods on Array for reference
 public class Array
 {
     public static void Clear(System.Array array); 
     public static void Fill(T[] array, T value);
     public static void Copy(System.Array sourceArray, System.Array destinationArray, int length); 
 }
```

## PemEncoding.WriteString

**Approved** | [#runtime/65144](https://github.com/dotnet/runtime/issues/65144#issuecomment-1138913397)

Looks good as proposed

```C#
namespace System.Security.Cryptography {
    public static class PemEncoding {
        public static string WriteString(ReadOnlySpan<char> label, ReadOnlySpan<byte> data);
    }
}
```
