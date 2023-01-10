# API Review 01/10/2023

## [LSG] Logging Source Generator fails to compile when `out` parameter modifier is present

**Approved** | [#runtime/64665](https://github.com/dotnet/runtime/issues/64665#issuecomment-1377723960) | [Video](https://www.youtube.com/watch?v=9NVOGevsa6A&t=0h0m0s)

Looks good as proposed.
## [LSG] Logging Source Generator fails to detect event name uniqueness within a class scope

**Approved** | [#runtime/64667](https://github.com/dotnet/runtime/issues/64667#issuecomment-1377726162) | [Video](https://www.youtube.com/watch?v=9NVOGevsa6A&t=0h6m4s)

Looks good as proposed
## Add ability to safely get the managed object from a ComWrappers CCW

**Approved** | [#runtime/79674](https://github.com/dotnet/runtime/issues/79674#issuecomment-1377735832) | [Video](https://www.youtube.com/watch?v=9NVOGevsa6A&t=0h7m53s)

Looks good as proposed.

```C#
namespace System.Runtime.InteropServices;

public partial class ComWrappers
{
    public static bool TryGetObject(void* unknown, [NotNullWhen(true)] out object? obj);
}
```
## Support getting IUnknown for ComWrappers RCWs

**Approved** | [#runtime/79679](https://github.com/dotnet/runtime/issues/79679#issuecomment-1377747811) | [Video](https://www.youtube.com/watch?v=9NVOGevsa6A&t=0h16m29s)

Looks good as proposed.

```C#
namespace System.Runtime.InteropServices;

public partial class ComWrappers
{
    public static bool TryGetComInstance(object? obj, out void* unknown);
}
```
## Change visibility of ComWrappers.GetIUnknownImpl to public

**Approved** | [#runtime/80451](https://github.com/dotnet/runtime/issues/80451#issuecomment-1377762886) | [Video](https://www.youtube.com/watch?v=9NVOGevsa6A&t=0h27m12s)

* It might be worthwhile adding an overload that returns functions pointers. One would need to agree on the shape though -- with and without `MemberFunction` for instance.

```C#
namespace System.Runtime.InteropServices;

public abstract class ComWrappers
{
    // Existing API; this changes visibility only
    /*protected*/ public static void GetIUnknownImpl (out IntPtr fpQueryInterface, out IntPtr fpAddRef, out IntPtr fpRelease);
}
```
