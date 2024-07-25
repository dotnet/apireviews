# API Review 07/25/2024

## Span parameter that could be a ReadOnlySpan parameter

**Approved** | [#runtime/96587](https://github.com/dotnet/runtime/issues/96587#issuecomment-2251025182) | [Video](https://www.youtube.com/watch?v=mUMGod4PVq8&t=0h0m0s)

* Detect when an input parameter or a local could be ReadOnlySpan or ReadOnlyMemory instead of Span or Memory
* The analyzer shouldn't analyze parameters on overridden members
* The analyzer will probably not provide analysis for a Span that gets used in a `fixed` statement, rather than decide if it has read semantics or write semantics on the pointer.
* It should be on by default for non-visible members, with an opt-in for public.
  * Pipedream: It should run on "new code", if we could determine that.

Fixer: Obviously
Category: Reliability
Severity: Info
## Warn on use of StreamReader.EndOfStream in async methods

**Approved** | [#runtime/98834](https://github.com/dotnet/runtime/issues/98834#issuecomment-2251024587) | [Video](https://www.youtube.com/watch?v=mUMGod4PVq8&t=0h15m40s)

* If we find multiple places that we would want to do this we should invent an attribute, but for now we can just special-case the StreamReader.EndOfStream property

Category: Reliability
Visibility: Warning
## [System.Text.Json] Add JsonPascalCaseNamingPolicy.

**Approved** | [#runtime/34114](https://github.com/dotnet/runtime/issues/34114#issuecomment-2251047129) | [Video](https://www.youtube.com/watch?v=mUMGod4PVq8&t=0h23m23s)

Looks good as proposed

```C#
namespace System.Text.Json
{
    public partial class JsonNamingPolicy
    {
        public static JsonNamingPolicy PascalCase { get; }
    }
}

namespace System.Text.Json.Serialization
{
    public partial enum JsonKnownNamingPolicy
    {
        PascalCase = 6,
    }
}
```
## `ObjectFactory<T>` delegate should be covariant

**Approved** | [#runtime/101829](https://github.com/dotnet/runtime/issues/101829#issuecomment-2251059470) | [Video](https://www.youtube.com/watch?v=mUMGod4PVq8&t=0h28m28s)

Looks good as proposed

```diff
namespace Microsoft.Extensions.DependencyInjection;
- public delegate T ObjectFactory<T>(IServiceProvider serviceProvider, object?[]? arguments);
+ public delegate T ObjectFactory<out T>(IServiceProvider serviceProvider, object?[]? arguments);
```
## Create and convenience APIs for System.Numerics types

**Approved** | [#runtime/103464](https://github.com/dotnet/runtime/issues/103464#issuecomment-2251088410) | [Video](https://www.youtube.com/watch?v=mUMGod4PVq8&t=0h35m21s)

* We decided to be opinionated and remove the setters.
* GetRow(int index) => GetRow(int row)

```C#
namespace System.Numerics
{
    public partial struct Matrix3x2
    {
        public static Matrix3x2 Create(float value);
        public static Matrix3x2 Create(Vector2 x, Vector2 y, Vector2 z);

        public Vector2 X { readonly get; }
        public Vector2 Y { readonly get; }
        public Vector2 Z { readonly get; }

        public Vector2 this[int row] { readonly get; }

        public readonly Vector2 GetRow(int row);
        public readonly Matrix3x2 WithRow(int row, Vector2 value);

        public readonly float GetElement(int row, int column);
        public readonly Matrix3x2 WithElement(int row, int column, float value);
    }

    public partial struct Matrix4x4
    {
        public static Matrix4x4 Create(float value);
        public static Matrix4x4 Create(Vector4 x, Vector4 y, Vector4 z, Vector4 w);

        public Vector4 X { readonly get; }
        public Vector4 Y { readonly get; }
        public Vector4 Z { readonly get; }
        public Vector4 W { readonly get; }

        public Vector4 this[int row] { readonly get; }

        public readonly Vector4 GetRow(int row);
        public readonly Matrix4x4 WithRow(int row, Vector4 value);

        public readonly float GetElement(int row, int column);
        public readonly Matrix4x4 WithElement(int row, int column, float value);
    }

    public partial struct Plane
    {
        public static Plane Create(Vector3 normal, float d);
        public static Plane Create(float x, float y, float z, float d);
    }

    public partial struct Quaternion
    {
        public static Quaternion Create(Vector3 vectorPart, float scalarPart);
        public static Quaternion Create(float x, float y, float z, float w);
    }

    public static partial class Vector
    {
        public static Vector2 AsVector2(Vector3 value);

        public static unsafe Vector<T> CreateScalar<T>(T value);
        public static Vector<T> CreateScalarUnsafe<T>(T value);
    }

    public partial struct Vector2
    {
        public static Vector2 CreateScalar(float x);
        public static Vector2 CreateScalarUnsafe(float x)
    }

    public partial struct Vector3
    {
        public static Vector3 CreateScalar(float x);
        public static Vector3 CreateScalarUnsafe(float x);
    }

    public partial struct Vector4
    {
        public static Vector4 CreateScalar(float x);
        public static Vector4 CreateScalarUnsafe(float x);
    }
}

namespace System.Runtime.Intrinsics
{
    public static partial class Vector128
    {
        public static Plane AsPlane(this Vector128<float> value);
        public static Quaternion AsQuaternion(this Vector128<float> value);

        public static Vector128<float> AsVector128(this Plane value);
        public static Vector128<float> AsVector128(this Quaternion value);
    }
}
```
## `GetKeyedService` overload that takes a `System.Type`

**Approved** | [#runtime/102816](https://github.com/dotnet/runtime/issues/102816#issuecomment-2251097466) | [Video](https://www.youtube.com/watch?v=mUMGod4PVq8&t=0h51m45s)

Looks good as proposed

```C#
namespace Microsoft.Extensions.DependencyInjection
{
  public static partial class ServiceProviderKeyedServiceExtensions
  {
    public static object? GetKeyedService(this IServiceProvider provider, Type serviceType, object? serviceKey);
  }
}
```
## Console.Write* convenience overloads for span

**Approved** | [#runtime/103500](https://github.com/dotnet/runtime/issues/103500#issuecomment-2251104362) | [Video](https://www.youtube.com/watch?v=mUMGod4PVq8&t=0h55m57s)

Looks good as proposed

```C#
namespace System;

public static class Console
{
    public static void Write(ReadOnlySpan<char> value);
    public static void WriteLine(ReadOnlySpan<char> value);
}
```
