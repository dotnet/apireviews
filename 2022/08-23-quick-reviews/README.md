# API Review 08/23/2022

## [Analyzer] Prevent behavioral change in built-in operators for IntPtr and UIntPtr

**Approved** | [#runtime/74022](https://github.com/dotnet/runtime/issues/74022#issuecomment-1224524650) | [Video](https://www.youtube.com/watch?v=2cVDxAEITn0&t=0h0m0s)

Looks good as proposed.

Severity: Warning
Category: Reliability
## List<T>.Slice

**Approved** | [#runtime/66773](https://github.com/dotnet/runtime/issues/66773#issuecomment-1224682686) | [Video](https://www.youtube.com/watch?v=2cVDxAEITn0&t=0h23m45s)

* Looks good as proposed. We should consider adding to immutable collections.

```diff
namespace System.Collections.Generic
{
    public class List<T>
    {
+        public List<T> Slice(int start, int length);
    }
}
```
