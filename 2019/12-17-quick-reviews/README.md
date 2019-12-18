# Quick Reviews 12/17/2019

## MemoryMarshal.GetRawArrayData

**Approved** | [#corefx/36133](https://github.com/dotnet/corefx/issues/36133#issuecomment-566723424) | [Video](https://www.youtube.com/watch?v=VRTCAkk7wKU&t=0h0m0s)

We considered this:

```C#
namespace System.Runtime.CompilerServices
{
    public static class Unsafe
    {
        public static ref T GetRawArrayRef<T>(ref T source);
    }
}
```

but we'd prefer this:

```C#
namespace System.Runtime.InteropServices
{
    public static class MemoryMarshal
    {
        public static ref T GetReference<T>(T[] array);
    }
}
```

(Please not that's OK because the existing `GetReference<T>()` method don't bind for arrays due to an ambiguous match, so the caller had to add a cast.)
## API request : CompressionLevel.Highest

**Approved** | [#corefx/36348](https://github.com/dotnet/corefx/issues/36348#issuecomment-566725990) | [Video](https://www.youtube.com/watch?v=VRTCAkk7wKU&t=1h49m9s)

* We prefer `SmallestSize` to make it more clear what the API does (`Highest` or `Best` seem ambiguous with `Optimal`).

```C#
namespace System.IO.Compression
{
    public partial enum CompressionLevel
    {
        SmallestSize

        // Existing:
        // Fastest,
        // NoCompression,
        // Optimal
    }
}
```
