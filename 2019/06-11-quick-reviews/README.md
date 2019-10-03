# Quick Reviews 6/11/2019

## Add a per Activity API to set the default ActivityIdFormat

**Approved** | [#corefx/36999](https://github.com/dotnet/corefx/issues/36999#issuecomment-500941214) | [Video](https://www.youtube.com/watch?v=XWSjh7qkHkg&t=-9h-52m-44s)

Personally, I'd prefer this being a property but it seems folks on the thread have strong opinion that this should be method, which is also accetable.

If that's a method it should be called `SetIdFormat`.

```C#
public void SetIdFormat(ActivityIdFormat idFormat);
```
## Add DisplayUnits property to EventCounter and friends

**Approved** | [#corefx/37510](https://github.com/dotnet/corefx/issues/37510#issuecomment-500943921) | [Video](https://www.youtube.com/watch?v=XWSjh7qkHkg&t=0h16m14s)

Looks like we're OK with this. The default return value would be `string.Empty`.

```C#
namespace System.Diagnostics.Tracing
{
    public partial abstract class DiagnosticCounter
    {
        public string DisplayUnits { get; }
    }
}
```
## [API review] Add Timers.Count

**Approved** | [#corefx/38422](https://github.com/dotnet/corefx/issues/38422#issuecomment-500951300) | [Video](https://www.youtube.com/watch?v=XWSjh7qkHkg&t=0h23m2s)

After long discussion, we agreed on:

```C#
namespace System.Threading
{
    public partial class Timer
    {
        public static long ActiveCount { get; }
    }
}
```
## Add NullableAttribute

**Rejected** | [#corefx/36222](https://github.com/dotnet/corefx/issues/36222#issuecomment-500957428) | [Video](https://www.youtube.com/watch?v=XWSjh7qkHkg&t=0h42m59s)

We decided to keep doing the code spit because

1. Any code reasoning about the attribute has to assume that attribute is emitted, so you can't optimize the path.
2. The size is small
