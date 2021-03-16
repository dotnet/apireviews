# Quick Reviews 03/16/2021

## Developers using JsonSerializer can asynchronously (de)serialize IAsyncEnumerable<T>

**NeedsWork** | [#runtime/1570](https://github.com/dotnet/runtime/issues/1570#issuecomment-800522775)

* Why do we need `SerializeAsyncEnumerable`? Can't we do that just in the implementation of the existing `SerializeAsync` method?
    - It seems the primary reason is to support additional parameters, but (1) it doesn't seem necessary and (2) it could be made part of the `JsonSerializerOptions`
* Do we need `DeserializeAsyncEnumerable`?
    - It seems we do, due to the different behavior of ownership of errors
    - Do we need to support this at all?

```C#
namespace System.Text.Json
{
    public static class JsonSerializer
    {
        public static Task SerializeAsyncEnumerable<TValue>(
            Stream utf8Json,
            IAsyncEnumerable<TValue> value,
            int flushInterval = 0,
            JsonSerializerOptions? options = null,
            CancellationToken cancellationToken = default);

        public static IAsyncEnumerable<TValue?> DeserializeAsyncEnumerable<TValue>(
            Stream utf8Json,
            JsonSerializerOptions? options = null,
            CancellationToken cancellationToken = default);
    }
}
```

## Enhance DynamicallyAccessedMembers Attribute to apply to class 

**Approved** | [#runtime/49465](https://github.com/dotnet/runtime/issues/49465#issuecomment-800524476)

* Looks good as proposed

```diff
 namespace System.Diagnostics.CodeAnalysis
 {
     [AttributeUsage(
+        AttributeTargets.Class |
         AttributeTargets.Field |
         AttributeTargets.GenericParameter |
+        AttributeTargets.Interface |
+        AttributeTargets.Struct |
         AttributeTargets.Method |
         AttributeTargets.Parameter |
         AttributeTargets.Property |
         AttributeTargets.ReturnValue,
         Inherited = false)]
     public partial class DynamicallyAccessedMembersAttribute : Attribute
     {
     }
 }
```
## Add `Empty` property to `BinaryData`

**Approved** | [#runtime/49670](https://github.com/dotnet/runtime/issues/49670#issuecomment-800527813)

* Looks good as proposed
* We can the analyzer/fixer suggestion from the existing analyzer but it should a get a new ID

```c#
namespace System
{
    public class BinaryData
    {
        public static BinaryData Empty { get; }
    }
}
```

