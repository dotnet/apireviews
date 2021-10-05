# API Review 10/05/2021

## Add ResourceManager.TryGetString API to avoid exceptions

**NeedsWork** | [#runtime/1290](https://github.com/dotnet/runtime/issues/1290#issuecomment-934650085) | [Video](https://www.youtube.com/watch?v=1-PVX45YGcU&t=0h0m0s)

Since the existing `GetString` returns `null` (vs throwing) when a specific resource is missing, it's unclear what the Try is trying... so we'd need more information to discuss this further.

Specific examples of where the exceptions are occurring would be useful, so we can decide what the appropriate remediation is.
## Add Indexer to Vectors

**Approved** | [#runtime/59924](https://github.com/dotnet/runtime/issues/59924#issuecomment-934656109) | [Video](https://www.youtube.com/watch?v=1-PVX45YGcU&t=0h11m19s)

Looks good as proposed.  We went ahead and added read-only indexers on the other Vector types

```C#
namespace System.Numerics
{
    public partial struct Vector2
    {
        public float this[int index] { get; set; }
    }

    public partial struct Vector3
    {
        public float this[int index] { get; set; }
    }
    
    public partial struct Vector4
    {
        public float this[int index] { get; set; }
    }

    public partial struct Quaternion
    {
        public float this[int index] { get; set; }
    }

    public partial struct Matrix3x2
    {
        public float this[int row, int column] { get; set; }
    }

    public partial struct Matrix4x4
    {
        public float this[int row, int column] { get; set; }
    }
}

namespace System.Runtime.Intrinsics
{
    public partial struct Vector64<T>
    {
        public T this[int index] { get; }
    }

    public partial struct Vector128<T>
    {
        public T this[int index] { get; }
    }

    public partial struct Vector256<T>
    {
        public T this[int index] { get; }
    }
}
```
## Add WhereNotNull to help with nullability tracking

**Rejected** | [#runtime/30381](https://github.com/dotnet/runtime/issues/30381#issuecomment-934681792) | [Video](https://www.youtube.com/watch?v=1-PVX45YGcU&t=0h19m29s)

Given that it would require people to change their code, and the current analysis says it's only about 3% of libraries where it's even possibly related; and that the main use should ideally be solved by the compiler... there's no need for it in the BCL.
